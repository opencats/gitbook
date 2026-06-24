# Configuration Reference

Most OpenCATS runtime configuration is stored in `config.php`. This page summarizes commonly changed settings. Always back up `config.php` before editing it.

## Database settings

| Setting | Purpose |
| --- | --- |
| `DATABASE_USER` | MariaDB user OpenCATS uses to connect. |
| `DATABASE_PASS` | Password for `DATABASE_USER`. |
| `DATABASE_HOST` | Database host name. Use `localhost` for local installs or the Docker service name in Docker. |
| `DATABASE_NAME` | OpenCATS database name. |

## Authentication settings

| Setting | Purpose |
| --- | --- |
| `AUTH_MODE` | Authentication mode. Supported values are `sql`, `ldap`, and `sql+ldap`. |

Use `sql` for normal OpenCATS-managed users. Use LDAP modes only after configuring the PHP LDAP extension and the LDAP connection settings required by your environment.

## Resume/document parser settings

| Setting | Purpose |
| --- | --- |
| `PARSING_ENABLED` | Enables external resume parsing service integration when configured. |
| `ANTIWORD_PATH` | Path to `antiword` for Word document text extraction. |
| `ANTIWORD_MAP` | Antiword character map. |
| `PDFTOTEXT_PATH` | Path to `pdftotext`. |
| `HTML2TEXT_PATH` | Path to `html2text`. |
| `UNRTF_PATH` | Path to `unrtf`. |

Parser utilities are optional for installation but useful for resume text extraction and search.

## Runtime paths

| Setting | Purpose |
| --- | --- |
| `CATS_TEMP_DIR` | Temporary directory writable by the web server. |
| `MODULES_PATH` | Path to OpenCATS modules. Usually does not need editing. |

## Sphinx search settings

| Setting | Purpose |
| --- | --- |
| `ENABLE_SPHINX` | Enables Sphinx search integration. |
| `SPHINX_HOST` | Sphinx host. |
| `SPHINX_PORT` | Sphinx port. |
| `SPHINX_INDEX` | Sphinx index names. |

Leave Sphinx disabled unless you have installed and configured Sphinx.

## Session and display settings

| Setting | Purpose |
| --- | --- |
| `CATS_SESSION_NAME` | Session cookie name. Change only if hosting multiple OpenCATS instances on the same domain. |
| `ENABLE_SINGLE_SESSION` | Enforces one active session per user when enabled. |
| `OFFSET_GMT` | Legacy GMT offset setting. |
| `HTML_ENCODING` | HTML output encoding. |
| `AJAX_ENCODING` | AJAX output encoding. |
| `SQL_CHARACTER_SET` | SQL character set. |

## Email settings

| Setting | Purpose |
| --- | --- |
| `MAIL_MAILER` | Mail method: `0` disabled, `1` PHP mail, `2` Sendmail, `3` SMTP. |
| `MAIL_SENDMAIL_PATH` | Sendmail binary path when using Sendmail. |
| `MAIL_SMTP_HOST` | SMTP server host. |
| `MAIL_SMTP_PORT` | SMTP server port. |
| `MAIL_SMTP_AUTH` | Whether SMTP authentication is required. |
| `MAIL_SMTP_USER` | SMTP username. |
| `MAIL_SMTP_PASS` | SMTP password. |
| `MAIL_SMTP_SECURE` | SMTP security mode: empty string, `ssl`, or `tls`. |

Calendar reminder email text is also configurable in `config.php`.

## Career portal and SSL settings

| Setting | Purpose |
| --- | --- |
| `SSL_ENABLED` | Enables SSL-aware behavior when your site is served over HTTPS. |
| `CAREERS_CANDIDATEAPPLY_SUBJECT` | Candidate email subject after career portal application. |
| `CAREERS_OWNERAPPLY_SUBJECT` | Owner email subject after career portal application. |
| `CANDIDATE_STATUSCHANGE_SUBJECT` | Candidate email subject when status changes. |

## Demo and testing settings

| Setting | Purpose |
| --- | --- |
| `ENABLE_DEMO_MODE` | Enables demo mode behavior. Do not enable on normal production systems. |
| `TESTER_*` constants | Automated testing defaults. Do not rely on these for production users. |
| `DEMO_*` constants | Demo login defaults. Do not expose demo credentials on production systems. |
