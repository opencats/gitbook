---
description: cronjob to send reminder emails for events / appointments
---

# Emails & reminders

OpenCATS can send email through PHP mail, Sendmail, or SMTP. Calendar reminders require both email configuration and a scheduled job that invokes `QueueCLI.php`.

## Configure email

Email settings live in `config.php`.

`MAIL_MAILER` selects the delivery method:

* `0` - disabled
* `1` - PHP built-in mail support
* `2` - Sendmail
* `3` - SMTP

Common SMTP settings are:

```php
define('MAIL_MAILER', 3);
define('MAIL_SMTP_HOST', 'smtp.example.org');
define('MAIL_SMTP_PORT', 587);
define('MAIL_SMTP_AUTH', true);
define('MAIL_SMTP_USER', 'user@example.org');
define('MAIL_SMTP_PASS', 'strong-password');
define('MAIL_SMTP_SECURE', 'tls');
```

For Sendmail, configure:

```php
define('MAIL_MAILER', 2);
define('MAIL_SENDMAIL_PATH', '/usr/sbin/sendmail');
```

Do not commit real SMTP passwords to public repositories.

## Scheduled email reminders

OpenCATS sends calendar event reminders when `QueueCLI.php` is run by cron or another scheduler. A typical Linux cron entry runs every minute:

```cron
* * * * * /usr/bin/php /var/www/html/opencats/QueueCLI.php >/dev/null 2>&1
```

Use the correct PHP binary and OpenCATS path for your server.

A URL-based scheduler can also call the script, but local CLI execution is preferred when available:

```cron
* * * * * curl -fsS http://example.org/QueueCLI.php >/dev/null 2>&1
```

## Reminder template

The event reminder email template is configured in `config.php` as `$GLOBALS['eventReminderEmail']`. Back up `config.php` before changing the template.

## SMTP certificate problems

If you see certificate verification errors from PHPMailer, fix the server trust store or SMTP configuration where possible. Disabling certificate verification weakens security and should only be used as a temporary troubleshooting step.

PHPMailer troubleshooting documentation is available at:

[https://github.com/PHPMailer/PHPMailer/wiki/Troubleshooting#certificate-verification-failure](https://github.com/PHPMailer/PHPMailer/wiki/Troubleshooting#certificate-verification-failure)
