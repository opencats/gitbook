# Developer Guide

This guide helps contributors run OpenCATS locally and mirror the current CI test workflow.

## Prerequisites

* Git
* Docker with Docker Compose v2
* PHP 8.4.1 or later within PHP 8 if running tests directly on the host
* Composer 2

Project CI currently tests PHP 8.4.1 and PHP 8.5.

## Clone and install dependencies

```bash
git clone https://github.com/opencats/OpenCATS.git
cd OpenCATS
composer install
```

Do not use `--no-dev` for development, because PHPUnit, Behat, and related tools are development dependencies.

## Prepare the Docker test environment

The local test workflow mirrors CI:

```bash
cp test/config.php ./config.php
touch ./INSTALL_BLOCK
cd docker/
docker compose -f docker-compose-test.yml up -d --build
```

The test Docker setup defaults to PHP 8.4.1. To build the test stack with another CI-supported PHP version, for example PHP 8.5:

```bash
PHP_VERSION=8.5 docker compose -f docker-compose-test.yml up -d --build
```

The test Docker setup uses two MariaDB 10.7 databases:

* `opencatsdb` on port `3306` for functional and Behat testing.
* `integrationtestdb` on port `3307` for disposable PHPUnit integration tests.

Install dependencies inside the PHP container if needed:

```bash
docker compose -f docker-compose-test.yml exec -T --workdir /var/www/public php composer install --no-interaction --prefer-dist
```

## Run tests

From the OpenCATS repository, unit tests can be run directly with:

```bash
./vendor/bin/phpunit --testsuite UnitTests
```

From the `docker/` directory, the Docker-backed suites are:

```bash
docker compose -f docker-compose-test.yml exec -T --workdir /var/www/public php ./vendor/bin/phpunit --testsuite IntegrationTests
docker compose -f docker-compose-test.yml exec -T --workdir /var/www/public php ./vendor/bin/behat -c ./test/behat.yml --suite="default"
docker compose -f docker-compose-test.yml exec -T --workdir /var/www/public php ./vendor/bin/behat -c ./test/behat.yml --suite="security"
```

Current CI runs PHPUnit unit tests, Docker-backed PHPUnit integration tests, the default Behat suite, and the security Behat suite for both PHP 8.4.1 and PHP 8.5. CI also performs a PHP syntax check over `src/` and a Composer audit. The audit is currently non-blocking so legacy dependency advisories can be reviewed without stopping all builds.

## Pull request title format

OpenCATS pull request titles must use:

```text
type: description
```

Allowed types are:

* `chore`
* `docs`
* `feat`
* `fix`
* `refactor`
* `security`
* `test`

Scopes are not currently allowed. For example, use `fix: correct login redirect`, not `fix(auth): correct login redirect`.

## Development notes

* Do not commit local credentials or production `config.php` changes.
* Reuse existing OpenCATS helpers and conventions rather than introducing duplicate utility code.
* Use the Docker test stack when changing database, authorization, upload, import, installer, or PHP-version-sensitive behavior.
* Add or update tests when changing application behavior.
* Shut down the test stack when finished:

```bash
cd docker
docker compose -f docker-compose-test.yml down
```
