# Docker - OpenCATS Installation Instructions

The Docker files in the OpenCATS repository are intended for **development and testing**. Some experienced users deploy OpenCATS with Docker in production, but the repository Docker Compose configuration is not the recommended production configuration without hardening, secret management, persistent backup planning, and web/database security review.

## Current architecture

The repository Docker Compose setup starts these services:

* `opencats_web`: Nginx web container, exposed on ports `80` and `443`.
* `opencats_php`: PHP-FPM container built from `docker/php/Dockerfile`.
* `opencats_data`: shared source/data volume mounting the repository into `/var/www/public`.
* `opencats_mariadb`: MariaDB database container.
* `opencats_phpmyadmin`: phpMyAdmin container exposed on port `8080`.

The PHP image is based on PHP 7.4 FPM Alpine and installs OpenCATS runtime tools and extensions, including document parser utilities, `mysqli`, `gd`, `soap`, `zip`, `ldap`, and `mcrypt`.

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
docker compose up -d --build
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

The CI/test environment uses `docker/docker-compose-test.yml`, MariaDB 10.7 containers, Selenium, PHPUnit, and Behat. See [Developer Guide](developer-guide.md) for the full local test workflow.

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
