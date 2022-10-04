---
title: "Java面向对象之继承 笔记"
date: 2022-01-29T18:54:26+08:00
draft: false
math: true
tags: ["java","note"]
categories: ["develop"]
toc: true
---

# Java面向对象之继承

## 类的继承机制

### 继承的的作用

- 继承的作用：减少重复的冗余的相同属性和方法

- 多个类中存在相同属性和行为时，将这些内容抽取到单独一个类中

- 那么多个类无需再定义这些相同属性和行为，只要继承那个类即可

```java
public class Person
{
    // 姓名
    private String name;
    // 年龄
    private int age;

    public void setName(String name)
    {
        this.name = name;
    }

    public void setAge(int age)
    {
        this.age = age;
    }

    public void eat()
    {
        System.out.println("吃饭");
    }
}
```

```java
public class Student extends Person
{
}
```

### 子类和父类的继承机制

- 继承关系是两个类，一个为子类（派生类），一个父类（基类）。
- 子类继承父类，使用关键字extends来表示
- extends的意思是“扩展”，子类是对父类的扩展
- java中类只有单继承，没有多继承（一个儿子只有一个直接的爸爸，但是爸爸可以有多个儿子）

### Java的单继承

- Java不支持多继承，只允许一个类直接继承另一个类
- 子类只能有一个父类，extends关键字后面只能有一个类名

### Java继承的顶级父类：Object类简介

- Object类是Java中所有类的始祖
- Java中的每一个类都是由它扩展而来，但是并不需要明确写出要继承它
- 自然的，所有Java类都拥有了其方法

`toString()`方法

> 该方法用来返回对象的字符串表示形式

```java
public class Person
{
    // 姓名
    private String name;
    // 年龄
    private int age;

    public String toString()
    {
        return "Person["+name+","+age+"]";
    }

    public static void main(String[] args)
    {
        Person p = new Person();
        p.name = "张五";
        p.age = 20;
        System.out.println(p.toString());
    }
}
```

`equals(Object obj)`方法

> 该方法用来判断两个对象是否相同
>
> 如果没有被重写过，其与==等价

```java
public class PersonOne
{
    public static void main(String[] args)
    {
        PersonOne p1 = new PersonOne();
        PersonOne p2 = new PersonOne();
        System.out.println(p1.equals(p2));
    }
}
```

`hashCode()`方法

> Object类的hashCode方法是返回对象存储地址

```java
public class PersonOne
{
    public static void main(String[] args)
    {
        PersonOne p1 = new PersonOne();
        System.out.println(p1.hashCode());

        PersonOne p2 = new PersonOne();
        System.out.println(p2.hashCode());
    }
}
```

### 对象向上转型

>  引用类型的转换

- 子类转换成为父类，向上转型
- 格式：父类名称 对象名称 = new 子类名称();
- 含义：把创建的子类对象当做父类看待和使用

```java
public class Person
{
    public void run()
    {
        System.out.println("Person Run");
    }
}
```

```java
public class Student extends Person
{
    public  void eat()
    {
        System.out.println("Son eat");
    }
}
```

```java
public class Application
{
    public static void main(String[] args)
    {
        // person(父类引用)可以指向子类
        // 向上转型
        Person s2 = new Student();
        s2.run();
        // 但不能调用子类特有的方法
        //s2.eat();
    }
}
```



### 对象向下转型

> 引用类型转换

- 父类转换为子类，向下转型
- 子类 引用 = （子类）父类对象
- 强制类型转换

```java
public class Person
{
    public void run()
    {
        System.out.println("Person Run");
    }
}
```

```java
public class Student extends Person
{
    public  void eat()
    {
        System.out.println("Son eat");
    }
}
```

```java
public class Test
{
    public static void main(String[] args)
    {
        Person p = new Student();
        // 报错
        // p.eat();
        
        // 向下转型
        Student s = (Student) p;
        s.eat();
    }
}
```

对象向下转型注意：

问题：可能会出现ClassCastException异常

