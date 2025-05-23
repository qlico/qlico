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
{% include "redis.yaml" %}
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
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "redis.yaml" %}

volumes:
  redis7-data:
    name: qlico-core_redis7-data
{% include "compose-end.yaml" %}
```
