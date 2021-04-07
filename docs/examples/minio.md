# MinIO configuration example

## About MinIO

MinIO is a high performance, distributed object storage system. It is
software-defined, runs on industry standard hardware and is 100% open source
with the dominant license being GNU AGPL v3.

## More information about MinIO

* [MinIO website](https://min.io/)
* [MinIO on Docker Hub](https://hub.docker.com/r/minio/minio)

## How to add MinIO to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  minio:
    image: minio/minio
    container_name: qlico-core_minio
    command: server /export
    environment:
      MINIO_ACCESS_KEY: ${MINIO_ACCESS_KEY:-qlicorocks}
      MINIO_SECRET_KEY: ${MINIO_SECRET_KEY:-qlicorocks}
    volumes:
      - minio-data:/export
    networks:
      - qlico-core
    labels:
      - "traefik.http.routers.minio.rule=Host(`minio.qlico`)"
      - "traefik.http.services.minio.loadbalancer.server.port=9000"
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml
  minio-data:
    name: qlico-core_minio-data
    driver: local
```

## Example in a full docker-compose file

This is a large example, so you know where to place the MinIO service and
volume.

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
  minio:
    image: minio/minio
    container_name: qlico-core_minio
    command: server /export
    environment:
      MINIO_ACCESS_KEY: ${MINIO_ACCESS_KEY:-qlicorocks}
      MINIO_SECRET_KEY: ${MINIO_SECRET_KEY:-qlicorocks}
    volumes:
      - minio-data:/export
    networks:
      - qlico-core
    labels:
      - "traefik.http.routers.minio.rule=Host(`minio.qlico`)"
      - "traefik.http.services.minio.loadbalancer.server.port=9000"
volumes:
  minio-data:
    name: qlico-core_minio-data
    driver: local
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
