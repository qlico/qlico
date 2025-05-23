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
{% include "adminer.yaml" %}
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Adminer service.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "adminer.yaml" %}
{% include "compose-end.yaml" %}
```
