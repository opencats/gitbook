# Install on Windows

### Windows Prerequisites[¶](broken-reference)

Installation instructions are given for the XAMPP default install environment only. WAMPP will also work if you prefer it. The steps will be a little different.

### Downloading software and preparing your system[¶](broken-reference)

* Download - [XAMPP](https://www.apachefriends.org/download.html)
* Install XAMPP

Note

You need to run MariaDB 10 and PHP 7.2, if there are various XAMPP options available, get the one with these package versions!!

* Download - [OpenCATS-0.9.7.2-FULL](https://github.com/opencats/OpenCATS/releases/download/v0.9.7.2/OpenCATS-v0.9.7.2-FULL.zip). You can not install this yet.
* Extract the file to `C:xampp\htdocs`&#x20;



### Start Xampp[¶](broken-reference)

* Click the Windows start button and type `xampp`
* Hit `enter`. This will open the XAMPP control panel.
* On the right side of Apache and MySQL, click `start` for each one.

Note

ONLY start the Apache and MySql services. You do NOT need any of the other services.

![\_images/start-services-xampp.png](<../../.gitbook/assets/start services xampp>)

* Stop the apache service (lower right corner, right click XAMPP, stop apache)
* Start the apache service

### OPTIONAL - Renaming your OpenCATS directory[¶](broken-reference)

The current default directory name for the OpenCATS files is OpenCATS-0.9.6. This will result in the web address in your browser being [http://localhost/OpenCATS-0.9.](http://localhost/OpenCATS-0.9.4-3)6

If you want to rename the main OpenCATS directory to something else, you can do it now.

* Simply navigate to `C:\xampp\htdocs`
* Right click on the OpenCATS directory
* Click `rename`
* Rename the directory whatever you want (example: ATS)

Now, to access it, your browser address will be [http://localhost/ATS](http://localhost/ATS)

### Launch phpMyAdmin[¶](broken-reference)

* In your browser, go to: [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/)

Note

If phpmyadmin does not load in this screen, stop and start your apache service again per the instructions above.

* On the left side, click `new` to create a new database

![\_images/phpmyadmin-main.png](<../../.gitbook/assets/phpmyadmin main>)

* In the box labelled `database name` type `opencats`.
* Hit `create`

**NOTE: The default encoding for new databases in MariaDB is latin-1 which will have problems with non-english characters. If you will encounter any non-english characters, please create your databases and select  UTF-8 encoding and collation;**\
****\
******CHARACTER SET utf8** \
**COLLATION utf8\_general\_ci**;&#x20;

![\_images/phpmyadmin-newdb.png](<../../.gitbook/assets/phpmyadmin newdb>)

You should now see “opencats” listed among the databases on the left.

* Click the opencats database
* In the top row of tabs, on the right side of the screen, click `privileges`
* Click `add user account`

![\_images/phpmyadmin-newuser.png](<../../.gitbook/assets/phpmyadmin newuser>)

* User name, make sure `use text field` is selected, in the empty box next to it type `opencats`
* Host name: In the first box, select `local` from the drop-down options. The second box should say `localhost`
* Type opencats for the database password twice
* In the “database for user account section”, confirm that the third checkbox `Grant all privileges on database "opencats"` is checked.
* Scroll down to the bottom and click `go`

![\_images/phpmyadmin-newuser2.png](<../../.gitbook/assets/phpmyadmin newuser2>)

Now go to [Run the installer](run-the-installer.md)

****
