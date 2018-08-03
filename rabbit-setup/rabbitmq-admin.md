# Access the Rabbit

### Cleint access
-  regular connections to node from clients -> port 5672


### Check broker:
- check status:         ````service rabbitmq-server status````
- get into container:   ```` docker container exec -it 4e21 /bin/bash````

--- 
### rabbitmqctl
https://www.rabbitmq.com/man/rabbitmqctl.8.html

Note: RabbitMQ plugins can extend rabbitmqctl tool to add new commands when enabled

---
### Management Plugin
https://www.rabbitmq.com/management.html

Provides an HTTP-based API for management and monitoring of server.

Default port: 15672

Comes wiht
- **HTTP-based API** --> http://server-name:15672/api/overview 
  - use Postman
  - basic auth (username & password) ()
- **Web-UI**  --> http://server-name:15672/
- **Command line tool [*rabbitmqadmin*](https://www.rabbitmq.com/management-cli.html)**. 
   - is a Python command line tool that interacts with the HTTP API. 
   - can be downloaded from any RabbitMQ node that has the management plugin enabled, see http://server-name:15672/cli/ 

---

### Parameters
global parameters vs per-host parameters

````
rabbitmqctl set_global_parameter myglobalparam 123

rabbitmqctl list_global_parameters_
````

---
### Policies

````
rabbitmqctl set_policy matspolicy ^amq. '{"max-length-bytes",4000}'

rabbitmqctl list_policies
````

---
### Configuration
Filename: **rabbitmq.conf**

[Here](https://github.com/rabbitmq/rabbitmq-server/blob/master/docs/rabbitmq.conf.example) an example with almost everything in it 

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
*??? NOT FOUND ???*

