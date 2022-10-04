---
title: "Java 面向对象之类和对象 笔记"
date: 2022-01-29T18:47:56+08:00
draft: false
math: false
tags: ["java","note"]
categories: ["develop"]
toc: true
---

# Java 面向对象之类和对象

## 面向对象与面向过程

### 面向过程

- 面向过程编程就是分析出解决问题的步骤
- 然后使用函数把这些步骤一步步实现
- 重心放在完成的每个过程上

### 面向对象

- 构成问题事务分解成各个对象
- 描述某个事物在整个解决问题的步骤中的行为

### 面向过程和面向对象区别

- 面向过程就是步骤，就是解决问题按部就班
- 面向对象关注的解决问题所需要的对象
- 面向过程就是自己办事，面向对象就是托人办事

![image-20210708193911362](/images/Java-Object/image-20210708193911362.png)

## 现实世界的面向对象

类和对象：

- 类(class)是抽象的
- 对象(object)是具体的

 汽车类(class) –new–>实例(instance)

## Java的类和对象

### Java的类

- 类可以看作是一个模板
- 用于描述一类对象的行为和状态

### Java的类的描述

```java
public class Person
{
    // 姓名
    String name;
    // 年龄
    int age;
    // 身高
    int height;
    // 唱歌
    void sing()
    {   
    }
    // 跳舞
    void dance()
    {
    }
}
```

### Java的对象

- 万物皆对象
- 对象是具体的物体
- 拥有属性
- 拥有行为
- 把很多零散的构建成一个整体
- 具有唯一性

### 类和创建对象

```java
public class Person
{
    // 姓名
    String name;
    // 年龄
    int age;
    // 身高
    int height;
    public static void main(String[] args)
    {
        Person p = new Person();
        p.name = "小鸿蒙";
        p.age = 2;
        p.height = 8848;
        System.out.println("这个人的名字是："+p.name+"，年龄是"+p.age+"，身高是："+p.height);
    }
}
```

类名 对象名=new 类名();

```
Person p = new Person();
```

### 对象与new关键字

- new关键字表示创建一个对象
- new关键字表示实例化对象
- new关键字表示申请内存空间

`Person person = null;`

![image-20210709075102873](/images/Java-Object/image-20210709075102873.png)

`Person person = new Person();`

![image-20210709075214789](/images/Java-Object/image-20210709075214789.png)

```java
Person person = new Person();
person.name = "小张";
person.age = 20;
```

![image-20210709081317483](/images/Java-Object/image-20210709081317483.png)

### Java的内存中创建多个对象

![image-20210709081600071](/images/Java-Object/image-20210709081600071.png)

### 类与对象的关系

类的作用 — 产生出具体的对象

对象 – 抽象 – 类 – 实例化 – 对象

## Java对象的组成

### 实例变量和静态变量

#### 实例变量

```java
public class Person
{
    // 姓名
    String name;
    // 年龄
    int age;
    // 身高
    int height;
    public static void main(String[] args)
    {
        Person p = new Person();
        p.name = "小鸿蒙";
        p.age = 2;
        p.height = 8848;
        System.out.println("这个人的名字是"+p.name+"，年龄是"+p.age+"，身高是"+p.height);
    }
}
```

#### 静态变量

- 独立于方法之外的变量，用`static`修饰，静态变量，也可以叫做类变量
- static不能修饰局部变量

### 构造函数

- java构造函数，也叫构造方法，是java中一种特殊的函数
- 构造函数没有返回类型，函数名和类名保持一致
- new对象产生后，就调用了对象的属性和方法
- 作用：一般用来初始化成员属性和成员方法

格式：

1. 修饰符 类名（参数列表）{ }
2. 直接类名 （参数列表）{ }

默认无参构造函数和有参构造函数

```java
// 无参构造函数
public class Employee
{
    public Employee()
    {
        
    }
}
// 有参构造函数
public class Employee
{
    public Employee(String name, int age)
    {
        
    }
}
```

