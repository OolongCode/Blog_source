---
title: "CSS3样式 笔记"
date: 2022-02-07T16:17:59+08:00
draft: false
math: true
tags: ["css","website","note"]
categories: ["develop"]
toc: true
---

# CSS3样式

## 盒子

> 与盒子模型相关的一些样式属性

### 圆角

复合属性

```css
div{
    width:100px;
    height:100px;
    border-radius:2px; /* 四个圆角值均为2px */
    border-radius:2px 4px; /* 左上角与右下角为2px，右上角和左下角为4px */
    border-radius:2px 4px 3px; /* 左上角为2px，右上角和左下角为4px，右下角为3px */
    border-radius:2px 4px 3px 1px; /* 左上角为2px，右上角为4px，右下角为3px，左下角为1px */
}
```

复合属性分开书写

```css
div{
    width:100px;
    height:100px;
    border-top-left-radius:2px;
    border-top-right-radius:4px;
    border-bottom-right-radius:3px;
    border-bottom-left-radius:1px;
}
```



### 盒阴影

```css
.box1{
    width:100px;
    height:100px;
    box-shadow:10px 5px blue;
    /* 设置竖直偏移10px 水平偏移5px的蓝色盒阴影 */
}
.box2{
    width:100px;
    height:100px;
    box-shadow:10px 5px 3px blue;
    /* 设置竖直偏移10px 水平偏移5px 模糊半径3px的蓝色盒阴影 */
}
.box3{
    width:100px;
    height:100px;
    box-shadow:10px 5px 3px 6px blue;
    /* 设置竖直偏移10px 水平偏移5px 模糊半径3px 扩散半径6px的蓝色盒阴影 */
}
.box4{
    width:100px;
    height:100px;
    box-shadow:10px 5px 5px 5px blue inset;
    /* 设置竖直偏移10px 水平偏移5px 模糊半径3px 扩散半径6px的蓝色盒内阴影 */
}
```

### 边界图片

构建美丽的可扩展按钮

```css
div{
    border-image:url(../img/img.png) 10 10 stretch;
}
hi{
    border-image:linear-gradient(red, yellow) 10 10 round;
}
p{
    border-image:url(../img/img.svg) 10 10 repeat;
}
```

### 背景

