---
title: "C语言核心内容 笔记"
date: 2022-02-01T19:01:04+08:00
draft: false
math: true
tags: ["C","note"]
categories: ["develop"]
toc: true
---

# C语言核心

## 函数

基本函数

```c
#include <stdio.h> 
// 函数的声明
void Hello();

int main()
{
    // 最简单函数的调用
    Hello();
    return 0;
}
void Hello()
{
    printf("Hello!\n");
}
```

```c
#include <stdio.h> 

void Hello()
{
    printf("Hello!\n");
}
int main()
{
    // 最简单函数的调用
    Hello();
    return 0;
}

```

### 函数的定义

```c
返回值类型 函数名(参数列表)
{
    函数体
}
```

### 函数声明

所谓声明就是（Declaration）,就是告诉编译器我要使用这个函数，你现在没有找到它的定义不要紧，请不要报错，稍后我会把定义补上

### 参数

形参 形式参数

实参 实际参数

#### 传值和传址

传递数值 会受到作用域的限制

传递地址 任意门不会受到作用域的限制

#### 可变参数

`#include <stdarg.h>`

- va_list
- va_start
- va_arg
- va_end

```c
#include <stdio.h>
#include <stdarg.h>

int sum(int n, ...);

int sum(int n, ...)
{
	int i, sum = 0;
	va_list vap;
	va_start(vap, n);
	for(i = 0;i<n;i++)
	{
		sum += va_arg(vap,int);
	}
	va_end(vap);

	return sum;
}
int main()
{
	int result;
	result = sum(3, 1, 2, 3);
	printf("result = %d\n", result);
}
```



---

带参数的函数

```c
#include <stdio.h>

void add(int a, int b);

int main()
{
    add(5, 6);
    return 0;
}

// 参数里的a和b都是形式参数（形参），我们在调用的时候，需要填写实际参数（实参）
void add(int a, int b){
    int c = a + b;
    printf("%d", c);
}
```

函数的返回值

```c
#include <stdio.h>
int add(int a, int b);

int main()
{
    int nNum = add(5, 6);
    printf("%d", nNum);
    return 0;
}
int add(int a, int b){
    int c = a + b;
    return c;
}
```



### 递归

```c
#include <stdio.h>

void hello(int n);

int main()
{
    hello(1);
    return 0;
}


void hello(int n)
{
    if(n <= 10)
    {
        printf("hello!\n");
        hello(n+1);
    }
    else
    {
        printf("end");
    }
}
```



## 数组

**定义**

* 类型 数组名 \[元素个数\]

  ```c
  int a[6];
  char b[24];
  double c[3];
  ```

* 数组不能动态定义

**访问**

* 数组名[下标]

  ```c
  a[0];//访问a数组中的第一个元素
  b[1];//访问b数组中的第二个元素
  c[5];//访问c数组中的第六个元素
  ```

* 注意

  ```c
  int a[5]; // 创建一个具有五个的数组
  a[0];// 访问第一个元素的下标是0，不是1
  a[5];// 报错，因为第五个元素的下标是a[4]
  ```

  **循环跟数组的关系**

  *我们常常需要使用循环来访问数组*

  ```c
  int a[10];
  for (i = 0;i < 10;i++)
  {
      a[i] = i;
  }
  ```

**初始化**

* 将数组中所用元素初始化为0，可以这么写：

  ``` c
  int a[10] = {0};// 事实上这里只是将第一个元素赋值为0
  ```

* 如果是赋予不同的值，那么用逗号分隔开即可：

  ```c
  int a[10] = {1,2,3,4,5,6,7,8,9,0};
  ```

* 你还可以只给一部分元素赋值，未被赋值的元素自动初始化为0：

  ```c
  int a[10] = {1,2,3,4,5,6};
  // 表示为前边6个元素赋值，后边4个元素系统自动初始化为0
  ```

* 有时候还可以偷懒，可以只给出各个元素的值，而不指定数组的长度（因为编译器会根据值的个数自动判断数组的长度）：

  ```c
  int a[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
  ```

