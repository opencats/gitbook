# Vital Security: Restrict access to upload folders (.htaccess)

To restrict accessing / executing php or other scripts from uploads or other restricted folders, use this code in .htaccess file put in that upload folder. If possible please have ownership of this .htaccess file as root and not your web server to prevent overwriting.

This is syntax for an Apache webserver.

### 1. Ownership

Ensure your upload directories are owned by whatever process runs your web server (usually www-data or apache) - and ensure permissions are set to 766 - 755 if this causes problems, and never 777!\
(I would prefer 666 but this seems to break the app)

`chown apache:apache -R uploads/`\
`chmod 766 -R uploads/`

I would set htaccess to be r/w by owner (root) and read by group/world

`chmod 644 .htaccess`

### 2 .htaccess

SO if possible I wouldn't use .htaccess, and use the main config file instead (Allow Overrides none) but if you do, then follow this guidance. Generally, htaccess is owned by the web process (www-data or apache) however in this instance as opencats will not rewrite it, it's more secure if .htaccess is owned and writable by root only.

This would need a htaccess of this type installed in

* opencats/upload, and
* opencats/attachments

#### for Apache 2.2

`# Don't list directory contents`\
`IndexIgnore *`\
`# Disable script execution`\
`AddHandler cgi-script .php .php2 .php3 .php4 .php5 .php6 .php7 .php8 .php9 .pl .py .js .jsp .asp .htm .html .shtml .sh .cgi`

`Options -ExecCGI -Indexes`\
\
`# Only the following file extensions are allowed (pdf, rtf, odt, doc, docx, txt, wpd)`\
`Order Allow,Deny`\
`Deny from all`\
\
`<FilesMatch "\.([Pp][Dd][Ff]|[Dd][Oo][Cc][Xx]?|[Rr][Tt][Ff]|[Oo][Dd][Tt]|[Tt][Xx][Tt]|[Ww][Pp][Dd])$">`

`Allow from all`\
`</FilesMatch>`

#### On Apache 2.4 the syntax changes - therefore

`IndexIgnore *`\
`# Disable script execution`\
`AddHandler cgi-script .php .php2 .php3 .php4 .php5 .php6 .php7 .php8 .php9 .pl .py .js .jsp .asp .htm .html .$`

`Options -ExecCGI -Indexes`

`#grant access if word-processing format`\
`<FilesMatch "(?i)\.(pdf|docx?|rtf|odt?g?|txt|wpd|jpe?g|png|csv|xlsx?|ppt|msg|heic|tiff?|html?|bmp|wps|xps)$">` `Require all granted`\
`</FilesMatch>`

**Finally of course - test, test, test.. once you add your htaccess file, please try to upload valid and invalid files.**

### 3. /etc/apache/apache.conf instead

So the most secure option is to configure your restrictions in apache.conf and to ignore .htaccess files (so if for some reason someone managed to upload a .htaccess file then it'll be ignored)

For Opencats, sample restrictions to add to apache.conf config would be as below;

`<Directory /var/www/>`\
`Options -Indexes +FollowSymLinks`\
`AllowOverride None`\
`Require all granted`\
`</Directory>`

`<Directory /var/www/html/OpenCATS-0.9.6/upload>`\
`IndexIgnore *`\
`# Disable script execution`\
`AddHandler cgi-script .php .php2 .php3 .php4 .php5 .php6 .php7 .php8 .php9 .pl .py .js .jsp .asp .htm .html .$`\
`Options -ExecCGI -Indexes`\
`# Only permit particular filetypes`\
`<FilesMatch "(?i)\.(pdf|docx?|rtf|odt?g?|txt|wpd|jpe?g|png|csv|xlsx?|ppt|msg|heic|tiff?|html?|bmp|wps|xps)$">`\
`Require all granted`\
`</FilesMatch>`\
`</Directory>`

`<Directory /var/www/html/OpenCATS-0.9.6/attachments>`\
`IndexIgnore *`\
`# Disable script execution`\
`AddHandler cgi-script .php .php2 .php3 .php4 .php5 .php6 .php7 .php8 .php9 .pl .py .js .jsp .asp .htm .html .$`\
`Options -ExecCGI -Indexes`\
`<FilesMatch "(?i)\.(pdf|docx?|rtf|odt?g?|txt|wpd|jpe?g|png|csv|xlsx?|ppt|msg|heic|tiff?|html?|bmp|wps|xps)$">`\
`Require all granted`\
`</FilesMatch>`\
`</Directory>`

### Reference material

https://blog.devolutions.net/2019/12/how-to-prevent-file-upload-vulnerabilities

https://stackoverflow.com/questions/5689423/how-to-ban-all-executable-files-on-apache

https://stackoverflow.com/questions/6368777/how-to-prevent-uploaded-file-from-being-executed

https://www.sitepoint.com/community/t/securing-image-upload-directory-via-htaccess/44659

https://tomolivercv.wordpress.com/2011/07/24/protect-your-uploads-folder-with-htaccess/

https://github.com/Leuchtfeuer/typo3-secure-downloads/blob/master/Resources/Private/Examples/\_.htaccess
