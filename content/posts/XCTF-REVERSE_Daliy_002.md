+++ 
draft = false
date = 2022-09-04T11:30:27+08:00
title = "攻防世界 RE 日常练习 002"
tags = ["ctf","writeup","reverse"]
categories = ["practice"]
toc = true
+++
# 攻防世界 RE 日常练习 002

> 由于攻防世界界面改版，题目划分与之前的题目划分出现了差异，变成了难度划分形式，和之前的不太一样。为了之前形式一致，故更改为日常练习

新版攻防世界界面：

![image-20220903161040700](/images/XCTF-REVERSE_Daliy_002/image-20220903161040700.png)

本次把攻防世界难度为1的题目补了下，找下CTF题目的手感。不过攻防世界难度1的题目，难度真的很不一致。

## xxxorrr

这道题目应该是攻防世界的一道新题目，难度应该是比较低的。

首先使用die查看下程序信息：![image-20220903163539118](/images/XCTF-REVERSE_Daliy_002/image-20220903163539118.png)

amd64架构的程序，无壳，使用ida pro直接看：

![image-20220903163849241](/images/XCTF-REVERSE_Daliy_002/image-20220903163849241.png)

进入到主程序可以发现：

![image-20220903163921702](/images/XCTF-REVERSE_Daliy_002/image-20220903163921702.png)

程序非常简单，仅仅只是进行了异或操作。觉得这个程序就是简单的异或操作。寻找关键的数据：

![image-20220903164142513](/images/XCTF-REVERSE_Daliy_002/image-20220903164142513.png)

以为这样就结束了，使用脚本跑，结果跑不出来flag，于是只能继续分析程序代码，主程序前面有一个程序函数执行，可能有进行操作：

![image-20220903164827415](/images/XCTF-REVERSE_Daliy_002/image-20220903164827415.png)

跟进这个函数发现返回了一个可能是系统函数的一个函数

![image-20220903165048716](/images/XCTF-REVERSE_Daliy_002/image-20220903165048716.png)

使用搜索引擎查找这个函数得到这个函数的功能描述：

![image-20220903165237194](/images/XCTF-REVERSE_Daliy_002/image-20220903165237194.png)

说明这个函数会注册一个函数在主函数结束的时候进行调用，跟进注册的函数：

![image-20220903165937553](/images/XCTF-REVERSE_Daliy_002/image-20220903165937553.png)

发现一个比较有意思的函数，应该就是进行判断的关键函数，中间进行了比较。

由于对s1变量的操作存在有一定的怀疑，于是查找s1变量的交叉引用来查看信息找到一个交叉引用，这个交叉引用对s1进行操作

![image-20220903170211040](/images/XCTF-REVERSE_Daliy_002/image-20220903170211040.png)

继续跟进交叉引用，发现这个函数在init函数内部进行调用

![image-20220903170302861](/images/XCTF-REVERSE_Daliy_002/image-20220903170302861.png)

因此，s1变量进行了两次操作，根据原理编写exp：

```python
s1 =[
    0x71, 0x61, 0x73, 0x78, 0x63, 0x79, 0x74, 0x67, 0x73, 0x61, 
    0x73, 0x78, 0x63, 0x76, 0x72, 0x65, 0x66, 0x67, 0x68, 0x6E, 
    0x72, 0x66, 0x67, 0x68, 0x6E, 0x6A, 0x65, 0x64, 0x66, 0x67, 
    0x62, 0x68, 0x6E, 0x00
]

s2 = [
    0x56, 0x4E, 0x57, 0x58, 0x51, 0x51, 0x09, 0x46, 0x17, 0x46, 
    0x54, 0x5A, 0x59, 0x59, 0x1F, 0x48, 0x32, 0x5B, 0x6B, 0x7C, 
    0x75, 0x6E, 0x7E, 0x6E, 0x2F, 0x77, 0x4F, 0x7A, 0x71, 0x43, 
    0x2B, 0x26, 0x89, 0xFE, 0x00
]
print(len(s1))
print(len(s2))
flag = []
for i in range(34):
    flag.append(s1[i]^(2 * i + 65)^s2[i])

print(bytes(flag))
```

运行得到flag：`flag{c0n5truct0r5_functi0n_in_41f}`



## happyctf

题目给到了一个exe文件和一个pdb符号文件。使用die查看文件架构和文件信息：

