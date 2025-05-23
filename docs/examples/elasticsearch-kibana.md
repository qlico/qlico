# Elasticsearch and Kibana configuration example

## About Elasticsearch

Elasticsearch is a distributed, RESTful search and analytics engine capable of
addressing a growing number of use cases. As the heart of the Elastic Stack, it
centrally stores your data for lightning fast search, fine‑tuned relevancy, and
powerful analytics that scale with ease.

## About Kibana

Kibana is a free and open user interface that lets you visualize your
Elasticsearch data and navigate the Elastic Stack. Do anything from tracking
query load to understanding the way requests flow through your apps.

## More information about Elasticsearch & Kibana

* [Elasticsearch website](https://www.elastic.co/elasticsearch/){:target="_blank"}
* [Kibana website](https://www.elastic.co/kibana){:target="_blank"}
* [Elastic Docker Images](https://www.docker.elastic.co/){:target="_blank"}

## How to add Elasticsearch to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "elasticsearch.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  elasticsearch9-data:
    name: qlico-core_elasticsearch9-data
    driver: local
```

## How to add Kibana to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file. Make sure you're running Elasticsearch!

```yaml title="qlico-core/docker-compose.yaml"
{% include "kibana.yaml" %}
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Elasticsearch & Kibana
service and volume.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "elasticsearch.yaml" %}
{% include "kibana.yaml" %}

volumes:
  elasticsearch9-data:
    name: qlico-core_elasticsearch9-data
    driver: local
{% include "compose-end.yaml" %}
```
