---
title: 从Git仓库中恢复已删除的分支或丢失的commit
date: 2012-03-20 16:54:53
categories: [Git]
tags:
---

在使用Git的过程中，有时可能会有一些误操作

比如：执行checkout -f 或 reset -hard 或 branch -d删除一个分支

结果造成本地（远程）的分支或某些commit丢失

可以通过reflog来进行恢复，前提是丢失的分支或commit信息没有被git gc清除

一般情况下，gc对那些无用的object会保留很长时间后才清除的

reflog是git提供的一个内部工具，用于记录对git仓库进行的各种操作

可以使用git reflog show或git log -g命令来看到所有的操作日志

恢复的过程很简单：

1. 通过git log -g命令来找到我们需要恢复的信息对应的commit_id，可以通过提交的时间和日期来辨别

2. 通过git branch recover_branch commit_id 来建立一个新的分支

这样，我们就把丢失的东西给恢复到了recover_branch分支上了