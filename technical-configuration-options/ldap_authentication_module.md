# LDAP Authentication

OpenCATS supports SQL authentication, LDAP authentication, and mixed SQL plus LDAP authentication through `AUTH_MODE` in `config.php`.

## Authentication modes

```php
define('AUTH_MODE', 'sql');
```

Supported values are:

* `sql` - OpenCATS authenticates users against the OpenCATS database.
* `ldap` - OpenCATS authenticates users through LDAP.
* `sql+ldap` - OpenCATS can use both SQL and LDAP authentication paths.

## Requirements

LDAP authentication requires the PHP LDAP extension. The repository Docker image installs the LDAP extension in its PHP container.

## Example settings

Your exact LDAP settings depend on your directory server. A typical configuration uses values like these in `config.php`:

```php
define('AUTH_MODE', 'ldap');
define('LDAP_HOST', 'ldap.example.org');
define('LDAP_PORT', '389');
define('LDAP_BASEDN', 'ou=users,dc=example,dc=org');
define('LDAP_UID', 'uid');
define('LDAP_CONNECT_DN', 'cn=readonly,dc=example,dc=org');
define('LDAP_PASSWORD', 'readonly-password');
```

For Active Directory, `LDAP_UID` is often `sAMAccountName`, but this depends on your directory design.

## Troubleshooting

If LDAP login fails:

* Confirm the PHP LDAP extension is installed and enabled.
* Confirm the OpenCATS server can reach the LDAP host and port.
* Confirm bind DN and password are valid.
* Confirm `LDAP_BASEDN` points to the subtree containing users.
* Confirm `LDAP_UID` matches the attribute users enter at login.
* Check whether your directory requires LDAPS, StartTLS, or firewall changes.
* Test with `sql` mode first to separate database/application issues from LDAP issues.

Do not publish LDAP bind credentials in public repositories or screenshots.
