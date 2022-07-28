# PostgreSQL configuration Example

## About PostgreSQL

PostgreSQL is a powerful, open source object-relational database system with
over 30 years of active development that has earned it a strong reputation for
reliability, feature robustness, and performance.

## More information about PostgreSQL

* [PostgreSQL website](https://www.postgresql.org/)
* [PostgreSQL on Docker Hub](https://hub.docker.com/_/postgres)

## How to add PostgreSQL to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  postgres14:
    image: postgres:14-alpine
    container_name: qlico-core_postgres14
    logging:
      driver: none
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-postgres}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-qlico}
    volumes:
      - postgres14-data:/var/lib/postgresql
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml
  postgres13-data:
    name: qlico-core_postgres13-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the PostgreSQL service and
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
  postgres14:
    image: postgres:14-alpine
    container_name: qlico-core_postgres14
    logging:
      driver: none
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-postgres}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-qlico}
    volumes:
      - postgres14-data:/var/lib/postgresql
    networks:
      - qlico-core
volumes:
  postgres13-data:
    name: qlico-core_postgres13-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
