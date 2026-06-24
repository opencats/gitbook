# Vital Security: Restrict access to upload folders (.htaccess)

OpenCATS performs server-side upload extension validation, but you should still configure your web server so uploaded files cannot execute as scripts. Treat web server restrictions as defense in depth.

## Upload directories

Review restrictions for these directories:

* `opencats/upload`
* `opencats/attachments`

The web server user must be able to write files OpenCATS needs, but uploaded content should not be executable. Avoid permissions such as `777` unless your hosting environment leaves no safer option.

Example ownership and permissions on many Linux systems:

```bash
sudo chown -R www-data:www-data upload attachments
sudo chmod 770 upload attachments
```

## Current allowed upload extensions

Current OpenCATS upload validation allows these extensions:

```text
bmp, csv, doc, docx, heic, jpeg, jpg, msg, odg, odt, pages, pdf, png, ppt, pptx, rtf, tiff, wpd, wps, xls, xlsx, xps
```

Your web server allow-list should be no broader than the file types you actually need.

## Apache 2.4 `.htaccess` example

Place an `.htaccess` file in each upload directory if your Apache configuration allows overrides:

```apache
IndexIgnore *
Options -ExecCGI -Indexes
AddHandler cgi-script .php .php2 .php3 .php4 .php5 .php6 .php7 .php8 .php9 .pl .py .js .jsp .asp .sh .cgi

<FilesMatch "(?i)\.(bmp|csv|docx?|heic|jpe?g|msg|odg|odt|pages|pdf|png|pptx?|rtf|tiff?|wpd|wps|xlsx?|xps)$">
    Require all granted
</FilesMatch>
```

For stronger control, put equivalent rules in the main Apache virtual host or server configuration and disable `.htaccess` overrides.

## Apache server configuration example

```apache
<Directory /var/www/html/opencats/upload>
    IndexIgnore *
    Options -ExecCGI -Indexes
    AddHandler cgi-script .php .php2 .php3 .php4 .php5 .php6 .php7 .php8 .php9 .pl .py .js .jsp .asp .sh .cgi
    <FilesMatch "(?i)\.(bmp|csv|docx?|heic|jpe?g|msg|odg|odt|pages|pdf|png|pptx?|rtf|tiff?|wpd|wps|xlsx?|xps)$">
        Require all granted
    </FilesMatch>
</Directory>

<Directory /var/www/html/opencats/attachments>
    IndexIgnore *
    Options -ExecCGI -Indexes
    AddHandler cgi-script .php .php2 .php3 .php4 .php5 .php6 .php7 .php8 .php9 .pl .py .js .jsp .asp .sh .cgi
    <FilesMatch "(?i)\.(bmp|csv|docx?|heic|jpe?g|msg|odg|odt|pages|pdf|png|pptx?|rtf|tiff?|wpd|wps|xlsx?|xps)$">
        Require all granted
    </FilesMatch>
</Directory>
```

Adjust paths for your installation.

## Test after changes

After changing upload restrictions:

* Upload a valid document and confirm OpenCATS can attach and download it.
* Try uploading an invalid script file and confirm OpenCATS rejects it.
* Try browsing directly to uploaded content and confirm scripts cannot execute.
* Review web server logs for unexpected denials or executable handling.

## Reference material

* https://blog.devolutions.net/2019/12/how-to-prevent-file-upload-vulnerabilities
* https://stackoverflow.com/questions/5689423/how-to-ban-all-executable-files-on-apache
* https://stackoverflow.com/questions/6368777/how-to-prevent-uploaded-file-from-being-executed
* https://www.sitepoint.com/community/t/securing-image-upload-directory-via-htaccess/44659
