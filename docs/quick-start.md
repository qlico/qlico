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

By default, Qlico only comes with a reverse proxy (Traefik), if you would like
more services use the Examples, for example Redis see
the [Examples/Redis](examples/redis.md). If you're missing a service, feel free
to contribute!

After customizing to your needs, you can do a `docker-compose up` inside
the `qlico-core` folder.

Congratulations, you're now running `qlico-core`.

## Adding Qlico to your (existing) project(s)

To start using Qlico, you'll need a `qlico` folder and `Dockerfile` inside
your (existing) project. You can find these files in
the [project-examples directory](https://github.com/qlico/qlico/tree/main/project-examples).

1. Copy the `qlico` folder (with all the files) to the (new or existing) project folder.
2. Copy a `Dockerfile.phpversion` to the projectfolder and rename it to: `Dockerfile`, for the `phpversion` you can take a version you want.

New since the `Dockerfile.php83` version is that there is only one `Dockerfile`, no different variants anymore.

## Add / remove PHP extensions

Since the `Dockerfile.php83` we've started using [docker-php-extension-installer](https://github.com/mlocati/docker-php-extension-installer/) by [Michele Locati](https://github.com/mlocati).

If you want to add or remove an PHP extension, please see the [Supported PHP extensions](https://github.com/mlocati/docker-php-extension-installer/?tab=readme-ov-file#supported-php-extensions).

In your `Dockerfile` search for the following part, and remove/add PHP extensions you would like to use.
We recommended to use the `-stable` suffix, to make sure you're using a stable version of the PHP extension.

For example:

```Docker
    && install-php-extensions \
            APCu-stable \
            bcmath-stable \
```
If you want to add `imagick` and remove `bcmath` you can change it to:

```Docker
    && install-php-extensions \
            APCu-stable \
            imagick-stable \
```

**Note:** There are multiple stages inside the Dockerfile, if you want to run Qlico in production with for example (Datadog profiling), you'll only need to change the `install-php-extensions` in the `prod` stage.

Please keep in mind, we're not maintaining the PHP extensions, so if an PHP extension if not working, please do not open an issue in this repository.


## dnsmasq

For the best Qlico experience please install [dnsmasq](dnsmasq.md), it's not
mandatory, you can also use a `/etc/hosts` file.
