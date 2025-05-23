# Mailpit configuration example

## About Mailpit

Mailpit is a small, fast, low memory, zero-dependency, multi-platform
email testing tool & API for developers.

It acts as an SMTP server, provides a modern web interface to view & test
captured emails, and contains an API for automated integration testing.

## More information about Mailpit

* [Mailpit website](https://mailpit.axllent.org/){:target="_blank"}
* [Mailpit GitHub project](https://github.com/axllent/mailpit){:target="_blank"}
* [Mailpit on Docker Hub](https://hub.docker.com/r/axllent/mailpit){:target="_blank"}

## How to add Mailpit to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "mailpit.yaml" %}
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Mailpit service.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "mailpit.yaml" %}

volumes:
  mailpit-data:
    name: qlico-core_mailpit-data
{% include "compose-end.yaml" %}
```
