---
title: "汇编语言 笔记"
date: 2022-02-03T09:56:53+08:00
draft: false
math: true
tags: ["Reverse","assembly","note"]
categories: ["reverse"]
toc: true
---

# 汇编语言

## 概述

编程形式

> 开关-->打孔-->输入设备

汇编语言的出现

```x86asm
mov eax, 5
mov ebx, 6
add eax, ebx
```

汇编程序的执行

> 汇编代码 -> 汇编程序 -> 处理器可识别 01010101 -> 处理器执行

### 学习汇编的意义

1. 开发时调试
2. 逆向时候的代码阅读
3. 某些特殊技术的使用（如shellcode、壳）

> shellcode：可以运行在任意位置的代码（汇编语言）
>
> 壳：加壳或脱壳都需要用的汇编语言

环境配置

- Ollydbg

- Visual Studio 2015

## x16 汇编

### 通用寄存器

| 16位寄存器 | 高8位 | 低8位 |
| ---------- | ----- | ----- |
| AX         | AH    | AL    |
| BX         | BH    | BL    |
| CX         | CH    | CL    |
| DX         | DH    | DL    |
| SI         | \     | \     |
| DI         | \     | \     |
| SP         | \     | \     |
| BP         | \     | \     |

![image-20211123194526478](/images/ASM/image-20211123194526478.png)

### 内存字节序

:chestnut: 0x12345678

每个地址只有存储1字节

| 大端序 | 小端序 |
| ------ | ------ |
| 12     | 78     |
| 34     | 56     |
| 56     | 34     |
| 78     | 12     |
| CC     | CC     |
| CC     | CC     |
| CC     | CC     |

### 段的概念

![image-20211123201417209](/images/ASM/image-20211123201417209.png)

> CS段只有16位，8086有20根地址线，那么地址如何存储？
>
> 简单粗暴 除以0x10，因为只有以零结尾的地址才能作为段地址

CS：代码段

DS：数据段

ES：扩展段

SS：堆栈段

### 16位汇编基本框架

```x86asm
assume cs:code ;设置代码段

code segment ;代码段开始
rkmain proc
	mov ax,0
	mov cx, 10H
rk:     
	inc ax
	loop rk
	mov ax, 4c00H
	int 21H
rkmain endp

start:  call rkmain ;指定开始位置
code ends ;代码段结束
end start
```

> `add` 加
>
> `int` 自增
>
> `sub` 减
>
> `dec` 自减
>
> `loop` 循环

### 函数传参

- 寄存器传参
- 堆栈传参
- 内存传参

```x86asm
assume cs:code

code segment

addx proc
	push bp
	mov bp,sp
	mov si,[bp+4]
	mov di,[bp+6]
	add si,di
	mov ax,si
	pop bp
	ret
addx endp

rkmain proc
	mov ax,5
	mov bx,6
	push ax
	push bx
	call addx
	add sp,4
	mov bx,ax
	mov ax, 4c00H
	int 21H
rkmain endp

start:  call rkmain

code ends
end start
```

### FLAG寄存器

#### 状态标志

CF：进位位

PF：奇偶位

AF：辅助进位位

ZF：零值位

SF：符号位

OF：溢出位

### CMP指令

> CMP OPRD1, OPRD2
>
> SUB
>
> 影响标志位

```x86asm
mov ax,2
mov bx,1
cmp ax.bx
```

相等

AX-BX = 0 ZF=1

不等

AX-BX !=0 ZF=0

AX < BX:

AX-BX = ? CF=1

AX > BX:

AX-BX=? CF=0 ZF=0

AX <= BX:

AX-BX=? CF=1 || ZF=1

AX >= BX:

AX-BX=? CF=0 || ZF=1 



### JCC指令

一类指令 跳转

```x86asm
jmp address 
je address ;等于则跳转 ZF=1
jne address ;不等于则跳转 ZF=0
jb address ;低于则跳转 CF=1
ja address ;高于则跳转 CF=0 && ZF=0
```



