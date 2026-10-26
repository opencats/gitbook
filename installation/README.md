# Installation

OpenCATS can be installed from a release archive, from source with Composer, or in the repository Docker environment for development and testing.

## Choose an installation path

* **Linux/Unix production-style install:** use the [Ubuntu installation guide](install-on-ubuntu.md) as the primary documented path.
* **Windows/WAMP/XAMPP install:** use the [Windows guide](install-on-windows.md). OpenCATS is known to be deployed on Windows, but the project builds and tests only in a Linux/Unix CI environment.
* **Docker:** use the [Docker guide](../technical-configuration-options/docker-opencats-installation-instructions.md) for development and testing. Some experienced users deploy Docker in production, but the repository Docker setup is not the recommended production configuration without hardening.
* **Existing installation upgrade:** read [Backup, Restore, and Upgrade](../technical-configuration-options/opencats-backup-restore-and-upgrade-instructions-this-section-incomplete.md) before changing files or database schema.

## Baseline requirements

OpenCATS 0.12.0 requires PHP 8.4.1 or later within PHP 8. Project CI tests PHP 8.4.27 and PHP 8.5.12, with MariaDB 10.7 used by the Docker-backed integration and Behat test environment. Install the required PHP extensions, create a MariaDB database and user, and ensure OpenCATS writable directories are owned by the web server user.

Use a currently patched PHP 8.4 or 8.5 runtime; the Composer minimum does not mean that old PHP patch releases are secure. See [Prerequisites](../technical-configuration-options/pre-requisites.md).

For normal installations, use a prepared **full** release archive from GitHub Releases so production Composer dependencies are already included.

If you use a source checkout or GitHub source archive instead, install dependencies with:

```bash
composer install --no-dev
```

After files, database, and permissions are ready, continue to [Run the Installer](run-the-installer.md).
