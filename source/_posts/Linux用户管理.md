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

4、 bash文件创建用户并生成随机密码
```bash
#!/bin/bash

ERR_USAGE=1
ERR_INT=2

ERR=$(tput setaf 1)     # red
WARN=$(tput setaf 3)    # yellow
INFO=$(tput setaf 6)    # cyan
SUCCESS=$(tput setaf 2) # green
BOLD=$(tput bold)

colorEcho(){
    echo -e $(tput bold)"$1""$2"$(tput sgr0)
}

pause(){
    colorEcho $INFO "\nNow, press any key to continue..."
    while true
    do
        printf "\r-"; sleep 0.1
        printf "\r\\"; sleep 0.1
        printf "\r|"; sleep 0.1
        printf "\r/"
        read -s -n1 -t 0.1
        [ $? -eq 0 ] && break
    done
    echo -en "\r \r" # wipe out the waiting -\|/
}

is_root(){
    if [ $(whoami) != "root" ]
    then
        colorEcho $ERR "此用户没有创建用户的权限。"
        exit $ERR_INT
    fi
}

note(){
	colorEcho $INFO "**********************************************"
	cat<<EOF
                用户管理脚本
- 作者: 爱梦多点
- 时间: 2022年1月15日01点30分
- 日志:
    1.创建用户并生成随机密码。
EOF
	colorEcho $INFO "**********************************************"

	is_root
}

congrats(){
	colorEcho $SUCCESS "**********************************************"
	cat<<EOF
    用户创建成功，请及时修改密码。
EOF
	colorEcho $SUCCESS "**********************************************"
	pause
}

adduser(){
    colorEcho $INFO "\n输入用户名或按 q 退出"
    stty erase ^h # 添加这一句后，可以使用删除按键
    while read -p "用户名:" USER_NAME
    do
		case $USER_NAME in
			q) exit $ERR_INT ;;
			*) 
                useradd "$USER_NAME" # 创建用户
                if [ $? != "0" ]
                then
                    colorEcho $ERR "此用户名不可用，请输入其他用户名。"
                else
                    break
                fi
             ;;
		esac
    done
   
    PW=$(tr -dc "0-9a-zA-Z" < /dev/urandom | head -c 8)    # 生成数字大小写，八位随机数
    echo "$USER_NAME $PW" >> /tmp/pw.txt  # 保存用户名和密码
    echo "$PW" | passwd --stdin $USER_NAME # 设置密码
    
    colorEcho $WARN "\n是否添加sudo权限。请谨慎选择。" 
    colorEcho $INFO "(1)添加" 
    colorEcho $INFO "(0)不添加" 
   
    while read -p "输入数字:" -n1 REP
    do
		case $REP in
			0) break ;;
			1) 
                echo
                echo "$USER_NAME    ALL=(ALL)    ALL" >> /etc/sudoers 
                break
                ;;
			*) 
                echo
                colorEcho $ERR "输入不合法，请重新输入" 
                ;;
		esac
    done
    echo
    colorEcho $INFO "用户密码:$PW"
}

note
adduser
congrats

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