* C99增加了一种新特性：指定初始化的元素。这样就可以只对数组中的某些指定元素进行初始化赋值，而来被赋值的元素自动初始化为0；

  ```c
  int a[10] = {[3] = 3, [5] = 5, [8] = 8};
  ```

最新的标准：会出现数组动态定义和越界问题的解决情况

### 字符数组

```c
#include
int main()
{	
    // 初始化字符数组的每个元素
    char str1[10] = {'F','i','s','h','C','\0'};
    
    // 可以不写元素的个数，因为编译器会自动计算
    char str3[] = {'F','i','s','h','C','\0'};
    
    // 使用字符串常量初始化字符数组
    char str4[] = {"FishC"};
    
    // 使用字符串常量初始化，可以省略大括号
    char str5[] = "FishC" ;
    // 初始化字符串
    char str6[128] = "FishC";
}
```

**字符串处理函数**

```c
int main()
{
	// strlen 字符串长度
	char str[] = "I love C language";
	printf("%u",str)
	// strcpy 和 strncpy 复制和拷贝字符串
	char str1[] = "Original String";
	char str2[] = "New String";
	strcpy(str1,str2);//大坑copy小坑会出现溢出
	strncpy(str1,str2,5);
    // strcat和strncat 连接字符串
    strcat(str1,str2);
    strncat(str1,str2,5);
    //strcmp和strncmp 比较字符串
    if (!strcmp(str1,str2))
    {
        printf("两个字符串完全一致\n");
    }
    else
    {
        printf("两个字符串存在差异！\n")
    }
    
    return 0
}
```



### 二维数组

平面、矩阵

**定义**

* 类型 数组名 \[常用表达式\]\[常量表达式\]

  ```c
  int a[6][6]; // 6*6, 6行6列
  char b[4][5]; // 4*5, 4行5列
  double c[6][3]; // 6*3, 6行3列
  ```

  *存放方式依然是线性*

**访问**

* 1数组名\[下标\][下标]

  ```c
  a[0][0]; // 访问a数组中第1行第1列的元素
  b[1][3]; // 访问b数组中第2行第4列的元素
  c[3][3]; // 访问c数组中第4行第4列的元素
  ```

* 同样需要注意下标的取值范围，以防数组的越界访问

**二维数组初始化**

* 由于二维数组在内存中是线性存放的，因此可以将所有的数据写在一个花括号内：

  ```c
  int a[3][4] = {1,2,3,4,5,6,7,8,9,10,11,12};
  ```

* 为了更直观地表示元素的分布，可以用大括号将每一行的元素括起来：

  ```c
  int a[3][4] = {{1,2,3,4},{5,6,7,8},{9,10,11,12}};
  ```

  或者

  ```c
  int a[3][4] = {
      {1,2,3,4},
      {5,6,7,8},
      {9,10,11,12}
  };
  ```

* 二维数组也可以仅对部分元素赋初值：

  ```c
  int a[3][4] = {{1},{5},{9}};
  ```

* 如果希望整个二维数组初始化为0，那么直接在大括号里写一个0即可：

  ```c
  int a[3][4] = {0};
  ```

* C99同样增加了一种新特性：指定初始化的元素。这样就可以只对数组中的某些指定元素进行初始化赋值，而未被赋值的元素自动初始化为0：

  ```c
  int a[3][4] = {[0][0] = 1, [1][1] = 2, [2][2] = 3};
  ```

* 二维数组的初始化也能偷懒，让编译器根据元素的数量计算数组的长度。但只有第1维的元素个数可以不写，其他维度必须写上：

  ```c
  int a[][4] = {{1,2,3,4},{5,6,7,8},{9,10,11,12}};
  ```

*矩阵的转置*

> 遍历数字变更
>
> 进行转置

## 字符串

```c
#include <stdio.h>

int main()
{
    // 这种声明方式会默认在最后添加一个\0
    // 在我们这个C和C++中，字符串都是以00，也就是、0结尾的
    char szStr[] = { "My name is rkvir" };
    // 像下面这种，没有默认添加的，就需要字节手动添加00，也就是\0
    char cStr[] = {'M','y',' ','n','a','m','e', ' ', 'i','s',' ','r','k','v','i','r','\0'};
    char * pStr = "My name is rkvir";
    
    printf("%s",cStr);
}
```

