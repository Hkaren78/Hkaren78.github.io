---
layout:     post
title:      "【Diango】从零开始学习Django（一）"
subtitle:   ""
author:     "HK"
date:		2019-11-19
header-img: "img/post-bg-Django.png"
catalog: true
tags:
    - 入门
---

>Django是用Python写的一个Web框架。课设需要自己搭一个网站，用Python尝试下~

# 环境配置

### 安装Python

[Python下载地址](https://www.python.org/downloads/)

下载 python-x.x.x.msi 文件，然后一直点击 "Next" 按钮即可。安装完成后你需要设置 Python 环境变量。（右击计算机->属性->高级->环境变量->修改系统变量 path，添加 Python 安装地址）
网上有很多详细的Python安装教程，这里就不详细讲了。

### 安装Django

配置好Python环境就可以安装Django啦。我的电脑是Windows系统，所以直接打开PowerShell（shift+鼠标右键），输入命令

```python
pip install Django==2.2.7 
```

2.2.7是版本号，你们也可以选择其他版本安装。

安装完大概是这个样子：

![install](https://img-blog.csdnimg.cn/20191113163513979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

查看Django是否安装成功。在powershell内输入命令

```python
python
import django
django.get_version()
```

![version](https://img-blog.csdnimg.cn/20191113164353422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)
安装成功！

# 一个简单实例

[参考](http://www.python3.vip/doc/tutorial/django/02/)

## 创建项目

在你想要创建项目的目录下打开命令行窗口，执行下面的命令创建目录

```python
django-admin startproject bysms
```

注意最后的  bysms 就是项目的根目录名，执行上面命令后，就会创建如下的目录结构：

```python
bysms/
    manage.py
    bysms/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

最外层  bysms/  就是项目根目录  d:\projects\bysms\ ， 项目文件都放在里面。
  
- **manage.py**  是一个工具脚本，用作项目管理的。以后我们会使用它执行管理操作。
  
  
 里面的  bysms/  目录是python包。 里面包含项目的重要配置文件。这个目录名字不能随便改，因为manage.py 要用到它。
  
  
  - **bysms/settings.py**  是 Django 项目的配置文件. 包含了非常重要的配置项，以后我们可能需要修改里面的配置。
  
  
  - **bysms/urls.py**  里面存放了 一张表， 声明了前端发过来的各种http请求，分别由哪些函数处理. 这个我们后面会重点的讲。

-  **<font color=#FF0000 >bysms/wsgi.py</font>**

python 组织制定了 web 服务网关接口（Web Server Gateway Interface） 规范 ，简称wsgi。< [参考文档](https://www.python.org/dev/peps/pep-3333/)>

遵循wsgi规范的 web后端系统， 我们可以理解为 由wsgi web server  和  wsgi web application两个部分组成，它们通常是运行在一个python进程中的两个模块，或者说两个子系统。wsgi web server 接受到前端的http请求后，会调用 wsgi web application 的接口（ 比如函数或者类方法）方法，由wsgi web application 具体处理该请求。然后再把处理结果返回给 wsgi web server， wsgi web server再返回给前端。

如下图所示

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119193419525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

**wsgi web server**负责 提供高效的http请求处理环境，可以使用多线程、多进程或者协程的机制。http 请求发送到 **wsgi web server** ， **wsgi web server** 分配 线程或者进程或者 轻量级线程(协程)，然后在 这些 线程、进程、或者协程里面，去调用执行 **wsgi web application** 的入口代码。**wsgi web application** 被调用后，负责 处理 业务逻辑。 业务逻辑的处理可能非常复杂， **wsgi web application** 需要精心的设计来正确处理。 django是 **wsgi web application**的框架，它只有一个简单的单线程**wsgi web server **。 供调试时使用。产品正式上线运行的时候，通常我们需要高效的 **wsgi web server ** 产品，比如 gunicorn，uwsgi，cherrypy等，结合Django ，组成一个高效的 后端服务。所以这个 ** wsgi.py **就是 提供给wsgi web server调用 的接口文件，里面的变量application对应对象实现了 ** wsgi**入口，供wsgi web server调用 。


## 运行 Django web服务
在命令行窗口中进入项目根目录（如F:\computer\1.Python_study\Django\bysms）后，输入命令

```python
python manage.py runserver 0.0.0.0:80
```

这样服务就会被启动。 我们就可以在浏览器访问web服务了。

- 其中  0.0.0.0:80  是指定 web服务绑定的 IP 地址和端口。

- 0.0.0.0 表示绑定本机所以的IP地址， 就是可以通过任何一个本机的IP (包括 环回地址  127.0.0.1 )  都可以访问我们的服务。

- 80 表示是服务启动在80端口上。

请打开浏览器，地址栏输入 ‘127.0.0.1’ ，就可以看到如下的界面，表示Django服务搭建成功，启动成功。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119215124661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0thcmVuX0Nhc3Npb3BlaWE=,size_16,color_FFFFFF,t_70)

有可能会以为端口被占用而报错，解决方案见我的另一篇文章[【Django】Error: [WinError 10013]](https://blog.csdn.net/Karen_Cassiopeia/article/details/103151686)



    




