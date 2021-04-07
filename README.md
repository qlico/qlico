# Qlico

Qlico is an open-source project that makes it easier to run your (web) project
inside Docker, at the moment it’s just a set of configuration files.

It aims to accelerate development teams by using Docker in their development
workflow by using a hassle free Docker stack.

To keep this README short, everything you need to know about Qlico is
available [inside the docs](https://docs.qlico.dev/).

## Getting started

You can read the [Quick start](https://docs.qlico.dev/quick-start/) guide to get
up and running.

## Contributing

Qlico is open-source, you are more than welcome to contribute to this project!
Fork and make a Pull Request, or create an Issue if you see any problem (even
typos or spelling mistakes and grammer improvements are welcome).

## Contributing to the docs

You can find the docs in the `docs` folder.

### Installation

We prefer using [virtualenv](https://pypi.org/project/virtualenv/):

```bash
virtualenv -p python3 venv
source ./venv/bin/activate
pip install -r requirements.txt
```

### Commands

* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.

### More information

For more information please see the [index.md file](docs/index.md) inside
the `docs` folder.

## Stargazers

If you like using Qlico, please star
the [Qlico GitHub project](https://github.com/qlico/qlico).