### 运算指令

四则运算

```x86asm
mov ax,16
mov bx,5
add ax,5
sub ax,3
mul bx ; >16bit DX:AX 32bit 默认与ax操作
div bx ; DX->余数 AX->商 默认与ax操作
```

位运算

```x86asm
xor ax,bx
and ax,bx
or ax,bx
not ax
```



## x86汇编

通用寄存器

| 寄存器 |
| ------ |
| EAX    |
| EBX    |
| ECX    |
| EDX    |
| ESI    |
| EDI    |
| ESP    |
| EBP    |

### 段

| 段   |
| ---- |
| CS   |
| DS   |
| ES   |
| SS   |
| FS   |
| GS   |

#### 段选择子

CS 代码段 1B

DS ES SS 数据段 23

FS 3B

- Index：3

- Ti：0

- RPL：3

#### 段描述符

代码段：

> `00cffb00 0000ffff`
>
> 1100 1111 1111 1011 0000 0000
>
> Base：`0000 0000`
>
> Limit：`ffffff`
>
> `ffffff * 0x1000`
>
> `FFFFFF000+0X1000`
>
> `10 0000 0000 - 1`
>
> `FFFF FFFF`
>
> 
>
> G = 1 Limit单位是页
>
> G = 0 Limit单位是字节
>
> 页
>
> - 大页 4M
> - 小页 4K
>
> 寻址空间：`0000 0000 ~ FFFF FFFF`

数据段

> `00cff300 0000ffff`
>
> BASE：`0000 0000`
>
> Limit：`FFFF FFFF`

FS段

> `0040f300 00000fff`
>
> G=0 字节
>
> `0000 0000 0100 0000 1111 0011 0000 0000`
>
> `fff * 1` `fff`字节
>
> `0 ~ fff`

4GB空间 虚拟的4GB空间

- 低2GB是每个进程独享的
- 高2GB是内核空间，是共享的

base+offset = 虚拟地址

### 页 

> 内存的一种管理模式

29912

CR3

PDPTE -> PTE - PDE -> 物理页 (4k)

76B26000 E9 A1 B0 76

01 (PDPTE) （1 \* 8)

110110101(PTE) (1B5 \* 8 )

100100110(PDE) (126 \* 8)

00000000000(页内偏移) (000)



---

> R3 用户层 TEB（线程环境块）
>
> R0 系统内核 驱动 KPCR（CPU状态块）

### 调用约定

1. `cdecl`
2. `stdcall`
3. `fastcall`
4. `thiscall`

> `lib` 静态链接库
>
> `dll` 动态链接库

#### cdecl

C语言调用约定

```x86asm
push eax
push eax
call printf
add esp,8
```

#### stdcall

Win32调用约定

```x86asm
push 1
push 2
push 3
push 4
call messagebox
```

#### fastcall

x64 调用约定

前四个参数使用寄存器传参，后面使用堆栈

> rcx rdx r8 r9

### x86基本框架

```x86asm
.586
.model flat,stdcall
option casemap:none

includelib ucrt.lib
includelib legacy_stdio_definitions.lib
includelib User32.lib
includelib Kernel32.lib

MessageBoxA PROTO hWnd:DWORD,lpText:BYTE,lpCaption:BYTE,uType:DWORD
ExitProcess PROTO uType:DWORD
extern printf:proc
extern scanf:proc
extern putchar:proc

.data
szStr db 'Hello World!',0
.code
main proc
	lea eax,szStr
	push eax
	call printf
	add esp,4
	push 0
	call ExitProcess
	add esp,4
main endp
end
```

### 内联汇编

单行内联汇编

```c
_ASM/ mov nNum, 100;
```

多行内联汇编

```c
_ASM/ {
		mov nNum,100
		mov eax,nNum
		push eax
		mov eax,fNum
		push eax
		call printf
		add esp,8
	}
```