![image-20220903171041190](/images/XCTF-REVERSE_Daliy_002/image-20220903171041190.png)

发现是一个PE32的命令行程序，无壳，使用ida直接看：

![image-20220903171221296](/images/XCTF-REVERSE_Daliy_002/image-20220903171221296.png)

加载程序的时候，也要加载pdb查看：

![image-20220903171509446](/images/XCTF-REVERSE_Daliy_002/image-20220903171509446.png)

看样子应该是一个C++的程序，很多程序名都已经被粉碎了，所在位置应该就是主函数的所在位置，直接F5来查看程序的伪代码：

![image-20220903171807207](/images/XCTF-REVERSE_Daliy_002/image-20220903171807207.png)

寻找到关键信息来进行突围，进行简单的代码审计发现：

![image-20220903172024279](/images/XCTF-REVERSE_Daliy_002/image-20220903172024279.png)

一串可疑的字符串，字符串应该是需要进行比较的字符串，但是这个字符串不像是flag的样子，应该需要进行一定的操作，发现字符串前面有一个lamda操作：

![image-20220903172340204](/images/XCTF-REVERSE_Daliy_002/image-20220903172340204.png)

跟进这个lamda操作的所在函数：

![image-20220904074208212](/images/XCTF-REVERSE_Daliy_002/image-20220904074208212.png)

发现有异或操作，这道题目的考点应该就是lamda函数的异或操作

根据异或操作编写exp：

