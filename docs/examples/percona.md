# Percona configuration examples

## About Percona

More enterprises rely on MySQL® performance, resilience and security to power
the applications and websites that make achieving their business goals possible.

Percona Server for MySQL® is a free, fully compatible, enhanced and open source
drop-in replacement for any MySQL database. It provides superior performance,
scalability and instrumentation.

## More information about Percona

* [Percona server website](https://www.percona.com/software/mysql-database/percona-server){:target="_blank"}
* [Percona server on Docker Hub](https://hub.docker.com/_/percona){:target="_blank"}

## How to add Percona to Qlico?

## How to add Percona 8 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "percona8.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  percona8-data:
    name: qlico-core_percona8-data
    driver: local
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Percona service(s) and
volume(s).

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "percona8.yaml" %}

volumes:
  percona8-data:
    name: qlico-core_percona8-data
    driver: local
{% include "compose-end.yaml" %}
```
