---
title: Ubuntu下SSH设置
date: 2012-04-26 20:30:01
categories: [Linux]
tags:
---
1. 首先安装ssh服务器

```bash
sudo apt-get install openssh-server
```

　　然后确认sshserver是否启动了：（或用“netstat -tlp”命令）
　　
```bash
ps -e | grep ssh
```
　　如果只有ssh-agent那ssh-server还没有启动，需要/etc/init.d/ssh start，如果看到sshd那说明ssh-server已经启动了。

　　ssh-server配置文件位于/ etc/ssh/sshd_config，在这里可以定义SSH的服务端口，默认端口是22，你可以自己定义成其他端口号，如222。然后重启SSH服务：
　　
```bash
sudo /etc/init.d/ssh resart
```
　　事实上如果没什么特别需求，到这里 OpenSSH Server 就算安装好了。但是进一步设置一下，可以让 OpenSSH 登录时间更短，并且更加安全。这一切都是通过修改 openssh 的配置文件 sshd_config 实现的。
　　首先，您刚才实验远程登录的时候可能会发现，在输入完用户名后需要等很长一段时间才会提示输入密码。其实这是由于 sshd 需要反查客户端的 dns 信息导致的。我们可以通过禁用这个特性来大幅提高登录的速度。首先，打开 sshd_config 文件：
　　
```bash
sudo nano /etc/ssh/sshd_config
```
　　找到 GSSAPI options 这一节，将下面两行注释掉：

    ```bash

    #GSSAPIAuthentication yes
    #GSSAPIDelegateCredentials no


```然后重新启动 ssh 服务即可：

```bash

　　sudo /etc/init.d/ssh restart


```　　再登录试试，应该非常快了吧
　　利用 PuTTy 通过证书认证登录服务器
　　SSH 服务中，所有的内容都是加密传输的，安全性基本有保证。但是如果能使用证书认证的话，安全性将会更上一层楼，而且经过一定的设置，还能实现证书认证自动登录的效果。
　　首先修改 sshd_config 文件，开启证书认证选项：

```bash

　　RSAAuthentication yes
   PubkeyAuthentication yes AuthorizedKeysFile %h/.ssh/authorized_keys


```修改完成后重新启动 ssh 服务。
　　下一步我们需要为 SSH 用户建立私钥和公钥。首先要登录到需要建立密钥的账户下，这里注意退出 root 用户，需要的话用 su 命令切换到其它用户下。然后运行：

```bash
　　ssh-keygen
```
　　这里，我们将生成的 key 存放在默认目录下即可。建立的过程中会提示输入 passphrase，这相当于给证书加个密码，也是提高安全性的措施，这样即使证书不小心被人拷走也不怕了。当然如果这个留空的话，后面即可实现 PuTTy 通过证书认证的自动登录。
　　ssh-keygen 命令会生成两个密钥，首先我们需要将公钥改名留在服务器上：
　　
```bash
cd ~/.ssh mv id_rsa.pub authorized_keys
```
然后将私钥 id_rsa 从服务器上复制出来，并删除掉服务器上的 id_rsa 文件。
　　服务器上的设置就做完了，下面的步骤需要在客户端电脑上来做。首先，我们需要将 id_rsa 文件转化为 PuTTy 支持的格式。这里我们需要利用 PuTTyGEN 这个工具：
　　点击 PuTTyGen 界面中的 Load 按钮，选择 id_rsa 文件，输入 passphrase（如果有的话），然后再点击 Save PrivateKey 按钮，这样 PuTTy 接受的私钥就做好了。
　　打开 PuTTy，在 Session 中输入服务器的 IP 地址，在 Connection->SSH->Auth 下点击 Browse 按钮，选择刚才生成好的私钥。然后回到 Connection 选项，在 Auto-login username 中输入证书所属的用户名。回到 Session 选项卡，输入个名字点 Save 保存下这个 Session。点击底部的 Open 应该就可以通过证书认证登录到服务器了。如果有 passphrase 的话，登录过程中会要求输入 passphrase，否则将会直接登录到服务器上，非常的方便。