构造函数可以有return关键字，但是不能有具体的返回值类型

构造函数

- 构造函数不是手动调用的，是对象被创建的时候jvm调用
- 如果一个类没有定义构造方法，jvm在编译的时候会给这个类默认添加一个无参构造方法
- 如果定义了构造方法，那么jvm不会再创建无参构造方法
- 创建对象的时候，有几个参数，就要有相应的构造方法，也是对应的要有几个参数
- 构造函数可以调用构造函数

构造函数的作用

```java
public class Employee
{
    String name;
    int age;
    public Employee() // 构造函数的作用是给类中的属性赋值初始化
    {
        name = "鸿蒙";
        age = 2;
    }
}
```

构造函数和创建对象

```java
public class Student
{
    String name;
    int age;
    public static void main(String[] args)
    {
        Student s = new Student(); // 创建对象会自动隐式的调用类中提供的构造函数
    }
}
```

```java
public class Student
{
    String name;
    int age;
    public Student(String name, int age)
    {
        
    }
    public static void main(String[] args)
    {
        Student s = new Student();// 错误
        Student s1 = new Student("鸿蒙",2);// 正确
        //  创建对象必须依赖类中现在提供的构造函数
    }
}
```



### 匿名构造块

- 构造代码块的格式：{ }
- 代码块的作用：对象统一初始化
- 对象创建之前都会执行这个代码块

匿名构造块执行

```java
public class Dept
{
    {
        System.out.println("这是匿名构造块");
    }
    public static void main(String[] args)
    {
        Dept dept = new Dept();// 创建对象，如果类中提供了匿名构造块，都会执行
    }
}
```

创建对象之前都会执行匿名构造块，执行匿名构造块和构造函数的参数无关

### 构造函数重载

- 构造函数重载是多态的一个典型的特例
- 类中有多个构造函数，参数列表不同
- 重载构造函数来表达对象的多种初始化行为

![image-20210709191419342](/images/Java-Object/image-20210709191419342.png)

构造函数重载

```java
public class Person
{
    String name;
    int age;
    public Person()
    {
        
    }
    public Person(String pname, int page)
    {
        name = pname;
        age = page;
    }
    public static void main(String[] args)
    {
        Person p1 = new Person();
        Person p2 = new Person("张员工",22);
    }
}
```



### 方法的定义

- 方法是类或对象的行为特征的抽象
- Java中的方法不能独立存在，必须定义在类体中

语法格式：

权限修饰符 返回值类型 方法名（参数类型 参数名）

{  

  // 方法体

  // 返回值

}

方法的定义

- 方法定义的先后顺序无所谓

- 方法的定义不能产生嵌套包含关系

- 方法定义中的返回值与传递的参数类型均为java中定义的数据类型

- 在方法中可以进行返回数据的处理，格式：

  return 返回数据类型

  void 不返回数据类型

```java
public class Person
{
    public void eat()
    {
        System.out.println("这个eat方法是声明了void类型，void类型就是没有返回值");
    }
    public int getAge()
    {
        System.out.println("这个getAge方法是声明了int类型，表示需要在方法的最后使用return返回具体的值");
        return 20;
    }
}
```

### 方法的调用

- 方法定义了，不会执行，如果想要执行，应进行方法调用

- 本类中的方法调用

  方法名(参数列表)

- 外部类中的方法调用

  调用类的对象.方法名(参数列表)

方法的调用

```java
public class Person
{
    public void eat()
    {
        System.out.println("这个eat方法是声明了void类型，void类型就是没有返回值");
    }
    public int getAge()
    {
        System.out.println("这个getAge方法是声明了int类型，表示需要在方法的最后使用return返回具体的值");
        return 20;
    }
    public static void main(String[] args)
    {
        Person p = new Person();
        p.eat();
        int age = p.getAge();
        System.out.println("年龄是"+age);
    }
}
```

#### 构造函数和普通方法的区别

