---
title: "HTML基础语法 笔记"
date: 2022-02-03T16:38:40+08:00
draft: false
math: true
tags: ["html","website","note"]
categories: ["develop"]
toc: true
---

# HTML基础
> Hypertext Markup Language
> 超文本标记语言

## 概述
**发展史**
1993-2014 W3C

**概念**
头部信息：网页不展示
网页内容：网页展示
标签：储存文本 成对出现
元素：标签+内容+标签
声明：`<!DOCTYPE html`
编码：`<meta/>`

**特点**
1. 不需要编译
2. 文本文件
3. html或htm为文件名后缀
4. 大小写不敏感

**语法**
```html
<tagName attributeName1="attributeValue" attributeName2="attributeValue" ...>...</tagName>
```

特殊符号采用实体符表示，即&...

标签使用：网页内容和整体分析得出**(语义化)**

网页调试：F12

*路径*
> 相对路径：相对于html文件
> 绝对路径：盘符写


## 标签

**基本**
	标题：h1~h6
	段落：p	
	辅助格式：br hr pre
	修饰：i em   b  stronge  sup  sub
	
**常用**
- 图片
	```html
	<img src="img" alt="image" width="400px" height="50%"/>
	```
- 列表
	```html
	<ul>
		<li>1</li>
		<li>2</li>
		<li>3</li>
	</ul>
	<ol>
		<li>1</li>
		<li>2</li>
		<li>3</li>
	</ol>
	<dl>
		<dt>item</dt>
		<dd>describe</dd>
	</dl>
	```
- 超链接
   ```html
   <a href="link" target="windowMethod" title="tips" name="name"> Link</a>
   <!--link: file externalLink blank(refresh) -->
   
   <a mailto="mailtoName">mail</a><!--mail link-->
   <!--file download-->
   
   <a href="#bttom" name="#top">top</a>
   <a href="#top" name="#bottom">bottom</a>
   <!-- anchor link-->
   ```
## 表格

```html
<table width="400px" bgcolor="blue" border="1px" cellpadding="2px" cellspacing="3px" frame="" rules="">
    <caption>Title</caption>
    <!--
		1.同时使用同时操作
		2.不影响表格布局
		3.结构化加载
	-->
    <thead> 
    	<tr align="center" valign="center" bgcolor="red">
        	<th height="20px" bgcolor="red">head1</th>
            <th>head2</th>
            <th>head3</th>
            <th>Itable</th>
        </tr>
    </thead>
    <tbody>
    	<tr>
        	<td bgcolor="green" height="20px">body1</td>
            <td>body2</td>
            <td>body3</td>
        	<td rowspan="2">
                <table>
                    <tr>
                    	<td>1</td>
                        <td>2</td>
                    </tr>
                    <tr>
                    	<td>3</td>
                        <td>4</td>
                    </tr>
                </table>
            </td>
        </tr>
        <tr>
            <td>body4</td>
            <td>body5</td>
            <td>body6</td>
        </tr>
    </tbody>
    <tfoot>
    	<tr>
        	<td>footer1</td>
            <td>footer2</td>
            <td colspan="2">footer3</td>
        </tr>
    </tfoot>
</table>
```



## 表单

```html
<form>
    <input type="text" placeholder="tipsText" maxlength="20px" name="text" size="10px" value="initValue"/>
    <input type="button" value="button"/>
    <input type="submit" value="submit"/>
    <input type="reset" value="reset"/>
    <input type="image" src="srcImage" alt="submit"/>
    <input type="hidden"/>
    <input type="file"/>
    <!--选择域-->
    <input type="radio" name="sex" value="female"/>female
    <input type="radio" name="sex" value="male" checked/>male
    
    <input type="checkbox" name="love" value="Math"/>Math<br/>
    <input type="checkbox" name="love" value="English"/>English<br/>    	<input type="checkbox" name="love" value="Biology"/>Biology<br/>
    
    <select name="color" size="" multiple="">
        <optgroup label="light">
			<option value="red" select="selected">
            	Red
            </option>
            <option value="yellow">
            	Yellow
            </option>
            <option value="orange">
            	Orange
            </option>
        </optgroup>
        <optgroup label="dark">
        	<option value="blue">
        		Blue
        	</option>
            <option value="green">
            	Green
            </option>
            <option value="purple">
            	Purple
            </option>
        </optgroup>
    </select>
    
    <textarea name="textarea" placeholder="more text input" cols="view width" rows="view hight"></textarea>
    
    <!--value值是传给服务器的值-->
</form>
```

***
