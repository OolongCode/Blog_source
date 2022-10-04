---
title: "CSS进阶 笔记"
date: 2022-02-07T15:55:47+08:00
draft: false
math: true
tags: ["css","website","note"]
categories: ["develop"]
toc: true
---

# CSS进阶

## 盒子模型

> 盒子模型用来“放”网页中的各种元素
>
> 网页设计中内容，如文字、图片等元素， 都可是盒子（div嵌套）

生活中的盒子：

- padding内填充
- border边框
- margin外边距

物体content内容：

- width宽
- height高

### 属性

```css
div{
    width:80%;
    max-width:1000px;
    min-width:300px;
    height:auto;
    max-height:1000px;
    min-height:300px;
    border-width:medium;
    border-color:red;
    border-style:solid;
   	/* 内边距的属性值不能为负值 */
    padding:20px; /* 4个方向都是20px */
    padding:20px 40px; /* 上下=20px 左右=40px */
    padding:20px 40px 10px; /* 上=20px 左右=40px 下=10px */
    padding:20px 30px 10px 40px; /* 上=20px 右=30px 下=10px 左=40px */
    
    margin:20px; 
    margin:20px 40px;
    margin:20px 40px 10px;
    margin:20px 30px 10px 40px;
	/* 
    垂直方向，两个相邻元素都设置外边距，外边距会发生合并
    合并后外边距高度=两个发生合并外边距的高度中最大值
    */
}
```

**宽高属性总结**

1. width和height属性设置是内容的高和宽
2. width和height属性设置对块级元素和替换元素有效
3. max-height(width)/min-height(width)有兼容问题，IE不支持

*盒子模型的属性值均有四个方向： `top` `bottom` `left` `right`*

**盒子模型属性值简写**

```css
div{
    margin:20px 30px 10px 40px;
    padding:20px 30px 10px 40px;
    border:2px red solid;
    height:auto;
    width:auto;
}
```



### 计算

标准盒子模型：

- 宽度=左边距+左边框+左填充+内容宽度+右填充+右边框+右边距
- 高度=上边距+上边框+上填充+内容高度+下填充+下边框+下边距

`doctype`声明 –> 标准盒子模型

### display属性

```css
div{
    display:inline;
}
span{
    display:block;
}
img{
    display:inline-block;
}
ul{
    display:none;
}
```

**样式继承关系**

![image-20210109144624068](/images/CSS-up/image-20210109144624068.png)

## 定位

> 定位机制
>
> - 普通流（标准流）：默认状态，元素自动从左往右，从上往下的排列
> - 浮动
> - 绝对定位

### 浮动

基础知识

- 会使元素向左或向右移动，只能左右，不能上下
- 浮动元素碰到包含框或另一浮动框，浮动停止
- 浮动元素之后的元素将围绕它，之前的不受影响
- 浮动元素会脱离标准流

**语法**

```css
.box_1{
    float:left;
}
.box_2{
    float:right;
}
.box_3{
    float:none;
}
```

*不会脱离文本流，会脱离文档流（普通流）*

**问题：** 浮动塌陷，浮动溢出

#### 清除浮动

```css
.box{
    clear:both;
}
```

*设置了float的元素会影响其他相邻元素，需要使用clear清除浮动，clear只会影响自身，不会对其他相邻元素造成影响*

**常用方法**

1. 在浮动元素后使用一个空元素加clear

2. 给浮动元素的容器添加`overflow:hidden`

3. 使用CSS3的`:after`[[CSS3-basic#伪元素|伪元素]]

   ```css
   .clearfix:after{
       content:"";
       display:block;
       height:0;
       visibility:hidden;
       clear:both;
   }
   .clearfix{
       zoom:1;/* 触发hasLayout兼容IE6、7 */
   }
   ```

   **其他方法**

   1. 父级元素定义`height`。只适合高度固定的布局。
   2. 父级元素也一起浮动。不推荐，会产生新的浮动问题。

#### 浮动总结

- 会使元素左右移动
- 浮动元素会脱离普通流
- 元素浮动后具备`inline-block`属性

清除方法推荐

1. 使用CSS3`:after`伪元素清除浮动
2. 使用`overflow:hidden`清除浮动

### 绝对定位

position模块

Positioned Layout Module

提供与**元素定位**和**层叠相关**功能，它是核心模块

> 1. 盒子模型的类型和尺寸
> 2. 布局模型
> 3. 元素之间的关系
> 4. 视口大小、图像大小等其他相关方面

*小知识点*

> Document tree 文档树
>
> normal-flow 自然顺序
>
> containing-block 容器

#### 定位模型

- 自然模型`static`

  > 静态定位/常规定位/自然定位 —— 定位中的一股清流-回归本真

  ![image-20210110154409258](/images/CSS-up/image-20210110154409258.png)

- 相对定位模型`relative`

  ![image-20210110154535105](/images/CSS-up/image-20210110154535105.png)

- 绝对定位模型`absoluate`

  ![image-20210110154643565](/images/CSS-up/image-20210110154643565.png)

- 固定定位模型`fixed`

  ![image-20210110154937232](/images/CSS-up/image-20210110154937232.png)

- 磁贴定位模型`sticky`

  > 磁贴定位/粘性定位/吸附定位 ——赛季新秀 实力布局糖

  ![image-20210110155151738](/images/CSS-up/image-20210110155151738.png)

#### 定位总结

![image-20210110155244861](/images/CSS-up/image-20210110155244861.png)

## 布局

认识布局

- 以最合适浏览的方式将图片和文字排放在页面的不同位置
- 布局模式有多种，不同的制作者有不同的布局设计

==前端工程师 \= 技术 + 艺术==

**经典行布局**

- 基础的行布局
- 行布局自适应限制最大宽
- 行布局垂直水平居中

**经典的列布局**

- 两列布局固定
- 两列布局自适应
- 三列布局固定
- 三列布局自适应

**混合布局**

- 混合布局固定
- 混合布局自适应

### 圣杯布局

> 由来
>
> 圣杯布局是由国外的Kevin Cornell 提出的一个布局模型概念
>
> 在国内由淘宝UED的工程师传播开来

布局要求

- 三列布局，中间宽度自适应，两边定宽
- 中间栏要在浏览器中优先展示渲染
- 允许任意列高度最高
- 用最简单的CSS、最少的HACK语句

```html
<div class=" container" >
    <div>
        中间
    </div>
    <div>
        左侧
    </div>
    <div>
        右侧
    </div>
</div>
```



### 双飞翼布局

- 经淘宝UED的工程师针对圣杯布局改良后得出双飞翼布局
- 去掉相对布局，只需要浮动和负边距

```html
<div class="main" >
    <div>
        中间
    </div>
</div>
<div>
    左侧
</div>
<div>
    右侧
</div>
```
