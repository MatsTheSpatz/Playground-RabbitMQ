# Exchange Federation

````
// rabbit 1
docker container run -d --hostname rabbit-host-1 --name rabbit-1 -e RABBITMQ_ERLANG_COOKIE='matserlangcookie' -e RABBITMQ_DEFAULT_USER=mats1 -e RABBITMQ_DEFAULT_PASS=mats1 -p 8080:15672 -p 5672:5672 rabbitmq:management

docker container exec rabbit-1 rabbitmq-plugins enable rabbitmq_federation

docker container exec rabbit-1 rabbitmq-plugins enable rabbitmq_federation_management


// rabbit 2
docker container run -d --hostname rabbit-host-2 --name rabbit-2 -e RABBITMQ_ERLANG_COOKIE='matserlangcookie' -e RABBITMQ_DEFAULT_USER=mats2 -e RABBITMQ_DEFAULT_PASS=mats2 -p 8081:15672 -p 5673:5672 --link rabbit-1:rabbit-host-1  rabbitmq:management

docker container exec rabbit-2 rabbitmq-plugins enable rabbitmq_federation

docker container exec rabbit-2 rabbitmq-plugins enable rabbitmq_federation_management



// federate
docker container exec rabbit-2 rabbitmqctl set_parameter federation-upstream my-upstream2 '{"uri":"amqp://mats1:mats1@rabbit-host-1","expires":3600000}' 

// MANUAL: create exchange on rabbit-2

docker container exec rabbit-2 rabbitmqctl set_policy --apply-to exchanges federate-test-mats "upstreamexchange" '{"federation-upstream-set":"all"}' 




docker run -it --rm --link rabbit-1:rabbit-host-1 -e RABBITMQ_ERLANG_COOKIE='matserlangcookie' rabbitmq:management bash
````