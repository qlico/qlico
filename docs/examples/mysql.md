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
  mysql9:
    image: mysql:9.2.0
    container_name: qlico-core_mysql9
    restart: unless-stopped
    logging:
      driver: none
    ports:
      - 3309:3306
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - mysql-data9:/var/lib/mysql
    networks:
      - qlico-core
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
  mysql9:
    image: mysql:9.2.0
    container_name: qlico-core_mysql9
    restart: unless-stopped
    logging:
      driver: none
    ports:
      - 3309:3306
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - mysql-data9:/var/lib/mysql
    networks:
      - qlico-core
volumes:
  mysql-data9:
    name: qlico-core_mysql9-data
    driver: local
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```

## How to add MySQL 8 to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  mysql8:
    image: mysql:8.0.41
    container_name: qlico-core_mysql8
    restart: unless-stopped
    logging:
      driver: none
    ports:
      - 3308:3306
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - mysql-data8:/var/lib/mysql
    networks:
      - qlico-core
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
---
# Author: Qlico <hello@qlico.dev>
services:
  traefik:
    image: traefik:v3.3.5
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
  mysql8:
    image: mysql:8.0.41
    container_name: qlico-core_mysql8
    restart: unless-stopped
    logging:
      driver: none
    ports:
      - 3308:3306
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-qlico}
    volumes:
      - mysql-data8:/var/lib/mysql
    networks:
      - qlico-core
volumes:
  mysql-data8:
    name: qlico-core_mysql8-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```

