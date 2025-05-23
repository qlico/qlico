# Adminer configuration example

## About Adminer

Adminer (formerly phpMinAdmin) is a full-featured database management tool
written in PHP. Conversely, to phpMyAdmin, it consists of a single file ready to
deploy to the target server. Adminer is available for MySQL, MariaDB,
PostgreSQL, SQLite, MS SQL, Oracle, Elasticsearch, MongoDB and others via
plugin.

## More information about Adminer

* [Adminer website](https://www.adminer.org/){:target="_blank"}
* [Adminer on Docker Hub](https://hub.docker.com/_/adminer){:target="_blank"}

## How to add Adminer to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  adminer:
    image: adminer:5.3.0-standalone
    container_name: qlico-core_adminer
    labels:
      - "traefik.http.routers.adminer.rule=Host(`adminer.qlico`)"
      - "traefik.http.services.adminer.loadbalancer.server.port=8080"
    networks:
      - qlico-core
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Adminer service.

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
  adminer:
    image: adminer:5.3.0-standalone
    container_name: qlico-core_adminer
    labels:
      - "traefik.http.routers.adminer.rule=Host(`adminer.qlico`)"
      - "traefik.http.services.adminer.loadbalancer.server.port=8080"
    networks:
      - qlico-core
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
