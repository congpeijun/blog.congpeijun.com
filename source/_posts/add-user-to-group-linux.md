---
title: Linux添加用户（user）到用户组（group）
date: 2012-08-02 20:58:00
categories: Linux
tags: [Linux,Ubuntu]
---

# 将一个用户添加到用户组中，千万不能直接用：

```bash
usermod -G groupA
```

这样做会使你离开其他用户组，仅仅做为 这个用户组 groupA 的成员。
应该用 加上 -a 选项：

```bash
    usermod -a -G groupA user
    (FC4: usermod -G groupA,groupB,groupC user)
    -a 代表 append， 也就是 将自己添加到 用户组groupA 中，而不必离开 其他用户组。
```

# 命令的所有的选项，及其含义：

```bash
Options:
-c, --comment COMMENT         new value of the GECOS field
-d, --home HOME_DIR           new home directory for the user account
-e, --expiredate EXPIRE_DATE set account expiration date to EXPIRE_DATE
-f, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
-g, --gid GROUP               force use GROUP as new primary group
-G, --groups GROUPS           new list of supplementary GROUPS
-a, --append          append the user to the supplemental GROUPS
                                mentioned by the -G option without removing
                                him/her from other groups
-h, --help                    display this help message and exit
-l, --login NEW_LOGIN         new value of the login name
-L, --lock                    lock the user account
-m, --move-home               move contents of the home directory to the new
                                location (use only with -d)
-o, --non-unique              allow using duplicate (non-unique) UID
-p, --password PASSWORD       use encrypted password for the new password
-s, --shell SHELL             new login shell for the user account
-u, --uid UID                 new UID for the user account
-U, --unlock                  unlock the user account
```

查看用户所属的组使用命令：$ groups user
或者查看文件：$ cat /etc/group

原文[http://xxx.11tea.com/blog/15654][1]


  [1]: http://xxx.11tea.com/blog/15654