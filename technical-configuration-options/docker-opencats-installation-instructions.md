# Docker - OpenCATS Installation Instructions

\##Docker

These instructions are for the [Docker](https://www.docker.com/) environment only. Download and install the following software:

* Docker and Docker ToolBox
* for Windows [https://docs.docker.com/engine/installation/windows/](https://docs.docker.com/engine/installation/windows/)
* for Linux [https://docs.docker.com/engine/installation/linux/](https://docs.docker.com/engine/installation/linux/)
* for MAC OSX [https://docs.docker.com/engine/installation/mac/](https://docs.docker.com/engine/installation/mac/)

\##Architecture

OpenCATS is installed in container without database. Database in in extra container. Optinaly it is also possible to create container with phpmyadmin to manage mysql database.

\##Dockerfile

OpenCats docker file is based on [php 7.2 docker image](https://hub.docker.com/layers/php-base/opencats/php-base/7.2-fpm-alpine-mcrypt/images/sha256-dd77f92ad45a4534473771b2e3ecb69876a6a02c1e9259273cdefa4697104b4f?context=explore) All dependencies are installed into the image.

\##docker-compose.yml

To build / install all images, it is possible to write `docker-compose.yml` file containg all containers definitions. Example of `docker-compose.yml`

```yml
opencats:
    container_name: opencats
    build: .
    ports:
        - 80:80
    links:
        - mysql

mysql:
    container_name: mysqlserver
    image: mysql:5.5
    ports:
        - 3306:3306
    volumes:
        - /var/lib/mysql
    environment:
        MYSQL_ROOT_PASSWORD: root
        MYSQL_DATABASE: cats_dev
        MYSQL_USER: cats
        MYSQL_PASSWORD: password

phpmyadmin:
    container_name: phpmyadmin
    image: phpmyadmin/phpmyadmin
    ports:
        - 8080:80
    links:
        - mysql:db
    environment:
        PMA_HOST: db
        PMA_USER: cats
        PMA_PASSWORD: password
```

Database server name for configuration is `mysql`, cats-dev database is created automaticaly with user `cats` and password `password`. phpmyadmin is listening on port `8080`, opencats on port `80`.

To build images:

```
docker-compose build
```

to start images:

```
docker-compose up -d
```

When image running, it is necessary to initialize DB schema and configure openCATS. Steps are described in \[Install on Windows Tutorial]

\#Using docker for development

It is possible to run opencats also in development mode, when sources are on host computer and web server is in docker. Slightly modified Dockerfile (php environment with pre-installed libraries) and docker-compose.yml is necessary

```
FROM php:5.5-apache
RUN apt-get update && \
    apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libncurses5-dev \
        libicu-dev \
        libmemcached-dev \
        libcurl4-openssl-dev \
		libpng-dev \
        libpng12-dev \
        libgmp-dev \
        libxml2-dev \
		libldap2-dev \
		php-soap \
        curl \
        zlib1g-dev \
        ssmtp

RUN apt-get install -y \
		antiword \
		poppler-utils \
		html2text \
		unrtf

RUN rm -rf /var/lib/apt/lists/* 
	
RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap && \
    docker-php-ext-configure mysql --with-mysql=mysqlnd && \
    docker-php-ext-configure mysqli --with-mysqli=mysqlnd && \
    docker-php-ext-install mysqli && \
    docker-php-ext-install mysql && \
    docker-php-ext-install soap && \
    docker-php-ext-install intl && \
    docker-php-ext-install mcrypt && \
    docker-php-ext-install gd && \
    docker-php-ext-install gmp && \
    docker-php-ext-install zip
```

and docker-compose.yml\` (web part])

```yml
opencats-dev:
    container_name: opencats-dev
    build: <path to dir with dockerfile>
    ports:
        - 80:80
    links:
        - mysql
    volumes:
        - .:/var/www/html
```

It mounts your local `.` directory as `/var/www/html` inside the container

\###Notes for windows users

* sources must be in user profiles directory, for example in Documents. This is docker limitation
* Files shall usu LF EOL (not CRLF) because sources dir on windows partition is mounted under Linux file system.
