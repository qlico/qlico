# Mailpit configuration example

## About Mailpit

Mailpit is a small, fast, low memory, zero-dependency, multi-platform
email testing tool & API for developers.

It acts as an SMTP server, provides a modern web interface to view & test
captured emails, and contains an API for automated integration testing.

## More information about Mailpit

* [Mailpit website](https://mailpit.axllent.org/)
* [Mailpit GitHub project](https://github.com/axllent/mailpit)
* [Mailpit on Docker Hub](https://hub.docker.com/r/axllent/mailpit)

## How to add Mailpit to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  mailpit:
    image: axllent/mailpit:v1.18.0
    container_name: qlico-core_mailpit
    restart: always
    volumes:
      - 'mailpit-data/data'
    ports:
      - 8025:8025
      - 1025:1025
    environment:
      MP_MAX_MESSAGES: 5000
      MP_DATA_FILE: /data/mailpit.db
      MP_SMTP_AUTH_ACCEPT_ANY: 1
      MP_SMTP_AUTH_ALLOW_INSECURE: 1
    labels:
      - "traefik.http.routers.mailpit.rule=Host(`mailpit.qlico`)"
      - "traefik.http.services.mailpit.loadbalancer.server.port=8025"
    networks:
      - qlico-core
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Mailpit service.

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
  mailpit:
    image: axllent/mailpit:v1.18.0
    container_name: qlico-core_mailpit
    restart: always
    volumes:
      - 'mailpit-data/data'
    ports:
      - 8025:8025
      - 1025:1025
    environment:
      MP_MAX_MESSAGES: 5000
      MP_DATA_FILE: /data/mailpit.db
      MP_SMTP_AUTH_ACCEPT_ANY: 1
      MP_SMTP_AUTH_ALLOW_INSECURE: 1
    labels:
      - "traefik.http.routers.mailpit.rule=Host(`mailpit.qlico`)"
      - "traefik.http.services.mailpit.loadbalancer.server.port=8025"
    networks:
      - qlico-core
volumes:
  mailpit-data:
    name: qlico-core_mailpit-data
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