遍历数组

按结尾数据遍历

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
	int nNum;
	char* fNum = "%d\n";
	int arr[] = { 1,2,3,4,5,6,7,8,9,0 };
	_ASM/ {
		xor esi,esi
		jmp loopX
	loopM:
		inc esi
	loopX:
		mov edi,[arr+esi*4]
		push edi
		mov eax,fNum
		push eax
		call printf
		add esp,8
		cmp edi,0
		jne loopM
	}
	system("pause");
	return 0;
}
```

按长度进行遍历

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
	int nNum;
	char* fNum = "%d\n";
	int arr[10];
	for (size_t i = 0; i < 10; i++)
	{
		arr[i] = i;
	}
	_ASM/ {
		xor edi,edi
		xor esi,esi
		xor edx,edx
		mov edi,9h
		jmp Mloop
		Xloop:
		inc esi
		Mloop:
		mov edx, [arr + esi * 4]
		push edx
		mov eax,fNum
		push eax
		call printf
		add esp,8
		cmp edi,esi
		jne Xloop
	}
	system("pause");
	return 0;
}
```

### 结构体和API

汇编语言结构体

```x86asm
.586
.model flat,stdcall

.data
info struct ;声明结构体
x dword ?
y dword ?
info ends

m_info info <> ;初始化结构体
.code

main proc
	mov eax,5
	mov m_info.x,eax
	mov ebx,m_info.x
	mov eax,eax
main endp

end main
```

## x64汇编

x64和x86汇编寄存器区别

> x86 : `eax ebx ecx edx esi edi ebp esp eip eflags`
>
> x64 : `rax rbx rcx rdx rsi rdi rbp rsp rip rflags `
>
> x64扩展 ： `r8 r9 r10 r11 r12 r13 r14 r15`
>
> 


## 汇编代码剽窃小技巧

1. 所有的jcc死地址，都要改成标号
2. 所有的call，都要确定，是系统函数，还是自己的函数，想办法call过去
3. 一切检查或者初始化类的无用代码，全部去掉
4. 一些必要逻辑里的不合理东西，去掉这些，然后自己写代码，让他合理化

## 16位/32位/64位汇编的区别

区别

> 16位：实模式，16位处理器内的内部，最多可以处理存储的长度为16位。
>
> 32位：保护模式，32位处理器内的内部，最多可以处理存储的长度为32位。
>
> 64位：保护模式，64位处理器内的内部，最多可以处理存储的长度为64位。

通用寄存器简介

| 位数 | 通用寄存器                      | 扩展                                  |
| ---- | ------------------------------- | ------------------------------------- |
| 16   | AX BX CX DX SI DI BP SP         | R8W R9W R10W R11W R12W R13W R14W R15W |
| 32   | EAX EBX ECX EDX ESI EDI EBP ESP | R8D R9D R10D R11D R12D R13D R14D R15D |
| 64   | RAX RBX RCX RDX RSI RDI RBP RSP | R8 R9 R10 R11 R12 R13 R14 R15         |

基本执行环境

| 32位              | 64位               |
| ----------------- | ------------------ |
| 8个32位通用寄存器 | 16个64位通用寄存器 |
| 标志寄存器EFLAGS  | 标志寄存器RFLAGS   |
| 指令寄存器EIP     | 指令寄存器RIP      |



### EFLAGS寄存器

Eflags寄存器，Eflags寄存器包含了独立的二进制位，用于控制CPU操作。或是反应一些CPU操作的结果。有些指令可以测试和控制这些单独的处理器标志位。

### MMX寄存器

在实现高级多媒体和通信应用时，MMX技术提高了Intel处理器的性能。8个64位MMX寄存器支持成为SIMD的特殊指令。顾名思义，MMX指令对MMX寄存器中的数据值直接进行并行操作。虽然它们看上去是独立的寄存器，但是MMX寄存器实际上是浮点单元中使用的同样寄存器的别名。