### 字符操作

获取一个字符并进行输出：

```c
char a;
a = getchar();
putchar();
```

C语言`ctype.h`

```c
#include <ctype.h>
isalpha();
isalnum();
isblank();
iscntrl();
isdigit();
isgraph();
islower();
isprint();
ispunct();
isspace();
isupper();
isxdigit();
```

大小写处理函数

```c
char a = 'a';
char flag = toupper(a);
char flags = tolower(flag);
```

字符串接收输出

```c
#include <stdio.h>

int main()
{
	char string;
	while ((string = getchar()) != EOF)
	{
		putchar(string);
	}
	return 0;
}
// EOF 
// CTRL+C
// CTRL+Z
```



### 字符串输入

```c
// gets fgets scanf
char szStr[50]
gets(szStr); // 以回车结尾
fget(szStr,50,stdin); // 接受指定长度,也会接受回车
scanf("%s",&szStr); // 接受地址
```

### 字符串输出

```c
char *pStr = "Hello World!\n";
// puts printf
// puts这个函数会自动添加换行符
puts(szStr);
fputs(szStr, stdout);
printf("%s",szStr);
```



### 字符串操作

字符串长度

```c
#include <string.h>
int main()
{
    // strlen
    char szStr[] = "Hello World!";
    int Length = strlen(szStr);
    
    return 0;
}
```

字符串拼接

```c
#include <string.h>
int main()
{
    // strcat strncat
    char szStr1[] = "Hello";
    char szStr2[] = "rkvir";
    /*
     *strcat有两个参数
     *第一个参数是目的字符串，第二个参数是源字符串
     *strcat的作用就是把源字符串拼接大屏目的字符串的末尾处，并且将拼接后的字符串存储到目的字符串中
     */
    strcat(); // 会出溢出
    // strncat 控制拼接长度
    strncat();
    return 0;
}
```

字符串比对

```c
#include <string.h>
int main()
{
    char szStr1[] = "Hello World!";
    char szStr2[] = "Hello";
    char szStr3[] = "Hello World!";
    // strcmp strncmp
    /* strcmp有两个参数，就是待比较的两个字符串
     * 返回值是结果
     * 如果返回值等于0，那么两个字符串就相等
     * 如果返回值不等于0，那么两个字符串就不相等
     */
    int ret = strcmp(szStr1, szStr3);
    /* strncmp有三个参数
     * 前两个参数是待比较的两个字符串
     * 但是第三个字符，是比对的字符数
     */
    int ret0 = strncmp(szStr1, szStr2, 6);
    return 0;
}
```

字符串拷贝

```c
#include <string.h>

int main()
{
    char szStr1[50] = {0};
    char szStr2[] = "Hello";
    
    // strcpy strncpy
    /* strcpy有两个参数
     * 第一个参数是目的字符串
     * 第二个参数是源字符串
     * strcpy的作用就是把源字符串拷贝到目的字符串
     */
    strcpy(szStr1,szStr2);
    strncpy(szStr1,szStr2.2);
    return 0;
}
```

字符串格式化

```c
#include <stdio.h>
#include <string.h>
int main()
{
    char szStr1[] = "Hello";
    int nNum = 11;
    char cS = 'A';
    
    char szStr2[100] = {0};
    /* sprintf 三个参数
     * 第一个参数，就是目的字符串
     * 第二个参数，是格式化符号
     * 第三个参数就是变量
     */
    sprintf(szStr2, "%s %d %c", szStr1,nNum,cs);
}
```



## 指针

内存储存的真相：数据储存在内存地址上

### 指针和指针变量

指针：指向内存储存的地址

#### 指针变量

**定义指针变量**

* 类型名 *指针变量名

  ```c
  char *pa;// 定义一个指向字符型的指针变量
  int *pb; // 定义一个指向整型的指针变量
  ```

**取地址运算符和取值运算符**

