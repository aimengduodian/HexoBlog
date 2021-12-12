---
title: centos卸载mysql
date: 2021-12-02 23:35:50
tags:
 - 卸载mysql
 - mysql
categories:
 - 运维
 - centos
keywords:
description:
top_img:
cover: https://uploadfile.bizhizu.cn/up/c5/ba/8a/c5ba8ae79e082577b50f1521afa338b2.jpg
---

## 准备工作

查看MySQL是否安装，命令：

方法一

```
rpm -qa | grep -i mysql
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx12cumz2dj30j305ztdy.jpg)

> 查询已安装软件中包含mysql字段的安装包。
>
> 

方法二 

```
yum list installed mysql*
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx12p6y1e2j30th0a9gw5.jpg)

> yum list installed 列出已安装的安装包。

## 卸载软件

一、 卸载软件

```
sudo yum remove mysql*
```

* 卸载软件需要管理员权限，卸载时添加命令前添加sudo或用root登录

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx13k14oskj30kn0iotpi.jpg)

> 卸载完毕，显示Complete。同时可以看到移除的（Removed）软件包名。


* 在使用yum命令时如果出现 `Another app is currently holding the yum lock; waiting for it to exit...`，进程被占用的情况。可以强制关掉yum进程：

```
sudo rm -f /var/run/yum.pid
```

二、 删除mysql数据库目录

默认安装路径 `/var/lib/mysql`。

```
sudo rm -rf /var/lib/mysql
```

三、 删除mysql的安装文件

```
whereis mysql
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx14pnzwayj30d401kq3i.jpg)

默认安装路径是 /usr/lib64/mysql。然后执行删除

```
sudo rm -rf /usr/lib64/mysql
```

## 验证删除

```
rpm -qa|grep mysql
```

如果查询还存在继续卸载

```
rpm -e 包名
```

**清理缓存**

```
yum clean all
```