[[CSS-selector#背景|查看CSS背景属性]]

```css
div{
    width:100px;
    height:100px;
    background-clip:content-box; /* 指定背景绘制区域，有border-box、padding-box和content-box属性值 */
    background-origin:content-box; /* 设置元素背景图片的原始起始位置，指定background-position属性应该是相对属性 */
    background-size:cover; /* 指定背景图片大小 */
    background-image:url(../img/1.jpg),url(../img/2.jpg); /* 设置多重背景（前遮后显） */
}
```

背景属性整合

```css
div{
    width:100px;
    height:100px;
	background:red center cover no-repeat content-box content-box fixed url(../img/img.png);
}
```

## 渐变

可以在两个或多个指定的颜色之间显示平稳的过渡

### 线性渐变

> 沿着一根轴线改变颜色，从起点到终点颜色进行顺序渐变（从一边拉向另一边）

`linear-gradient`属性

```css
.box1{
    width:100px;
    height:100px;
    background:linear-gradient(red,yellow);
}
.box2{
	width:100px;
    height:100px;
    background:linear-gradient(to right,red,yellow);
}
.box3{
    width:100px;
    height:100px;
    background:linear-gradient(to right top,red,yellow);
}
/* 使用角度 */
.box4{
    width:100px;
    height:100px;
    background:linear-gradient(45deg,red,yellow); /* 默认情况 */
    background:-webkit-linear-gradient(45deg,red,yellow) /* -webkit-情况 */
}
/* 使用百分比调节 */
.box5{
    width:100px;
    height:100px;
    background:linear-gradient(red 0%, orange 25%, yellow 50%, green 75%, blue 100%);
}
```

默认情况

![image-20210116231404634](/images/CSS-style/image-20210116231404634.png)

`-webkit-`情况

![image-20210116231605432](/images/CSS-style/image-20210116231605432.png)

**拓展：重复渐变**

```css
.box{
    width:100px;
    height:100px;
    background:repeating-linear-gradient(red 0%,yellow 10%,red 20%);
}
```

### 径向渐变

从起点到终点颜色从内到外进行圆形渐变（从中间向外拉）

```css
.box1{
    background:radial-gradient(#e66465, #9198e5);
}
.box2{
	background:radial-gradient(circle, #e66465, #9198e5);
}
.box3{
    background:radial-gradient(circle closest-side, #e66465, #9198e5);
}
.box4{
    background:radial-gradient(circle at 50%, #e66465, #9198e5);
}
```

尺寸关键字

- `closest-side`最近边
- `farthest-side`最远边
- `closest-corner`最近角
- `farthest-corner`最远角

**拓展：重复渐变**

```css
.box{
    background:repeating-radial-gradient(red 0%, yellow 10%, red 20%);
}
```

### IE渐变

```css
.box{ 		filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='red',endColorstr='yellow',GradientType=0);
}
```

## 文本

```css
p{
    text-shadow:5px 5px 3px yellow; /* 设置水平偏移5px竖直偏移5px模糊半径3px颜色是黄色的文本阴影 */
    text-outline:2px 1px red; /* 设置文本轮廓粗细为2px模糊半径为1px颜色为红色的文本轮廓 */
    word-break:keep-all; /* 规定自动换行的处理方法 */
    word-wrap:break-word; /* 允许长单词或URL地址换行到下一行 */
}
```

**新文本属性**

**`text-align-last`属性**

> 规定如何对齐文本的最后一行

```css
.text1{
    text-align-last:auto;
}
.text2{
    text-align-last:center;
}
.text3{
    text-align-last:justify;
}
```

兼容性：`text-align-last`属性只有IE支持，在Firefox中需要加上其前缀 “`-moz`” ，Chrome50.0.2661.102以上

注意：`text-align-last`属性只有在`text-align`属性设置为 ”`justify`”时才起作用

**`text-overflow`属性**

> 规定当文本溢出包含元素时发生的事情

```css
.text1{
    text-overflow:clip; /* 截断 */
}
.text2{
    text-overflow:ellipsis; /* 省略号 */
}
.text3{
    text-overflow:string; /* 火狐独有 */
}
```

## 字体

`@font-face`字体

CSS3字体模块，把字体文件放在服务器上，读取字体

**字体格式**

- `TrueType(.ttf)`格式

  > `.ttf`字体是Windows和Mac的最常用字体，是一种RAW格式，因此他不为网站优化

- `OpenType(.otf)`格式

  > `.otf`字体被认为是一种原始的字体格式，其内置在`TureType`的基础上，所以也提供了更多的功能

- `Web Open Font Format(.woff)`格式

  > `.woff`字体是Web字体中最佳格式，他是一个开发的`TrueType`/`OpenType`的压缩版本，同时也支持元数据包的分离
  
- `Embedded Open Type(.eot)`格式

  > `.eot`字体是IE专用字体，可以从`TrueType`创建此格式字体

- `SVG(.svg)`格式

  > `.svg`字体是基于`SVG`字体渲染的一种格式

通用书写模板

```css
@font-face{
    font-family:'YourWebFontName';
    src:url("YourWebFontName.eot");
    src:url("YourWebFontName.eot?#iefix")format('embedded-opentype'),
        url("YourWebFontName.ttf")format('turetype'),
        url("YourWebFontName.woff")format('woff'),
        url("YourWebFontName.svg#YourWebFontName")format("svg");
}
```

> 取值说明
>
> `YourWebFontName` ：自定义的字体名称，他将被引用到Web元素中的`font-family`
>
> `source` ：自定义的字体的存放路径，可以是相对路径也可以是绝对路径
>
> `format` ：自定义字体的格式，主要用来帮助浏览器识别

[获取特殊字体](https://www.fontsquirrel.com/tools/webfont-generator)
