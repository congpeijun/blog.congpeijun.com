---
title: "phpbrew 安装 php gd扩展"
date: 2018-07-14
categories: PHP
tags:
     - PHP
     - phpbrew
---

今天升级下php版本，升级到php7.2.7，由于好久没升级，导致以前用`phpbrew` 安装的 `GD` 扩展的命令记不住了。
因为涉及到`jpeg`, `freetype`, `png` 等运行库的指定。
查看相关文档终于找到解决办法，现在记录下

[phpbrew cookbook](https://github.com/phpbrew/phpbrew/wiki/Cookbook#setting-up-prefered-lookup-prefix)

```bash
# 安装 gd 扩展
phpbrew ext install gd -- --with-gd=shared \
    --with-png-dir=/usr/local/opt/lib \
    --with-jpeg-dir=/usr/local/opt/jpeg \
    --with-freetype-dir=/usr/local/opt/freetype  \
    --enable-gd-native-ttf
```
