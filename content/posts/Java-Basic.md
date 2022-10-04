---
title: "Java基础语法 笔记"
date: 2022-01-29T16:27:52+08:00
draft: false
math: true
tags: ["java","note"]
categories: ["develop"]
toc: true
---

# Java基础语法

## 概述

**JVM**

- JVM：Java虚拟机，简称JVM
- Java程序的跨平台性的核心是JVM

**JRE和JDK**

- JRE：Java程序运行环境
- JDK：Java程序开发工具包

![image-20200929132634842](/images/Java-basic/image-20200929132634842.png)

## 安装

Java官方网站：http://oracle.com

Java JDK SE8

环境变量：Java_Home

%Java_Home% /bin

> bin目录：
>
> java.exe
>
> javac.exe

## 第一个Java程序

Java是纯面向对象高级编程语言

**开发步骤**

1. 定义类 class  `public class`class名与源文件名一致，且一个文件只能有一个
2. 主方法 main 入口函数
3. 系统输出：编译

![image-20220129124723960](/images/Java-basic/image-20220129124723960.png)



```java
public class HelloWorld 
{ 
  /**
   * public class：公共类名，一个文件只有一个
   * 类名：HelloWorld 与文件名一致
   */
    public static void main(String[] args)
    {
      /**
       * static：静态
       * void：返回值的类型
       * main：方法名，严格定义
       * String：字符串
       * []：数组
       * args：参数名
       */
        Syetem.out.print("1.Hello World!"); // 不换行输出
        System.out.println("2.Hello World"); // 换行输出
        
    }
}
```

## 编码规范

### 标识符

标识符：字符序列

标识符规范：

![image-20210623151855271](/images/Java-basic/image-20210623151855271.png)

### 命名规范

1. 类、接口

   >单个单词：小驼峰命名 `Hello`
   >
   >多个单词：大驼峰命名 `HelloWorld`

2. 变量、方法

   > 单个单词：全小写 `check()`
   >
   > 多个单词：首单词全小写，后面每个单词首字母大写 `checkUserName()` 

3. 常量

   > 单个单词：全大写 `NUMBER`
   >
   > 多个单词：全大写中间下划线 `MAX_VALUE`

### 注释

注释：不进行编译处理 可读性

类型：

- 单行注释

  ```java
  // 单行注释 可嵌套
  ```

- 多行注释（区块注释）

  ```java
  /*
   多行注释 不可嵌套
  */
  ```

- 文档注释 `javadoc`

  ```java
  /**
   * 文档注释 Javadoc生成文档
   */
  ```

`javadoc` 用法：

```shell
javadoc [options] [packagenames] [sourcefiles]
```

## 数据类型和变量

### 变量

存储程序的数据 -> 申请存储空间 -> 变量

| 比喻       | 概念     |
| ---------- | -------- |
| 房间       | 变量     |
| 房间名     | 变量名   |
| 房间类型   | 变量类型 |
| 入住的客人 | 变量值   |



#### 使用步骤

1. 声明变量，即根据数据类型在内存申请空间

   ```java
   // 数据类型 变量名;
   int age;
   ```

   

2. 赋值，即将数据存储至对应的内存空间

   ```java
   // 变量名 = 数值;
   age = 20
   ```

3. 使用变量

>1 和 2 可进行合并

变量名：标识符 -> 遵循规范

#### 变量类型

- 局部：某方法或代码块
- 全局：类的属性
- 静态：`static`修饰，整个类成员共享

作用域：全局 局部

### 数据类型

数据类型 –> 分类数据

![image-20220129130446200](/images/Java-basic/image-20220129130446200.png)

#### 基本数据类型

**整型**

| 整型  | bit  | 范围             |
| ----- | ---- | ---------------- |
| byte  | 8    | -128~127         |
| short | 16   | -32768~32767     |
| int   | 32   | -2^31^ ~ 2^31^-1 |
| long  | 64   | -2^63^ ~ 2^63^-1 |

整型数据存在有符号为类进行补码：

| bit  | 符号 |
| ---- | ---- |
| 0    | 正数 |
| 1    | 负数 |



**浮点型**

| 浮点型             | bit  |
| ------------------ | ---- |
| float              | 32   |
| double（默认类型） | 64   |

```java
float a = 3.14f;
double b = 3.14;
```

**布尔类型**

只有两个值 `true`和`false`

```java
boolean flag1 = true;
boolean flag2 = false;
if (flag1)
{
    System.out.println("flag1 is true");
}
else if(flag2)
{
    System.out.println("flag2 is true");
}
```

**字符型**

编码：二进制类型映射到字符

编码表：如何编码字符

![image-20210623162225439](/images/Java-basic/image-20210623162225439.png)

ASCII码：编码美国字符（8位） 7位二进制 剩下一位为0

Unicode码：万国码（16位） 65536个字符

`char` 2字节 [0, 65536]

```java
char c = "中" ;
int i = c;
// 数据类型自动转换
char c = 20013;
```

##### 数据类型转换

大范围 -> 小范围 强制转换

小范围 -> 大范围 自动转换

强制转换语法：

```java
// 目标数据类型 变量名 = （目标数据类型）值/变量
int c = 5;
short s = (short) c;
```

![image-20220129132435327](/images/Java-basic/image-20220129132435327.png)

#### 引用类型

在C和C++通过指针操作内存中的元素，Java使用“引用”，java中一切都被视为对象

引用数据类型：

- 数组
- 接口
- 对象

引用类型在内存中的存储方式：

![image-20210623202017056](/images/Java-basic/image-20210623202017056.png)

引用类型`==`比的是值，即内存地址

