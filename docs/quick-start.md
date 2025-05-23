# Quick start

## Cloning the repo

Clone the Qlico repository as `qlico-core`, after cloning enter the `qlico-core`
folder, copy `docker-compose.yaml` and `.env.dist` from the `dist` folder to the
mainroot of `qlico-core`.
(you can also run the following commands on your favourite terminal)

```bash
git clone https://github.com/qlico/qlico.git qlico-core
cd qlico-core
cp dist/.env.dist .env
cp dist/docker-compose.yaml docker-compose.yaml
```

By default, Qlico only comes with a reverse proxy
[Traefik](https://traefik.io/traefik/){:target="_blank"},
if you would like more services use the Examples, for example Valkey see
the [Examples/Valkey](examples/valkey.md).
If you're missing a service, feel free to contribute!

After customizing to your needs, you can do a `docker compose up` inside
the `qlico-core` folder.

Congratulations, you're now running `qlico-core`.

More information about Qlico architecture can be found in the [Qlico architecture](usage/qlico-architecture.md) page.

## Adding Qlico to your (existing) project(s) - NGINX (default)

To start using Qlico, you'll need a `qlico` folder and `Dockerfile` inside
your (existing) project. You can find these files in
the [project-examples directory](https://github.com/qlico/qlico/tree/main/project-examples).

1. Copy the `project/examples/qlico` folder (with all the files) to the (new or existing) project folder.
2. Copy the `projects/examples/Dockerfile` to the root of the project folder.
3. Copy the `qlico/.env.dist` to the `qlico` folder and rename it to `.env`.
4. Edit the `.env` file to your needs.

## Adding Qlico to your (existing) project(s) - Apache2

To start using Qlico, you'll need a `qlico` folder and `Dockerfile` inside
your (existing) project. You can find these files in
the [project-examples directory](https://github.com/qlico/qlico/tree/main/project-examples).

1. Copy the `project/examples/qlico-apache2` folder (with all the files) to the (new or existing) project folder as `qlico`, so make sure to rename `qlico-apache` to `qlico`.
2. Copy the `Dockerfile` to the root of the project folder.
3. Copy the `qlico/.env.dist` to the `qlico` folder and rename it to `.env`.
4. Edit the `.env` file to your needs.

!!! note

    Since PHP 8.3 we've renamed `Dockerfile.phpX` to `Dockerfile`, you can
    edit the `Dockerfile` for the version you want.
    We've also added connecting through a unix socket, instead of a port.

## Add / remove PHP extensions

Since the PHP 8.3 we've started using
[docker-php-extension-installer](https://github.com/mlocati/docker-php-extension-installer/){:target="_blank"}
by [Michele Locati](https://github.com/mlocati){:target="_blank"}.

If you want to add or remove an PHP extension, please see the
[Supported PHP extensions](https://github.com/mlocati/docker-php-extension-installer/?tab=readme-ov-file#supported-php-extensions){:target="_blank"}.

In your `Dockerfile` search for the following part, and remove/add PHP
extensions you would like to use.
We recommended to use the `-stable` suffix, to make sure you're using a stable
version of the PHP extension.

For example:

Edit `Dockerfile`

Before:
```Dockerfile title="Dockerfile"
    && install-php-extensions \
            APCu-stable \
            bcmath-stable \
```
If you want to add `imagick` and remove `bcmath` you can change it to:

After:
```Dockerfile title="Dockerfile"
    && install-php-extensions \
            APCu-stable \
            imagick-stable \
```

**Note:** There are multiple stages inside the Dockerfile, if you want to run
Qlico in production with for example (Datadog profiling), you'll only need to
change the `install-php-extensions` in the `prod` stage.

Please keep in mind, we're not maintaining the PHP extensions, so if an PHP
extension if not working, please do not open an issue in this repository.


## How do I connect to services running in `qlico-core`?

In this example we will use `postgres17` as an example service, based on
[postgresql 17 example](examples/postgresql.md).

### From your host machine

You can connect to a service if you open the port in the `docker-compose.yaml`
file, you can open the port by adding the following line to the service:

```yaml title="qlico-core/docker-compose.yaml"
services:
  postgres17:
    image: postgres:17-alpine
    ports:
      - 5432:5432
```

So in this example would connect to: `localhost:5432` to access the service.

### From a container

You can connect to services running in `qlico-core` by using the service name,
for example, if you have a service called `postgres17` you can connect to it by
using `postgres17` as the hostname.

```yaml title="qlico-core/docker-compose.yaml"
services:
  postgres17:
    image: postgres:17-alpine
```

## dnsmasq

For the best Qlico experience please install [dnsmasq](dnsmasq.md), it's not
mandatory, you can also use a `/etc/hosts` file.
