---
title: "CSS选择器 笔记"
date: 2022-02-07T15:55:54+08:00
draft: false
math: true
tags: ["css","website","note"]
categories: ["develop"]
toc: true
---

# 选择器

- 标签选择器

  ```css
  a{
      text-decoration:none;
  }
  ```

- class选择器

  ```css
  .class{
      font-size:24px;
      color:#666;
  }
  ```

- ID选择器

  ```css
  #id{
      line-height:30px;
      background-color:#666;
  }
  ```

- 全局选择器

  ```css
  *{
      margin:0;
      padding:0;
      border:none;
  }
  ```

- 群组选择器

  ```css
  a,.class,#id{
      font-size:20px;
  }
  ```

- 后代选择器

  ```css
  div a{
      color:#000;
  }
  ```

*拓展应用：组合使用多种选择器*

## 伪类选择器

- 特点

  1. 定义特殊状态下的样式
  2. 无法使用标签、id、class及其他属性实现

- [[CSS3-basic#伪类选择器|伪类]]

  - 链接

    ```css
    a:active{/*链接激活*/
        color:#fff;
    }
    a:visited{/*链接已访问*/
        color:#00ff00;
    }
    a:link{/*链接未访问*/
        color:#ff0000;
    }
    a:hover{/*鼠标悬停*/
        color:#0000ff;
    }
    ```

    顺序：`link`>`visited`>`hover`>`active`

    *说明：伪类对大小写不敏感、`link`和`visited`顺序无所谓*

  - `active`和`hover`可以适用其他元素

注意：兼容性问题

# 样式

## ==单位==

**绝对单位**

> 不能根据浏览器或父元素大小的改变而改变

- in、cm、mm、pt、pc
- 属性xx-small、x-small、small、medium、large、x-large、xx-large

**相对单位**

- px(受分辨率影响)、em/%(相对于父元素 *继承计算值*)
- 属性值：large、smaller(相对父元素)

## 字体

```css
p{
    font-family:"微软雅黑";/* 字体 */
    font-size:16px;/* 字体大小 */
    color:#000; /* 前景颜色 */
    font-weight:normal; /* 字体粗细 */
    font-style:normal; /* 字体样式 */
    font-variant:small-caps; /* 字体变形 */
}
h1{
    font:italic normal bolder 24px/32px Serif;
    /*
    注意书写顺序：font-style font-variant font-weight(顺序任意) font-size/line-height font-family
    不设置自己单独下载的字体
    */
}
```

## 文字

```css
p{
    text-indent:2em;
    text-align:center;/* 水平对齐 对于块级元素进行设置，可以继承*/
    line-height:24px;/* 可以继承、继承是继承计算值*/
}
span{
    vertical-align:middle;/* 对于行内元素和单元格元素进行设置 文字基线 */
}
```

![image-20201126184903158](/images/CSS-selector/image-20201126184903158.png)

```css
p{
    word-spacing:1px;/* 单词间距 */
    letter-spacing:0.5px;/* 字母间距 */
    text-transform:capitalize;/* 设置大小写 */
    text-decoration:none;/* 设置装饰，应用于所用元素，不能继承*/
}
```

## 背景
[[CSS3-style#背景|了解CSS3的新增背景属性]]

```css
div{
    background-color:#fff; /* 设置背景颜色，可用颜色名、rgb、rgba、十六进制 */
    background-image:url(../img/img.png);
    background-position:center;
    /* 背景图片的起始位置，可以使用百分比和位置关键词 */
    background-attachment:fixed; /* 显示方式 */
    background-repeat:no-repeat; /* 背景图片重复 */
}
```

**background-position属性说明**

![image-20210109101628522](/images/CSS-selector/image-20210109101628522.png)

```css
section{
    background:#fff url(../img/img.png) no-repeat fixed center;
    /* 顺序不固定 */
}
```

## 列表

```css
ul{
    list-style-type:none;
    list-style-image:url(../icon/icon.svg);
    list-style-positon:outside;
    /* 
    列表标记文字，有两个属性值：inside和outside.
    inside: 列表项目标标记放置在文本内，且环绕文本根据标记对齐
    outside： 默认值，列表项目标记放置在文本意外，且环绕文本不根据标记对齐
    */
}
```

**列表项标记说明**

![image-20210109102922259](/images/CSS-selector/image-20210109102922259.png)

```css
ol{
    list-style: none url(../icon/icon.svg) outside;
    /* 顺序不固定，list-style-image属性会覆盖list-style-type的设置 */
    opacity:0;/* 透明度设置 */
}
```