![image-20210709221321240](/images/Java-Object/image-20210709221321240.png)

### 方法的重载

- 一个类中多个方法名称相同
- 参数的列表不同
- 返回值类型无关
- 与修饰符无关

方法重载的具体表现形式

```java
public class Employee
{
    public void eat()
    {
        System.out.println("员工默认吃工作餐");
    }
    public void eat(String food)
    {
        System.out.println("在一些节日员工可以定制具体的食物，具体的事物是"+food);
    }
    public static void main(String[] args)
    {
        Employee e = new Employee();
        e.eat();
        e.eat("北京烤鸭");
    }
}
```

#### 编译时多态

- 早期绑定

  早期绑定就是指被调用的目标方法如果在编译期可知

  且运行期保持不变时，即可将这个方法与所属的类型进行绑定

- 重载的方法是早期绑定完成

  调用了一个重载的方法，在编译时根据参数列表就可以确定方法

### 面向对象的封装

- 封装是指隐藏对象的属性和实现细节，仅对外提供公式访问方式

封装原则：

1. 将不需要对外提供的内容都隐藏起来
2. 把属性都隐藏，提供公共方法对其访问

封装好处：

1. 提高数据访问的安全性
2. 隐藏了实现细节

- 从数据安全角度

  没有封装，在外部类中可以直接访问修改数据，造成数据不安全

- 封装的控制与实现：private私有访问修饰符修饰变量

```java
public class Employee
{
    private double salary;
}
```



该露的露，该藏的藏

1. 我们程序设计要追求“高内聚，低耦合”
2. 高内聚就是类的内部数据操作细节自己完成，不允许外部干涉
3. 低耦合：仅暴露方法给外部使用
4. 应禁止直接访问一个对象中数据的实际表示，而通过操着方法来访问，这称为数据的隐藏

## this关键字

- this代表当前对象的一个引用
- 所谓当前对象，指的是调用类中方法或属性的那个对象
- this只能在方法内部使用，表示对“调用方法的那个对象”的引用
- this属性名：表示本对象自己的属性

对象的一个属性被方法或构造器的参数屏蔽

```java
public class Person
{
    String name;
    int age;
    public Person(String name, int age)
    {
        this.name = name;
        this.age = age;
    }
    public static void main(String[] args)
    {
        Person p1 = new Person("小张",20);
        Person p2 = new Person("小李",22);
    }
}
```

this关键字调用本类构造函数

- this关键字调用类的重载构造函数
- this关键字必须位于构造函数的第一行

```java
public class Person
{
    String name;
    int age;
    public Person(int age)
    {
        this.age = age;
    }
    public Person(String name)
    {
        this(1);
        this.name = name;
    }
    public static void main(String[] args)
    {
        Person p1 = new Person("出生婴儿1");
        Person p2 = new Person("出生婴儿2");
    }
}
```

this关键字调用本类方法

- this.方法名 ：表示当前对象自己的方法

```java
public class Student
{
    public void eat()
    {
        System.out.println("同学先吃点食物");
    }
    public void talk()
    {
        this.eat();
        System.out.println("同学吃完饭再说");
    }
}
```

this关键字使用注意

- this不能用于静态方法和静态块
- main方法也是静态的，所以this也不能用于main方法

```java
public class Person
{
    String name;
    int age;
    public static void main(String[] args)
    {
        this.name = "小妮子";
        this.age = 20;
    }
}
```

## static 关键字

static修饰变量

- static变量也称作静态变量，也叫做类变量
- 静态变量被所用的对象所共享，在内存中只有一个副本
- 当且仅当在类初次加载时会被初始化
- 静态变量属于类
- 通过类名就可以调用静态变量
- 也可以通过对象名.静态变量名调用

static变量

