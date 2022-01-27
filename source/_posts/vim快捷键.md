---
title: vim快捷键
date: 2022-01-27 10:29:12
tags: 
 - vim快捷键
categories:
 - 运维
 - centos
keywords:
description:
top_img:
cover: https://tva1.sinaimg.cn/large/005SoUZ5ly1gys40cbr0jj30hs0hsdgm.jpg
---

熟话说“磨刀不误砍柴工”，如果单纯的记快捷键而不经常使用，很快就会忘记。把自己不熟悉记录下来。方便下次使用的时候快速查找。  

## 快捷键
### 删除文本中所有内容
正常模式下`gg`将光标移动到文件的第一行,`dG`删除光标所在行以及其下所有行的内容；

扩展：

1. g可视化的时候：g是goto的意思

> gd: 将光标所在位置单词高亮显示，然后使用 'n'来顺序查找  
  gg: 将光标移动到文件的开始位置。  
  G : 将光标移动到文件的最后一行。  
  gf: 跳转文件

2. d是delete的意思  

> dd: 删除光标所在行  
  dw: 删除光标所在单词以后的字符
  daw: 删除光标所在单词，daw（delete a word）
  dG: 删除光标所在行以及其下所有行的内容



## 插曲
由于windows的使用习惯，在vim编辑器中会无意识按Ctrl+s保存，之后发现窗口不动了，无法编辑。一直以为是系统卡死了。后面实在受不了就查了一下，恍然大悟，原理Ctrl+s在Linux下是锁定屏幕的快捷键。  
Ctrl+q ：解锁屏幕  
Ctrl+s ：锁定屏幕  