## 通用寄存器

eax：累加器，操作数和结果数据累加器，返回值运算结果一般都存储在这里

![image-20210811183106370](/images/ASM/image-20210811183106370.png)

ebx：基地址，DS段的数据指针，在内存寻址的时候存放基地址

![image-20210811183206737](/images/ASM/image-20210811183206737.png)

ecx：计数器，字符串和循环操作的计数器

![image-20210811183244169](/images/ASM/image-20210811183244169.png)

edx：用于存储部分乘法结果和部分除法被除数

![image-20210811183411998](/images/ASM/image-20210811183411998.png)

ebp：基址指针，SS段的数据指针

![image-20210811183548209](/images/ASM/image-20210811183548209.png)

esp：栈帧指针，一般指向栈顶，所以也被称为栈顶指针

![image-20210811183809151](/images/ASM/image-20210811183809151.png)

edi：字符串操作的目标指针，ES段的数据指针

![image-20210811183910749](/images/ASM/image-20210811183910749.png)

esi：字符串操作的源指针，SS段的数据指针

![image-20210811184031993](/images/ASM/image-20210811184031993.png)

## 冯诺依曼

计算机科学的奠基者

- 艾伦·麦锡森·图灵  - - - 图灵机
- 约翰·冯·诺依曼  - - - 数据存储的体系结构

约翰·冯·诺依曼

> 1903年 出生犹太家庭
>
> 1926年 布达佩斯大学数学博士学位
>
> 1930年 接受了普林斯顿大学客座教授的职位
>
> 1931年 普林斯顿大学终身教授
>
> 1933年 普林斯顿高等研究院
>
> 1937年 美国公民
>
> 1938年 获博修奖
>
> 1954年 美国原子能委员会委员
>
> 1957 在华盛顿德里医院去世

### 冯诺依曼体系

计算机由控制器、运算器、存储器、输入设备、输出设备五部分组成。

冯诺依曼提出的计算机体系结构，奠定了现代计算机的结构理念

![image-20210811190118360](/images/ASM/image-20210811190118360.png)

## 内存基础

### 什么是内存

在冯诺依曼结构中用来存储程序和数据的部件叫做存储器，我们的计算机可以正常的运行，主要依靠的就是存储器的记忆能力。存储器分为主存储器和辅助存储器，主存储器也叫内存储器，也就是我们常说的内存

### 内存寻址范围

> 现在主流的系统是32位系统和64位系统
>
> 32位系统内存的寻址范围是0x00000000-0xFFFFFFFF
>
> 32位系统内存最大寻址范围是0xFFFFFFFF+1(4294967296) - - - 4GB
>
> 64位内存的寻址范围是0x0000000000000000-0xFFFFFFFFFFFFFFFF

![image-20210811191257593](/images/ASM/image-20210811191257593.png)

### 内存和寄存器的区别

寄存器：数量少，在CPU内部，速度极快，但是价格昂贵

内存：数量庞大，相对寄存器而言，速度较慢，但是价格便宜

### 内存的五种表现形式

立即数：

```x86asm
MOV EAX, DWORD PTR DS:[0x???????]
```

寄存器：

```x86asm
MOV EBX, 0x????????
MOV EAX, DWORD PTR DS:[EBX]
```

寄存器+立即数

```x86asm
MOV EBX, 0x???????
MOV EAX, DWORD PTR DS:[EBX+4]
```

比例因子：[REG+REG*{1,2,4,8}]

数组元素地址=数组首地址+元素索引*数组元素占用空间

```x86asm
MOV EAX, 0x????????
MOV EBX, 0x2
MOV ECX, DWORD PTR DS:[EAX+EBX*4]
```

比例因子+立刻数：

