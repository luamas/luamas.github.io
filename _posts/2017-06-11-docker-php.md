---
title: Docker 安装php
layout: post
date: '2017-05-10 00:00:00'
categories: docker
tags: docker php
author: luamas
original: true
---

* content
{:toc}

{% raw %}

## 定义Dockerfile,采用官方php镜像安装，下面安装了拓展:

```bash
FROM php:5.6-fpm
RUN  apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
     && docker-php-ext-install -j$(nproc) iconv mcrypt gd mysqli mysql pdo pdo_mysql  \
     && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
     && docker-php-ext-enable mysql
```


### `docker-php-ext-install [-jN] ext-name [ext-name ...]`

安装模块，`docker-php-ext-install`，如`mysql`模块：'docker-php-ext-install mysql'

可能的模块：
`bcmath bz2 calendar ctype curl dba dom enchant exif fileinfo filter ftp gd gettext gmp hash iconv imap interbase intl json ldap mbstring mcrypt mssql mysql mysqli oci8 odbc opcache pcntl pdo pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql pdo_sqlite pgsql phar posix pspell readline recode reflection session shmop simplexml snmp soap sockets spl standard sybase_ct sysvmsg sysvsem sysvshm tidy tokenizer wddx xml xmlreader xmlrpc xmlwriter xsl zip`




### `docker-php-ext-enable [options] module-name [module-name ...]`

启用模块，`docker-php-ext-enable`，如'mysqli'模块：'docker-php-ext-enable mysql'

可能的模块：
`gd.so iconv.so mcrypt.so mysql.so mysqli.so opcache.so pdo.so pdo_mysql.so`




### `docker-php-ext-configure ext-name [configure flags]`

配置模块,`docker-php-ext-configure`,如`gd`配置：`docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/`

可能的值：
`bcmath bz2 calendar ctype curl dba dom enchant exif fileinfo filter ftp gd gettext gmp hash iconv imap interbase intl json ldap mbstring mcrypt mssql mysql mysqli oci8 odbc opcache pcntl pdo pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql pdo_sqlite pgsql phar posix pspell readline recode reflection session shmop simplexml snmp soap sockets spl standard sybase_ct sysvmsg sysvsem sysvshm tidy tokenizer wddx xml xmlreader xmlrpc xmlwriter xsl zip`




### 如果需要安装外置拓展必须要用到`pecl`命令，如安装`redis`:`pecl install redis-3.1.0`.
最后要启用`docker-php-ext-enable redis`


## 定义docker-compose.yml:

```docker
php:
  build: ./php
  container_name: php56
  expose:
    - 9000
  volumes:
    - /data/wwwroot:/var/www/html/data/wwwroot
  restart: always
  external_links:
    - mysql
```

这样在我们的php连接数据的host可以采用mysql去代替

{% endraw %}

