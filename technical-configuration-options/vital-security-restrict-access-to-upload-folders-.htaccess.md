# Vital Security: Restrict access to upload folders (.htaccess)

OpenCATS validates attachment extensions server-side, but the web server must also prevent uploaded files from executing and prevent direct access to private attachments.

## Attachments must be served through OpenCATS

The 0.12.0 `attachments/.htaccess` denies all direct HTTP access. Download attachments through OpenCATS so authentication and authorization checks run. Do not replace this restriction with `Require all granted` for document extensions: that would expose private files outside the application's permission checks.

For Apache 2.4, the equivalent virtual-host configuration is:

```apache
<Directory /var/www/html/opencats/attachments>
    AllowOverride None
    Options -ExecCGI -Indexes
    Require all denied
</Directory>
```

Adjust the path to the real installation directory. This blocks HTTP access; the PHP process must still have filesystem access to read and write attachments. If relying on the shipped `.htaccess` instead, Apache must allow its directives to take effect.

## Upload staging directory

Keep the release's `upload/.htaccess` execution restrictions when using Apache. Check that your virtual host's PHP handler cannot execute files in `upload/`, including scripts with unexpected or multiple extensions. An extension allow-list alone does not establish safe PHP-FPM or CGI handling.

If you maintain equivalent restrictions in the server configuration, check them against the actual PHP handler and import workflow. Do not enable directory listings or grant general access to staged private documents.

## Nginx and other servers

Nginx does not read `.htaccess`. Add equivalent rules to its configuration. For an installation at `/opencats/`, this location denies direct attachment requests and takes precedence over generic PHP regex locations:

```nginx
location ^~ /opencats/attachments/ {
    deny all;
}
```

Adjust the URL prefix, including removing `/opencats` for a root installation. Separately prevent script execution in the upload staging directory and check all PHP routing rules. The repository's development Docker configuration should not be assumed to provide a hardened production deployment.

## Allowed attachment extensions

The 0.12.0 attachment upload validator allows:

```text
bmp, csv, doc, docx, heic, jpeg, jpg, msg, odg, odt, pages, pdf, png, ppt, pptx, rtf, tiff, wpd, wps, xls, xlsx, xps
```

This describes attachment validation, not permission to make those files publicly accessible.

## Filesystem permissions

Give the PHP/web server account only the filesystem access it needs. For example, from the installation directory on an Ubuntu system using `www-data`:

```bash
sudo chown -R www-data:www-data upload attachments
sudo chmod 770 upload attachments
```

Avoid world-writable permissions. Filesystem access and HTTP access are separate controls.

## Verify the deployment

* Upload an allowed document and download it through OpenCATS as an authorized user.
* Confirm an unauthenticated request to its direct attachment URL is denied.
* Confirm a user without the relevant record permission cannot download it through OpenCATS.
* Confirm invalid script uploads are rejected and files in upload directories cannot execute.
* Check web server logs for unexpected denials, then retest bulk imports if used.

Reference the versioned [attachment restrictions](https://github.com/opencats/OpenCATS/blob/v0.12.0/attachments/.htaccess), [upload restrictions](https://github.com/opencats/OpenCATS/blob/v0.12.0/upload/.htaccess), and [attachment validator](https://github.com/opencats/OpenCATS/blob/v0.12.0/lib/Attachments.php) when comparing your configuration.
