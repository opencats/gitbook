---
description: cronjob to send reminder emails for events / appointments
---

# Emails & reminders



### Scheduled E-Mail reminders

\
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

### Email configuration problems

If you are seeing 'SSL Socket errors' then it's really a problem with the underlying PHPMailer application that handles SMTP Emails..&#x20;

`Warning: stream_socket_enable_crypto(): SSL operation failed with code 1. OpenSSL Error messages: error:14090086:SSL routines:ssl3_get_server_certificate:certificate verify failed in C:\wamp64\www\CATS\lib\phpmailer\class.smtp.php on line 369` &#x20;

Please check [https://github.com/PHPMailer/PHPMailer/wiki/Troubleshooting#certificate-verification-failure](https://github.com/PHPMailer/PHPMailer/wiki/Troubleshooting#certificate-verification-failure)



You can skip verification of the ssl certificate by using the below:

```
$mail->SMTPOptions = array(
    'ssl' => array(
        'verify_peer' => false,
        'verify_peer_name' => false,
        'allow_self_signed' => true
    )
);
```
