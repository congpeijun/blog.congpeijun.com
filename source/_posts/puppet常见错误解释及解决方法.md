---
title: puppet常见错误解释及解决方法
date: 2012-06-05 21:59:53
categories: [Linux]
tags: 
---
转自：<a href="http://www.mysqlops.com/2011/11/08/puppet-errors.html" title="http://www.mysqlops.com/2011/11/08/puppet-errors.html" target="_blank">http://www.mysqlops.com/2011/11/08/puppet-errors.html</a>

【导读】
puppet在运维管理是个自动化的工具，作用非常明显，但是苦于puppet 中文资料不多，puppet学习难度
大，在puppet使用过程中，碰到很多各种奇怪的问题，这里是 sky的个人总结的一些puppet常见错误，以
及相应的解决方法，也感谢部分群友的分享：坚持创新和Ninja
以及再一年等QQ好友，也希望更多的人分享puppet知识，共同进步。
【puppet 常见错误列表】
1.Failed to retrieve current state of resource: Could not retrieve information from source(s)
err: //test/File[/tmp/sky]: Failed to retrieve current state of resource: Could not retrieve information from source(s) puppet:///test/foo at /etc/puppet/modules/test/manifests/init.pp:6
解决方法：这是一般大多人犯的问题，这个问题一般是出现在puppetmaster上，大部分是source这个路径没有写对。可以查看init.pp的第6行，
一般正确的写法是source => “puppet:///test/sky”，关于文件服务器的写法，可以参阅之前的sky的文档.
2.Could not retrieve information from environment production source(s) puppet://server.puppet.com/plugins
 
