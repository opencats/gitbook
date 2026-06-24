# Install on Ubuntu

These instructions describe a Linux/Apache/MariaDB/PHP installation path for OpenCATS 0.10.0. Commands may need small adjustments for your Ubuntu release and package repositories.

## Install MariaDB and Apache

```bash
sudo apt-get update
sudo apt-get install mariadb-server mariadb-client apache2 unzip wget
sudo mysql_secure_installation
```

MariaDB is the documented database family for OpenCATS. Current CI Docker tests use MariaDB 10.7.

## Install PHP 7.4 and extensions

OpenCATS 0.10.0 requires PHP 7.4. If your Ubuntu release does not provide PHP 7.4 packages directly, use a trusted package source such as the Ondrej Surý PHP PPA.

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install php7.4 php7.4-cli php7.4-fpm php7.4-mysql php7.4-gd php7.4-soap php7.4-ldap php7.4-xml php7.4-curl php7.4-mbstring php7.4-zip
sudo systemctl restart apache2
```

Depending on your Apache configuration, you may use PHP-FPM or an Apache PHP module. Confirm that the web server is actually using PHP 7.4 before running the installer.

## Optional document parsing utilities

Install these if you want OpenCATS to extract text from common resume and document formats:

```bash
sudo apt-get install antiword poppler-utils html2text unrtf
```

After installation, verify or update the parser paths in `config.php` if your distribution installs binaries in non-standard locations.

## Create the MariaDB database and user

Log in to MariaDB as root:

```bash
sudo mysql -u root -p
```

Then create a database and user. Replace `databasepassword` with a strong unique password.

```sql
CREATE DATABASE opencats CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER 'opencats'@'localhost' IDENTIFIED BY 'databasepassword';
GRANT ALL PRIVILEGES ON opencats.* TO 'opencats'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

These database credentials are separate from your OpenCATS application login.

## Download OpenCATS

Download the latest release archive from [GitHub Releases](https://github.com/opencats/OpenCATS/releases). For OpenCATS 0.10.0, an example release archive workflow is:

```bash
cd /var/www/html
sudo wget https://github.com/opencats/OpenCATS/releases/download/v0.10.0/opencats-v0.10.0.zip
sudo unzip opencats-v0.10.0.zip -d opencats
```

If the release asset name differs, use the exact filename shown on GitHub Releases.

## Installing from source instead

If you clone the repository or download source code instead of using a prepared release archive, install Composer dependencies:

```bash
cd /var/www/html/opencats
composer install --no-dev
```

Use `--no-dev` for production-style installs.

## Configure ownership and permissions

The web server user must own or be able to write OpenCATS runtime directories:

```bash
sudo chown -R www-data:www-data /var/www/html/opencats
sudo chmod 750 /var/www/html/opencats
sudo chmod 770 /var/www/html/opencats/attachments
sudo chmod 770 /var/www/html/opencats/upload
sudo chmod 770 /var/www/html/opencats/temp
```

If you restore from a GUI backup file, also create and make a `restore/` directory writable by the web server user.

## Start the installer

Open your browser at your OpenCATS URL, for example:

```text
http://your-server/opencats/
```

If an `INSTALL_BLOCK` file exists before first install, remove it so the installer can run:

```bash
sudo rm /var/www/html/opencats/INSTALL_BLOCK
```

After installation, confirm `INSTALL_BLOCK` exists so the installer is not exposed again.

Now continue to [Run the Installer](run-the-installer.md).
