# RabbitMQ configuration example

## About RabbitMQ

RabbitMQ is the most widely deployed open source message broker.

RabbitMQ is lightweight and easy to deploy on premises and in the cloud. It
supports multiple messaging protocols. RabbitMQ can be deployed in distributed
and federated configurations to meet high-scale, high-availability requirements.

## More information about RabbitMQ

* [RabbitMQ website](https://www.rabbitmq.com/)
* [RabbitMQ on Docker Hub](https://hub.docker.com/_/rabbitmq)

## How to add RabbitMQ to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  rabbitmq3:
    image: rabbitmq:3.13.2-management-alpine
    container_name: qlico-core_rabbitmq3
    ports:
      - 5672:5672
      - 15672:15672
    volumes:
      - rabbitmq3-data:/data/mnesia
    labels:
      - "traefik.http.routers.rabbitmq3.rule=Host(`rabbitmq3.qlico`)"
      - "traefik.http.services.rabbitmq3.loadbalancer.server.port=15672"
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml
  rabbitmq3-data:
    name: qlico-core_rabbitmq3-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the RabbitMQ service and
volume.

```yaml
---
# Author: Qlico <hello@qlico.dev>
services:
  traefik:
    image: traefik:v2.8
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
    image: rabbitmq:3.13.2-management-alpine
    container_name: qlico-core_rabbitmq3
    ports:
      - 5672:5672
      - 15672:15672
    volumes:
      - rabbitmq3-data:/data/mnesia
    labels:
      - "traefik.http.routers.rabbitmq3.rule=Host(`rabbitmq3.qlico`)"
      - "traefik.http.services.rabbitmq3.loadbalancer.server.port=15672"
    networks:
      - qlico-core
volumes:
  rabbitmq3-data:
    name: qlico-core_rabbitmq3-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
