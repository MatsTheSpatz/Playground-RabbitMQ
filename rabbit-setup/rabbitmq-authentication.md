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

