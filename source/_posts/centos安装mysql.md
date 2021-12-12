---
title: centos安装mysql
date: 2021-12-03 22:05:26
tags:
 - yum安装mysql
 - mysql
categories:
 - 运维
 - centos
keywords:
description:
top_img:
cover: https://uploadfile.bizhizu.cn/up/ce/a5/bf/cea5bf4085fca2e59168f7a22f478d1a.jpg
---
## 准备工作  

### 检查是否已经安装过mysql

如果已经安装，先卸载。卸载方法{% post_link centos卸载mysql 传送门 %}

### 添加在线mysql源

查找我们要下载的版本号，网站[传送门](http://repo.mysql.com/)。

右键复制下载地址，我这里选择8.0全平台支持版本

一、下载mysql的repo源

我选择的下载是 mysql80-community-release-`el8`-2.noarch.rpm 版本

```
wget http://repo.mysql.com/mysql80-community-release-el8-2.noarch.rpm
```

> 下载时要特别注意`el`选择，el8对应的是centos8版本，el7对应的是centos7版本

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx16zqlrhwj30vf0d749w.jpg)

二、安装mysql80-community-release-el8-2.noarch.rpm包

```
sudo rpm -ivh mysql80-community-release-el8-2.noarch.rpm
ls /etc/yum.repos.d/
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx17315woij30vz0ifneq.jpg)

三、更新yum命令

```
sudo yum clean all
sudo yum makecache
```

## 安装mysql

### 查看yum仓库中mysql版本

```
yum repolist all | grep mysql
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx18ew305uj30ra08vwp8.jpg)

### 安装mysql

```
sudo yum install -y mysql-community-server
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1aijnl73j30op07pady.jpg)

> 如果安装失败，请检查版本是否安装正确。之前下载的时el8版本，安装失败，卸载后重新安装。

### 启动MySQL服务

```
sudo systemctl start mysqld.service
```

停止mysql服务的命令是： sudo systemctl stop mysqld.service

### 查看MySQL服务状态

```
systemctl status mysqld.service
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1aqsbe4kj30vm0ag120.jpg)

### 首次登录首先生产随机密码

```
sudo grep 'temporary password' /var/log/mysqld.log
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1auqwzdjj310u04xn25.jpg)

> 有两个随机密码，选择日期最新的一个。

### 登录mysql

```
mysql -uroot -p
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1axdup7gj30t20avgrl.jpg)

### 修改密码

```
ALTER USER 'root'@'localhost' IDENTIFITED BY 'GPJ@123123';
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1b27n3ztj30o301jjsl.jpg)


### 查看密码策略，并更改

①查看

```
SHOW VARIABLES LIKE 'validate_password%';
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1b6uc8onj30gn09idlc.jpg)

mysql 密码策略相关参数；

* validate_password.length  固定密码的总长度；
* validate_password.dictionary_file 指定密码验证的文件路径；
* validate_password.mixed_case_count  整个密码中至少要包含大/小写字母的总个数；
* validate_password.number_count  整个密码中至少要包含阿拉伯数字的个数；
* validate_password.special_char_count 整个密码中至少要包含特殊字符的个数；
* validate_password.policy 指定密码的强度验证等级，默认为 MEDIUM；

validate_password.policy 的取值：

* 0/LOW：只验证长度；
* 1/MEDIUM：验证长度、数字、大小写、特殊字符；
* 2/STRONG：验证长度、数字、大小写、特殊字符、字典文件；

② 修改

```
SET GLOBAL validate_password.policy=LOW;
SET GLOBAL validate_password.length=6; 
```

> 首先需要设置密码的验证强度等级，设置 validate_password_policy 的全局参数为 LOW，此时只验证长度。
>
> 这时就可以修改密码为简单容易记得纯数字了

### 设置开机自启

```
sudo systemctl enable mysqld.service
```

## 其他问题

### 1130错误，无法远程连接

以上步骤设置完成后，用navicat测试连接，报1130错

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1bv4k2oij30m60eyn0e.jpg)

> 原因：主机不被允许连接
>
> 解决：更改"mysql" 数据库里的 "user" 表里的 "host"项，从"localhost"改称"%"

运行语句

```
mysql -u root -p

mysql>use mysql;

mysql>select 'host' from user where user='root';

mysql>update user set host = '%' where user ='root';

mysql>flush privileges;
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1c2m5ijjj30ls0huk0k.jpg)

### 2059错误（MySQL8）

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1c3kjx6ej30p50kh0xc.jpg)

安装了mysql8后出现2059错误。

> 原因为安装时选择了强加密规则caching_sha2_password，与之前的mysql5.7的mysql_native_password规则不同，本人使用的navicate驱动不支持新加密规则。
> 更改加密规则

运行语句

```
mysql -u root -p

mysql>use mysql;

mysql>select user,plugin from user where user='root';

mysql>alter user 'root'@'%' identified with mysql_native_password by '123456';

mysql>flush privileges;
```

![image.png](http://tva1.sinaimg.cn/mw690/005SoUZ5ly1gx1cfzx8k6j30gr0fw46r.jpg)

测试连接成功

![image.png](http://tva1.sinaimg.cn/large/005SoUZ5ly1gx1cnmtc3lj30ou0kvq7g.jpg)