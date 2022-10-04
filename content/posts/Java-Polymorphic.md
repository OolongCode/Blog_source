---
title: "Java面向对象之多态 笔记"
date: 2022-01-29T19:01:51+08:00
draft: false
math: false
tags: ["java","note"]
categories: ["develop"]
toc: true
---

# Java面向对象之多态

## 方法重写

### 子类和父类同名方法

- 子类和父类同名方法，方法重写

- 前提：需要有继承关系

- 方法重写表现：

  方法名必须相同

  参数列表必须相同

  返回值类型必须相同

  修饰符：范围可以扩大或相同，但是不能缩小 `public` > `protected` >`default`

方法重写

```java
public class Animal
{
    public void eat()
    {
        System.out.println("动物去吃东西");
    }
}
```

```java
public class Cat extends Animal
{
    public void eat()
    {
        System.out.println("猫喜欢吃鱼");
    }
}
```

```java
public class Dog
{
    public void eat()
    {
        System.out.println("狗喜欢吃骨头");
    }
}
```

*不能重写父类的private方法，如果定义的话只是定义了一个新方法，不是方法重写*

#### 运行时多态

- 后期绑定

  如果被调用的方法在编译期无法被确定下来，只能够在程序运行期根据实际的类型绑定相关的方法，这种绑定方式也被称之为后期绑定

- 运行时多态

  方法重写是根据实际的类型决定调用哪个重写的方法，发生在运行期间，也叫做运行时多态

多态

```java
public class Animal
{
    public void eat()
    {
        System.out.println("动物去吃东西");
    }
}
```

```java
public class Cat extends Animal
{
    public void eat()
    {
        System.out.println("猫喜欢吃鱼");
    }
}
```

```java
public class Dog
{
    public void eat()
    {
        System.out.println("狗喜欢吃骨头");
    }
}
```

```java
public class Test
{
    public static void main(String[] args)
    {
        Animal an = new Cat();
        an.eat();
    }
}
```

### 子类和父类static修饰的同名方法

- 子类和父类static修饰的同名方法

  static修饰的方法是静态方法，也叫做类方法使用

  使用private或static或final修饰的变量或者方法，是早期绑定

```java
public class Animal
{
    public static void eat()
    {
        System.out.println("动物在吃");
    }
}
```

```java
public class Cat extends Animal
{
    public static void eat()
    {
        System.out.println("猫吃鱼");
    }
}
```

```java
public class Test
{
    public static void main(String[] args)
    {
        Animal an = new Cat();
        an.eat();
    }
}
```

### 动态绑定和解耦合简介

- 动态绑定

  在运行时根据具体对象的类型进行绑定，也就是后期绑定

- 解耦合简介

  解耦合，字面意思就是解除耦合关系

  设计的核心思想：

  > 尽可能减少代码耦合，如果发现代码耦合，就要采用解耦技术
  >
  > 数据模型，业务逻辑和视图显示三层之间彼此降低耦合

解耦合简介

- 父子关系和夫妻关系的区别

  从编程角度，父子关系是不能拆分的

  从编程角度，夫妻关系是可以拆分的

### 同名变量和方法重写

子类和父类出现同名变量

```java
public class Father
{
    int a = 1;
    static int b = 1;
    public Father()
    {
        a = 10;
        b = 10;
    }
}
```

```java
public class Son extends Father
{
    int a = 2;
    static int b = 2;
    public Son()
    {
        a = 20;
        b = 20;
    }
    public static void main(String[] args)
    {
        Son s = new Son();
        Father f = s;

        System.out.println("f.a = "+f.a+",f.b="+f.b);
        System.out.println("s.a = "+s.a+",s.b="+s.b);
    }
}
```

### 方法重载和方法重写的区别和应用

|    名称    |                 方法重载                 |      方法重写      |
| :--------: | :--------------------------------------: | :----------------: |
|     类     |                  一个类                  |      继承关系      |
|   方法名   | 参数个数不同、参数类型不同、参数顺序不同 |    参数列表相同    |
| 返回值类型 |                 可以不同                 |      必须相同      |
|  调用方式  |                 参数决定                 | 创建的实际对象决定 |
| static修饰 |                是方法重载                |    不是方法重写    |

## 抽象类

### 抽象类



为什么需要抽象类？

> 动物Animal都有自己的行为，小鸟和老虎继承了动物的行为，但小鸟和老虎的行动方式不一样。在动物中能给出行动的具体实现吗？
>
> 抽象类和抽象方法来解决这个问题

什么是抽象类？

