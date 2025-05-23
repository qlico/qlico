# MySQL configuration example

## About MySQL

MySQL is the world's most popular open source database. Whether you are a fast-growing web property, technology ISV or large enterprise, MySQL can
cost-effectively help you deliver high performance, scalable database
applications.

## More information about MySQL

* [MySQL website](https://www.mysql.com/){:target="_blank"}
* [MySQL on Docker Hub](https://hub.docker.com/_/mysql){:target="_blank"}

## How to add MySQL 9 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "mysql9.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  mysql-data9:
    name: qlico-core_mysql9-data
    driver: local
```

## Example in a full docker-compose file

This is a large example, so you know where to place the MySQL service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "mysql9.yaml" %}

volumes:
  mysql-data9:
    name: qlico-core_mysql9-data
    driver: local
{% include "compose-end.yaml" %}
```

## How to add MySQL 8 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "mysql8.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  mysql-data8:
    name: qlico-core_mysql8-data
    driver: local
```

## Example in a full docker-compose file

This is a large example, so you know where to place the MySQL service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "mysql8.yaml" %}

volumes:
  mysql-data8:
    name: qlico-core_mysql8-data
{% include "compose-end.yaml" %}
```
