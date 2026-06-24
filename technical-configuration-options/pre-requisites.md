# Prerequisites

This page lists the current baseline for OpenCATS 0.10.0.

## Tested CI/CD baseline

OpenCATS is built and tested in CI on Linux with:

* PHP 7.4
* Composer 2
* MariaDB 10.7 for Docker-backed integration and Behat tests

Other PHP, database, operating system, or web server combinations may work, but this is the baseline used by project CI/CD.

## Required runtime components

* A Linux/Unix web server environment such as Apache or Nginx with PHP-FPM.
* PHP 7.4.
* MariaDB. MariaDB 10.7 is used by current Docker test services.
* Composer if you install from source rather than from a prepared release archive.
* Writable OpenCATS directories for uploads, attachments, temporary files, and restore operations when used.

## PHP extensions

The repository Docker image installs the PHP extensions OpenCATS expects in that environment:

* `mysqli`
* `gd`
* `soap`
* `zip`
* `ldap`
* `mcrypt`

On distribution packages, extension package names vary. Ubuntu-style package names are usually similar to `php7.4-mysqli`, `php7.4-gd`, `php7.4-soap`, `php7.4-zip`, and `php7.4-ldap`.

## Composer dependencies

OpenCATS runtime Composer dependencies include CKEditor, Sphinx search API support, PHPMailer, and FPDF. For production source installs, run:

```bash
composer install --no-dev
```

For development and testing, omit `--no-dev` so PHPUnit, Behat, and related test packages are installed.

## Optional document parsing utilities

OpenCATS can extract text from uploaded documents when parser utilities are installed and configured in `config.php`:

* `antiword`
* `pdftotext` / poppler utilities
* `html2text`
* `unrtf`

These utilities are useful for resume search and parsing, but they are not required to launch the installer.

## Optional Sphinx search

Sphinx is optional. Enable it only after Sphinx is installed and the `ENABLE_SPHINX`, `SPHINX_HOST`, `SPHINX_PORT`, and `SPHINX_INDEX` settings are configured in `config.php`.

## Writable directories

The web server user must be able to write to the directories OpenCATS uses for runtime files. Common directories include:

* `attachments/`
* `upload/`
* `temp/`
* `restore/` when restoring from a GUI backup file

Avoid world-writable permissions such as `777` unless you fully understand the risk and have no safer option in your hosting environment.
