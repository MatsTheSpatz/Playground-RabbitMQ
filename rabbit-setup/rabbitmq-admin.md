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