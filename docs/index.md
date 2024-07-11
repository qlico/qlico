# Welcome to the Qlico Documentation

Qlico is an open-source project designed to simplify running your web projects
inside Docker. At its core, it is a set of configuration files intended to
streamline the development process for teams by providing a hassle-free Docker
stack.

## Why Choose Qlico?

- **Easy Configuration**: Select only the parts/services you need.
- **Customizable**: Use and extend your own configuration files effortlessly.
- **User-Friendly**: Maintains the default `docker compose` experience with
  enhanced testing.
- **Team Collaboration**: Standardize configurations across projects (
  e.g., `postgres16` for PostgreSQL 16, `mysql8` for MySQL 8).

## Prerequisites

- [Docker >= 26.0](https://docs.docker.com/get-docker/){:target="_blank"}
- [dnsmasq (optional, but recommended)](dnsmasq.md)
- Basic knowledge of Docker and Docker Compose

For beginners:

- [Docker Get Started](https://docs.docker.com/get-started/){:target="_blank"} (Skip "Share the application part" as Docker Hub account is not needed)
- [Docker for Beginners](https://docker-curriculum.com/){:target="_blank"} (Skip all AWS parts, because it's using Elastic Beanstalk)

## Best Docker Experience

- **Windows**: [Docker Desktop with WSL2](https://www.docker.com/products/docker-desktop/){:target="_blank"}.
- **macOS**: [OrbStack](https://orbstack.dev/){:target="_blank"}
- **Linux**:
    - [Arch Linux, see this excellent wiki page](https://wiki.archlinux.org/title/docker){:target="_blank"}
    - [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/){:target="_blank"}
    - [Debian](https://docs.docker.com/install/linux/docker-ce/debian/){:target="_blank"}
    - [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/){:target="_blank"}
    - [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/){:target="_blank"}

## Getting Started

To start using Qlico, please refer to the [Quick Start Guide](quick-start.md).

## History

Qlico began in 2017 as a side project to manage multiple Dockerfiles for various
PHP versions. Initially, all projects were mounted inside a single webserver,
leading to performance issues on macOS and difficulties in managing PHP versions
per project. To overcome these challenges, [Tom](https://github.com/TomKeur){:target="_blank"}
created a new, more efficient Docker stack, and Qlico was born.

## The Name Qlico

The name Qlico is derived from "Kliko," a term for plastic trash cans in the
Netherlands, symbolizing the use of "containers" in Docker/Kubernetes.

> "There are only two hard things in Computer Science: cache invalidation and
> naming things."
> -- Phil Karlton

[Picture of Kliko's](https://nl.wikipedia.org/wiki/Minicontainer#/media/Bestand:Kliko_op_Bonaire,_Nederlandse_Antillen_-_IMG_8372.JPG){:target="_blank"}

## Qlico in Production

Qlico is suitable for both development and production environments. 
Use the `prod` stage of the `Dockerfile` for production deployment. 
Many companies successfully run Qlico in production within Kubernetes clusters. 
For assistance, contact [Tom](https://github.com/TomKeur){:target="_blank"} or start
a [GitHub Discussion](https://github.com/qlico/qlico/discussions){:target="_blank"}.

## Contributing

Qlico is open-source, and contributions are welcome! Fork the project, make a
Pull Request, or create an Issue for any problems, including typos and grammar
improvements.

## License 

Qlico is licensed under the [MIT License](https://github.com/qlico/qlico/blob/main/LICENSE){:target="_blank"} with one exception:
The name of a copyright holder shall not
be used in advertising or otherwise to promote the sale, use or other dealings
in this Software without prior written authorization of the copyright holder.

## Stargazers

If you enjoy using Qlico, please star
the [Qlico GitHub project](https://github.com/qlico/qlico){:target="_blank"}.
