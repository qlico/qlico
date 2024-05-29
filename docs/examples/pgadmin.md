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
  pgadmin4:
    container_name: qlico-core_pgadmin
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_DEFAULT_EMAIL:-pgadmin4@pgadmin.qlico}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_DEFAULT_PASSWORD:-qlico}
    volumes:
      - pgadmin4-data:/root/.pgadmin
    labels:
      - "traefik.http.routers.pgadmin4.rule=Host(`pgadmin4.qlico`)"
      - "traefik.http.services.pgadmin4.loadbalancer.server.port=80"
    networks:
      - qlico-core
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
  pgadmin4:
    container_name: qlico-core_pgadmin
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_DEFAULT_EMAIL:-pgadmin4@pgadmin.qlico}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_DEFAULT_PASSWORD:-qlico}
    volumes:
      - pgadmin4-data:/root/.pgadmin
    labels:
      - "traefik.http.routers.pgadmin4.rule=Host(`pgadmin4.qlico`)"
      - "traefik.http.services.pgadmin4.loadbalancer.server.port=80"
    networks:
      - qlico-core
volumes:
  pgadmin4-data:
    name: qlico-core_pgadmin4-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
