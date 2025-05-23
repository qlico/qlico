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
  elasticsearch9:
    image: docker.elastic.co/elasticsearch/elasticsearch:9.0.0
    container_name: qlico-core_elasticsearch9
    logging:
      driver: none
    ports:
      - 9209:9200
    volumes:
      - elasticsearch9-data/:/usr/share/elasticsearch/data
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - discovery.type=single-node
    ulimits:
      memlock:
        soft: -1
        hard: -1
    labels:
      - "traefik.http.routers.elasticsearch9.rule=Host(`elasticsearch9.qlico`)"
      - "traefik.http.services.elasticsearch9.loadbalancer.server.port=9200"
    networks:
      - qlico-core
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
  kibana9:
    image: docker.elastic.co/kibana/kibana:9.0.0
    container_name: qlico-core_kibana9
    logging:
      driver: none
    links:
      - elasticsearch9
    environment:
      ELASTICSEARCH_URL: http://elasticsearch9:9200
      ELASTICSEARCH_HOSTS: '["http://elasticsearch9:9200"]'
    labels:
      - "traefik.http.routers.kibana9.rule=Host(`kibana9.qlico`)"
      - "traefik.http.services.kibana9.loadbalancer.server.port=5601"
    networks:
      - qlico-core
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Elasticsearch & Kibana
service and volume.

```yaml title="qlico-core/docker-compose.yaml"
---
# Author: Qlico <hello@qlico.dev>
services:
  traefik:
    image: traefik:v3.4.0
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
  elasticsearch9:
    image: docker.elastic.co/elasticsearch/elasticsearch:9.0.0
    container_name: qlico-core_elasticsearch9
    logging:
      driver: none
    ports:
      - 9209:9200
    volumes:
      - elasticsearch9-data/:/usr/share/elasticsearch/data
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - discovery.type=single-node
    ulimits:
      memlock:
        soft: -1
        hard: -1
    labels:
      - "traefik.http.routers.elasticsearch9.rule=Host(`elasticsearch9.qlico`)"
      - "traefik.http.services.elasticsearch9.loadbalancer.server.port=9200"
    networks:
      - qlico-core
  kibana9:
    image: docker.elastic.co/kibana/kibana:9.0.0
    container_name: qlico-core_kibana9
    logging:
      driver: none
    links:
      - elasticsearch9
    environment:
      ELASTICSEARCH_URL: http://elasticsearch9:9200
      ELASTICSEARCH_HOSTS: '["http://elasticsearch9:9200"]'
    labels:
      - "traefik.http.routers.kibana9.rule=Host(`kibana9.qlico`)"
      - "traefik.http.services.kibana9.loadbalancer.server.port=5601"
    networks:
      - qlico-core
volumes:
  elasticsearch9-data:
    name: qlico-core_elasticsearch9-data
    driver: local
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
