# Percona configuration examples

## About Percona

More enterprises rely on MySQL® performance, resilience and security to power
the applications and websites that make achieving their business goals possible.

Percona Server for MySQL® is a free, fully compatible, enhanced and open source
drop-in replacement for any MySQL database. It provides superior performance,
scalability and instrumentation.

## More information about Percona

* [Percona server website](https://www.percona.com/software/mysql-database/percona-server)
* [Percona server on Docker Hub](https://hub.docker.com/_/percona)

## How to add Percona to Qlico?

Is it possible to use Percona 5 or 8, it's even possible to run both at the same
time.

## How to add Percona 5 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  percona5:
    image: percona:5.7.32-centos
    container_name: qlico-core_percona5
    logging:
      driver: none
    ports:
      - 3305:3306
    environment:
      MYSQL_USER: ${MYSQL_USERNAME:-root}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - percona5-data:/var/lib/mysql
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml
  percona5-data:
    name: qlico-core_percona5-data
    driver: local
```

## How to add Percona 8 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  percona8:
    image: percona:8.0.22-13-centos
    container_name: qlico-core_percona8
    logging:
      driver: none
    ports:
      - 3308:3306
    environment:
      MYSQL_USER: ${MYSQL_USERNAME:-root}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - percona8-data:/var/lib/mysql
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml
  percona8-data:
    name: qlico-core_percona8-data
    driver: local
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Percona service(s) and
volume(s).

```yaml
---
# Author: Qlico <hello@qlico.dev>
version: "3.9"
services:
  traefik:
    image: traefik:v2.4.5
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
  percona5:
    image: percona:5.7.32-centos
    container_name: qlico-core_percona5
    logging:
      driver: none
    ports:
      - 3305:3306
    environment:
      MYSQL_USER: ${MYSQL_USERNAME:-root}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - percona5-data:/var/lib/mysql
    networks:
      - qlico-core
  percona8:
    image: percona:8.0.22-13-centos
    container_name: qlico-core_percona8
    logging:
      driver: none
    ports:
      - 3308:3306
    environment:
      MYSQL_USER: ${MYSQL_USERNAME:-root}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - percona8-data:/var/lib/mysql
    networks:
      - qlico-core
volumes:
  percona5-data:
    name: qlico-core_percona5-data
    driver: local
  percona8-data:
    name: qlico-core_percona8-data
    driver: local
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
