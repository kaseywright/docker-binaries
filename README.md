
# Run PHP, Composer, Node, and NPM via Docker or Podman

Easily run popular developer tools without installing them natively—everything runs in containers for maximum portability and cleanliness.

# Requirements

[Docker Engine](https://docs.docker.com/get-docker/) or [Podman](https://podman.io/)

# Supported 

## Supported operating systems

- All unixes with kernel over 3.10
- Docker for mac

For OSX limitations and workaround go to [OSX Doc](OSX.md)


_NOTE: the OSX Doc is not up to date_

## Supported commands

- `node`
- `npm`
- `yarn`
- `php`
- `composer`
- `go`
- `python`
- `uv`
- `bun`

## Container Images Used

| Script | Base Image | Registry | Notes |
|--------|------------|----------|-------|
| `node`, `npm`, `yarn` | `node` | `docker.io/library/node` | Official Node.js images |
| `php` | `php` | `docker.io/library/php` | Official PHP images |
| `composer` | `composer` | `docker.io/library/composer` | Official Composer images |
| `python` | `python` | `docker.io/library/python` | Official Python images |
| `go` | `golang` | `docker.io/golang` | Official Go images |
| `bun` | `oven/bun` | `docker.io/oven/bun` | Official Bun images |
| `uv` | `astral/uv` | `docker.io/astral/uv` | Official UV images |

All images use explicit `docker.io/` registry prefixes for optimal compatibility with both Docker and Podman.

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
which yarn
which go
which python
which uv
```

If the output path points to this repository's `bin/` directory, the Dockerized version will run. If it points elsewhere, adjust your `PATH` so that `bin/` comes first.

#### Troubleshooting
- If you want to use the native version for a command temporarily, you can specify the full path (e.g., `/usr/local/bin/php -v`).
- If you want to revert to the native version by default, move or remove the `bin/` directory from the front of your `PATH`.

## Running commands

You can now use the following commands, which will be executed in containers:

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

Run very speficic `node.js` release:

```
DB_NODE_VERSION=8.0.0 node -v
> v8.0.0
```

## Container Runtime Selection

The scripts automatically detect which container runtime is available on your system:

- **Podman** is preferred if available (detected first)
- **Docker** is used as a fallback if Podman is not found
- If neither is found, an error is displayed

You can override the automatic detection by setting the `DB_CONTAINER_RUNTIME` environment variable:

```
DB_CONTAINER_RUNTIME=podman node -v    # Force use of Podman
DB_CONTAINER_RUNTIME=docker node -v    # Force use of Docker
```

The automatic detection makes the scripts work out of the box on most systems without requiring manual configuration.

## Enabling the TTY mode

In some cases you need to interact with command. Like for example giving answers on `yarn init` command.

```
error An unexpected error occurred: "Can't answer a question unless a user TTY".
```

You can enable the TTY option with the environment settings:

```
TTY=enable yarn init
```