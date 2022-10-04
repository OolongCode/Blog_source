---
title: "HTML5 笔记"
date: 2022-02-03T16:39:07+08:00
draft: false
math: true
tags: ["html","website","note"]
categories: ["develop"]
toc: true
---

# HTML5

## 介绍

**HTML5发展历程**

![image-20200927150853419](/images/HTML5/image-20200927150853419.png)

* 标签变化

  DTD、新增的标签、删除的标签、重定义标签

* 网页布局

  新的页面布局、区别和意义

* 属性变化

  input、表单属性、链接属性、其他属性



## 标签

**HTML<!DOCTYPE>标签**

> **定义和用法**
>
> `<!DOCTYPE>`声明必须是HTML文档的第一行，位于`<html>`标签之前
>
> **不是HTML标签**
>
> 指示web浏览器关于页面使用哪个HTML版本进行编写的指令

**常用的DOCYPE声明**

![image-20200927151817436](/images/HTML5/image-20200927151817436.png)

**DTD文档类型定义**

![image-20200927151948200](/images/HTML5/image-20200927151948200.png)

![image-20200927152012495](/images/HTML5/image-20200927152012495.png)

### 新增元素

**结构标签（块状元素）——有意义的div**

```html
<article>article</article>
<header>header of page</header>
<nav>nav of page</nav>
<section>the area</section>
<hgroup>information about</hgroup>
<figure>multimedia</figure>
<footer>footer of page</footer>
<dialog>
	<dt>
        chat title
    </dt>
    <dd>
    	chat content
    </dd>
</dialog>

<!--
补充
1. header/section/aside/article/footer 不要使用嵌套
2. header/section/footer级别最高>aside/article/figure/hgroup/nav >div>figcaption 

-->
```

**多媒体标签（意义：富媒体的发展，提升用户体验）**

```html
<video src="video path" autoplay="" controls="" width="400px" height="300px">video</video>

<audio src="audio path" autoplay="" loop="-1" control="">not read the text</audio>

<source src="path" type="recode type"/>

<canvas>draw</canvas>
<embed src="extra path" width="100px" height="20px">
```
