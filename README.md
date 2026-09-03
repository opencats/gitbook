---
description: >-
  This documentation explains how to install, enhance and use OpenCATS, the free
  open-source applicant tracking system available at opencats.org.
cover: .gitbook/assets/opencats-logo.svg
coverY: -19.322916666666668
---

# Current release: 0.11.1

## Release information

The current OpenCATS application release documented here is **0.11.1**. The latest release packages are published at [https://github.com/opencats/OpenCATS/releases](https://github.com/opencats/OpenCATS/releases).

## Supported platform baseline

OpenCATS requires **PHP 8.4.1 or later within PHP 8**. Project CI currently tests **PHP 8.4.1** and **PHP 8.5** on Linux/Unix with **MariaDB 10.7** for Docker-backed integration and Behat testing. Other database, operating system, or web server combinations may work, but they are not the project CI/CD baseline.

The main runtime dependencies are PHP, MariaDB, a web server, and the PHP extensions required by OpenCATS. Optional document parsing utilities such as `antiword`, `pdftotext`, `html2text`, and `unrtf` improve resume/document text extraction and search, but are not required just to complete installation.

MySQL is not the documented database target. MariaDB is the recommended and tested database family for current OpenCATS deployments.

## Which package should I install?

For a normal installation, download one of the prepared **full** release archives from GitHub Releases. These include the production Composer dependencies and are intended to be easier to install than GitHub's automatically generated source archives.

If you install from source, clone the repository, or download source code instead of a prepared full release archive, install PHP dependencies with Composer:

```bash
composer install --no-dev
```

Use `--no-dev` for production so development and test packages are not installed. Developers who need to run the test suite should run Composer without `--no-dev`.

## Before upgrading

Before upgrading an existing OpenCATS installation, take a CLI database backup, back up `attachments/`, preserve your `config.php`, and test the restore/upgrade process outside production first. See [Backup, Restore, and Upgrade](technical-configuration-options/opencats-backup-restore-and-upgrade-instructions-this-section-incomplete.md).

## Documentation status

This documentation is maintained by the OpenCATS community. If you find a mistake or a missing workflow, please submit a pull request to the GitBook documentation repository.
