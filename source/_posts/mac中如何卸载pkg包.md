---
title: mac中如何卸载pkg包
date: 2015-04-12 20:47:00
categories: OSX
tags:
---

```bash
# pkgutil --pkgs

    com.soulmen.ulysses3
    com.taobao.aliwangwang
    com.tencent.qq
    com.tencent.snip.Snip.pkg
    com.tencent.xinWeChat

# cd /private/var/db/receipts
# ls -al

    com.tencent.snip.Snip.pkg.bom
    com.tencent.snip.Snip.pkg.plist
    com.tencent.xinWeChat.bom
    com.tencent.xinWeChat.plist

# lsbom com.tencent.snip.Snip.pkg.bom

    .	40700	501/0
    ./Snip.app	40755	0/0
    ./Snip.app/Contents	40755	0/0
    ./Snip.app/Contents/Info.plist	100644	0/0	1941	3686474346
    ......
```

#删除相应文件

```bash
sudo rm -r /Applactions/Snip.app
```

[pkg_uninstall][1]

  [1]: https://github.com/mpapis/pkg_uninstaller