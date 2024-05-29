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

```yaml title="qlico-core/docker-compose.yaml"
  postgres16:
    image: postgres:16-alpine
    container_name: qlico-core_postgres14
    logging:
      driver: none
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-postgres}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-qlico}
    volumes:
      - postgres16-data:/var/lib/postgresql/data
    networks:
      - qlico-core
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  postgres16-data:
    name: qlico-core_postgres16-data
```

## Example in a full docker-compose file

This is a large example, so you know where to place the PostgreSQL service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
---
# Author: Qlico <hello@qlico.dev>
services:
  traefik:
    image: traefik:v3.0
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
  postgres16:
    image: postgres:16-alpine
    container_name: qlico-core_postgres14
    logging:
      driver: none
    ports:
      - 5432:5432
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-postgres}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-qlico}
    volumes:
      - postgres16-data:/var/lib/postgresql/data
    networks:
      - qlico-core
volumes:
  postgres16-data:
    name: qlico-core_postgres16-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
