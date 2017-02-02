---
title: ubuntu12.04安装jdk7
date: 2012-07-21 12:13:00
categories: [Ubuntu]
tags:
---

1、首先到oracle下载jdk-7u5-linux-x64.tar.gz
2、将jdk-7u5-linux-x64.tar.gz 解压到 /usr/lib/jvm/目录下面，这里如果没有jvm文件夹，则创建该文件夹,命令:

```bash
sudo mkdir jvm  #创建文件夹jvm
sudo tar zxvf  ~/Downloads/jdk-7u5-linux-x64.tar.gz -C /usr/lib/jvm #解压缩文件
```

3、设置环境变量，用gedit打开/etc/profile文件

```bash
sudo gedit /etc/profile
```

在文件的最后面增加：

```bash
export JAVA_HOME=/usr/lib/jdk1.7.0_05
export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH
export CLASSPATH=$CLASSPATH:.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
```

4、将系统默认的jdk修改过来

```bash
$ sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_05/bin/java 300
$ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_05/bin/javac 300
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
```

5、检测，输入


```bash
java -version
java version "1.7.0_05"
Java(TM) SE Runtime Environment (build 1.7.0_05-b06)
Java HotSpot(TM) 64-Bit Server VM (build 23.1-b03, mixed mode)
```