* 如果需要获取某个变量的地址，可以使用取地址运算符（&）：

  ```c
  char *pa = &a;
  int *pb = &f;
  ```

* 如果需要访问指针变量指向的数据，可以使用取值运算符（*）：

  ```c
  printf("%c,%d\n",*pa,*pd);
  ```

**避免访问未初始化的指针**

```c
#include <studio.h>

int main()
{
    int *a;

    *a = 123;

    return 0;
}
```

*操作非常危险*

### 指针和数组

**数组名的真实身份**

数组名其实是数组第一个元素的地址！

**指向数组的指针**

* 如果用一个指针指向数组，应该怎么做呢？

  ```c
  char *p;
  p = a; // 语句1
  p = &a[0]; // 语句2
  ```

==**指针的运算**==

* 当指针指向数组元素的时候，我们可以对指针变量进行加减运算，这样做的意义相当于指向距离指针所在位置向前或向后第n个元素
* 对比标准的下标法访问数组元素，这种使用指针进行间接访问的方法叫做指针法
* 需要郑重强调的是：p+1并不是简单地将地址加1，而是指向数组的下一个元素

#### 指针和数组的区别

*数组名只是一个地址，而指针是一个左值*

**指针数组**

```c
int *p1[5]; //指针数组
// 初始化
#include<stdio.h>

int main()
{
    int a = 1;
    int b = 2;
    int c = 3;
    int d = 4;
    int e = 5;
    int *pl[5] = {&a ,&b ,&c ,&d ,&e };
    int i;
    
    for (i = 0;i < 5; i++)
    {
        printf("%d\n",*pl[i]);
    }
    return 0;
}
```

*指针数组是一个数组，每个数组元素存放一个指针变量*

**数组指针**

```c
int (*p2)[5];
```

*数组指针是一个指针，它指向的是一个数组*

#### 指针和二维数组

**array表示的是什么？**

array表示指向5个元素的数组指针（即指向一维数组的数组指针）

```c
*(array+i) == array[i];
*(*(array+i)+j) == array[i][j];
*(*(*(array+i)+j)+k) == array[i][j][k];
```

#### 数组指针和二维指针

- 初始化二维数组是可以偷懒的：

  ```c
  int array[2][3] = {{0,1,2}, {3,4,5}};
  ```

  可以写成

  ```c
  int array[][3] = {{0,1,2},{3,4,5}};
  ```

- 定义一个数组指针是这样的：

  ```c
  int (*p)[3];
  ```

### void指针

void指针我们把它称之为通用指针，就是可以指向任意类型的数据。也就是说，任何类型的指针都可以赋值给void指针

取出void指针的数值需要使用到强制类型转换

### NULL指针

NULL指针即为空指针

```c
#define NULL ((void *)0)
```

当你还不清楚要将指针初始化为什么地址时，请将它初始化NULL；在对指针进行解引用时，先检查该指针是否为NULL。这种策略可以为你今后编写大型程序节省大量的调试时间。

### 指针之指针

指向指针的指针

```c
#include <stdio.h>

int main()
{
    int num = 520;
    int *p = &num;
    int **pp = &p;
    return 0;
}
```

#### 指针数组和指向指针的指针

至少有两个好处：

- 避免重复分配内存
- 只需要进行一处修改

代码的灵活性和安全性都有了显著地提高！

数组指针和二维数组

### 常量和指针

const关键字，使变量可读取而不可修改

```c
const int price = 520;
const char a = 'a';
const float pi = 3.14
```

- 指针可以修改为指向不同的常量
- 指针可以修改为指向不同的变量
- 可以通过解引用来读取指针指向的数据
- 不可以通过解引用修改指针指向的数据

常量指针

```c
int num = 520;
int * const p = &num;
```

- 指向非常量的常量指针
  - 指针自身不可以被修改
  - 指针指向的值可以被修改
- 指向常量的常量指针
  - 指针自身不可以被修改
  - 指针指向的值也不可以被修改

### 指针与函数

指针函数，是函数

函数指针，是指针

#### 函数指针

```c
int add(int a, int b)
{
    return a + b;
}
typedef int(*Myadd)(int a, int b);
Myadd my = add;
int c = my(1, 2);
```

