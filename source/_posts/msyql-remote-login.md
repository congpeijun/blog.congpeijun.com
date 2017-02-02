---
title: msyql无法远程访问的解决方法
date: 2012-04-26 21:21:55
categories: [mysql]
tags: 
---

先设置mysql的配置  /etc/mysql/my.cnf
找到 bind_address 修改后面的ip， 或是 注释掉本行
　解决方法：

　　一、改表法。可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"

　　mysql -u root -pvmwaremysql>use mysql;

　　mysql>update user set host = '%' where user = 'root';

　　mysql>select host, user from user;

　　二、授权法。

　　msyql>GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WI TH GRANT OPTION;

　　如果你想允许用户myuser从ip为10.0.1.5的主机连接到mysql服务器，并使用mypassword作为密码

　　mysql>GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'10.0.1.5' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;

　　mysql>FLUSH RIVILEGES

　　使修改生效.就可以了

　　三、授权办法

　　在安装mysql的机器上运行：

　　1、d:/mysql/bin/>mysql -h localhost -u root

　　//这样应该可以进入MySQL服务器

　　2、mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION

　　//赋予任何主机访问数据的权限

　　3、mysql>FLUSH PRIVILEGES

　　//修改生效

　　4、mysql>EXIT

　　//退出MySQL服务器

　　这样就可以在其它任何的主机上以root身份登录啦。