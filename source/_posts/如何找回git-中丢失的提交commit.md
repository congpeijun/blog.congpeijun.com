---
title: 如何找回git 中丢失的提交(commit)
date: 2012-02-11 08:54:54
categories: 
tags:
    - Linux
    - git
---
用git fsck --lost-found命令找出刚才删除的分支里面的提交对象。

此时可以看到如这样的信息

```bash
dangling commit 01023d42e60dceca75e25fd5c9016d9ca66ddd2d
```

然后用

```bash
git show 01023d42e60dceca75e25fd5c9016d9ca66ddd2d
```

显示信息如下

```bash
commit 01023d42e60dceca75e25fd5c9016d9ca66ddd2d
Author: Cong Peijun &lt;congpeijun888@gmail.com&gt;
Date:   Tue Oct 11 22:17:42 2011 +0800
Topic 2
```

如果确认这是你丢失的commit你就可以 找回了

```bash
git checkout 01023d42e60dceca75e25fd5c9016d9ca66ddd2d
git checkout -b new_branch

```