解决方法：这是一般都是通过yum或者apt-get安装了puppet，在puppetmaster和客户端的配置文件 里有pluginsync=true ,
把两端/etc/puppet.conf里pluginsync=true ,改成pluginsync=false，并重启puppetmaster即可解决。
再补充一种方法，如果不设置pluginsync=false，那么就需要到少要建个插件。
3.Could not request certificate: undefined method `closed?’
err: Could not request certificate: undefined method `closed?' for nil:NilClass Exiting;
failed to retrieve certificate and watiforcert is disabled
解决方法：a.确保puppet的运行用户是否有权限读ssl认证文件。
b.确认防火墙是否打开8140端口。
4.Change from absent to file failed
err: //test/File[/tmp/sky/www.mysqlops.com]/ensure: change from absent to file failed: Could not set file on ensure: No such file or directory 
解决方法：很明显，可能是没有/tmp/sky这个目录，如果没有，请使用 mkdir -p /tmp/sky.
5.Run of Puppet configuration client already in progress
解决方法，很明显，a.可以通过ps -axf|grep puppet是否有puppet进程在运行。如果有，则停掉puppet，再运行，即可。
b.没有进程，那有可能puppetdlock存在，则删除之，使用 rm -rf /var/puppet/state/puppetdlock
6.Change failed ... Could not find server
err: //test/File[/tmp/sky]/content: change from {md5}068008008418dff20750a94336318974 to {md5}8db2d67767577c70b1251fd80ad32943 failed: Could not find server puppet
解决方法：这是设置了filebucket, 名称为puppet,但并不没有使用真名。在配置文件/etc/puppet.conf里设置如下：
filebucket {
puppetmaster: server => "www.mysqlops.com" # server后面要用全名，即fqdn.
}
7.Could not retrieve catalog: can't convert nil into String
err: Could not retrieve catalog: can't convert nil into String at /etc/puppet/modules/test/manifests/init.pp:29 on node web-01.test.com
解决方法：确认模板文件是否存在。一般都是文件不存在的时候报的。
8.undefined method `closed?’ for nil:NilClass
err: Could not retrieve catalog from remote server: undefined method `closed?' for nil:NilClas
解决方法：经常可能是语法错误，少了双引号或者 大概号什么的。
9.certificate verify failed
err: /File[/var/lib/puppet/lib]: Failed to generate additional resources using 'eval_generate': SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
err: /File[/var/lib/puppet/lib]: Failed to retrieve current state of resource: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed Could not retrieve file metadata for puppet://puppet.example.com/plugins: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
err: Could not retrieve catalog from remote server: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
解决方法：这可能是换了不同的两台puppetmaster服务器引起的。解决方法，删除现有ssl证书。find /var/lib/puppet -type f -print0 |xargs -0r rm
10.Could not retrieve catalog from remote server
err: Could not retrieve catalog from remote server: No such file or directory - /var/lib/puppet/client_yaml/catalog
解决方法：明显提示，/var/lib/puppet/client_yaml/可能不存在，没有则创建之。
11.no certificate found and waitforcert is disabled
warning: peer certificate won’t be verified in this SSL session
Exiting; no certificate found and waitforcert is disabled
解决方法：    这个是需要puppetmaster端给客户端ssl签名.
a.puppetca -l #列出等待签名的客户端
b.puppetca -s hostname # hostname 是客户端主机的名字。
12.Could not intern from pson: Could not convert from pson: Could not find relationship target ”
解决方法：这是一个bug, 针对版本0.25.1.在puppet0.25.5.1版本中可以在/etc/puppet/puppet.conf里添加
preferred_serialization_format = yaml 即可解决。
13.Error 400 on SERVER: No support for http method POST
err: Could not retrieve catalog from remote server: Error 400 on SERVER: No support for http method POST
解决方法：这个问题可能是puppetmaster是2.6版本，puppet客户端版本是2.7。请记住 ，puppetmaster版本可以大于或者等于客户端的版本。
不能小于，互换下puppet角色，即可。
14.You cannot specify more than one of content, source, target
err: Could not run Puppet configuration client: You cannot specify more than one of content, source, target at /etc/puppet/modules/test/manifests/init.pp:10
解决方法：错误提示很显示，不能为一个文件指定多个来源。
15.Could not retrieve catalog from remote server: wrong header line format
解决方法，可能是模板语法错误，请使用命令检查模板语法，示例：erb -x -T ‘-’ -P  test/templates/cron/cron.erb  |ruby -c
16.Cannot override local resource on node
err: Could not retrieve catalog from remote server: Error 400 on SERVER: Exported resource skyd[foo] cannot override local resource on node web-01.mysqlops.com
解决方法：这个问题，一般是在使用export 虚拟资源的时候会出现，可能是有重复的定义。
这可能是由于旧节点，运行puppet clean node。查看数据库。使用下面的sql
“select hosts.name from hosts,resources where restype=’skyd’ and title=’foo’ and hosts.id = resources.host_id;”
17.Could not render to pson: invalid utf8 byte
err: Could not retrieve catalog from remote server: Error 400 on SERVER: Could not render to pson: invalid utf8 byte:
解决方法：可能是有无效的utf8字符，可以使用命令：od -c filename 进行检查。
18. 在安装dashboad的时候，报
RAILS_ENV=production db:migrate --trace
(in /usr/local/puppet-dashboard)
** Invoke db:migrate (first_time)
** Invoke environment (first_time)
** Execute environment
** Execute db:migrate
** Invoke db:schema:dump (first_time)
** Invoke environment
** Execute db:schema:dump
rake aborted!
undefined method `reenable' for <Rake::Task db:schema:dump => [environment]>:Rake::Task
解决访问，可能是gem版本不对，升级版本，请确认版本是否满足安装dashboad的要求。
19.在执行facter命令的时候报：
err: Could not run Puppet configuration client: Could not retrieve local facts: execution expired
解决方法，由于ubuntu升级导致的ec2.rb有新的改动，可以修改/usr/lib/ruby/1.8/facter这个目录下面的ec2.rb文件后从新启动后解决。
具体解决方法如下：1. cd /usr/lib/ruby/1.8/facter
                2. cp ec2.rb{,.`date -I`} #备份新的ec2.rb这个文件
                3. 从正常执行的主机复制ec2.rb这个文件进行覆盖。或者比较两个文件的不同，进行修改。
20.在使用gem 安装的时候会报：
ERROR:  Could not find a valid gem 'rake' (>= 0) in any repository
ERROR:  While executing gem ... (Gem::RemoteFetcher::FetchError)
解决方法：可能是没有gem源，或者无法访问源，大部分是网络问题。
可以多添加下几个gem 源来尝试解决：
使用命令：
gem sources -a http://gems.rubyforge.org
gem sources -a http://gems.rubyonrails.org
gem sources -a http://gemcutter.org
gem sources -a http://rubygems.org
添加了上面的四个gem源，希望能解决，另外使用gem 安装的过程比较慢，需要大家耐心点等待提示。
21.err: Could not request certificate: Retrieved certificate does not match private key; please remove certificate from server and regenerate it with the current key
解决方法：按照下面a-d四个步骤，即可。
a.在客户端可以删除rm -rf /var/lib/puppet/ssl/,
b.在puppetmaster端，执行 puppetca -c 客户端主机名
c. 客户端在重新生成证书请求: puppet –test –server puppetmaster主机名
d.在puppetmaster端，执行 puppetca -s 客户端主机名
 