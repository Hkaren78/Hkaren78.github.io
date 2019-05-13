---
layout:     post
title:      "逆向破解re.exe(Windows)"
subtitle:   ""
date:       2019-05-05
author:     "HK"
header-img: "img/math.jpg"
catalog: true
tags:
    - 算法
---

先试着运行一下re.exe(电脑防火墙可能会提示是病毒，不用管点开就好)

![img](https://github.com/Hkaren78/Hkaren78.github.io/raw/master/img/in-post/reexe/run.png)

所以是要求我们写入一个密钥

拖入IDA32中，分析程序。（下图为程序的基本结构，其中红线是False分支执行的，绿线是True分支执行的）

![img](https://github.com/Hkaren78/Hkaren78.github.io/raw/master/img/in-post/reexe/structure.png)

