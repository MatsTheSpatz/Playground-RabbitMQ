# Access Control

**Links**
- [Access Control: Authentication and Authorization on Server](https://www.rabbitmq.com/authentication.html)
- [Client authentication: SASL](https://www.rabbitmq.com/authentication.html)
- [SASL, AuthN, AuthZ](http://www.rabbitmq.com/blog/2011/02/07/who-are-you-authentication-and-authorisation-in-rabbitmq-231/)

**3 pieces of info**:
1. where does user authentication info for server live (-> RabbitMQ > Access Control > AuthN)
2. where does permission info for server live (-> RabbitMQ > Access Control > AuthZ)
3. how does the client provide its identity over the wire  (-> RabbitMQ > SASL)




## Use RSR authentication

---
#### Start a RabbitMQ container
```` 
// Defaullt container
// username: guest / password: guest
// 8080 -> mgmt access
// 5672 -> regular cleint connections
docker container run -d --hostname host-rabbit-1 --name container-rabbit-1 
  -e RABBITMQ_ERLANG_COOKIE='matserlangcookie' -p 8080:15672 -p 5672:5672 rabbitmq:management

// Get into container
 docker container exec -it container-rabbit-1 /bin/bash 

````

---
#### Find location of Rabbit config-file

The active configuration file can be verified by inspecting the RabbitMQ log file. 
It will show up in the log file at the top, along with the other broker boot log entries.

RabbitMQ-image is configured to log to syslog - hence check docker container log: 
````
docker logs container-rabbit-1
// config-location is listed at beginning of file, e.g.:
//   node           : rabbit@host-rabbit-1
//   home dir       : /var/lib/rabbitmq
//   config file(s) : /etc/rabbitmq/rabbitmq.conf

````

Remark 1: Config-file location can be overridden by environment-variable. Check: ````printenv RABBITMQ_CONFIG_FILE ````

Reamrk 2: Config-file location can also be found in the management UI, together with other details about nodes.
*??? NOT FOUND ???**



---
#### Active Configuration

The effective configuration (user provided values merged into defaults) can be listed using:
````
rabbitmqctl environment
````


---
#### Enable Plugin 'rabbitmq_auth_backend_http'

See [plugin info](https://github.com/rabbitmq/rabbitmq-auth-backend-http)

````
rabbitmq-plugins enable rabbitmq_auth_backend_http
````

**QUESITON**: Is this needed? Is is (standrad-supported) plugin started automatically when something applies to it in config? 


---
#### Modifying "rabbitmq.conf"
Added the following info (in the middle of the config-file - not sure order matters).

````
auth_backends.1 = internal
auth_backends.2 = http
auth_http.user_path = http://a4e947cd.ngrok.io/auth/user
auth_http.vhost_path = http://a4e947cd.ngrok.io/auth/vhost
auth_http.resource_path = http://a4e947cd.ngrok.io/auth/resource
auth_http.topic_path = http://a4e947cd.ngrok.io/auth/topic
````

Remarks: 
- URL wired because used ngrok
- example keeps internal provider ("internal" is an alias for "rabbit_auth_backend_internal")
- could also split authN and authZ (see [here](https://www.rabbitmq.com/access-control.html) for more examples)


---
#### Copying "rabbitmq.conf" file into container
````
docker container cp ./rabbitmq.conf container-rabbit-1:/etc/rabbitmq/rabbitmq.conf
````
**QUESTION**: When do this config-changes take effect?

- Stop and start container seems to work: 
  - ````docker container stop container-rabbit-1````
  - ````docker container start container-rabbit-1````
-  What about ````rabbitmqctl stop_app```` then ````rabbitmqctl reset```` then ````rabbitmqctl start_app````


---
#### WebServer implementing 'Auth-Http' needs

See [here](https://github.com/rabbitmq/rabbitmq-auth-backend-http/blob/v3.7.7/README.md) what is expected by the web-server.

Draft implementation (working so far only for user-requests):
(requirest c# 7.2 at least)
```js
using System;
using System.Collections.Specialized;
using System.IO;
using System.Net;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleAppServerMock
{
    internal class Program
    {
        public static async Task Main(string[] args)
        {
            var server = new Server(new[] {"http://localhost:8080/auth/"}, ProcessRequestAsync);
            await server.StartAsync();
        }

        private static async Task ProcessRequestAsync(HttpListenerContext context)
        {
            HttpListenerRequest request = context.Request;
            Console.WriteLine("Processing new request at {0}", request.Url);
            string body = await new StreamReader(request.InputStream).ReadToEndAsync();

            NameValueCollection queryString = request.QueryString;
            for (int i = 0; i < queryString.Count; i++)
            {
                Console.WriteLine("{0}={1}", queryString.GetKey(i), queryString.Get(i));
            }
            
            // Response
            byte[] buf = Encoding.UTF8.GetBytes("allow administrator");

            HttpListenerResponse response = context.Response;
            response.StatusCode = 200;
            response.KeepAlive = false;
            response.Headers.Add("content-type: text/html; charset=UTF-8");
            response.Headers.Add("cache-control:no-store, no-cache, must-revalidate");
            response.Headers.Add("pragma:no-cache");
            response.ContentLength64 = buf.Length;
            await response.OutputStream.WriteAsync(buf, 0, buf.Length);
            await response.OutputStream.FlushAsync();
            response.Close(); // documentation: "send the response data to the client by calling the Close method."
        }

        private class Server
        {
            private readonly string[] prefixes;
            private readonly Func<HttpListenerContext, Task> processRequestAsync;

            public Server(string[] prefixes, Func<HttpListenerContext, Task> processRequestAsync)
            {
                this.prefixes = prefixes;
                this.processRequestAsync = processRequestAsync;
            }

            public async Task StartAsync()
            {
                HttpListener listener = new HttpListener();
                foreach (var prefix in prefixes)
                {
                    listener.Prefixes.Add(prefix);
                }

                listener.Start();
                Console.WriteLine("Server has started...");

                while (listener.IsListening)
                {
                    var context = await listener.GetContextAsync();
                    await processRequestAsync(context);
                }
            }
        }
    }
}

```
   