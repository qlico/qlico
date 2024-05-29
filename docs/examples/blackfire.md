# Blackfire configuration example

## About Blackfire

Blackfire.io makes it possible to write performance tests, automate test
scenarios, and drill down to the finest details whenever performance issues
arise. Teams can collaborate on performance testing in all environments:
development, testing, staging and production.

## More information about Blackfire

* [Blackfire website](https://www.blackfire.io/)
* [Blackfire on Docker Hub](https://hub.docker.com/r/blackfire/blackfire)

## How to add Blackfire to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
  blackfire:
    image: blackfire/blackfire:2
    container_name: qlico-core_blackfire
    env_file:
      - .env
    networks:
      - qlico-core
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Blackfire service.

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
  blackfire:
    image: blackfire/blackfire:2
    container_name: qlico-core_blackfire
    env_file:
      - .env
    networks:
      - qlico-core
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
