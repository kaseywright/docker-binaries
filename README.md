
# Run PHP, Composer, Node, and NPM via Docker

Easily run popular developer tools without installing them natively—everything runs in Docker containers for maximum portability and cleanliness.

# Requirements

Docker Engine

# Supported 

## Supported operating systems

- All unixes with kernel over 3.10
- Docker for mac
- Docker for mac edge

For OSX limitations and workaround go to [OSX Doc](OSX.md)

## Supported commands

- `node`
- `npm`
- `yarn`
- `php`
- `composer`

# Installation

Clone this repository.

```
git clone git@github.com:galileo/docker-binaries
```

Add the `bin` directory to your PATH by editing your shell profile (e.g. `~/.zshrc` or `~/.bash_profile`):

```
export PATH="$PATH:/path/to/this/repository/bin"
```

Reload your shell or run `source ~/.zshrc` (or appropriate file) to apply.


# Usage

## Prerequisites

If you already have local installations of node, npm, php, or composer, you can still use these Docker-based wrappers by ensuring your PATH prioritizes this repo's `bin/` directory. No need to uninstall your local tools unless you want to always use the Dockerized versions.

### How to Prioritize the Docker Binaries

Your shell uses the first matching executable it finds in the directories listed in your `PATH` variable, searching from left to right. By placing this repository's `bin/` directory at the front of your `PATH`, you ensure that commands like `php`, `composer`, `node`, and `npm` run the Dockerized versions instead of any native ones installed elsewhere.

**Example:**

Add this line to the top of your `~/.zshrc`, `~/.bash_profile`, or `~/.bashrc`:

```sh
export PATH="/path/to/this/repository/bin:$PATH"
```

After saving, reload your shell:

```sh
source ~/.zshrc   # or source ~/.bash_profile, depending on your shell
```

#### Checking Which Version Will Run

You can check which binary will be used by running:

```sh
which php
which composer
which node
which npm
```

If the output path points to this repository's `bin/` directory, the Dockerized version will run. If it points elsewhere, adjust your `PATH` so that `bin/` comes first.

#### Troubleshooting
- If you want to use the native version for a command temporarily, you can specify the full path (e.g., `/usr/local/bin/php -v`).
- If you want to revert to the native version by default, move or remove the `bin/` directory from the front of your `PATH`.

## Running commands

You can now use the following commands, which will be executed in Docker containers:

```bash
npm -v
node -v
php -v
composer -v
yarn -v
```

# Version switching

You can control which version of each tool is used by setting the appropriate environment variable before the command. For example:

```
DB_NODE_VERSION=20 node -v   # Runs Node.js 20.x
DB_PHP_VERSION=8 php -v      # Runs PHP 8.x
DB_COMPOSER_VERSION=2 composer -v # Runs Composer 2.x
```

By default, the latest version is used. Most images use the `alpine` variant for smaller size.
```

Run very speficic `node.js` release:

```
DB_NODE_VERSION=8.0.0 node -v
> v8.0.0
```

## Enabling the TTY mode

In some cases you need to interact with command. Like for example giving answers on `yarn init` command.

```
error An unexpected error occurred: "Can't answer a question unless a user TTY".
```

You can enable the TTY option with the environment settings:

```
TTY=enable yarn init
```

# Resources

Article about how you can setup your own repository and how to install this one.
