# Developer Guide

This guide helps contributors run OpenCATS locally and mirror the current CI test workflow.

## Prerequisites

* Git
* Docker with Docker Compose v2
* PHP 8.4.1 or later within PHP 8 if running tests directly on the host
* Composer 2

Project CI currently tests PHP 8.4.27 and PHP 8.5.12.

## Clone and install dependencies

```bash
git clone https://github.com/opencats/OpenCATS.git
cd OpenCATS
composer install
```

Do not use `--no-dev` for development, because PHPUnit, Behat, and related tools are development dependencies.

## Prepare the Docker test environment

Use a dedicated development checkout: the preparation command replaces `config.php` with test credentials. The local test workflow mirrors CI:

```bash
cp test/config.php ./config.php
touch ./INSTALL_BLOCK
cd docker/
docker compose -f docker-compose-test.yml pull php
docker compose -f docker-compose-test.yml up -d
```

The test stack uses the prebuilt `opencats/php-base:8.4.27` image by default. To select the other 0.12.0 CI image, pull and start it with the same version value:

```bash
PHP_VERSION=8.5.12 docker compose -f docker-compose-test.yml pull php
PHP_VERSION=8.5.12 docker compose -f docker-compose-test.yml up -d
```

The repository also provides `./test/runLocalTests.sh` from the project root to run integration and Behat regressions. It recreates the Docker test environment and test volumes and restores the developer configuration afterward; run it only against disposable test data. Set `PHP_VERSION=8.5.12` when running it to select that image. Run unit tests separately as shown below. See the [versioned testing notes](https://github.com/opencats/OpenCATS/blob/v0.12.0/README-testing.md).

The test Docker setup uses two MariaDB 10.7 databases:

* `opencatsdb` on container port `3306` for functional and Behat testing; Compose assigns its published host port.
* `integrationtestdb` on container port `3306`, published on host port `3307`, for disposable PHPUnit integration tests. These tests recreate their test database; never point them at production.

Install dependencies inside the PHP container if needed:

```bash
docker compose -f docker-compose-test.yml exec -T --workdir /var/www/public php composer install --no-interaction --prefer-dist
```

For the manual workflow, wait for both database containers to report healthy in `docker compose -f docker-compose-test.yml ps`. Before running the suites, check application database readiness and finalize its schema version, as CI does:

```bash
docker compose -f docker-compose-test.yml exec -T --workdir /var/www/public php php test/scripts/waitForDb.php
docker compose -f docker-compose-test.yml exec -T --workdir /var/www/public php php test/scripts/finalizeInstallSchemaVersion.php
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

Current CI runs PHPUnit unit tests, Docker-backed PHPUnit integration tests, the default Behat suite, and the security Behat suite for both PHP 8.4.27 and PHP 8.5.12. CI also performs a PHP syntax check over `src/` and a Composer audit. The audit is currently non-blocking so legacy dependency advisories can be reviewed without stopping all builds.

## Custom templates and extensions in 0.12.0

Preserve the Bootstrap markup, existing permission checks, CSRF handling and shared dialogs when changing templates. Rich-text fields now use `js/suneditor-manager.js` and locally bundled SunEditor. Chart rendering uses `lib/Charts.php`, `js/charts.js` and locally bundled Chart.js. Graph hooks receive structured chart data rather than Artichow objects. See the [chart migration notes](https://github.com/opencats/OpenCATS/blob/v0.12.0/docs/chart-migration.md) and [SunEditor notes](https://github.com/opencats/OpenCATS/blob/v0.12.0/js/suneditor/README.md) before adapting extensions.

Read [CONTRIBUTING.md](https://github.com/opencats/OpenCATS/blob/master/CONTRIBUTING.md) and the [Modernisation & Contributor Roadmap](https://github.com/orgs/opencats/projects/2) before starting substantial work.

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
