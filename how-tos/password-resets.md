# Password resets

OpenCATS stores passwords with PHP `password_hash()` and verifies them with `password_verify()`. Older installations may still contain legacy hashes that OpenCATS migrates after successful login.

## Preferred reset method

When possible, have an administrator reset the user's password from inside OpenCATS. This avoids direct database edits and ensures the new password is stored with the current hashing method.

## Emergency database reset

If you are locked out and must reset a password directly in MariaDB, do **not** use MD5. Generate a modern PHP password hash first.

On a server with PHP 7.4 available:

```bash
php -r "echo password_hash('NewStrongPasswordHere', PASSWORD_DEFAULT), PHP_EOL;"
```

Copy the generated hash. Then update the relevant user record in the OpenCATS database using phpMyAdmin or the MariaDB CLI. The exact user table and user ID depend on your installation, so make a database backup before changing anything.

Example CLI workflow:

```bash
mysqldump -u opencats -p opencats > before-password-reset.sql
mysql -u opencats -p opencats
```

Then inspect the users table, identify the correct account, and update only that account's password field with the generated hash.

## Security notes

* Do not store or share the temporary password in tickets, chat, or email.
* Require the user to change the password after login.
* Keep the pre-reset database backup only as long as required and protect it because it contains personal data and password hashes.
* If LDAP authentication is enabled, reset the password in the LDAP directory instead of the OpenCATS database.
