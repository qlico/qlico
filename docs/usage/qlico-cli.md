# Qlico CLI

Qlico CLI is a shell script which makes it easier to work with your containers, projects and Qlico core.

## Setup

Copy the qlico shell script to your local bin directory. This way you can easily name it to your liking and make personal 	adjustments if needed. Also make sure the script is executable:

```bash
cp ~/qlico-core/qlico ~/.local/bin/
chmod u+x ~/.local/bin/qlico
```

Of course adjust the above commands if your directories are located elsewhere.

### Directory settings

Optionaly you can change the directory settings where qlico core is located and where your projects are:

```bash title="~/.local/bin/qlico"
#!/bin/bash

QLICO_CORE_DIR=~/qlico-core
QLICO_PROJECTS_DIR=~/qlico-core/projects
---
```

By convention `php` is used as the php container name and `nodejs` as the node.js container name for each project. If for some reason you prefer to use other names, you can change these as well. Make sure you always use these names in each of your projects.

```bash title="~/.local/bin/qlico"
---
QLICO_PHP=php
QLICO_NODEJS=nodejs
---
```

## Usage

```bash
qlico <project> <command> [arguments]
```

The qlico script should always be followed by the name of the directory in the QLICO_PROJECTS_DIR where your project resides. The name `core` is reserved and is meant to execute commands on the Qlico core stack.

### Common commands

These commands are available for all projects and core containers. All commands can be followed by container names if you want to apply the command for specific containers.

#### build

Build all of the images or the specified one(s):

```bash
# build all images in my-project
qlico my-project build

# build only the php and nodejs images in my-project
qlico my-project build php nodejs
```

#### destroy

Remove the entire Docker environment. All containers, images and volumes will be deleted! Useful if you want to purge your project. Use with caution, confirmation will be asked.

```bash
qlico my-project destroy
```

#### down

Stop and destroy all containers.

```bash
qlico my-project down
```

#### logs

Display and tail the logs of all containers or the specified one's.

```bash
qlico my-project logs
```

#### refresh

Shortcut for stopping and starting all containers.

```bash
qlico my-project refresh
```

#### restart

Restart the containers.

```bash
qlico my-project restart
```

#### stop

Stop the containers.

```bash
qlico my-project stop
```

#### up

Start the containers.

```bash
qlico my-project up
```

### PHP container commands

These commands are only available for the QLICO_PHP containers.

#### artisan

Run a Laravel artisan command

```bash
qlico laravel-project artisan make:command SendEmails
```

#### composer

Run a Composer command.

```bash
qlico my-project composer install
```

#### console

Run a Symfony console command.

```bash
qlico symfony-project console config:dump-reference framework
```

#### run

Run a command within the php container. This can be any executable available in the container.

```bash
qlico my-project run bin/my-shell-script
qlico my-project run php vendor/bin/ecs check src/Controller --fix
qlico my-project run ls -lR
```

#### xphp

Run a php command with Xdebug enabled. For more info on Xdebug, see: [Usage/Debugging with Xdebug](usage/xdebug.md)

```bash
qlico symfony-project xphp bin/console app:my-debug-command
```

### Node.js container commands

These commands are only available for the QLICO_NODEJS containers.

#### npm

Run a NPM command.

```bash
qlico my-project npm init -y
```

#### yarn

Run a Yarn command

```bash
qlico my-project yarn upgrade
```


## Tips

If you use one or more projects frequently, register an alias to save you some typing:

```bash
# without alias
qlico my-awesome-super-project composer update
# with alias masp="qlico my-awesome-super-project"
masp composer update
```