```java
public class Student
{
    private String name;
    private static String schoolName;
    private static int count;
    public Student(String name)
    {
        this.name = name;
        count++;
    }
    public void showStuInfo()
    {
        System.out.println("学生的姓名是"+this.name+"，学校的名字是"+Student.schoolName);
    }
    public static void main(String[] args)
    {
        Student.schoolName = "第五十七中";
        
        Student s1 = new Student("小张");
        Student s2 = new Student("小王");
        Student s3 = new Student("小美");
        
        s1.showStuInfo();
        s2.showStuInfo();
        s3.showStuInfo();
        
        System.out.println("学生的数量是"+Student.count);
    }
}
```

static修饰方法

- static修饰的方法叫静态方法，也叫做类方法
- 静态方法中不能直接访问类的非静态成员变量和非静态成员方法
- 静态方法中不能使用this关键字
- 通过类名就可以调用静态方法
- 也可以通过对象名.静态方法名调用

静态方法和静态方法访问

```java
public class Student
{
    private String name;
    private int age;
    private int studentId;
    private static String classRoom;
    public static void showClassRoom()
    {
        System.out.println("自习教室是公共教室1001");
    }
    public static void main(String[] args)
    {
        Student.showClassRoom();
    }
}
```

static块

- 静态代码块在类加载时执行，并且只执行一次
- 静态代码块在类中可以有多个
- 静态代码块中不能有this关键字

静态块可以有多个按照顺序执行

```java
public class Emp
{
    static
    {
        System.out.println("欢迎您员工");
    }
    static
    {
        System.out.println("每天上班都要打卡");
        System.out.println("每天下班都要打卡");
    }
}
```

注意静态块，匿名构造块，构造函数的执行顺序

## 类的生命周期

类的生命周期由五部分组成：

1. 加载
2. 连接
3. 初始化
4. 使用
5. 卸载

其中，连接阶段又分为三个阶段：

1. 验证
2. 准备
3. 解析

总的生命周期如下图所示：

![image-20210712082608962](/images/Java-Object/image-20210712082608962.png)

- 加载
- 验证
- 准备
- 解析
- 初始化
- 使用
- 卸载

## Java方法参数

方法参数的基本类型和引用类型

- 在java方法中参数列表有两种类型的参数，基本类型和引用类型
- 参数类型是基本数据类型，那么传过来的就是这个参数的一个副本
- 参数类型是引用类型，那么传过来的就这个引用参数的脚本，这个副本存放的是参数的地址

Java方法的参数基本数据类型

```java
public class Test1
{
    int a = 5;
    int b = 6;
    public void change(int a, int b)
    {
        a = 20;
        b = 30;
        System.out.println("chang方法里，a的值是："+a+";b的值是："+b);
    }
    public static void main(String[] args)
    {
        Test1 t = new Test1();
        t.change(t.a,t.b);
        System.out.println();
        System.out.println("交换结束后，a的值是："+t.a+";b的值是："+t.b);
    }
}
```

Java方法的参数引用数据类型

```java
public class Test2
{
    int a = 5;
    int b = 6;
    public void change(Test2 t)
    {
        t.a = 20;
        t.b = 30;
        System.out.println("change方法里，a的值是："+a+";b的值是："+b);
    }
    public static void main(String[] args)
    {
        Test2 t = new Test2();
        t.change(t);
        System.out.println();
        System.out.println("交换结束后，a的值是："+t.a+";b的值是："+t.b);
    }
}
```

Java的可变参数列表

```java
public class Test
{
    public void add(int... a)
    {
        for(int i=0; i<a.length; i++)
        {
            System.out.println(a[i]);
        }
    }
    public static void main(String[] args)
    {
        Test t = new Test();
        t.add();
        t.add(1);
        t.add(1,2,5,8);
    }
}
```

### Java参数传递基本数据类型和引用类型区别

| 说明 |      基本数据类型      |      引用数据类型      |
| :--: | :--------------------: | :--------------------: |
| 根本 |       会创建副本       |      不会创建副本      |
| 所以 | 函数中无法改变原始对象 | 函数中可以改变原始对象 |
