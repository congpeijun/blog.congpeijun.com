---
title: 利用linode vps 架设vpn服务器
date: 2012-06-09 19:48:00
categories:
tags: [Linux,vpn,vps,linode]
---

再次吐槽下天国的网络

我的系统为Debian

## 安装PPTP服务器,以及 iptables

```bash
apt-get install ppp
apt-get install pptpd
apt-get install iptables
```
2. 配置PPTP服务器
编辑/etc/pptpd.conf 
查找如下内容：
```bash
#localip 192.168.0.1
#remoteip 192.168.0.234-238,192.168.0.245
```
替换为：
```bash
localip 192.168.183.1 #这里 应该 和你vps的private 的ip在一个网段。我就是这样弄的。
remoteip 192.168.183.234-238,192.168.183.245
```
上面的两行为VPN服务器的IP和VPN客户端连接后获取到的IP范围。
3. 添加PPTP VPN用户
编辑 /etc/ppp/chap-secrets 
文件 内容如下

```
#client        server  secret                  IP addresses
```

你主要按照上面的内容格式填写就行
bash echo "youname pptpd password  *" >> /etc/ppp/chap-secrets```

4. 修改DNS服务器

编辑/etc/ppp/options，添加如下内容
：
```bash
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```
5.开启ipv4转发 并设定nat转发规则
编辑/etc/sysctl.conf
去掉net.ipv4.ip_forward=1前的注释

```bash
sysctl -p #使更改生效
# 设定nat转发规则
iptables -t nat -A POSTROUTING -s 192.168.183.1/24 -o eth0 -j MASQUERADE
```

6. 重启PPTP服务
```bash
service pptpd restart
```

7. 拨上VPN上出墙吧