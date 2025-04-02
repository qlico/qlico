# Using TLS/S

mkcert is a simple tool for making locally-trusted development certificates. It
requires no configuration.

## How to get TLS/SSL for your local websites?

In this example we will create a wildcard self signed certificated
for `*.s.qlico`, you can change this if you want.

First install [mkcert](https://github.com/FiloSottile/mkcert)

After installing mkcert, go to the qlico-core folder and run the following
commands

```bash
mkdir -p traefik/certs
cd traefik/certs
mkcert  "*.s.qlico"
mkcert -install
```

Now let's create a Traefik configuration file for TLS: `traefik/tls.toml`

```toml title="qlico-core/traefik/tls.toml"
[tls]

[tls.stores]

[tls.stores.default]

[tls.stores.default.defaultCertificate]
certFile = "/configuration/certs/_wildcard.s.qlico.pem"
keyFile = "/configuration/certs/_wildcard.s.qlico-key.pem"
```

Now change the `docker-compose.yaml` file inside Qlico core:

Change the `command`, `ports`, `volumes` and `labels`.

```yaml title="qlico-core/docker-compose.yaml" hl_lines="7-14 19 22 24-25"
---
# Author: Qlico <hello@qlico.dev>
services:
  traefik:
    image: traefik:v3.3.5
    container_name: qlico-core_traefik
    command:
      - --providers.docker
      - --api.insecure
      - --entryPoints.web.address=:80
      - --entryPoints.websecure.address=:443
      - --api.insecure=true
      - --api.dashboard=true
      - --providers.file.filename=/configuration/tls.toml
    networks:
      - qlico-core
    ports:
      - 80:80
      - 443:443
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./traefik:/configuration
    labels:
      - "traefik.http.routers.traefik.entrypoints=web,websecure"
      - "traefik.http.routers.traefik.rule=Host(`traefik.qlico`) || Host(`traefik.s.qlico`)"
      - "traefik.http.services.traefik.loadbalancer.server.port=8080"
networks:
  qlico-core:
    driver: bridge
    name: qlico-core
```

## Change your project `.env`

Make sure to change the `PROJECT_HOST` in the `.env` file inside your project.

```bash title="qlico/.env"
PROJECT_HOST=example.s.qlico
```

## In your project change the `docker-compose.yaml`

### NGINX (default)
```yaml title="qlico/docker-compose.yaml" hl_lines="18"
# Author: Qlico <hello@qlico.dev>
---
services:
  nginx:
    image: nginxinc/nginx-unprivileged:1.27.0-alpine3.19
    container_name: ${PROJECT_NAME}_nginx
    volumes:
      - ./services/nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./services/nginx/mime.types:/etc/nginx/mime.types
      - ../:/var/www/html
      - php-socket:/php-socket
    depends_on:
      - php
    labels:
      # use this if you have multiple subdomains using this project
      #kk- "traefik.http.routers.${PROJECT_NAME}_nginx.rule=HostRegexp(`{subdomain:api|assets|www}.${PROJECT_HOST}`)"
      - "traefik.http.routers.${PROJECT_NAME}_nginx.rule=Host(`${PROJECT_HOST}`)"
      - "traefik.http.routers.${PROJECT_NAME}_nginx.tls=true"
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
  nodejs:
    build: services/nodejs
    container_name: ${PROJECT_NAME}_nodejs
    volumes:
      - ../:/var/www/html:cached
    networks:
      - qlico-core
volumes:
  php-socket:
    driver: local
networks:
  qlico-core:
    external: true
    name: qlico-core
```

### Apache
```yaml title="qlico/docker-compose.yaml" hl_lines="17"
# Author: Qlico <hello@qlico.dev>
---
services:
  apache2:
    image: httpd:2.4.59-bookworm
    container_name: ${PROJECT_NAME}_apache2
    volumes:
      - ./services/apache2/httpd.conf:/usr/local/apache2/conf/httpd.conf
      - ../:/var/www/html
      - php-socket:/php-socket
    depends_on:
      - php
    labels:
      # use this if you have multiple subdomains using this project
      #- "traefik.http.routers.${PROJECT_NAME}_apache2.rule=HostRegexp(`{subdomain:api|assets|www}.${PROJECT_HOST}`)"
      - "traefik.http.routers.${PROJECT_NAME}_apache2.rule=Host(`${PROJECT_HOST}`)"
      - "traefik.http.routers.${PROJECT_NAME}_apache2.tls=true"
      - "traefik.http.services.${PROJECT_NAME}_apache2.loadbalancer.server.port=8080"
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
  nodejs:
    build: services/nodejs
    container_name: ${PROJECT_NAME}_nodejs
    volumes:
      - ../:/var/www/html:cached
    networks:
      - qlico-core
volumes:
  php-socket:
    driver: local
networks:
  qlico-core:
    external: true
    name: qlico-core

```


## Visit your project

Open the browser: https://example.s.qlico
