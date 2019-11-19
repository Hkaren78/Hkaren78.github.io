---
layout:     post
title:      "【Python 3】SyntaxError: (unicode error) 'utf-8' codec can't decode byte 0xcf in position 0"
subtitle:   ""
author:     "HK"
date:		2019-11-19
header-img: "img/post-bg-Error.png"
catalog: true
tags:
    - 错误调试
---

>Django中views.py文件添加含中文代码后报错。

### 问题描述

用Django命令自动生成代码文件，结果在加入含有中文的代码后报错，将中文改为英文后能够成功运行。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119225901152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

### 原因

文件存储的格式是ANSI，只要将保存文件的格式换成UTF-8就好了。

### 解决方案

在文件目录下找到报错文件，用NewNote（也可用其他编辑器如notepad++）打开。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119230545871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

在菜单处点击重新编码，即可改变编码方式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119230736695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

保存文件后，重新启动即可运行。

### 结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119230906458.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119230945950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

网页成功显示文字。

