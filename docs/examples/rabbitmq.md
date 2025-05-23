# RabbitMQ configuration example

## About RabbitMQ

RabbitMQ is the most widely deployed open source message broker.

RabbitMQ is lightweight and easy to deploy on premises and in the cloud. It
supports multiple messaging protocols. RabbitMQ can be deployed in distributed
and federated configurations to meet high-scale, high-availability requirements.

## More information about RabbitMQ

* [RabbitMQ website](https://www.rabbitmq.com/){:target="_blank"}
* [RabbitMQ on Docker Hub](https://hub.docker.com/_/rabbitmq){:target="_blank"}

## How to add RabbitMQ to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  rabbitmq3:
    image: rabbitmq:4.1.0-management-alpine
    container_name: qlico-core_rabbitmq4
    ports:
      - 5672:5672
      - 15672:15672
    volumes:
      - rabbitmq4-data:/data/mnesia
    labels:
      - "traefik.http.routers.rabbitmq4.rule=Host(`rabbitmq4.qlico`)"
      - "traefik.http.services.rabbitmq4.loadbalancer.server.port=15672"
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  rabbitmq4-data:
    name: qlico-core_rabbitmq4-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the RabbitMQ service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
---
# Author: Qlico <hello@qlico.dev>
services:
  traefik:
    image: traefik:v3.3.5
    container_name: qlico-core_traefik
    command: [ '--providers.docker', '--api.insecure' ]
    networks:
      - qlico-core
    ports:
      - 80:80
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    labels:
      - "traefik.http.routers.traefik.rule=Host(`traefik.qlico`)"
      - "traefik.http.services.traefik.loadbalancer.server.port=8080"
  rabbitmq3:
    image: rabbitmq:4.1.0-management-alpine
    container_name: qlico-core_rabbitmq4
    ports:
      - 5672:5672
      - 15672:15672
    volumes:
      - rabbitmq4-data:/data/mnesia
    labels:
      - "traefik.http.routers.rabbitmq4.rule=Host(`rabbitmq4.qlico`)"
      - "traefik.http.services.rabbitmq4.loadbalancer.server.port=15672"
    networks:
      - qlico-core
volumes:
  rabbitmq4-data:
    name: qlico-core_rabbitmq4-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
