# PostgreSQL configuration Example

## About PostgreSQL

PostgreSQL is a powerful, open source object-relational database system with
over 30 years of active development that has earned it a strong reputation for
reliability, feature robustness, and performance.

## More information about PostgreSQL

* [PostgreSQL website](https://www.postgresql.org/){:target="_blank"}
* [PostgreSQL on Docker Hub](https://hub.docker.com/_/postgres){:target="_blank"}

## How to add PostgreSQL 18 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "postgres18.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  postgres18-data:
    name: qlico-core_postgres18-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the PostgreSQL service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "postgres18.yaml" %}

volumes:
  postgres18-data:
    name: qlico-core_postgres18-data
{% include "compose-end.yaml" %}
```

## How to add PostgreSQL 17 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "postgres17.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  postgres17-data:
    name: qlico-core_postgres17-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the PostgreSQL service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "postgres17.yaml" %}

volumes:
  postgres17-data:
    name: qlico-core_postgres17-data
{% include "compose-end.yaml" %}
```
