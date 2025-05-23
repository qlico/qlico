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
{% include "rabbitmq.yaml" %}
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
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "rabbitmq.yaml" %}

volumes:
  rabbitmq4-data:
    name: qlico-core_rabbitmq4-data
{% include "compose-end.yaml" %}
```
