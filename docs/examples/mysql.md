# MySQL configuration example

## About MySQL

MySQL is the world's most popular open source database. Whether you are a fast
growing web property, technology ISV or large enterprise, MySQL can
cost-effectively help you deliver high performance, scalable database
applications.

## More information about MySQL

* [MySQL website](https://www.mysql.com/)
* [MySQL on Docker Hub](https://hub.docker.com/_/mysql)

## How to add MySQL 5 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  mysql5:
    image: mysql:5.7.39
    container_name: qlico-core_mysql5
    logging:
      driver: none
    ports:
      - 3305:3306
    environment:
      MYSQL_USER: ${MYSQL_USERNAME:-root}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - mysql-data5:/var/lib/mysql
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml
  mysql-data5:
    name: qlico-core_mysql5-data
    driver: local
```

## Example in a full docker-compose file

This is a large example, so you know where to place the MySQL service and
volume.

```yaml
---
# Author: Qlico <hello@qlico.dev>
version: "3.9"
services:
  traefik:
    image: traefik:v2.8
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
  mysql5:
    image: mysql:5.7.39
    container_name: qlico-core_mysql5
    logging:
      driver: none
    ports:
      - 3305:3306
    environment:
      MYSQL_USER: ${MYSQL_USERNAME:-root}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - mysql-data5:/var/lib/mysql
    networks:
      - qlico-core
    driver: local
volumes:
  mysql-data5:
    name: qlico-core_mysql5-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
