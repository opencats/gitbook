# Installation

OpenCATS can be installed from a release archive, from source with Composer, or in the repository Docker environment for development and testing.

## Choose an installation path

* **Linux/Unix production-style install:** use the [Ubuntu installation guide](install-on-ubuntu.md) as the primary documented path.
* **Windows/WAMP/XAMPP install:** use the [Windows guide](install-on-windows.md). OpenCATS is known to be deployed on Windows, but the project builds and tests only in a Linux/Unix CI environment.
* **Docker:** use the [Docker guide](../technical-configuration-options/docker-opencats-installation-instructions.md) for development and testing. Some experienced users deploy Docker in production, but the repository Docker setup is not the recommended production configuration without hardening.
* **Existing installation upgrade:** read [Backup, Restore, and Upgrade](../technical-configuration-options/opencats-backup-restore-and-upgrade-instructions-this-section-incomplete.md) before changing files or database schema.

## Baseline requirements

OpenCATS 0.10.0 is built and tested with PHP 7.4 and MariaDB 10.7. Install required PHP extensions, create a MariaDB database and user, and ensure OpenCATS writable directories are owned by the web server user.

If you use a source checkout or source archive instead of a prepared release archive, install dependencies with:

```bash
composer install --no-dev
```

After files, database, and permissions are ready, continue to [Run the Installer](run-the-installer.md).