应用场景：

- 系统库API
- load dll
- 模块基质
- GetProcAddress 获取函数地址

```c
#include <stdio.h>

int add(int a, int b);

int main()
{
    int (*Myadd)(int a, int b);
    //不带括号不带参数的函数名，其实就是函数的首地址
    MyAdd = add;
    
    return 0;
}
int add(int a, int b)
{
    return a+b;
}
```

#### 指针函数

```c
#include <stdio.h>

int *add(int *a);

int main()
{
    int arrNum[5] = {1,2,3,4,5};
    int *ret = add(arrNum);
    int nNum = ret[2]
    return 0;
}
int *add(int *a)
{
    return a;
}
```



## 存储类

- 静态存储时期
- 自动存储时期

静态存储类

- 自动
- 寄存器 register
- 具有外部链接的静态存储类 extern
- 具有内部链接的静态存储类 static
- 空链接的静态存储类 static

## 动态内存管理

> 申请内存
>
> 释放内存

数组是默认给你开辟了一块连续空间

`malloc`在堆上面申请内存，返回的是`void *` 单位是字节 

`memset`，把一段内存，全部刷成你想要的值

`free`，释放一段内存的空间

```c
#include <stdlib.h>

int main()
{
    char * szStr;
    szStr = (char *)malloc(200 * sizeof(char));
    memset(szStr, 0, 200 * sizeof(char));
    free(szStr);
    return 0;
}
```

## 文件操作

文件指针：位于文件头部的指针，然后可以对其移动，这样就可以读取文件任意位置的数据了，文件指针移动到末尾出的时候，文件就读取完了

fopen的模式

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| “r”  | 打开一个用于读取的文件，该文件必须存在。                     |
| “w”  | 创建一个用于写入的空文件，如果文件名称与已存在的文件相同，则会删除已有文件的内容，文件视为一个新的空文件。 |
| “a”  | 追加到一个文件，写操作向文件末尾追加数据，如果文件不存在，则创建文件。 |
| “r+” | 打开一个用于更新的文件，可读取也可写入，该文件必须存在。     |
| “w+” | 创建一个用于读写的空文件。                                   |
| “a+” | 打开一个用于读取和追加的文件。                               |



```c
#include <stdio.h>
int main()
{
    // 声明文件指针
	FILE * pFile;
    // 存储读取到的文件的内存空间，也就是一个char * 类型的字符串
    char * szReadTextBuffer;
    // 读取到的文件的尺寸
    int nReadFileSize;
    // 获取读取到的文件缓冲区字节数
    int nReadRetSize = 0;
    /* fopen,fopen有两个参数
     * 第一个参数是需要打开的文件的路径
     * 第二个参数是打开的模式
     */
    if((pFile = fopen("file.txt","rb")) == NULL)
    {
        // 处理一下如果打开文件失败了
        printf("Open File Failed!\n");
        exit(0);
    }
    // 因为设置文件指针到末尾处了，我们可以通过文件指针位置的方式，获取文件大小
    //设置文件指针到末尾处
    fseek(pFile, 0, SEEK_EMD);
    // 获取文件指针的位置，获取文件大小
    nReadFileSize = ftell(pFile);
    //文件指针复位到初始位置
    rewind(pFile);
    // 根据文件尺寸申请一块足够大小的内存空间
    szReadTextBuffer = (char *)malloc(sizeof(char)*nReadFileSize +1);
    if(szReadTextBuffer == NULL)
    {
        // 处理内存申请失败代码
        printf("malloc memory failed!\n");
        exit(0)
    }
    // 对申请到的内存进行初始化
    menset(szReadTextBuffer, 0, nReadFileSize + 1);
    // 将已经获取到文件指针的文件的内容读取到已经申请好的缓冲区中，并且返回真实读取到的内容长度
    nReadRetSize = fread(szReadTextBuffer, 1, nReadFileSize, pFile);
    // 判断是否读取失败，真实读取到的内存长度
    if(nReadFileSize != nReadRetSize)
    {
        // 处理读取失败的代码
        printf("Read File Failed!\n");
        exit(0);
    }
    fclose(pFile);
    return 0;
}
```

