# Valkey Configuration example

## About Valkey

Valkey is a high-performance data structure server that primarily serves key/value workloads. It supports a wide range of native structures and an extensible plugin system for adding new data structures and access patterns.

The Valkey project was forked from the open source Redis project right before the transition to their new source available licenses.


# More information about Valkey

* [Valkey website](https://valkey.io/){:target="_blank"}
* [Valkey on Docker Hub](https://hub.docker.com/r/valkey/valkey){:target="_blank"}

## How to add Valkey to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  valkey:
    image: valkey/valkey:8.1.0
    container_name: qlico-core_valkey
    ports:
      - 6379:6379
    volumes:
      - valkey-data:/data
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  valkey-data:
    name: qlico-core_valkey-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Valkey service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
---
# Author: Qlico <hello@qlico.dev>
services:
  traefik:
    image: traefik:v3.0.1
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
  valkey:
    image: valkey/valkey:8.1.0
    container_name: qlico-core_valkey
    ports:
      - 6379:6379
    volumes:
      - valkey-data:/data
    networks:
      - qlico-core
volumes:
  valkey-data:
    name: qlico-core_valkey-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