> 使用abstract关键字修饰的方法叫做抽象方法，抽象方法没有方法体。当一个类中包含了抽象方法，那么该类也必须使用abstract关键字来修饰，这种使用abstract关键字修饰的类就是抽象类。

抽象类及抽象方法定义的语法格式

```java
[修饰符] abstract class 类名
{
    // 定义抽象方法
    [修饰符] abstract 方法返回值类型 方法名([参数列表]);
    // 其他方法或属性
}
```

### 抽象类的作用

抽象类的作用类似于“模板”，其目的是方便开发人员根据抽象类的格式来修改和创建新类。

抽象类主要用于继承，有利于程序的扩展。

```java
public abstract class Book
{
    public abstract String getAuthor();
}
```

```java
public class ComputerBook extends Book
{
    @Override
    public String getAuthor()
    {
        return "詹姆斯·高斯林 James Gosling";
    }
}
```

```java
public class EnglishBook extends Book
{
    @Override
    public String getAuthor()
    {
        return "Tom";
    }
}
```

#### 抽象类的特点

1. 抽象类不能创建对象，如果创建，编译无法通过而报错，只能创建其非抽象子类的对象。
2. 抽象类中，可以有构造器，是供子类创建对象时，初始化父类成员使用的。
3. 抽象类中，不一定包含抽象方法，但是有抽象方法的类必定是抽象类。
4. 抽象类的子类，必须重写抽象父类中所有的抽象方法，否则子类也必须定义成抽象类，不然编译无法通过而报错。
5. 抽象类中的抽象方法不能用private、final、static修饰
6. 抽象类存在的意义是为了被子类继承，抽象类体现的是模板思想。

## 接口

### 接口的定义

为什么需要接口？

- 可以使用接口解决Java多继承的问题

什么是接口

- 接口就是某个事物对外提供的一些功能的声明
- 可以利用接口实现多态，同时接口也弥补了Java单一继承的弱点
- 使用interface关键字定义接口

### 接口的特性

JDK 1.8之前接口的特性：

- 接口允许多继承
- 接口没有构造方法
- 接口中的属性默认是用public static final修饰
- 接口中的方法默认是用public abstract修饰的
- 接口继承接口用extends，不能implement

JDK 1.8 之后接口的语法：

```java
[修饰符] interface 接口名 [extends 父接口1,父接口2,...]
{
    [public] [static] [final] 常量类型 常量名 = 常量值;
    [public] [abstract] 方法返回值类型 方法名([参数列表]);
    [public] default 方法返回值类型 方法名([参数列表]){
        // 默认方法的方法体
    }
    [public] static 方法返回值类型 方法名([参数列表]){
        // 类方法的方法体
    }
}
```

JDK 1.8之后接口的特性：

- 在接口内部可以定义多个常量和抽象方法，定义常量时必须进行初始化赋值，定义默认方法和静态方法时，可以有方法体。
- 在接口中定义常量时，可以省略“public static final”修饰符，接口会默认为常量添加”public static final“修饰符。与此类似，在接口中定义抽象方法时，也可以省略”public abstract“修饰符，定义default默认方法和static静态方法时，可以省略”public“修饰符，这些修饰符系统都会默认进行添加。

### 接口的作用

- 接口表示一种能力，例如：”做这项工作需要一个钳工/木匠/程序员“

- 接口是一种能力

  - 体现在接口方法上

- 面向接口编程

  ![image-20210717200449346](/images/Java-Polymorphic/image-20210717200449346.png)



### 接口的设计

- 面向接口编程
- 需求：开发打印机
  - 墨盒：彩色、黑白
  - 纸张类型：A4、B5
  - 墨盒和纸张都不是打印机厂商提供的
  - 打印机厂商要兼容市场上的墨盒、纸张
- 结果：
  - 使用黑白墨盒在A4纸上打印
  - 使用彩色墨盒在B5纸上打印
  - 使用彩色墨盒在A4纸上打印

## 接口与抽象类

### 抽象类的特点

何时使用继承？

- 继承与真实世界类似，只要说”猫是哺乳动物“，猫的很多属性、行为就不言自明了。

  **符合is-a关系的设计使用抽象类继承**

- 继承是代码重用的一种方式，将子类共有的属性和行为放到父类中，子类与父类是is-a关系

### 接口的特点

> USB接口本身没有实现任何功能
>
> USB接口规定了数据传输的要求
>
> USB接口可以被多种USB设备实现

可以使用Java接口来实现

- 编写USB接口 根据需求设计方法
- 实现USB接口 实现所有方法
- 使用USB接口 用多态的方式使用

**符合has-is关系的设计使用接口**
