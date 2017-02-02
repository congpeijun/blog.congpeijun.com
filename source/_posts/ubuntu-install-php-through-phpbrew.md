---
title: ubuntu16.04 通过 phpbrew 安装 php
date: 2016-04-26 23:33:00
categories:
tags: [Linux,PHP]
---

1. 安装phpbrew

[https://github.com/phpbrew/phpbrew#install-phpbrew][1]

2. 安装编译php所依赖的软件

```bash
sudo apt-get install \
libreadline-dev \
libmcrypt-dev \
libbz2-devlibxml2-dev \
libreadline-dev \
libmcrypt-dev \
libbz2-devlibxml2-dev \
libcurl4-openssl-dev \
libjpeg-dev \
libpng-dev \
libxpm-dev \
libmysqlclient-dev \
libpq-dev \
libicu-dev \
libfreetype6-dev \
libldap2-dev \
libxslt-dev

sudo apt-get install autoconf
```

3. 安装php

```bash
phpbrew install 5.6.20 +default+dbs+openssl+mb+iconv+gettext+icu  +apxs2=/usr/bin/apxs2
```

具体参数参考 phpbrew 文档  [https://github.com/phpbrew/phpbrew#variants][2]

4. 安装 GD 扩展

```bash
phpbrew -d ext install gd -- --with-libdir=lib/x86_64-linux-gnu --with-gd=shared --enable-gd-native-ttf --with-jpeg-dir=/usr --with-png-dir=/usr --with-freetype-dir=/usr/include/freetype2/ft2build.h --with-xpm-dir=/usr
```

  [1]: https://github.com/phpbrew/phpbrew#install-phpbrew
  [2]: https://github.com/phpbrew/phpbrew#variants