读取文件

```c
int main()
{
	FILE* pFile;
	char* szReadTextBuffer;
	int nReadFileSize;
	int nReadRetSize;
	pFile = fopen("C:\\Users\\15890\\Desktop\\file.txt","rb");
	if (pFile == NULL)
	{
		printf("open file failed!\n");
		exit(0);
	}
	fseek(pFile, 0, SEEK_END);
	nReadFileSize = ftell(pFile);
	rewind(pFile);
	szReadTextBuffer = (char*)malloc((nReadFileSize * sizeof(char)) + 1);
	if (szReadTextBuffer == NULL)
	{
		printf("mallco memory failed!\n");
		exit(0);
	}
	memset(szReadTextBuffer, 0, nReadFileSize + 1);
	nReadRetSize = fread(szReadTextBuffer, 1, nReadFileSize, pFile);
	if (nReadFileSize != nReadRetSize)
	{
		printf("Read file failed!\n");
		exit(0);
	}
	puts(szReadTextBuffer);
	fclose(pFile);
    return 0;
}
```

写入文件

```c
int main()
{
    char* szWriteBuffer = "I love the reverse program!\n";
	int nWriteSize = 0;
	FILE* pFile;
	pFile = fopen("C:\\Users\\15890\\Desktop\\myfile.txt", "wb");
	if (pFile == 0)
	{
		printf("failed!\n");
		exit(0);
	}
    fwrite(szWriteBuffer, strlen(szWriteBuffer), 1, pFile);
	fclose(pFile);
	return 0;
}
```



## 结构体、联合体、枚举

### 结构体

```c
int main()
{
    // 用struct标记为结构体，然后后面是结构体的名字
    struct NPC
    {
        // 名字
        char * Name;
        // 血量
        int HP;
        // 魔法值
        int MP;
        // 坐标
        int x;
        int y;
        int z;
    }
    // 给结构体小儿的各种变量赋值
    NPC xiaoer;
    xiaoer.Name = "wangxiaoer";
    xiaoer.HP = 100;
    xiaoer.MP = 0;
    xiaoer.x = 111;
    xiaoer.y = 222;
    xiaoer.z = 0;
    
}
```

结构体指针

```c
int main()
{
    // 用struct标记为结构体，然后后面是结构体的名字
    struct NPC
    {
        // 名字
        char * Name;
        // 血量
        int HP;
        // 魔法值
        int MP;
        // 坐标
        int x;
        int y;
        int z;
    }
    struct NPC npcarry[100];
    struct NPC *npcindex;
    npcindex = &npcarry[0];
    npcindex -> Name = "xiaoyi";
    //  *npcindex.Name = "xiaoyi";
    npcindex++;
    npcindex -> Name = "xiaoer";
    // *npcindex.Name = "xiaoer";
    return 0;
}
```

结构体参数

#### 位域

```c
struct {
    int a : 8;
    int b : 8;
    int c : 8;
    int d : 8;
}rk
```



### 联合体

```c
union Info
{
    char PlayerName[20];
    int MP;
    float x;
}
union Info MyInfo;
strcpy(MyInfo.PlayerName, "NPC");
printf("%s\n%d\n%f\n", MyInfo.PlayerName, MyInfo.MP, MyInfo.x);
```

联合体所有成员共用一个地址，只能使用一个成员

### 枚举

```c
int main()
{
    enum color
    {
        red;
        green;
        blue;
    }
    int flag = 0;
    scanf("%d", &flag);
    switch(flag)
    {
        case red:
            printf("red\n");
            break;
        case green:
            printf("green\n");
            break;
        case blue:
            printf("blue\n");
            break;
        default:
            break;
    }
    return 0;
}
```

## 高级数据表示

### 顺序结构

数组

栈

队列

### 链式结构

链表

链栈

链队列

### 树

二叉树

1. 度不能超过2
2. 有序树
3. 无序树

遍历

- 先序遍历：根 左 右
- 中序遍历：左 根 右
- 后序遍历：左 右 根
