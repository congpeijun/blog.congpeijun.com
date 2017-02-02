---
title: Puppet 模块
date: 2012-06-14 21:26:14
categories:
tags: [Linux,Puppet,分布式]
---
## puppet 模块基础

puppet模块可以导入，复用都很方便，在这里先回答下之前的两个问题：
1. 查看puppet 模块路径，可以使用如下命令：


```bash[root@c1.inanu.net]# puppetmasterd --configprint modulepath
/etc/uppet/modules:/usr/share/puppet/modules  #可以看到这两个目录是puppet 模块默认所在的目录。
```

2. 要引用 puppet模块，如果模块所在上面的两个默认的路径可以使用：
import “模块名”
如果提示模块不存在，比如我在 /data/modules，那么有两种解决方法：
1. 是修改puppet.conf文件，添加目录到modulepath.举例 ：

<code>modulepath = /data/modules:/etlc/puppet/modules</code>
2. 是在引用的是时候用绝对路径。
import “/data/modules/模块名”

了解完puppet 模块基础后，接下来就为大家写个简单模块示例：

puppet 模块实例

```bash
root@s1:/etc/puppet/modules# cd /etc/puppet/modules
root@s1:/etc/puppet/modules# mkdir -p test/{manifests,files,templates}

```

这三个目录说明：files目录是用来存放同步远程客户端的文件或者文件夹，manifests目录下放.pp文件，且必须要有init.pp，templates是存放的puppet 模板文件，是以.erb结尾的。

建立init.pp文件

```bash
# /etc/puppet/modules/test/manifests/init.pp
class test::test {
    file { "/tmp/test":
        owner   => root,
        group   => root,
        ensure  => present,
        content => "Hello word",
        mode    => "0644",
     }
}

```

在/etc/puppet/manifests/site.pp里添加：

```bash
node "default" {
    include test::test
}

```

注：不建议这样操作，实际生产中，我会在site.pp里添加 import “nodes.pp”，然后在nodes.pp里添加上面的内容。

这样我们就建立了我们第一个puppet 模块，在到客户端 c2.inanu.net 上运行puppet查看结果：

```bash
congpeijun@s2:/tmp$ puppet agent --server s1.ubuntu.local --test
info: Caching catalog for s2.ubuntu.local
info: Applying configuration version '1339679532'
notice: /Stage[main]/Test/File[/tmp/test]/ensure: created
notice: Finished catalog run in 0.05 seconds
congpeijun@s2:/tmp$ cat test
Hello word

```

再次验证，可以看到已经成功运行，已经达到预期的效果。在/tmp/目录下生成了nanu这个文件，有个问题，不知道大家注意到没有，这里并没有import “test”模块，而直接使用了include test::test类。有兴趣的同学可以试试，再看下效果。

本文参考 <a href="http://www.inanu.net/post/730.html" title="http://www.inanu.net/post/730.html" target="_blank">http://www.inanu.net/post/730.html</a>