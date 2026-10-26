# Install on Windows

OpenCATS is known to be deployed in many Windows/WAMP/XAMPP environments. However, the OpenCATS project builds and tests in a Linux/Unix CI environment only. WAMP and XAMPP can work, but Windows-specific behavior is not covered by automated project testing.

## Windows prerequisites

Install a Windows web stack that provides:

* PHP 8.4.1 or later within PHP 8
* MariaDB
* Apache or another PHP-capable web server
* phpMyAdmin or another MariaDB administration tool

OpenCATS CI currently tests PHP 8.4.27 and PHP 8.5.12. XAMPP and WAMP are common Windows choices, but make sure the version you install provides a compatible PHP release or allows you to configure one.

Use a currently patched PHP 8.4 or 8.5 build. Upgrading OpenCATS alone does not patch the Windows PHP runtime.

## Download OpenCATS

Download the current prepared **full** release archive from [GitHub Releases](https://github.com/opencats/OpenCATS/releases). The full release archive includes production Composer dependencies. Extract it under your web root, for example:

```text
C:\xampp\htdocs\opencats
```

If you download source code or clone the repository instead of using a prepared full release archive, install Composer for Windows and run this from the OpenCATS directory:

```bat
composer install --no-dev
```

## Start Apache and MariaDB

Open your XAMPP or WAMP control panel and start only the services you need:

* Apache
* MariaDB/MySQL service provided by the stack

Even if the control panel labels the service as MySQL, use a MariaDB-backed stack for the documented OpenCATS path.

## Create the database in phpMyAdmin

Open phpMyAdmin, usually at:

```text
http://localhost/phpmyadmin/
```

Create a database named `opencats`. Use UTF-8 character set and collation when available, for example:

```sql
CREATE DATABASE opencats CHARACTER SET utf8 COLLATE utf8_general_ci;
```

Create a database user, for example `opencats`, and grant that user all privileges on the `opencats` database. Use a strong password and save it for the installer.

## Configure parser utility paths on Windows

Document parser utilities are optional, but if you install them, Windows paths in `config.php` must use escaped backslashes. For example:

```php
define('ANTIWORD_PATH', 'C:\\antiword\\antiword.exe');
define('PDFTOTEXT_PATH', 'C:\\path\\to\\pdftotext.exe');
define('HTML2TEXT_PATH', 'C:\\path\\to\\html2text.exe');
define('UNRTF_PATH', 'C:\\path\\to\\unrtf.exe');
```

## Run the installer

Open your browser at:

```text
http://localhost/opencats/
```

If an `INSTALL_BLOCK` file exists before first install, remove it so the installer can start. After installation, confirm `INSTALL_BLOCK` exists again so the installer is not left exposed.

Continue to [Run the Installer](run-the-installer.md).
