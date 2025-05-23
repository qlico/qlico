# Redis Configuration example

## About Redis

Redis is an open source (BSD licensed), in-memory data structure store, used as
a database, cache, and message broker. Redis provides data structures such as
strings, hashes, lists, sets, sorted sets with range queries, bitmaps,
hyperloglogs, geospatial indexes, and streams. Redis has built-in replication,
Lua scripting, LRU eviction, transactions, and different levels of on-disk
persistence, and provides high availability via Redis Sentinel and automatic
partitioning with Redis Cluster.

The name Redis means REmote DIctionary Server.

# More information about Redis

* [Redis website](https://redis.io/){:target="_blank"}
* [Redis on Docker Hub](https://hub.docker.com/_/redis){:target="_blank"}

## How to add Redis to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  redis7:
    image: redis:7.4.2-alpine
    container_name: qlico-core_redis7
    ports:
      - 6379:6379
    volumes:
      - redis7-data:/data
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  redis7-data:
    name: qlico-core_redis7-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Redis service and
volume.

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
  redis7:
    image: redis:7.4.2-alpine
    container_name: qlico-core_redis7
    ports:
      - 6379:6379
    volumes:
      - redis7-data:/data
    networks:
      - qlico-core
volumes:
  redis7-data:
    name: qlico-core_redis7-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