```java
public class Person
{
    public void run()
    {
        System.out.println("Person Run");
    }
}
```

```java
public class Student extends Person
{
    public  void eat()
    {
        System.out.println("Son eat");
    }
}
```

```java
package TestDemo;

public class Test1
{
    public static void main(String[] args)
    {
        Person p = new Person();
        Student s = (Student) p;
        s.eat();
    }
}
```

## super关键字

### super在继承构造函数中定义和作用

- 在继承中子类的构造函数必须依赖父类提供的构造函数
- super（参数列表）访问父类的构造函数
- super调用父类的构造函数，必须在构造函数的第一行

> 如果父类的构造函数是无参构造函数，可以默认不写super关键字

- 在继承中子类的构造函数必须依赖父类提供的构造函数

```java
public class Person
{
    private String name;
    private int age;
    public Person(String name, int age)
    {

    }
}
```

```java
public class Student extends Person
{
    public Student()
    {
        // 1. 如果父类提供的只有有参数的构造函数，子类的构造必须依赖父类提供的现有构造函数
        // 2. super(参数列表)去访问父类的提供的构造函数，必须明确写出参数
        // 3. super必须在第一行
        super("小张", 20);
    }
}
```

### super访问父类的属性

- 在子类的方法或构造器中，通过使用super属性
- 特殊情况：当子类和父类中定义了同名的属性时，想调用父类中声明的属性，就需要通过super.属性的方式来声明调用的是父类中声明的属性

```java
public class Person
{
    // 身份证号
    int id = 1001;
}
```

```java
public class Student extends Person
{
    // 学生证号
    int id = 80;
    
    public void show()
    {
        System.out.println("身份证号是"+super.id+"，学生证号是"+id);
    }
}
```

```java
package super_Demo;

public class Test
{
    public static void main(String[] args)
    {
        Student s = new Student();
        s.show();
    }
}
```

### super访问父类的方法

在子类的方法或构造器中，通过使用super.方法名

```java
public class Person
{
    public void eat()
    {
        System.out.println("人都要吃饭");
    }
}
```

```java
public class Student extends Person
{
    public Student()
    {
        // 访问父类的方法
        super.eat();
    }
    public static void main(String[] args)
    {
        Student s = new Student();
    }
}
```

### super和this区别

- super()调用父类的构造函数，必须在构造函数的第一行
- this()调用本类的构造函数，必须在构造函数的第一行
- super()和this()不能同时调用构造函数

代表对象不同

- this：本身调用者这个对象
- super：代表父类对象的引用

前置

- this：没有继承也可以使用
- super：只能在继承条件下才能使用

构造方法

- this()：本类的构造
- super()：父类的构造

## final关键字

### final关键字修饰变量

- final关键字修饰的变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能修改
- final关键字修饰的变量，如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象
- final修饰的变量都是常量
- final可以修饰局部变量

![image-20210713083547227](/images/Java-Extends/image-20210713083547227.png)

### final关键字修饰方法

- final修饰的成员方法不能被子类重写

  当父类的方法为final时，子类不能与父类有方法名、参数类型、参数个数及参数顺序都一样的方法

![image-20210713085130171](/images/Java-Extends/image-20210713085130171.png)



![image-20210713085145406](/images/Java-Extends/image-20210713085145406.png)

- 父类方法为private修饰符的final方法

```java
public class Person
{
    private final void show()
    {
        System.out.println("Person:show");
    }
}
```

```java
public class Student extends Person
{
    public final void show()
    {
        System.out.println("Student:show");
    }
}
```

```java
public class Test
{
    public static void main(String[] args)
    {
        Person person = new Person();
        Student stu = new Student();

        //private的方法仅本类可见，该方法不可见
        //person.show();

        stu.show();
    }
}
```

### final修饰类

- final修饰的类不能被子类继承
- final类中的成员方法也默认为final
- final类中的变量值是可以改变的

![image-20210713091825631](/images/Java-Extends/image-20210713091825631.png)

![image-20210713091741790](/images/Java-Extends/image-20210713091741790.png)
