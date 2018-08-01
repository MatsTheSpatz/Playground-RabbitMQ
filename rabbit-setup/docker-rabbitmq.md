# Setup RabbitMQ from Docker-Image

### Install Docker:
  Follow instructions: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/

### Get Image
Image-Info: https://hub.docker.com/_/rabbitmq/
```` 
  docker image pull rabbitmq:3-management
```` 

### Run Image
Configuration-Info: https://hub.docker.com/_/rabbitmq/
```` 
  docker container run -d --hostname my-rabbit-host --name my-rabbit-container -p 8080:15672 -p 5672:5672 rabbitmq:3-management
  // username: guest / password: guest

  docker container run -d 
  --hostname rabbit-host-1 
  --name rabbit-1 
  -e RABBITMQ_ERLANG_COOKIE='matserlangcookie' 
  -e RABBITMQ_DEFAULT_USER=mats1 -e RABBITMQ_DEFAULT_PASS=mats1 
  -p 8080:15672 -p 5672:5672 
  rabbitmq:management

  // 15672 -> mgmt
  // 5672 -> regular connections to node from clients
````

### Management:



### Check broker
  - get into container:   ```` docker container exec -it 4e21 /bin/bash````
  - check status:         ````service rabbitmq-server status````


