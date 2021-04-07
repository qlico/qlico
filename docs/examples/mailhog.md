# MailHog configuration example

## About MailHog

MailHog is an email testing tool for developers:

* Configure your application to use MailHog for SMTP delivery
* View messages in the web UI, or retrieve them with the JSON API
* Optionally release messages to real SMTP servers for delivery

## More information about MailHog

* [MailHog GitHub project](https://github.com/mailhog/MailHog)
* [MailHog on Docker Hub](https://hub.docker.com/r/mailhog/mailhog)

## How to add MailHog to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml
  mailhog:
    image: mailhog/mailhog:v1.0.0
    container_name: qlico-core_mailhog
    logging:
      driver: none
    labels:
      - "traefik.http.routers.mailhog.rule=Host(`mailhog.qlico`)"
      - "traefik.http.services.mailhog.loadbalancer.server.port=8025"
    networks:
      - qlico-core
```

## Example in a full docker-compose file

This is a large example, so you know where to place the MailHog service.

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
  mailhog:
    image: mailhog/mailhog:v1.0.0
    container_name: qlico-core_mailhog
    logging:
      driver: none
    labels:
      - "traefik.http.routers.mailhog.rule=Host(`mailhog.qlico`)"
      - "traefik.http.services.mailhog.loadbalancer.server.port=8025"
    networks:
      - qlico-core
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```