```x86asm
MOV EAX, 0x????????
MOV EBX, 0x2
MOV ECX, DWORD PTR DS:[EAX+EBX*4+1]
```

### 数据存储模式

大端序：数据高位在内存低位，数据低位在内存高位

小端序：数据高位在内存高位，数据低位在内存低位



地址0x77 66 55 44

大端序：念的时候是77 66 55 44

小端序：念的时候是44 55 66 77

大端序常用于ARM架构

小端序常用于x86、AMD64架构

## EFLAGS寄存器

CF：进位借位标志位

PF：奇偶标志位

AF：辅助进位标志位

ZF：0标志位

SF：符号标志位

TF：单目标志位

IF：中断标志位

DF：方向标志位

OF：溢出标志位

## 数学运算

### 加法

加法指令 ADD（Addition）

格式：ADD OPRD1, OPRD2

功能：两数相加

加法指令运算的结果对CF、SF、OF、PF、ZF、AF都会有影响

不允许OPRD1与OPRD2同时为存储器

---

带进位加法指令 ADC（Addition Carry)

格式：ADC OPRD1, OPRD2

功能：OPRD1 = OPRD1 + OPRD2 + CF

### 减法

减法指令 SUB（Subtract）

格式：SUB OPRD1, OPRD2

功能：两个操作数的相减，即从OPRD1中减去OPRD2，其结果放在OPRD1中。指令的类型及对标志位的影响与ADD指令相同，注意立即数不能用于目的操作数，两个存储器操作数之间不能直接相减，操作数可为8位或16位的无符号数或带符号数。

---

带借位减法指令 SBB（Subtraction with Borrow）

格式：SBB OPRD1, OPRD2

功能：进行两个操作数的相减再减去CF进位标志位，即从OPRD1 = OPRD1 - OPRD2 -CF，其结果放在OPDR1中。

### 乘法

无符号数乘法指令 MUL（Multiply）

格式：MUL OPRD

---

带符号数乘法指令 IMUL（Integer Multiply）

格式：IMUL OPRD、

功能：乘法操作

OPRD为通用寄存器或寄存器操作数

本指令影响标志位CF及OF

### 除法

无符号数除法指令 DIV（Division）

格式：DIV OPRD

功能：实现两个无符号二进制数除法运算

---

带符号数除法指定 IDIV（Integer Division）

格式：IDIV OPRD

功能：这实现两个带符号数的二进制除法运算

> 16bit，分存2个8bit AH:AL 商AL 余AH
>
> 32bit，分存2个16bit DX:AX 商AX 余DX
>
> 64bit，分存2个32bit EDX:EAX 商EAX 余EDX
>
> 128bit，分存2个64bit RDX:RAX 商RAX 余RDX

### 自增

加1指令 INC（Increment by 1）

格式：INC OPRD

功能：OPRD = OPRD + 1

### 自减

减1指令 DEC（Decrement by 1）

格式：DEC OPRD

功能：OPRD = OPRD - 1

## 逻辑运算

### 与

逻辑与运算指令 AND

格式：AND OPRD1, OPRD2

功能：对两个操作数实现按位逻辑与运算，结果送至目的操作数。本指令可以进行字节或字的 ‘与‘ 运算，OPRD1 < - - OPRD1 and OPRD2.

本指令影响标志位PF、SF、ZF，使CF=0、OF=0.例如，在同一个通用寄存器自身相与时，  操作数虽不变，但使CF置零。本指令主要用于修改操作数或置某些位为零。

### 或

逻辑或指令 OR

格式：OR OPRD1, OPRD2

功能：OR指令完成对两个操作数按位的 ‘或’ 运算，结果送至目的操作数中，本指令可以进 - - - - 行字节或字的 ‘或’ 运算。OPRD1 < - - OPRD1 OR  OPRD2。

### 异或操作

逻辑异或运算指令 XOR

格式：XOR OPRD1, OPRD2

