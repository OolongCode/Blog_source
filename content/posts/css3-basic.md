---
title: "CSS3基础 笔记"
date: 2022-02-07T16:17:53+08:00
draft: false
math: true
tags: ["css","website","note"]
categories: ["develop"]
toc: true
---

# CSS3基础

## 选择器

### 基础选择器

``` css
section > div{ /* 子元素选择器 */
    color:#ffffff;
}
.brother + div{ /* 相邻兄弟元素选择器 */
    color:#ff0000;
}
.brother ~ div{ /* 通用兄弟元素选择器 */
    color:#00ff00;
}
div, p, section, .brother, #id{ /* 群组选择器 */
    color:#0000ff;
}
```

### 属性选择器

```css
a[href]{
    color:#000000;
}
a[href="http://www.baidu.com"]{ /* 元素属性值 */
    color:#666666;
}

input[value~="password"]{ /* 元素包含指定属性值 */
    color:#333333;
}
input[value^="pass"]{ /* 元素开头属性值 */
    color:#223333;
}
input[value$="rd"]{ /* 元素结尾属性值 */
    color:#222233;
}
input[value*="word"]{ /* 元素属性值包含 */
    color:#222222;
}
input[name|="use"]{ /* 元素属性值-开头 */
	color:#454545;
}
```

### 伪类选择器

#### 动态伪类

> 不存在于HTML中，只有当用户和网站交互的时候才能体现出来

```css
a:link{
    color:red;
}
a:visited{
    color:green;
}
a:hover{
    color:blue;
}
a:action{
    color:white;
}
input:focus{
    border:green 2px solid;
}
```

#### UI元素状态伪类

```css
input:enabled{ /* 设置可编辑状态样式 */
    background-color:#ffff00;
}
input:disabled{ /* 设置不可编辑状态样式 */
    background-color:#dddddd; 
}
input:checked{
    background-color:#ff0000; /* 设置多选框选中颜色，只兼容Opera浏览器 */
}
```

#### 结构类选择器`:nth`选择器

```css
div:first-child{ /* 相对父元素首个子元素 */
    background-color:#000000;
}
div:last-child{ /* 相对父元素最后一个子元素 */
    backgroud-color:#880000;
}
/* 相对于父元素的第n个子元素 */
div:nth-child(2n-1){
    background-color:#008800;
}
div:nth-child(1){
    background-color:#000088;
}
div:nth-child(odd){
	background-color:#800000;
}
/* 相对于父元素倒数第n个子元素 */
div:nth-last-child(2n-1){
    background-color:#080000;
}
/* 相对于父元素特定类型的第n个子元素 */
div:nth-of-type(2n-1){
    background-color:#008000;
}
/* 相对于父元素特定类型的倒数第n个子元素 */
div:nth-last-of-type(2n-1){
    background-color:#000800;
}
/* 相对于父元素特定类型的第一个子元素 */
div:first-of-type{
    background-color:#000080;
}
/* 相对于父元素特定类型的最后一个子元素 */
div:last-of-type{
    background-color:#000008;
}
/* 相对于父元素唯一子元素 */
div:only-child{
    background-color:#808000;
}
/* 相对于父元素唯一特定类型的子元素 */
div:only-of-type{
    background-color:#800800;
}
/* 匹配没有子元素（包括文本节点）的每一个元素 */
div:empty{
    background-color:#800080;
}
```

#### 否定选择器（：not）

``` css
 /* 匹配非指定元素/选择器的每个元素 */ 
section:not(div){
    background-color:#008888;
}
section:not(.child){
    background-color:#888800;
}
```



## 伪类和伪元素

> 伪元素：只是一个选择器，不存在的一种元素
>
> 伪类：实际存在的一种元素

### 伪元素

CSS伪元素用于向某些选择器设置特殊效果

语法格式：`元素::伪元素`

```css
/* 类型 */
div::first-line{ /* 对元素的第一行文本进行格式化， 只用于块级元素 */
    font-size:24px;
}
div::first-letter{ /* 对元素的第一个字符进行格式化， 只用于块级元素 */
    font-size:32px;
}

div::before{ /* 在元素的内容前面插入新内容，常用"content"配合使用 */
    content:"";
    display:block;
    width:100px;
    height:100px;
    background-color:#ff0000;
}
div::after{ /* 在元素的内容后面插入新内容，常用"content"配合使用。多用于清除浮动 */
    content:"";
    clear:both;
    width:100px;
    height:100px;
    background-color:#00ff00;
}
::selection{ /* 用于设置在浏览器中选中文本后的背景色与前景色 */
    color:#ff0000;
    background-color:#00ff00;
}
```

`::after`和`::before`特点总结

1. 行级元素
2. 最后/第一个子元素
3. 内容通过`content`写入
4. 支持一切css属性
5. 标签里找不到对应标签

## CSS权重

> 当很多的规则被应用到某一个元素上时，权重是一个决定哪种规则生效，或者是优先级的过程

**权重等级与[[CSS-basic#权值|权值]]**

行内样式(1000)>ID选择器(100)>类、属性选择器和伪类选择器(10)>元素和伪元素(1)>`*`(0)

**权重计算口诀**

从0开始，一个行内样式+1000，一个id+100，一个属性选择器、class或者伪类+10，一个元素名或者伪元素+1

### CSS权重规则

包含更高权重选择器的一条规则拥有更高的权重

ID选择器(`#idValue`)的权重比属性选择器(`[id="idValue"]`)高

带有上下文关系的选择器比单纯的元素选择器权重要高

与元素 “挨得近” 的规则生效

> 最后定义的这条规则覆盖上面与之冲突的规则
>
> 无论多少给元素组成的选择器，都没有一个class选择器权重高
>
> 通配符选择器权重是0，被继承的css属性也带有权重，权重也是0

