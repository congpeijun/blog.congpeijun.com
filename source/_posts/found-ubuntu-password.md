---
title: 进Ubuntu的single模式不需密码的方法
date: 2012-02-20 19:07:58
categories: [Linux]
tags:
---

1，按Esc进入系统启动菜单，选择

```
Ubuntu, kernel 2.6.20-16-generic (recovery mode)然后按e。
```

2，在第二层菜单选择
```
kernel /boot/vmlinux-2.6.20-16-generic root=UUID=ae424e-bod0-475c-2342433 ro single
```
 按下e进行编辑。

3，修改启动参数为
```
kernel /boot/vmlinux-2.6.20-16-generic root=UUID=ae424e-bod0-475c-2342433 rw single init=/bin/bash
```
然后按回车。

4，按b启动系统就可以进入single模式而不需要密码了。

5，如果忘记了密码，建议用passwd root命令修改密码变成888888，方便好记。

本文转自 <a href="http://keyknight.blog.163.com/blog/static/3663784020085334812928/">http://keyknight.blog.163.com/blog/static/3663784020085334812928/</a>