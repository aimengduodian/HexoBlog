---
title: git提交方式
date: 2021-11-12 00:29:16
tags:
categories:
keywords:
description:
top_img:
cover: http://tva1.sinaimg.cn/large/005SoUZ5ly1gwdyv8i3w5j30rs0m8dll.jpg
---

git 在提交时，总是超时。科学上网后还是不能提交。
> git push
fatal: unable to access 'https://github.com/*/': Failed to connect to github.com port 443 after 21119 ms: Timed out

在网上收罗了一些解决方案：  

### 修改代理

```powershell
 git config --global http.proxy
 git config --global https.proxy
 env|grep -I proxy
```
查询二者均无返回信息，没有代理

### 修改`https`为 `http`

- [参考博客](https://blog.csdn.net/u010003835/article/details/78816481)

由于git 的仓库 进行初始化 的时候配置的是 https 的方式，  
在 git push 的时候每次都要输入用户名 跟 密码。非常的不方便，  
究其原因，该配置是在 ./git/config 文件中配置的  