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

* [Elasticsearch website](https://www.elastic.co/elasticsearch/)
* [Kibana website](https://www.elastic.co/kibana)
* [Elastic Docker Images](https://www.docker.elastic.co/)

## How to add Elasticsearch to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  elasticsearch7:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.11.1
    container_name: qlico-core_elasticsearch7
    logging:
      driver: none
    ports:
      - 9207:9200
    volumes:
      - elasticsearch7-data/:/usr/share/elasticsearch/data
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - discovery.type=single-node
    ulimits:
      memlock:
        soft: -1
        hard: -1
    labels:
      - "traefik.http.routers.elasticsearch7.rule=Host(`elasticsearch7.qlico`)"
      - "traefik.http.services.elasticsearch7.loadbalancer.server.port=9200"
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml
  elasticsearch7-data:
    name: qlico-core_elasticsearch7-data
    driver: local
```

## How to add Kibana to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file. Make sure you're running Elasticsearch!

```yaml
  kibana7:
    image: docker.elastic.co/kibana/kibana:7.11.1
    container_name: qlico-core_kibana7
    logging:
      driver: none
    links:
      - elasticsearch7
    environment:
      ELASTICSEARCH_URL: http://elasticsearch7:9200
      ELASTICSEARCH_HOSTS: '["http://elasticsearch7:9200"]'
    labels:
      - "traefik.http.routers.kibana7.rule=Host(`kibana7.qlico`)"
      - "traefik.http.services.kibana7.loadbalancer.server.port=5601"
    networks:
      - qlico-core
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Elasticsearch & Kibana
service and volume.

```yaml
---
# Author: Qlico <hello@qlico.dev>
version: "3.9"
services:
  traefik:
    image: traefik:v2.4.5
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
  elasticsearch7:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.11.1
    container_name: qlico-core_elasticsearch7
    logging:
      driver: none
    ports:
      - 9207:9200
    volumes:
      - elasticsearch7-data/:/usr/share/elasticsearch/data
    environment:
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - discovery.type=single-node
    ulimits:
      memlock:
        soft: -1
        hard: -1
    labels:
      - "traefik.http.routers.elasticsearch7.rule=Host(`elasticsearch7.qlico`)"
      - "traefik.http.services.elasticsearch7.loadbalancer.server.port=9200"
    networks:
      - qlico-core
  kibana7:
    image: docker.elastic.co/kibana/kibana:7.11.1
    container_name: qlico-core_kibana7
    logging:
      driver: none
    links:
      - elasticsearch7
    environment:
      ELASTICSEARCH_URL: http://elasticsearch7:9200
      ELASTICSEARCH_HOSTS: '["http://elasticsearch7:9200"]'
    labels:
      - "traefik.http.routers.kibana7.rule=Host(`kibana7.qlico`)"
      - "traefik.http.services.kibana7.loadbalancer.server.port=5601"
    networks:
      - qlico-core
volumes:
  elasticsearch7-data:
    name: qlico-core_elasticsearch7-data
    driver: local
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
