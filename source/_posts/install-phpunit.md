---
title: 安装PHPUnit
date: 2012-02-14 15:28:26
categories:
tags: [PHP,PHPUnit,PHP pear]
---

##通过PHP pear 安装PHPUnit

安装PHPUnit最简单的方法是通过PHP paer 来安装，

### 1. 安装php pear
首先 下载 <a title="go-pear.phar" href="http://pear.php.net/go-pear.phar">go-pear.phar</a>
然后放到PHP的目录下，就是与php.exe 在同一个目录，

然后 进入php的目录 在命令行中运行
```bash
php go-pear.phar
```

这样一直回车下来就可以了。然后点击有生成的注册表文件 PEAR_ENV.reg 来注册环境变量。注册完环境变量就可以通过pear来安装PHPUnit了。
### 2. 安装PHPUnit

在命令行中输入
```bash
pear config-set auto_discover 1
pear install pear.phpunit.de/PHPUnit
```

这样就可以完成PHPUnit的安装了。
如果需要安装其他软件包，可以继续。
DBUnit， 用于测试数据。
```bash
pear install phpunit/DbUnit
```

如果需要其他的软件，可以到按照<a href="http://www.phpunit.de/manual/current/en/installation.html" title="PHPUnit Manual" target="_blank">PHPUnit的用户手册</a>来进行按照
