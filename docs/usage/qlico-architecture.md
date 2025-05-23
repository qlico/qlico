# Qlico architecture


![qlico-architecture](/assets/img/qlico-architecture.png "qlico-architecture")
[Click here for a larger image](/assets/img/qlico-architecture.png){:target="_blank"}

In this image we have a few services and projects running.
You can run as many services and projects you want to run, in this example we're
running 4 services and 3 projects.

To get a good overview, all `docker-compose.yaml` files for all these services
and projects are on this page.

* [Qlico core](#qlico-core)
* [A Symfony project](#a-symfony-project)
* [A Laravel project](#a-laravel-project)
* [A Go microservice](#a-go-microservice)


## Qlico core

Qlico core with Traefik is the only neccessary service to run.
Traefik is a reverse proxy, and is used to route HTTP traffic to the correct
service.
You are free to add more services to `qlico-core` if you want to.

In example image we're running:

* Traefik
* Valkey
* Postgres
* RabbitMQ

Since more than one project is using for example Valkey, Postgresql and RabbitMQ,
we want to run these services in `qlico-core`.

```yaml title="qlico-core/docker-compose.yaml"
{% include "compose-start.yaml" %}
{% include "valkey.yaml" %}
{% include "postgres17.yaml" %}
{% include "rabbitmq.yaml" %}

volumes:
  valkey-data:
    name: qlico-core_valkey-data
  postgres17-data:
    name: qlico-core_postgres17-data

networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```

From our host machine where Docker is running we can connect to:

* Traefik: `http://localhost:80`
* Valkey: `localhost:6379`
* Postgresql: `localhost:5432`
* RabbitMQ: `localhost:5672` or the web interface: `http://rabbitmq4.qlico`

From the projects running (in Docker) we can access everything by the `service`
name:

* Traefik (is not needed, since it's a reverse proxy)
* Valkey: `valkey:6379`
* Postgresql: `postgres17:5432`
* RabbitMQ: `rabbitmq4:5672` (the webinterface is not needed for applications)


## A Symfony project

```yaml title="a-symfony-project/qlico/docker-compose.yaml"
---
# Author: Qlico <hello@qlico.dev>
services:
  nginx:
    image: {{ container_images.nginx_unprivileged }}
    container_name: ${PROJECT_NAME}_nginx
    volumes:
      - ./services/nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./services/nginx/mime.types:/etc/nginx/mime.types
      - ../:/var/www/html
      - php-socket:/php-socket
    depends_on:
      - php
    labels:
      - "traefik.http.routers.${PROJECT_NAME}_nginx.rule=Host(`${PROJECT_HOST}`)"
      - "traefik.http.services.${PROJECT_NAME}_nginx.loadbalancer.server.port=8080"
    networks:
      - qlico-core

  php:
    build:
      context: ../
      target: dev
    container_name: ${PROJECT_NAME}_php
    volumes:
      - ../:/var/www/html
      - ~/.composer:/home/qlico/.composer
      - php-socket:/php-socket
    environment:
      - PHP_IDE_CONFIG=serverName=${PROJECT_NAME}
    networks:
      - qlico-core
    env_file:
      - .env

volumes:
  php-socket:
    driver: local

networks:
  qlico-core:
    external: true
    name: qlico-core
```

### The `.env` file from this Symfony project

This is a small example, the actual `.env` file is larger

```shell title="a-symfony-project/qlico/.env"
PROJECT_NAME=a-symfony-project
PROJECT_HOST=a-symfony-project.qlico
COMPOSE_PROJECT_NAME=a-symfony-project
```

We're not using the `.env` in the root of the project.

### Connecting to services

To connect to services running in `qlico-core` we're using:
These are inside the projects `.env` file:

```shell title="a-symfony-project/.env"
DATABASE_URL="postgresql://postgres:qlico@postgres17:5432/a-symfony-project?serverVersion=17&charset=utf8"
REDIS_DSN="redis://valkey:6379"
```

If we want to access the Symfony project in a
webbrowser: http://a-symfony-project.qlico

Debugging with Xdebug? The project name is `a-symfony-project`.


## A Laravel project

```yaml title="a-laravel-project/qlico/docker-compose.yaml"
---
# Author: Qlico <hello@qlico.dev>
---
services:
  nginx:
    image: nginxinc/nginx-unprivileged:1.25.5-alpine3.19
    container_name: ${PROJECT_NAME}_nginx
    volumes:
      - ./services/nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./services/nginx/mime.types:/etc/nginx/mime.types
      - ../:/var/www/html
      - php-socket:/php-socket
    depends_on:
      - php
    labels:
      - "traefik.http.routers.${PROJECT_NAME}_nginx.rule=Host(`${PROJECT_HOST}`)"
      - "traefik.http.services.${PROJECT_NAME}_nginx.loadbalancer.server.port=8080"
    networks:
      - qlico-core

  php:
    build:
      context: ../
      target: dev
    container_name: ${PROJECT_NAME}_php
    volumes:
      - ../:/var/www/html
      - ~/.composer:/home/qlico/.composer
      - php-socket:/php-socket
    environment:
      - PHP_IDE_CONFIG=serverName=${PROJECT_NAME}
    networks:
      - qlico-core
    env_file:
      - .env

volumes:
  php-socket:
    driver: local

networks:
  qlico-core:
    external: true
    name: qlico-core
```

### The `.env` file from this Laravel project

This is a small example, the actual `.env` file is larger

```shell title="a-laravel-project/qlico/.env"
PROJECT_NAME=a-laravel-project
PROJECT_HOST=a-laravel-project.qlico
COMPOSE_PROJECT_NAME=a-laravel-project
```

We're not using the `.env` in the root of the project.

### Connecting to services

To connect to services running in `qlico-core` we're using:
These are inside the projects `.env` file:

```shell title="a-laravel-project/.env"
DATABASE_URL="pgsql://postgres:qlico@postgres17:5432/a-laravel-project?charset=utf8"
RABBITMQ_DSN="redis://valkey:6379"
QUEUE_CONNECTION="rabbitmq"
RABBITMQ_HOST="rabbitmq3"
RABBITMQ_PASSWORD="guest"
RABBITMQ_USERNAME="guest"
RABBITMQ_VHOST="my-distributed-queue"
```

If we want to access the Laravel project in a
webbrowser: http://a-laravel-project.qlico

Debugging with Xdebug? The project name is `a-laravel-project`.

## The difference between the Symfony and Laravel project

It's a small difference, but Laravel is using RabbitMQ for the queue connection
and Symfony is using Valkey for the cache connection. Both are using Postgresql
for the database connection.

But the `qlico/docker-compose.yaml` files are identical.
This makes it easy to switch between projects, or to add more projects.

The Dockerfiles can be different (for example different PHP versions).
This gives you the flexibility to use the PHP version you want to use, and use
different PHP extensions.

## A Go microservice

If you want to run a Go microservice, you can use the
same `qlico/docker-compose.yaml`, but this could also be for example a Spring
Boot application, Ruby, Python or any other project.

```yaml title="a-go-microservice/qlico/docker-compose.yaml"
# Author: Qlico <hello@qlico.dev>
---
services:
  a-go-microservice:
    build:
      context: ../
    container_name: ${PROJECT_NAME}_go
    depends_on:
      - php
    labels:
      - "traefik.http.routers.${PROJECT_NAME}_nginx.rule=Host(`${PROJECT_HOST}`)"
      - "traefik.http.services.${PROJECT_NAME}_nginx.loadbalancer.server.port=8080"
    networks:
      - qlico-core

networks:
  qlico-core:
    external: true
    name: qlico-core
```

### The `.env` file from this Go microservice

```shell title="a-go-microservice/qlico/.env"
PROJECT_NAME=a-go-microservice
PROJECT_HOST=a-go-microservice.qlico
COMPOSE_PROJECT_NAME=a-go-microservice
```

We're not using the `.env` in the root of the project.

### Connecting to services

To connect to services running in `qlico-core` we're using:
These are inside the projects `.env` file:


```shell title="a-go-microservice/.env"
REDIS_DSN=redis://valkey:6379
RABBITMQ_DSN=amqp://guest:guest@rabbitmq4:5672/my-distributed-queue
```

The Go microservice is using Valkey for the cache connection and RabbitMQ for the
queue connection. But it could also be using Postgresql for the database, if needed.

If we want to access the Go microservice project in a
webbrowser: http://a-go-microservice.qlico
