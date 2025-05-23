# pgAdmin Example

## About pgAdmin

pgAdmin is the most popular and feature rich Open Source administration and
development platform for PostgreSQL, the most advanced Open Source database in
the world.

## More information about pgAdmin

* [pgAdmin website](https://www.pgadmin.org/){:target="_blank"}
* [pgAdmin on Docker Hub](https://hub.docker.com/r/dpage/pgadmin4){:target="_blank"}

## How to add pgAdmin to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "pgadmin4.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  pgadmin4-data:
    name: qlico-core_pgadmin4-data
```

This is a large example, so you know where to place the pgAdmin service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "pgadmin4.yaml" %}

volumes:
  pgadmin4-data:
    name: qlico-core_pgadmin4-data
{% include "compose-end.yaml" %}
```
