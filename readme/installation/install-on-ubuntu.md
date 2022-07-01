# Install on Ubuntu

These instructions will walk you through setting up LAMP (Linux Apache Mysql PHP) software and install OpenCATS on a Ubuntu  machine. These instructions will work with a VPS, or a home/office machine.

### Ubuntu -Installing pre-reqs[¶](broken-reference)

**Note**

mysql and mariadb have diverged somewhat in base functionality. Whereas MariaDB and MySQL were interchangable in previous releases, now only MariaDB is supported. If you need to move from MySQL to MariaDB it's as simple as installing MariaDB over MySQL.&#x20;

* `$ sudo apt-get update`
* `$ sudo apt-get install mariadb-server mariadb-client`
* `$ sudo mysql_secure_installation`\
  ``
* `$ sudo apt-get install apache2`

**Note for PHP 7.2**\
****Versions higher than PHP7.2 are not currently supported



1: add the PPA maintained by Ondrej Surý

```
sudo add-apt-repository ppa:ondrej/php
```

2: install PHP versions 7.2

```
sudo apt install php7.2
```

3: Select the standard version of PHP

```
sudo update-alternatives --set php /usr/bin/php7.2
```

* `$ sudo apt-get install php7.2 php7.2-soap php7.2-ldap`
* `$ sudo apt-get install php7.2-mysqli php7.2-gd php7.2-xml`
* `$ sudo apt-get install php7.2-curl php7.2-mbstring php7.2-zip`
* `$ sudo service apache2 restart`

### Setting up your MySQL/MariaDB database[¶](broken-reference)

**Note**

**This is the backend database that stores all your OpenCATS information. You likely will NOT be messing with this much after installation unless you choose to. The login/password you set up here will NOT be the same as your login/password for OpenCATS.**

* `$ sudo mysql -u root -p`

It will ask you for your Ubuntu Root password

Then it will ask you for your mysql root password

* You should see a prompt like this: `mysql>`
* `mysql>` CREATE USER [‘opencats’@’localhost](mailto:'opencats'%40'localhost)’ IDENTIFIED BY ‘databasepassword’;
* `mysql>` CREATE DATABASE opencats;
* `mysql>` GRANT ALL PRIVILEGES ON ‘opencats’.\* TO [‘opencats’@’localhost](mailto:'opencats'%40'localhost)’ IDENTIFIED BY ‘databasepassword’;
* `mysql>` exit;

Note

Make sure you don’t forget the ; on the end of every line!

### Download the OpenCATS files[¶](broken-reference)

* `$ cd /var/www/html`
* `$ sudo wget`[`https://github.com/opencats/OpenCATS/releases/download/0.9.6/opencats-0.9.6-full.zip`](https://github.com/opencats/OpenCATS/releases/download/0.9.6/opencats-0.9.6-full.zip)``
* `$ sudo unzip opencats-0.9.6-full.zip`
* `$ sudo mv opencats-0.9.6 opencats`
* `$ rm /var/www/html/opencats/INSTALL_BLOCK`

Note

By default in this documentation for OpenCATS version 0.9.6 the directory would be `opencats`. You can name it whatever you want. Just remember that all of the directory locations from here on must match the name of the directory you create, including capitol letters.

Note

If you have tried installing OpenCATS before, or for any reason see something called INSTALL\_BLOCK in this directory, you MUST delete it. This will prevent OpenCATS from running the installer. The command for that would be `$ sudo rm INSTALL_BLOCK`.

### Server and Directory permissions[¶](broken-reference)

* `$ sudo chown www-data:www-data opencats`
* `$ sudo chown -R www-data:www-data opencats`
* `$ sudo chmod 770 opencats/attachments`
* `$ sudo chmod 770 opencats/upload`

Now go to [Run the installer](run-the-installer.md)

###
