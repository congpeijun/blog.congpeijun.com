---
title: ubuntu12.04 安装 hadoop
date: 2012-07-21 12:47:00
categories: [Linux]
tags:
---
1. 下载 cloudera CDH4的安装包 <a href="http://archive.cloudera.com/cdh4/one-click-install/precise/amd64/cdh4-repository_1.0_all.deb"> this link for a Precise system. </a>
2. 安装

```bash
sudo dpkg -i ~/Downloads/cdh4-repository_1.0_all.deb
```
3. 添加
Repository Key
```bash
$ curl -s http://archive.cloudera.com/cdh4/ubuntu/lucid/amd64/cdh/archive.key | sudo apt-key add -
```
4. 安装

```bash
sudo apt-get update #更新软件源
sudo apt-cache search hadoop #查看hadoop是否存在
sudo apt-get insall hadoop-0.20-mapreduce-jobtracker
```
