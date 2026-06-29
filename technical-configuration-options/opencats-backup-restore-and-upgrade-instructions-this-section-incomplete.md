# Backup, Restore, and Upgrade

This page describes the recommended backup, restore, and upgrade approach for OpenCATS.

## Recommendation

Use **CLI database backups** and filesystem backups as your primary backup method. OpenCATS includes GUI backup features, but command-line database and attachment backups are easier to automate, inspect, test, and restore consistently.

## What to back up

A complete OpenCATS backup must include:

* The MariaDB database.
* The `attachments/` directory.
* The `upload/` directory if your workflow uses pending bulk imports.
* `config.php` and any local web server configuration.
* Any custom templates, patches, or integrations you maintain outside the database.

## CLI database backup

Replace database name, user, and output path with your own values:

```bash
mysqldump --single-transaction --routines --triggers -u opencats -p opencats > opencats-$(date +%F).sql
```

Store backups somewhere other than the OpenCATS web directory.

## Attachments backup

From the OpenCATS installation directory:

```bash
tar -czf opencats-attachments-$(date +%F).tar.gz attachments/
```

If your installation uses `upload/` for pending import files, back it up too:

```bash
tar -czf opencats-upload-$(date +%F).tar.gz upload/
```

## Test your backups

A backup is not complete until you have restored it in a separate test environment. Test restores should verify:

* users can log in;
* candidates, companies, contacts, and job orders load;
* attachments download correctly;
* search and parsing features work if enabled;
* email settings are not accidentally sending production email from a test system.

## Restore from CLI backup

1. Install OpenCATS files for the target version.
2. Create a fresh MariaDB database and user.
3. Import the SQL backup:

   ```bash
   mysql -u opencats -p opencats < opencats-backup.sql
   ```

4. Restore `attachments/`:

   ```bash
   tar -xzf opencats-attachments-backup.tar.gz -C /path/to/opencats/
   ```

5. Restore or recreate `config.php` with the correct database settings.
6. Ensure writable directory ownership and permissions are correct.
7. Run the installer/upgrade path only when intentionally upgrading an existing database.
8. Confirm `INSTALL_BLOCK` exists after installation or upgrade.

## Upgrade checklist

Before upgrading production:

* Read release notes for the target OpenCATS release.
* Confirm your server meets the current PHP and MariaDB baseline.
* Back up the database with `mysqldump`.
* Back up `attachments/`, `upload/` if used, and `config.php`.
* Test the upgrade on a copy of production data.
* Schedule a maintenance window.
* Have a rollback plan using the backups you just tested.

## Upgrading OpenCATS files

A typical upgrade flow is:

1. Put the site into maintenance or stop web traffic.
2. Back up database and files.
3. Deploy the new OpenCATS release files.
4. Restore or merge the existing `config.php` settings carefully.
5. Ensure Composer dependencies are installed if using source:

   ```bash
   composer install --no-dev
   ```

6. Remove `INSTALL_BLOCK` only when you are ready to run the installer/upgrade workflow.
7. Visit the site in a browser and choose the existing installation/upgrade path if presented.
8. After upgrade, confirm `INSTALL_BLOCK` exists.
9. Test login, records, attachments, reports, search, email, and scheduled reminders.

## GUI backup and restore

OpenCATS has GUI backup screens under Settings and Administration. These may be useful for small installations or quick exports, but CLI backups are the recommended operational backup method.

If you use GUI restore, the backup file must be named `catsbackup.bak` and placed in a writable `restore/` directory under the OpenCATS installation before running the installer restore path.

## Security notes

* Do not keep backup files inside the public web root longer than necessary.
* Protect SQL backups because they contain personal data and password hashes.
* Protect attachment backups because they may contain resumes and other sensitive documents.
* Do not expose restored test systems publicly with production data.
