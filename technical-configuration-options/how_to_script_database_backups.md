# How to script database backups

Use a scheduled CLI database dump alongside attachment and configuration backups. Read [Backup, Restore, and Upgrade](opencats-backup-restore-and-upgrade-instructions-this-section-incomplete.md) for the complete recovery process. A database dump alone does not include resumes or other files.

## Credentials

Create `/etc/opencats-backup.cnf` outside the web root, readable only by the account running the backup (for a root-run job, use owner root and mode `600`):

```ini
[client]
user=opencats_backup
password=replace-with-a-strong-password
host=localhost
```

Use a database account with the privileges needed to dump the OpenCATS database, routines and triggers. Verify the dump with that account before scheduling it. Do not put a database password in cron or command-line arguments.

## Backup script

Save this as `/usr/local/sbin/opencats-db-backup`, owned by the backup account and executable only by that account. Adjust the database name, paths and `mysqldump` binary for your server (`mariadb-dump` may be used instead):

```bash
#!/usr/bin/env bash
set -euo pipefail
umask 077

backup_dir=/var/backups/opencats
mkdir -p "$backup_dir"
backup_file="$backup_dir/opencats-$(date -u +%Y%m%dT%H%M%SZ).sql.gz"
partial_file=$(mktemp "$backup_dir/.opencats-dump.XXXXXX")
trap 'rm -f -- "$partial_file"' EXIT

mysqldump --defaults-extra-file=/etc/opencats-backup.cnf \
    --single-transaction --routines --triggers opencats | gzip > "$partial_file"
mv -- "$partial_file" "$backup_file"
```

The script exits on a dump or compression failure and only gives completed dumps their final name. Monitor failures and confirm successful backups are not empty. `--single-transaction` provides a consistent snapshot for transactional tables; check older installations for non-transactional tables and avoid schema changes during the dump.

## Schedule and verify

For example, in the backup account's crontab:

```cron
30 3 * * * /usr/local/sbin/opencats-db-backup
```

Keep cron error reporting or another monitoring mechanism enabled. Set a retention policy, keep a protected copy off the server, and regularly restore a dump plus its matching attachments into an isolated test environment. The example deliberately does not delete older backups.
