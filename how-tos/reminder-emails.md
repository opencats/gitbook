---
description: cronjob to send reminder emails for events / appointments
---

# Reminder emails



1.  Scheduled E-Mail reminders\
    OpenCATS can send out E-Mail reminders for calendar events before they happen.\
    To enable this feature, configure cron or another scheduling daemon to\
    invoke QueueCLI.php every minute. An example crontab line would look like:

    ```
    * * * * * /usr/local/bin/php /var/www/html/opencats/QueueCLI.php
    ```

    Or,

    ```
    * * * * * curl http://mysite.com/QueueCLI.php > /dev/null
    ```
