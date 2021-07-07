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

## dnsmasq

For the best Qlico experience please install [dnsmasq](dnsmasq.md), it's not
mandatory, you can also use a `/etc/hosts` file.