功能：实现两个操作数按位 ‘异或’ 运算，结果送至目的操作数中。OPRD1 < - - OPRD1 XOR OPRD2

### 非运算

逻辑操作符 NOT

格式：NOT exp

功能：按位求反

## 堆栈操作

### 什么是堆栈

1. 栈是一个后进先出的存储区域，位于堆栈段中，SS段寄存器描述的就是堆栈段的地址
2. 栈的数据出口位于栈顶，也就是esp寄存器所指向的位置
3. 栈顶是低位，也就是地址较小的一侧，由ebp寄存器指向的栈底，并不会改变

### 栈操作指令

PUSH：压栈操作，32位汇编首先ESP-4，留出一个空间，然后把要压入栈中的内容压入

POP：出栈指令，32位汇编首先将栈顶的数据弹出给指定的目标，然后ESP+4，清掉空间

### 栈的作用

1. 存储少量数据
2. 保存寄存器环境
3. 传递参数

## 数据移动指令

### MOV指令

数据传送指令MOV

格式：MOV OPRD1, OPRD2

功能：本指令将一个源操作数送到目的操作数中，即OPRD1 < - - OPRD2

说明：

- OPRD1为目的操作数，可以是寄存器、存储器、累加器
- OPRD2为源操作数，可以是寄存器、存储器、累加器和立即数

### LEA指令

有效地址传送指令 LEA

格式：LEA OPRD1，OPRD2

功能：将源操作数给出的有效地址传送到指定的寄存器中

OPRD1必须是寄存器

### XCHG指令

数据交换指令 XCHG

格式：XCHG OPRD1, OPRD2 其中的OPRD1为目的操作数，OPRD2为源操作数

功能：将两个操作数相互交换位置，该指令把源操作数OPRD2与目的操作数OPRD1交换

## 比较指令

### CMP指令

比较指令 CMP（Compare）

格式：CMP OPRD1, OPRD2

功能：对两数进行相减，进行比较

### TEST指令

测试指令 TEST

格式：TEST OPRD1,OPRD2

功能：其中OPRD1、OPRD2的含义同AND指令一样，也是对两个操作数进行按位的 ‘与’ 运算，- - - - 唯一不同之处是不将 ‘与’ 的结果送目的操作数，即本指令对两个操作数的内容均不进行修改，仅是在逻辑与操作后，对标志位重新置位 

## JCC条件转移指令

### 常用的JCC指令

JMP：无条件跳转

JZ/JE：ZF = 1 等于0或相等跳转

JNZ/JNE：ZF = 0 不等于0或者不相等跳转

JBE/JNA：CF = 1/ZF = 1 低于等于/不高于跳转

JNBE/JA：CF = 0/ ZF = 0 不低于等于/高于跳转

JL/JNGE：SF！=OF 小于/不大于等于跳转

JNL/JGE：SF = OF 不小于/大于等于跳转

### JCC表

