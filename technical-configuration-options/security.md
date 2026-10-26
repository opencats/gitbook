# Security

This page summarizes security practices for current OpenCATS installations. It complements the application repository `Security.MD` file and the upload-folder hardening guide.

## Reporting security issues

Use [GitHub Private Vulnerability Reporting](https://github.com/opencats/OpenCATS/security/advisories/new). In the application repository, open **Security and quality → Report a vulnerability**. Include affected versions, reproduction steps, impact and any proposed fix.

Do not send vulnerability reports by email or through public issues, discussions or pull requests. Follow the current [OpenCATS security policy](https://github.com/opencats/OpenCATS/blob/master/Security.MD) for private coordination.

## Application and PHP updates

Install OpenCATS 0.12.0 and keep PHP and server packages patched separately. The 0.12.0 test images use PHP 8.4.27 and 8.5.12. Installing an OpenCATS archive does not update the host PHP runtime.

## Career Portal authentication

Career Portal and candidate registration are disabled by default. If enabled, returning-candidate verification requires CAPTCHA and a currently public, visible vacancy. Candidate identity is stored server-side in the PHP session. Persistent “Remember my information” authentication cookies are no longer used. Remove obsolete remember-me wording from custom registration templates.

For a public deployment, consider throttling candidate authentication at the web server or reverse proxy. OpenCATS 0.12.0 does not implement application-level rate limiting. Rules must cover both the main and `careers/index.php` entry points and POST routing, not just a query-string match. See the [deployment guidance](https://github.com/opencats/OpenCATS/blob/v0.12.0/docs/career-portal-authentication.md) for details and limitations.

## Authorization changes

CSV exports through the export module require Site Administrator access. Permission checks also protect tag administration, job-order attachment ownership, hidden pipeline candidates, email/phone lookups, and activity/pipeline parent records. Preserve these checks in custom code; changing a link or hiding a button does not grant access.

## Password storage

OpenCATS stores user passwords with PHP `password_hash()` and verifies them with `password_verify()` using the running PHP version's `PASSWORD_DEFAULT` algorithm. Legacy password hashes may be migrated when users successfully log in.

Do not reset passwords by writing MD5 hashes into the database. If a database-level reset is unavoidable, generate a modern hash with PHP `password_hash()`.

## CSRF protection

Current OpenCATS validates CSRF tokens for normal POST requests and logged-in AJAX POST requests. If you customize templates, forms, JavaScript, modules, or AJAX endpoints, preserve CSRF token output and submission.

## Upload security

OpenCATS applies server-side upload extension validation. Current allowed upload extensions include:

```text
bmp, csv, doc, docx, heic, jpeg, jpg, msg, odg, odt, pages, pdf, png, ppt, pptx, rtf, tiff, wpd, wps, xls, xlsx, xps
```

This validation is not a replacement for web server hardening. Configure Apache or Nginx so uploaded files cannot execute as scripts, and review [Vital Security: Restrict access to upload folders (.htaccess)](vital-security-restrict-access-to-upload-folders-.htaccess.md).

## Installer protection

OpenCATS runs the installer when `INSTALL_BLOCK` is missing. Remove `INSTALL_BLOCK` only when intentionally installing or upgrading, and confirm it exists again afterward.

## Composer and dependencies

If installing from source, run production installs with:

```bash
composer install --no-dev
```

Review Composer audit output and update dependencies when security fixes are available. Development dependencies are not needed on production systems.

## Deployment checklist

Before exposing OpenCATS to users:

* Use HTTPS.
* Use strong unique MariaDB credentials.
* Do not expose phpMyAdmin publicly.
* Ensure `attachments/`, `upload/`, and `temp/` are writable only as needed.
* Prevent script execution from upload and attachment directories.
* Keep `INSTALL_BLOCK` in place after installation.
* Keep `config.php` out of public repositories and backups with weak access controls.
* Back up MariaDB and `attachments/` regularly and test restores.
