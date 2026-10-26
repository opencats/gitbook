# Docker - OpenCATS Installation Instructions

The Docker files in the OpenCATS repository are intended for **development and testing**. Some experienced users deploy OpenCATS with Docker in production, but the repository Docker Compose configuration is not the recommended production configuration without hardening, secret management, persistent backup planning, and web/database security review.

## Current architecture

The repository Docker Compose setup starts these services:

* `opencats_web`: Nginx web container, exposed on ports `80` and `443`.
* `opencats_php`: PHP-FPM container using the prebuilt `opencats/php-base` image; `docker/php/Dockerfile` is the source used to build that image.
* `opencats_data`: shared source/data volume mounting the repository into `/var/www/public`.
* `opencats_mariadb`: MariaDB database container.
* `opencats_phpmyadmin`: phpMyAdmin container exposed on port `8080`.

The PHP image defaults to **PHP 8.4.27 FPM Alpine**. The 0.12.0 CI matrix also uses **PHP 8.5.12**. The image installs OpenCATS runtime tools and extensions including document parser utilities, `mysqli`, `gd`, `soap`, `zip`, `ldap`, and `mbstring`.

## Default development credentials

The default Docker Compose database values are for local development only:

* MariaDB root password: `root`
* Database: `cats`
* User: `dev`
* Password: `dev`
* phpMyAdmin: [http://localhost:8080](http://localhost:8080)
* OpenCATS web: [http://localhost](http://localhost)

Do not expose these defaults to the internet.

## Start the development environment

From the OpenCATS application repository:

```bash
cd docker
docker compose pull php
docker compose up -d
```

If you are working from source and dependencies are missing, install Composer dependencies in the PHP container:

```bash
docker compose exec --workdir /var/www/public php composer install
```

For production-style dependency installation, use `composer install --no-dev` instead.

## Running the installer in Docker

Open [http://localhost](http://localhost) and follow the installer. Use the database values from the Docker Compose configuration:

* Database host: `opencatsdb`
* Database name: `cats`
* Database user: `dev`
* Database password: `dev`

If an existing `INSTALL_BLOCK` file prevents the installer from starting in a fresh development environment, remove it from the mounted repository. After installation, ensure `INSTALL_BLOCK` exists again.

## Test Docker environment

The CI/test environment uses `docker/docker-compose-test.yml`, MariaDB 10.7 containers, Selenium, PHPUnit, and Behat. It defaults to the prebuilt `opencats/php-base:8.4.27` image and uses `PHP_VERSION` to select another published image tag, such as `8.5.12`. The Compose files do not build the PHP image.

For example:

```bash
cd docker
PHP_VERSION=8.5.12 docker compose -f docker-compose-test.yml pull php
PHP_VERSION=8.5.12 docker compose -f docker-compose-test.yml up -d
```

See [Developer Guide](developer-guide.md) for the full local test workflow.

## Production warning

If you choose to adapt Docker for production, at minimum you should:

* Replace all default passwords.
* Use managed secrets rather than committing credentials.
* Pin image versions.
* Use durable database and attachment storage volumes.
* Back up MariaDB and `attachments/` outside the containers.
* Put TLS termination and HTTP security controls in front of OpenCATS.
* Restrict phpMyAdmin or remove it entirely.
* Review upload directory execution restrictions.

This is an advanced deployment path and is not the primary recommended production installation in this documentation.