```java
String s1 = new String("abc");
String s2 = new String("abc");
System.out.println(s1 == s2);
```

运行结果：

```bash
false
```

## 运算符

简介：对常量或变量进行操作的符号

表达式：运算符和操作数的组合
$$
\begin{equation}
\underbrace{Y = X * \overbrace{(2+10)}^{子表达式}}_{表达式}
\end{equation}
$$
分类：

![image-20220129143503745](/images/Java-basic/image-20220129143503745.png)

![image-20220129143659794](/images/Java-basic/image-20220129143659794.png)



### 赋值运算符 

自右向左运行

```java
int a,b,c;
a = b = c = 1; 
```

### 算术运算符 

```java
int a = 5;
int b = 2;
double c = 4.00;
double m = 20.00;
System.out.println(a+b);
System.out.println(a-b);
System.out.println(a*b);
System.out.println(a/b); // 值为 int
System.out.println(a%b);

System.out.println(a/c); // 值为 float
System.out.println(m/b); // 值为 float
```

复合运算符：赋值运算符+算术运算符 （自左向右） 效率高于算术运算符

```java
int a = 1;
a += 2;
a -= 1;
a *= 10;
a /= 2;
a %= 26;

a += a += 6 // 等价于 a = a+(a+6)
```

自增/自减运算

前缀运算“先运算后使用”

```java
int a = 5;
int b = ++a;
System.out.println("a="+a);
System.out.println("b="+b);
```

后缀运算“先使用后运算”

```java
int a = 5;
int b = a++;
System.out.println("a="+a);
System.out.println("b="+b);
```

### 关系运算符

运算结果为`boolean`类型 `true`或`false`

```java
int a = 10;
int b = 15;
System.out.println(a >= b);
System.out.println(a <= b);
System.out.println(a == b); // 引用类型不能使用 == 进行比较
System.out.println(a > b);
System.out.println(a < b);
```



### 逻辑运算符

逻辑运算符也叫短路运算符 -> 复杂的逻辑表达式 -> boolean类型

```java
boolean a = true;
boolean b = false;
a && b; // false
a || b; // true
!a; // false
```

### 三目运算符

语法：布尔表达式？表达式1：表达式2

![image-20220129144526912](/images/Java-basic/image-20220129144526912.png)

### instanceof运算符

判断类的实例是否属于该类

引用实例-> 属于instanceof -> 引用数据类型



### 位运算符

C语言的低级操作 -> 操作二进制位

| 位操作符 | 含义       | 说明                               |
| -------- | ---------- | ---------------------------------- |
| &        | 与         |                                    |
| \|       | 或         |                                    |
| ^        | 异或       |                                    |
| <<       | 左移       | 高位丢弃，低位补0                  |
| >>       | 右移       | 低位丢弃，高位补1或0，根据计算结果 |
| >>>      | 无符号右移 | 忽略符号位，高位补0                |



---

运算符优先级：

| 顺序 | 运算符                       |
| ---- | ---------------------------- |
| 1    | `()` `[]`                    |
| 2    | `++` `--` `!`                |
| 3    | `*` `/` `%` `+` `-`          |
| 4    | `>` `>=` `<` `<=` `==` `!=`  |
| 5    | `&&` `||`                    |
| 6    | `?:` `=` `*=` `/=` `+=` `-=` |

## 逻辑流程控制语句

### main函数和顺序结构

![image-20220129161200091](/images/Java-basic/image-20220129161200091.png)

默认运行 public class -> main (可以传入参数)

```java
public static void main(String[] args){}
```

顺序结构 自上而下运行

### 选择结构



![image-20210624094807166](/images/Java-basic/image-20210624094807166.png)

if语句：

```java
// if形式
if(expr)
{
    code block
}
// if-else形式
if(expr)
{
    code block 1   
}else
{
    code block 2
}
// if-else if-else形式 多条件分支
if(expr1)
{
    code block 1
}
else if(expr2)
{
    code block 2
}
else
{
	code block 3    
}
```

switch语句：

```java
switch(expr){
    case value1:
        codeBlock1;
        break;
    case value2:
        codeBlock2;
        break;
    default:codeBlockn;
}
```

> JDK 1.5 以前 expr: `byte` `short` `int` `char`
>
> JDK 1.5 支持 `enmu`
>
> JDK 1.7 支持 `String`

![image-20220129162518022](/images/Java-basic/image-20220129162518022.png)

if条件分支 可以进行嵌套，灵活性更高

switch-case条件分支 不能嵌套：

- 效率更高
- 批处理
- 可读性更高

### 循环结构

![image-20210624094747664](/images/Java-basic/image-20210624094747664.png)

while循环结构：

```java
while(expr){
    Loop body
}
```

![image-20210624094719748](/images/Java-basic/image-20210624094719748.png)

do-while循环结构：

```java
do
{
    Loop body
}while(expr)
```

for循环结构：

循环最清晰，for循环，预定次数执行语句

```java
int result = 0;
for(int i = 1;i<=100;i++)
{
    result += i;
}
System.out.println(result);

for(;;)
{
    //死循环 for三个语句都去掉依然可以执行
}
```

### 循环控制

`break` 结束当前循环，语句块

`continue` 结束本次循环，进入下次循环

`return` 方法的结束，返回指定类型的值，也可以是对象

```java
return; // 无 void
return i; // 有
```

### 循环嵌套

```java
for(int i=1;i<=9;i++)
{
    for(int j=1;j<=i;j++)
    {
        System.out.print(j+"*"+i+"="+(j*i)+" ");
    }
    System.out.println();
}
```