| JCC指令     | 中文含义                                           | 英文原意                                                | 检查符号位       | 典型C应用                |
| ----------- | -------------------------------------------------- | ------------------------------------------------------- | ---------------- | ------------------------ |
| JZ/JE       | 若为0则跳转；若相等则跳转                          | jump if zero;jump if equal                              | ZF=1             | if (i == j);if (i == 0); |
| JNZ/JNE     | 若不为0则跳转；若不相等则跳转                      | jump if not zero;jump if not equal                      | ZF=0             | if (i != j);if (i != 0); |
| JS          | 若为负则跳转                                       | jump if sign                                            | SF=1             | if (i < 0);              |
| JNS         | 若为正则跳转                                       | jump if not sign                                        | SF=0             | if (i > 0);              |
| JP/JPE      | 若1出现次数为偶数则跳转                            | jump if Parity (Even)                                   | PF=1             | (null)                   |
| JNP/JPO     | 若1出现次数为奇数则跳转                            | jump if not parity (odd)                                | PF=0             | (null)                   |
| JO          | 若溢出则跳转                                       | jump if overflow                                        | OF=1             | (null)                   |
| JNO         | 若无溢出则跳转                                     | jump if not overflow                                    | OF=0             | (null)                   |
| JC/JB/JNAE  | 若进位则跳转；若低于则跳转；若不高于等于则跳转     | jump if carry;jump if below;jump if not above equal     | CF=1             | if (i < j);              |
| JNC/JNB/JAE | 若无进位则跳转；若不低于则跳转；若高于等于则跳转； | jump if not carry;jump if not below;jump if above equal | CF=0             | if (i >= j);             |
| JBE/JNA     | 若低于等于则跳转；若不高于则跳转                   | jump if below equal;jump if not above                   | ZF=1或CF=1       | if (i <= j);             |
| JNBE/JA     | 若不低于等于则跳转；若高于则跳转                   | jump if not below equal;jump if above                   | ZF=0或CF=0       | if (i > j);              |
| JL/JNGE     | 若小于则跳转；若不大于等于则跳转                   | jump if less;jump if not greater equal                  | SF != OF         | if (si < sj);            |
| JNL/JGE     | 若不小于则跳转；若大于等于则跳转；                 | jump if not less;jump if greater equal                  | SF = OF          | if (si >= sj);           |
| JLE/JNG     | 若小于等于则跳转；若不大于则跳转                   | jump if less equal;jump if not greater                  | ZF != OF 或 ZF=1 | if (si <= sj);           |
| JNLE/JG     | 若不小于等于则跳转；若大于则跳转                   | jump if not less equal;jump if greater                  | SF=0F 且 ZF=0    | if(si>sj)                |

## 串操作指令

 ### MOVS指令

字符串传送指令 MOVS

格式：MOVS OPRD1, OPRD2 MOVSB MOVSW

功能：OPRD1 < - - OPRD2

说明：

1. 其中OPRD2为源串符号地址，OPRD1为目的串符号地址

### STOS指令

字符串存储指令 STOS

格式：STOS OPRD

功能：把AL(字节)或AX(字)中的数据存储到DI为目的串地址指针所寻址的存储器单元中去指针DI将根据DF的值进行自动调整

### REP指令

重复前缀说明

格式：

- REP  ;CX<>0 重复执行字符串指令
- REPZ/REPE ;CX<>0 且ZF=1重复执行字符串指令
- REPNZ/REPNE ;CX<>0 且ZF=0重复执行字符串指令

功能：在串操作指令前加上重复前缀，可以对字符串进行重复处理。由于加上重复前缀后，对应的指令代码是不同的，所以指令的功能便具有重复处理的功能，重复的次数存放在CX寄存器中

## CALL与RETN

### CALL指令

过程调用指令 CALL

格式：CALL OPRD

功能：过程调用指令

相当于：

```x86asm
push eip
jmp OPRD
```

### RETN指令

	返回指令，相当于：

```x86asm
pop eip
jmp eip
```

## 函数

过程调用-函数

```x86asm
function proc
	code
function end
```

参数传递方式：

1. 寄存器传参
2. 堆栈传参

## WIN32汇编入门

什么是API？

API（Application Programming Interface, 应用程序接口）是一些预先定义的函数，或指软件系统不同组成部分衔接的约定。目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问原码，或理解内部工作机制的细节

Windows系统除了协调应用程序的执行、内存的分配、系统资源的管理外，同时他也是一个很大的服务中心。调用这个服务中心的各种服务（每一种服务就是一个函数）可以帮助应用程序达到开启视窗、描绘图形和使用周边设备等目的，由于这些函数服务的对象是应用程序，所以称之为Application Programming Interface, 简称API函数。WIN32 API也就是Microsoft Windows 32位平台的应用程序编程接口

凡是在 Windows 工作环境底下执行的应用程序，都可以调用Windows API
