---
title: 复制或克隆虚拟机后无eth0或eth0变eth1
date: 2012-06-05 20:43:42
categories: [Linux]
tags: 
---
原文<a href="http://blog.csdn.net/cenziboy/article/details/7345539" title="http://blog.csdn.net/cenziboy/article/details/7345539"></a>
复制或克隆虚拟机后无 eth0 或 eth0 变eth1 ( 同理eth[x] 变 eth[x+1] )解决方法如下：

在虚拟机里直接删除掉文件 /etc/udev/rules.d/70-persistent-net.rules