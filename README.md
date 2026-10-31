---
description: >-
  This documentation explains how to install, enhance and use OpenCATS, the free
  open-source applicant tracking system available at opencats.org.
cover: .gitbook/assets/opencats-logo.svg
coverY: -19.322916666666668
---

# Current release: 0.12.0

## Release information

The current OpenCATS application release documented here is **0.12.0**. The latest release packages are published at [https://github.com/opencats/OpenCATS/releases](https://github.com/opencats/OpenCATS/releases).

## Supported platform baseline

OpenCATS requires **PHP 8.4.1 or later within PHP 8**. Project CI currently tests **PHP 8.4.27** and **PHP 8.5.12** on Linux/Unix with **MariaDB 10.7** for Docker-backed integration and Behat testing. Other database, operating system, or web server combinations may work, but they are not the project CI/CD baseline.

The main runtime dependencies are PHP, MariaDB, a web server, and the PHP extensions required by OpenCATS. Optional document parsing utilities such as `antiword`, `pdftotext`, `html2text`, and `unrtf` improve resume/document text extraction and search, but are not required just to complete installation.

**MariaDB is the supported and tested database family. MySQL is not supported.** On some MySQL/shared-hosting installations, MySQL's `ONLY_FULL_GROUP_BY` mode causes existing OpenCATS queries to fail. If your host offers only MySQL, see the [known limitation and optional session-only workaround](installation/run-the-installer.md#unsupported-mysql-hosting-and-only_full_group_by). This is not a supported configuration, and additional incompatibilities may remain.

The Composer minimum is a compatibility requirement, not a security recommendation. Use a currently patched PHP 8.4 or 8.5 runtime, including your distribution's security fixes. Updating OpenCATS does not update PHP on the server.

## What changed in 0.12.0?

* Bootstrap 5.3 styling across the main application, login, installer and public Career Portal.
* Locally bundled SunEditor replaces CKEditor; Chart.js replaces Artichow, with accessible tables for chart values.
* ISO dates (`YYYY-MM-DD`) alongside US and UK formats.
* Stronger permission checks, administrator-only CSV exports, and mandatory default administrator password changes.
* Career Portal CAPTCHA and server-side candidate sessions replace persistent candidate authentication cookies.

Read the [0.12.0 release notes](https://github.com/opencats/OpenCATS/releases/tag/v0.12.0), [current user guidance](using-the-software/README.md), and [upgrade notes](technical-configuration-options/opencats-backup-restore-and-upgrade-instructions-this-section-incomplete.md#upgrading-to-0120) before deploying.

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

This documentation is maintained by the OpenCATS community. If you find a mistake or a missing workflow, please submit a pull request to the [GitBook documentation repository](https://github.com/opencats/gitbook). Some older integration guides are retained as historical reference and are labelled accordingly.
