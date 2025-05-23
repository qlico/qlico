# MinIO configuration example

## About MinIO

MinIO is a high performance, distributed object storage system. It is
software-defined, runs on industry standard hardware and is 100% open source
with the dominant license being GNU AGPL v3.

## More information about MinIO

* [MinIO website](https://min.io/){:target="_blank"}
* [MinIO on Docker Hub](https://hub.docker.com/r/minio/minio){:target="_blank"}

## How to add MinIO to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "minio.yaml" %}
```

Add the following YAML to the `volumes` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  minio-data:
    name: qlico-core_minio-data
    driver: local
```

## Example in a full docker-compose file

This is a large example, so you know where to place the MinIO service and
volume.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "minio.yaml" %}

volumes:
  minio-data:
    name: qlico-core_minio-data
    driver: local
{% include "compose-end.yaml" %}
```
