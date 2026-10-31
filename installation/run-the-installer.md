# Run the Installer

After the web server, PHP, MariaDB database, OpenCATS files, and directory permissions are ready, open OpenCATS in your browser.

For a local installation, use:

```text
http://localhost/opencats/
```

For a server or VPS, use the hostname and path you configured.

## If the installer does not start

OpenCATS starts the installer when `INSTALL_BLOCK` is missing. If you are doing a first-time installation and the installer does not appear, check whether `INSTALL_BLOCK` exists in the OpenCATS directory and remove it only for the installation step.

After installation or upgrade, confirm `INSTALL_BLOCK` exists again so the installer is not exposed.

## Database connectivity

Enter the MariaDB database name, user, password, and host you created during installation.

Common host values:

* `localhost` for a normal local Linux or Windows stack.
* `opencatsdb` for the repository Docker Compose development environment.
* A hosting-provider database hostname for shared hosting or managed database services.

Use the installer database connectivity test before continuing. If the test fails, confirm:

* the database exists;
* the database user has privileges on that database;
* the password is correct;
* the database host is reachable from the web server;
* PHP has the MariaDB/MySQL extension installed.

## Unsupported MySQL hosting and `ONLY_FULL_GROUP_BY`

OpenCATS is developed and tested with **MariaDB**, not Oracle MySQL. Shared hosting providers (including some Bluehost plans) may supply MySQL rather than MariaDB. A successful database connection does **not** mean that all OpenCATS queries will run correctly on MySQL.

A known compatibility problem is MySQL's `ONLY_FULL_GROUP_BY` SQL mode. Existing OpenCATS queries can fail with an error similar to `Expression ... is not in GROUP BY clause`. This has been reported previously in [issue #329](https://github.com/opencats/OpenCATS/issues/329) and [issue #413](https://github.com/opencats/OpenCATS/issues/413); these are historical reports, not promises of MySQL support.

**Recommended solution:** Use a hosting service offering a supported MariaDB database. If you must use MySQL and understand the risks, the following *site-local, untested compatibility workaround* removes **only** `ONLY_FULL_GROUP_BY` for OpenCATS' database connection. Make a backup first. In `lib/DatabaseConnection.php`, inside `DatabaseConnection::connect()`, after the successful `mysqli_select_db()` check and immediately before `return true;`, add:

```php
if (!mysqli_query(
    $this->_connection,
    "SET SESSION sql_mode = sys.list_drop(@@SESSION.sql_mode, 'ONLY_FULL_GROUP_BY')"
)) {
    throw new RuntimeException(
        'Unable to change MySQL session SQL mode: ' . mysqli_error($this->_connection)
    );
}
```

This uses MySQL's `sys.list_drop()` function. Some shared-hosting accounts may not have access to the `sys` schema; if this fails, contact your hosting provider rather than disabling all SQL modes. The change applies **only to the current connection**, not the server's global configuration, and must be reapplied to local OpenCATS files after application upgrades. It is **not** an OpenCATS project patch.

**Do not use** `SET SESSION sql_mode = ''` as a general workaround: it turns off additional SQL validation protections, including strict data handling. Even the narrower workaround relaxes `GROUP BY` validation and can allow ambiguous grouped results; it does not correct the underlying SQL. Test candidate lists, reports, data edits and upgrades carefully. Other MySQL incompatibilities may remain, and MySQL installations are outside the project's support and CI matrix.

## Resume indexing and document parsing

OpenCATS can use external tools to extract text from uploaded resumes and documents. The installer may ask for paths to these tools. Typical Linux paths are:

```text
/usr/bin/antiword
/usr/bin/pdftotext
/usr/bin/html2text
/usr/bin/unrtf
```

OpenCATS can be installed without these tools. Resume indexing and text extraction will be limited until you install the utilities and configure the paths in `config.php`.

## Mail settings

OpenCATS can send mail through PHP mail, Sendmail, or SMTP. If you do not want OpenCATS to send email immediately, disable mail during installation and configure it later.

For SMTP, collect these values before enabling mail:

* SMTP host
* SMTP port
* authentication username
* authentication password or app password
* security mode such as `tls` or `ssl`

OpenCATS uses PHPMailer for email delivery.

## Installation type

The installer may offer options such as:

* new installation;
* demo/test data installation;
* restore from backup;
* use an existing OpenCATS database and perform required upgrades.

For production upgrades, use the existing installation/upgrade path only after taking CLI backups and testing the upgrade on a copy of production data.

## First login

For a new installation, the default login is:

```text
admin / cats
```

OpenCATS 0.12.0 requires the default administrator password to be changed after login. Complete that step before using the application.

## Post-install checklist

After clicking `Start OpenCATS` and logging in:

* Change the default administrator password.
* Confirm `INSTALL_BLOCK` exists.
* Confirm `attachments/`, `upload/`, and `temp/` permissions are no broader than necessary.
* Configure upload-directory execution restrictions before exposing the career portal or accepting uploads.
* Configure mail and scheduled reminders if needed.
* Take an initial CLI database and attachments backup.

If you are exposing OpenCATS to the web, review [Security](../technical-configuration-options/security.md) and [Vital Security: Restrict access to upload folders (.htaccess)](../technical-configuration-options/vital-security-restrict-access-to-upload-folders-.htaccess.md).
