---
title: Mysql 备份还原
date: 2012-06-14 21:44:00
categories: [mysql]
tags:
---

```bash
msyqldump -u root -p dbname table1 table2 > db.sql

mysql -u root -p dbname > db.sql


msyqldump -u root -p dbname table1 table2 | gzip > db.sql.gz

mysql -uroot -p'passwd' database < <(zcat database.sql.gz)

```