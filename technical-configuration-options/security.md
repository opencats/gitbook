# Security

This page summarizes security practices for current OpenCATS installations. It complements the application repository `Security.MD` file and the upload-folder hardening guide.

## Reporting security issues

Report security issues privately to the OpenCATS maintainers so the community has time to respond and upgrade. The application repository currently directs reports to `russh@opencats.org`.

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
