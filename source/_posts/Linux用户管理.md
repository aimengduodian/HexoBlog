---
title: Linux用户管理
date: 2021-12-19 21:05:36
tags: 
 - Linux用户管理
categories:
 - 运维
 - centos
keywords:
description:
top_img:
cover: https://www.mangoxo.com/album/getPic?id=108447&url=album/data/2433/5a90071b59714d459b11f78cb0bb4a88.jpg
---
## 用户新建和删除

1、 新建用户
```
useradd 用户名
```

2、 修改用户密码
```
passwd 用户名
```

3、 删除用户以及
```
userdel -r 用户名
```

## 用户列表和用户组
1、用户列表文件：/etc/passwd/

2、用户组列表文件：/etc/group

3、查看系统中有哪些用户：
```
cut -d : -f 1 /etc/passwd
```

4、查看可以登录系统的用户：
```
cat /etc/passwd | grep -v /sbin/nologin | cut -d : -f 1
```

5、查看用户属于哪个组
groups命令可以查看某个用户所属的用户组
groups 用户名

## ssh无法用密码连接服务器
/etc/ssh/sshd_config 文件中，在初始化时就被添加了一行禁止密码登录的配置：
PasswordAuthentication no
使用vim编辑配置文件将其改为：
PasswordAuthentication yes
即可使用密码进行ssh登录了。
https://www.cnblogs.com/wangsongbai/p/13403795.html