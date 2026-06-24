---
description: >-
  This documentation explains how to install, enhance and use OpenCATS, the free
  open-source applicant tracking system available at opencats.org.
cover: .gitbook/assets/opencats-logo.png
coverY: -19.322916666666668
---

# Current release: 0.10.0

## Release information

The current OpenCATS application release documented here is **0.10.0**. The latest release packages are published at [https://github.com/opencats/OpenCATS/releases](https://github.com/opencats/OpenCATS/releases).

## Supported platform baseline

OpenCATS is built and tested in CI on a Linux/Unix environment with **PHP 7.4** and **MariaDB 10.7**. Other versions or platforms may work, but they are not the CI/CD baseline used by the project.

The main runtime dependencies are PHP, MariaDB, a web server, and the PHP extensions required by OpenCATS. Optional document parsing utilities such as `antiword`, `pdftotext`, `html2text`, and `unrtf` improve resume/document text extraction and search, but are not required just to complete installation.

MySQL is not the documented database target. MariaDB is the recommended and tested database family for current OpenCATS deployments.

## Which package should I install?

For a normal installation, download the release archive from GitHub Releases. Release archives are intended to be easier to install than a raw source checkout.

If you install from source, clone the repository, or download source code instead of a prepared release archive, install PHP dependencies with Composer:

```bash
composer install --no-dev
```

Use `--no-dev` for production so development and test packages are not installed. Developers who need to run the test suite should run Composer without `--no-dev`.

## Before upgrading

Before upgrading an existing OpenCATS installation, take a CLI database backup, back up `attachments/`, preserve your `config.php`, and test the restore/upgrade process outside production first. See [Backup, Restore, and Upgrade](technical-configuration-options/opencats-backup-restore-and-upgrade-instructions-this-section-incomplete.md).

## Documentation status

This documentation is maintained by the OpenCATS community. If you find a mistake or a missing workflow, please submit a pull request to the GitBook documentation repository.
