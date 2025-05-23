# Blackfire configuration example

## About Blackfire

Blackfire.io makes it possible to write performance tests, automate test
scenarios, and drill down to the finest details whenever performance issues
arise. Teams can collaborate on performance testing in all environments:
development, testing, staging and production.

## More information about Blackfire

* [Blackfire website](https://www.blackfire.io/){:target="_blank"}
* [Blackfire on Docker Hub](https://hub.docker.com/r/blackfire/blackfire){:target="_blank"}

## How to add Blackfire to Qlico?

Add the following YAML to the `services` section of your `docker-compose.yaml`
file.

```yaml title="qlico-core/docker-compose.yaml"
{% include "blackfire.yaml" %}
```

## Example in a full docker-compose file

This is a large example, so you know where to place the Blackfire service.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "traefik.yaml" %}
{% include "blackfire.yaml" %}
{% include "compose-end.yaml" %}
```
