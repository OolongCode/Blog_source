---
title: "CSS基础 笔记"
date: 2022-02-07T15:55:42+08:00
draft: false
math: true
tags: ["css","website","note"]
categories: ["develop"]
toc: true
---

# CSS基础

> Cascading Style Sheets 层叠样式表

## 发展历史

1996W3C：CSS1

1998W3C：CSS2

现在W3C：CSS3

## 特点

- CSS简化[[HTML]]相关标签，网页体积小，下载快
- 解决内容与表现相分离的问题
- 更好地维护网页，提高工作效率

## 样式规则

选择器，声明（声明有属性和值构成）_不区分大小写_

> 书写规范：
>
> 书写采用小写书写
>
> 每一个属性占一行

注释：

```css
/*注释*/
```

**使用方法**

- 行内样式表（内联样式表） \[同时加载\]

  ```html
  <h1 style="attribute:value">
      title
  </h1>
  ```

- 内部样式表    \[同时加载\]

  ```html
  <head>
      <style type="text/css">
      *{
          margin:0;
          border:none;
          padding:0;
      }
  	</style>
  </head>
  <body>
      <h1>
          title
      </h1>
  </body>
  ```

- **外部样式表（外联样式表）：**

  创建CSS文件（扩展名是`.css`)、引用CSS样式    \[html加载时，同时加载CSS\]

  ```html
  <head>
      <link rel="stylesheet" type="text/css" href="style.css" />
  </head>
  ```

  > 外部样式表的优势：
  >
  > 1. css与html分离
  > 2. 多个文件可以同时使用一个样式文件
  > 3. 多文件引用同一个css文件，css只需下载一次

- 导入式 (不推荐使用) \[先html后css\]

  ```html
  <style type="text/css">
  	@import "style.css";
      @import url(style.css);
      @import url("style.css");
  </style>
  ```

  

**优先级：**

行内样式>内部样式>导入式样式？外部样式

*注意：内部样式和外部样式根据就近原则*

## 拓展

HTML文档结构是一个文档树

### css继承：从父元素继承属性

好处：

1. 父元素设置样式，子元素可以继承__部分属性__
2. 减少css代码

特点：继承优先级较低，冲突时采用默认

### css层叠：样式叠加效果

- 可以定义多个样式
- 不冲突时，多个样式可层叠为一个
- 冲突时，按不同样式规则优先级来应用样式

### 优先级

选择器优先级

- id选择器>class选择器>标签选择器
- 后面定义的优先级>前面定义的优先级

**css优先级规则**

同一样式表中：

1. 权值相同：就近原则
2. 权值不同
   1. 根据权值来判断CSS样式
   2. 哪种CSS样式权值高，就使用哪种样式

#### 权值

选择器权值

- 标签：1
- class：10
- ID：100
- 通配符`*`：0
- 行内：1000

权值规则

- 统计不同选择器的个数
- 每类选择器的个数乘以相应权值
- 把所有的值相加得出选择器的权值

### 命名

命名规则：

- 采用英文字母、数字以及‘-’和‘_’命名
- 以小写字母开头，不能以数字和‘-’、‘_’开头
- 命名形式：单字、连字符、下划线和驼峰
- 使用意义命名

常用的CSS样式命名

1. 页面结构
   - 页头：header
   - 页面主体：main
   - 页尾：footer
   - 内容：content/container
   - 导航：nav
   - 侧栏：sidebar
   - 栏目：column
   - 页面外围控制：wrapper
   - 左右中：left right center
2. 导航
   - 导航：nav
   - 主导航：mainnav
   - 子导航：subnav
   - 顶导航：topnav
   - 边导航：sidebar
   - 右导航：rightsidebar
   - 左导航：leftsidebar
   - 菜单：menu
   - 子菜单：submenu
   - 标题：title
   - 摘要：summary
3. 功能
   - 标志：logo
   - 广告：banner
   - 登录：login
   - 登录条：loginbar
   - 注册：register
   - 搜索：search
   - 功能区：shop
   - 标题：title

## 使用id&class

- id不要滥用，谨慎使用
- class适当使用
