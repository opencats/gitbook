# How\_to\_script\_database\_backups

\###How to script database backups

### ####Backup all databases nightly w/ mysqldump

(from linux.org) So, I want to take a shell script and be able to put it on any machine - and have it backup the databases on that machine using mysqldump.. and put them each separately into a backup directory.. here's what I came up with.

Can you make it better?

`#!/bin/bash` ``DB_BACKUP="/backups/mysql_backup/`date +%Y-%m-%d`"`` `DB_USER="root"` `DB_PASSWD="secretttt"` `` HN=`hostname | awk -F. '{print $1}'` `` `# Create the backup directory` `mkdir -p $DB_BACKUP` `# Remove backups older than 10 days` `find /backups/mysql_backup/ -maxdepth 1 -type d -mtime +10 -exec rm -rf {} ;` `# Option 1: Backup each database on the system using a root username and password` `for db in $(mysql --user=$DB_USER --password=$DB_PASSWD -e 'show databases' -s --skip-column-names|grep -vi information_schema);` `do mysqldump --user=$DB_USER --password=$DB_PASSWD --opt $db | gzip > "$DB_BACKUP/mysqldump-$HN-$db-$(date +%Y-%m-%d).gz";` `done` `# Option 2: If you aren't using a root password then comment out option 1 and use this` `# for db in $(mysql -e 'show databases' -s --skip-column-names|grep -vi information_schema);` `# do mysqldump --opt $db | gzip > "$DB_BACKUP/mysqldump-$HN-$db-$(date +%Y-%m-%d).gz";` `# done` `# Make it so only root can read the backup files` `chmod -R 600 $DB_BACKUP`

If you use this, throw this text into something like /usr/local/bin/mysql\_backup.sh and since it has mysql's root password in it, make sure that you chmod 700 to it so no one else can read it. Then just call it from cron like:

`30 3 * * * /usr/local/bin/mysql_backup.sh`

BTW, a simpler way to grab all of them is to use the --all-databases flag in the mysqldump command.. but it doesn't make nice separate files for you..
