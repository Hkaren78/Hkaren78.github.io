---
layout:     post
title:      "HTML从入门到入土(一)"
subtitle:   ""
author:     "HK"
date:		2020-04-04
header-img: "img/post-bg-Error.jpg"
catalog: true
tags:
    - HTML
    - 前端
---

## 基本的HTML标签

| 标题 | \<h1> - \<h6> |
| :--: | :-----------: |
| 段落 |     \<p>      |
| 链接 |     \<a>      |
| 图像 |    \<img>     |

```html
<html>
<meta name="referrer" content="never" charset="utf-8">

<body>

<!--不要为了粗体文本使用标题标签，请仅用于标题文本。粗体文本使用其它标签或 CSS 代替。-->
<h1>This is heading 1</h1>
<h2>This is heading 2</h2>
<h3>This is heading 3</h3>
<h4>This is heading 4</h4>
<h5>This is heading 5</h5>
<h6>This is heading 6</h6>

<p>下面是一张图片</p>

<!--图像的名称和尺寸是以属性的形式提供的。-->
<img  src="picaqiu.png" width="100" height="200">

<p><!--在 href 属性中指定链接的地址。-->
<a href="http://www.w3school.com.cn">This is a link</a></p>

</body>
</html>
```

![在这里插入图片描述](https://github.com/Hkaren78/Hkaren78.github.io/blob/master/img/in-post/HTMLlearning1/try1.png)

遇到的问题：

1.图片无法加载

[img标签引用图片资源无法显示的问题](https://blog.csdn.net/qq_38039015/article/details/82080037)

[相对路径](https://blog.csdn.net/qq_34769573/article/details/80445681)

2.网页上汉字出现乱码

在\<head>\</head>内添加\<meta charset="utf-8">



## HTML元素

HTML文档由HTML元素定义。

HTML元素是指从开始标签到结束标签的所有代码。

|         开始标签         |      元素内容       | 结束标签 |
| :----------------------: | :-----------------: | :------: |
|           \<p>           | This is a paragraph |  \</p>   |
| \<a href="default.htm" > |   This is a link    |  \</a>   |
|         \<br />          |                     |          |

> - 某些 HTML 元素具有*空内容*。没有内容的 HTML 元素被称为空元素。空元素是在开始标签中关闭的。\<br> 就是没有关闭标签的空元素（\<br> 标签定义换行）。所有元素都必须被关闭，故应使用\<br/>
> - 空元素在开始标签中进行关闭（以开始标签的结束而结束）
> - 大多数 HTML 元素可拥有*属性*
> - 大多数 HTML 元素可以嵌套（可以包含其他 HTML 元素）。
> - HTML 文档由嵌套的 HTML 元素构成。
> - \<html> 元素定义了**整个 HTML 文档**。\<body> 元素定义了 HTML 文档的**主体**。
> - HTML 标签对大小写不敏感：<P> 等同于 <p>。

```html
<!--以下包含3个HTML元素-->
<html>

<body>
<p>This is my first paragraph.</p>
</body>

</html>
```

## HTML属性

属性是HTML元素提供的附加信息。

 