```python
c = b'brxusoCqxw{yqK`{KZqag{r`i'
flag = []
for i in c:
    flag.append(i^0x14)
print(bytes(flag))
```

运行脚本得到flag，`vflag{Welcome_to_Neusoft}`

纯纯的签到题目

## crypt

题目名是密码，应该会涉及到加密的问题

使用die进行查看程序信息：

![image-20220904082301049](/images/XCTF-REVERSE_Daliy_002/image-20220904082301049.png)

PE64程序，无壳，直接丢进ida里面进行静态审计：

![image-20220904082656813](/images/XCTF-REVERSE_Daliy_002/image-20220904082656813.png)

整个程序写到挺简单的，没有复杂的逻辑

看样子是进行了异或操作。

前面有两个函数进行了加密操作，关键的应该就是这两个函数：

![image-20220904082948509](/images/XCTF-REVERSE_Daliy_002/image-20220904082948509.png)

两个函数，应该就是关键的加密函数，跟进函数进行审计：

![image-20220904083856381](/images/XCTF-REVERSE_Daliy_002/image-20220904083856381.png)

第一个函数的加密，在逻辑上还是稍微有些复杂的，至少我现在还审计不出来具体是什么样的加密算法。

看第二个函数的加密：

![image-20220904084312735](/images/XCTF-REVERSE_Daliy_002/image-20220904084312735.png)

第二个函数的加密对于输入的数据进行异或操作。最开始，尝试使用正向的方式进下加密后求解，发现出错的可能性相对来说比较高，因此，尝试思考另一种思路：

使用x64dbg动态调试，得到进行异或的数据，然后使用输入数据进行异或来获取需要进行异或的数据，再进行主程序的异或0x22来进行获取的flag。

根据这个思路进行调试：

在ida找到相应的地址位置

![image-20220904091610507](/images/XCTF-REVERSE_Daliy_002/image-20220904091610507.png)

然后再x64dbg定位的相应地址下断

![image-20220904092209344](/images/XCTF-REVERSE_Daliy_002/image-20220904092209344.png)

找到相应位置的数据，在内存中寻找：

![image-20220904092255232](/images/XCTF-REVERSE_Daliy_002/image-20220904092255232.png)

大致就是在这个位置的数据，将这个位置的数据复制出来，根据思路编写exp：

```python
c = 'EB 98 42 2B CF 7C DC 23 2F 57 6D 5C BD 0D A7 78 CC 45 EE 72 EB 45 00 00'.split(' ')

target = [
  158, 231,  48,  95, 167,   1, 166,  83,  89,  27, 
   10,  32, 241, 115, 209,  14, 171,   9, 132,  14, 
  141,  43
]
flag = []
for i in range(22):
    flag.append(target[i]^0x22^(int(c[i], 16)^0x31))
print(bytes(flag))
```

运行脚本得到flag，`b'flag{nice_to_meet_you}'`



## 1000Click

题目应该是要点击1000次才会给出flag，首先使用die查看程序信息：

![image-20220904093023881](/images/XCTF-REVERSE_Daliy_002/image-20220904093023881.png)

PE32程序，无壳。使用64dbg进行调试找下位置进行下断修改数据，这是最开始的思路。这个思路相对来说是要进行定位。但是谁能想到flag就写在内存里面了呢？

![image-20220904093301219](/images/XCTF-REVERSE_Daliy_002/image-20220904093301219.png)

这样就get到了flag，`flag{TIBntXVbdZ4Z9VRtoOQ2wRlvDNIjQ8Ra}`



## reverse_re3

看下题目描述：

![image-20220904093625659](/images/XCTF-REVERSE_Daliy_002/image-20220904093625659.png)

看题目描述，应该是一道迷宫题目，使用die进行查看：

![image-20220904095725987](/images/XCTF-REVERSE_Daliy_002/image-20220904095725987.png)

ELF程序，无壳，拖进ida pro直接看

![image-20220904102213621](/images/XCTF-REVERSE_Daliy_002/image-20220904102213621.png)

主程序非常简单关键的应该是函数sub_940

跟进这个函数：

![image-20220904102511806](/images/XCTF-REVERSE_Daliy_002/image-20220904102511806.png)

应该是一个走迷宫的程序，ascii为97是`a` ，ascii为100是`d`，ascii为115是`s`，ascii为119是`w` 。这些操作就是很多小游戏都会采用到的移动操作，可以判断应该是一道迷宫题。既然是迷宫题，地图会在哪里，函数最开始有一个sub_86c的函数，不会根据我们的输入而进行修改，这个函数应该是一个初始化函数，跟进函数查看下信息：

![image-20220904105505287](/images/XCTF-REVERSE_Daliy_002/image-20220904105505287.png)

这个应该是迷宫数组，跟进查看发现果真就是迷宫数组的信息：

![image-20220904105641637](/images/XCTF-REVERSE_Daliy_002/image-20220904105641637.png)

看样子应该就是迷宫数组，下面应该是跟进迷宫逻辑：

![image-20220904110437184](/images/XCTF-REVERSE_Daliy_002/image-20220904110437184.png)

在初始化的过程中进行了入口点寻找来找到入口点，也就迷宫的起点位置。

进行看操作逻辑：

![image-20220904110720487](/images/XCTF-REVERSE_Daliy_002/image-20220904110720487.png)

如果是1就更新一下所在位置，重新设置下起点，如果是4返回1，都不是就返回0。

返回的1和0是什么意思？需要看一下主函数逻辑：

![image-20220904110931642](/images/XCTF-REVERSE_Daliy_002/image-20220904110931642.png)

根据主函数的逻辑，如果返回1就说明走到终点了，进入到下一层。相当于就是三层迷宫，走三遍路径，根据走出迷宫的操作数来获取相应的flag数据。所以这题还是要将迷宫的地图输出出来，看看究竟是什么样子：

```text
[1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[1, 1, 1, 1, 1, 0, 3, 1, 1, 0, 0, 0, 0, 0, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0]
[1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 4, 0]
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
[1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[1, 1, 0, 3, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0]
[1, 1, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0]
[1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0]
[1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 0]
[1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0]
[1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0]
[1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0]
[1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0]
[1, 1, 0, 1, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0]
[1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0]
[1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0]
[1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 4, 0]
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 3, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
[0, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0]
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 4, 0]
```

这个迷宫，如果写代码跑需要寻路算法来跑出来，手动走的话会比写程序快很多。因此，本菜鸡采用手动走迷宫的方式进行寻路，来找到路径，最终走出来的路径是：`ddsssddddsssdssdddddsssddddsssaassssdddsddssddwddssssssdddssssdddss`

然后将这个数据加密成md5得到flag：`flag{aeea66fcac7fa80ed8f79f38ad5bb953}`



## lucknum

题目纯纯签到题，使用die进行查看：

![image-20220904112557450](/images/XCTF-REVERSE_Daliy_002/image-20220904112557450.png)

ELF程序，AMD64架构的，无壳程序。

这道题目，可以直接丢进hex editor进行查找flag来得到flag。也可以使用ida pro直接看，本质上没有多大区别。

这里直接使用ida看：

![image-20220904112855205](/images/XCTF-REVERSE_Daliy_002/image-20220904112855205.png)

flag直接就有了，flag是`flag{c0ngr@tul@ti0n_f0r_luck_numb3r}`