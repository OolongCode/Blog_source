+++
draft = false
date = 2022-08-31T17:28:44+08:00
title = "2022 网鼎杯 青龙组 Writeup"
math = true
tags = ["writeup",  "ctf"]
categories = ["challange" ]
toc = true
+++

# 2022 网鼎杯 青龙组 Writeup

今年网鼎杯的逆向题目是相对来说比较简单的，在比赛的时候做出了两道逆向题目。

第二道逆向apk的题目找到文章了，但是去看Crypto的题目了，也就没有认真去钻逆向的那道题目。

Crypto题目由于自身的数学敏感度不够没能解出，还需要继续提高

## 解出

### fakeshell

题目给到了一个exe文件，使用die查看该文件：

![image-20220828123956402](/images/2022_WDB_QL-writeup/image-20220828123956402.png)

发现是一个upx压缩壳加密的程序，尝试使用upx脱壳：

![image-20220828133038392](/images/2022_WDB_QL-writeup/image-20220828133038392.png)

发现壳可能被修改过，需要手动脱壳。先尝试运行下程序，寻找下可能存在的锚点字符串信息：

![image-20220828140533175](/images/2022_WDB_QL-writeup/image-20220828140533175.png)

找到两个锚点字符串，一个是`<<Input your flag:`，另一个是`Wrong.`

使用x64dbg进行手动脱壳，进行入口点的测试找到一个关键跳转地址0x1400276CB

![image-20220828140317972](/images/2022_WDB_QL-writeup/image-20220828140317972.png)

这是一个长跳转指令，运行到这个指令的时候程序已经完成解密。可以对程序的内存引用进行查看：

![image-20220828140739058](/images/2022_WDB_QL-writeup/image-20220828140739058.png)

发现程序的内存引用中已经出现了锚点字符串，说明此时的程序已经完成了解密。然后跟进到锚点字符串所在的函数位置：

![image-20220828140905779](/images/2022_WDB_QL-writeup/image-20220828140905779.png)

这个位置应该是主函数空间，在这个主函数空间中对函数开头位置下断点，然后让程序运行到断点位置

![image-20220828141834266](/images/2022_WDB_QL-writeup/image-20220828141834266.png)

然后使用x64dbg的Scylla插件进行dump内存：

![image-20220828142149592](/images/2022_WDB_QL-writeup/image-20220828142149592.png)

从当前位置进行dump即可，得到一个dump的程序。

现在完成手动脱壳，进入到下一步，进行ida pro的静态分析：

![image-20220828142350437](/images/2022_WDB_QL-writeup/image-20220828142350437.png)

使用F5插件来查看程序反编译的代码进行分析

![image-20220828142457029](/images/2022_WDB_QL-writeup/image-20220828142457029.png)

输入的数据存储到v4变量中，对v4变量处理的函数有两个，对这两个函数依次进行审计。



首先分析第一个函数，这个函数要一直跟进跟进到和参数有关的位置

![image-20220828142835318](/images/2022_WDB_QL-writeup/image-20220828142835318.png)

第一个函数中，存在一个判断和一个异或运算。

判断是对输入的数据进行了处理，猜测应该是进行长度判断，判断长度是否是20位。

异或操作是单纯对于输入数据的运行对于程序运行流程没有太多影响



分析第二个函数，和第一个函数的跟进方法类似

![image-20220828144648807](/images/2022_WDB_QL-writeup/image-20220828144648807.png)

第二个函数同样也有一个异或操作和一个特殊的函数，跟进这个函数查看：

![image-20220828145157336](/images/2022_WDB_QL-writeup/image-20220828145157336.png)

发现判断的位置有密文，跟进密文查看：

![image-20220828145310441](/images/2022_WDB_QL-writeup/image-20220828145310441.png)

使用`shift`+ `E`将数据提取出来，可以得到：

```c
unsigned char ida_chars[] =
{
  0x4B, 0x00, 0x00, 0x00, 0x48, 0x00, 0x00, 0x00, 0x79, 0x00, 
  0x00, 0x00, 0x13, 0x00, 0x00, 0x00, 0x45, 0x00, 0x00, 0x00, 
  0x30, 0x00, 0x00, 0x00, 0x5C, 0x00, 0x00, 0x00, 0x49, 0x00, 
  0x00, 0x00, 0x5A, 0x00, 0x00, 0x00, 0x79, 0x00, 0x00, 0x00, 
  0x13, 0x00, 0x00, 0x00, 0x70, 0x00, 0x00, 0x00, 0x6D, 0x00, 
  0x00, 0x00, 0x78, 0x00, 0x00, 0x00, 0x13, 0x00, 0x00, 0x00, 
  0x6F, 0x00, 0x00, 0x00, 0x48, 0x00, 0x00, 0x00, 0x5D, 0x00, 
  0x00, 0x00, 0x64, 0x00, 0x00, 0x00, 0x64, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00
};
```

显然是以4字节，小端序存储的

现在获取的密文和两次运算，编写脚本：

```python
c = [
  0x4B, 0x00, 0x00, 0x00, 0x48, 0x00, 0x00, 0x00, 0x79, 0x00, 
  0x00, 0x00, 0x13, 0x00, 0x00, 0x00, 0x45, 0x00, 0x00, 0x00, 
  0x30, 0x00, 0x00, 0x00, 0x5C, 0x00, 0x00, 0x00, 0x49, 0x00, 
  0x00, 0x00, 0x5A, 0x00, 0x00, 0x00, 0x79, 0x00, 0x00, 0x00, 
  0x13, 0x00, 0x00, 0x00, 0x70, 0x00, 0x00, 0x00, 0x6D, 0x00, 
  0x00, 0x00, 0x78, 0x00, 0x00, 0x00, 0x13, 0x00, 0x00, 0x00, 
  0x6F, 0x00, 0x00, 0x00, 0x48, 0x00, 0x00, 0x00, 0x5D, 0x00, 
  0x00, 0x00, 0x64, 0x00, 0x00, 0x00, 0x64, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00 ]
c = c[::4]
flag = [0]*0x14
for i in range(0x14):
    flag[i] = chr(((c[i]^0x50)-10)^0x66)
print("".join(flag))
```

运行得到flag：

```text
why_m0dify_pUx_SheLL
```



### Handmake

这道题目是手工，应该是需要手工寻找一些内容

题目给到了一个go程序的源码：

![image-20220828150430370](/images/2022_WDB_QL-writeup/image-20220828150430370.png)

总共有大约15w行的代码，尝试编译运行：

![image-20220828150855335](/images/2022_WDB_QL-writeup/image-20220828150855335.png)

应该是英文的阅读理解找函数的题目，理解这两句话的意思就可以求解。

`Input the first function, which has 6 parameters and the third named gLIhR: `

这句话就是找到一个有六个参数的函数而且第三个参数是`gLIhR`



`Input the second function, which has 3 callers and invokes the function named cHZv5op8rOmlAkb6:`

这句话就是找到一个函数，这个函数被调用了3次并且调用了一个名为`cHZv5op8rOmlAkb6`函数。



根据要求去寻找函数名：

![image-20220828151548954](/images/2022_WDB_QL-writeup/image-20220828151548954.png)

然后进行程序的测试就可以得到flag：

![image-20220828151706620](/images/2022_WDB_QL-writeup/image-20220828151706620.png)





## 复现



### Crypto091

这道题目被队友解答，我就没有进行看了，赛后进行复现下

![image-20220828152017556](/images/2022_WDB_QL-writeup/image-20220828152017556.png)

这道题目要寻找到关键信息，本道题目的关键信息有：

```text
170号段首批放号的联通号码

Hash：
c22a563acc2a587afbfaaaa6d67bc6e628872b00bd7e998873881f7c6fdc62fc

flag格式：flag{13位电话号码（纯数字，含国家代码）}
```

使用搜索引擎进行查找：

![image-20220828153105747](/images/2022_WDB_QL-writeup/image-20220828153105747.png)

1709是首批号码段，下面是对hash的类型进行猜测，首先看看哈希的长度：

![image-20220828153953231](/images/2022_WDB_QL-writeup/image-20220828153953231.png)

hash数据的长度是64位可能是sha256加密，可以使用hacker工具Hashcat进行爆破求解：

```bash
hashcat -m 1400 -a 3 c22a563acc2a587afbfaaaa6d67bc6e628872b00bd7e998873881f7c6fdc62fc 861709?d?d?d?d?d?d?d
```

运行得到：

![image-20220828155215870](/images/2022_WDB_QL-writeup/image-20220828155215870.png)

由此可以得到tel是8617091733716，故flag是`flag{8617091733716}`



### grasshopper

>  这道题目当时代码没有审计清楚，采用Z3-solver进行求解，未能求解成功

题目给到一个python源码：

```python
from Crypto.Util.number import *
from random import randrange
from grassfield import flag

p = getPrime(16)

k = [randrange(1,p) for i in range(5)]

for i in range(len(flag)):
    grasshopper = flag[i]
    for j in range(5):
        k[j] = grasshopper = grasshopper * k[j] % p
    print('Grasshopper#'+str(i).zfill(2)+':'+hex(grasshopper)[2:].zfill(4))
```

进行审计可以发现：


$$
k_{10} = g_0 =  C_0 \cdot k_{00}\ \text{mod}\ p \\\\
k_{11} = g_0 =  C_0 \cdot k_{00} \cdot k_{01}\ \text{mod}\ p \\\\
k_{12} = g_0 =  C_0 \cdot k_{00} \cdot k_{01}\cdot k_{02}\ \text{mod}\ p \\\\
k_{13} = g_0 =  C_0 \cdot k_{00} \cdot k_{01}\cdot k_{02}\cdot k_{03}\ \text{mod}\ p \\\\
k_{14} = g_0 =  C_0 \cdot k_{00} \cdot k_{01}\cdot k_{02}\cdot k_{03} \cdot k_{04}\ \text{mod}\ p \\\\
$$

$$
k_{20} = g_0 =  C_1 \cdot k_{10}\ \text{mod}\ p \\\\
k_{21} = g_0 =  C_1 \cdot k_{10} \cdot k_{11}\ \text{mod}\ p \\\\
k_{22} = g_0 =  C_1 \cdot k_{10} \cdot k_{11}\cdot k_{12}\ \text{mod}\ p \\\\
k_{23} = g_0 =  C_1 \cdot k_{10} \cdot k_{11}\cdot k_{12}\cdot k_{13}\ \text{mod}\ p \\\\
k_{24} = g_0 =  C_1 \cdot k_{10} \cdot k_{11}\cdot k_{12}\cdot k_{13} \cdot k_{14}\ \text{mod}\ p \\\\
$$
可以发现有规律存在，已知$k_{14}\ k_{24}\ k_{34}\ k_{44}\ k_{54}$ 可以推出$k_{23}\ k_{33}\ k_{43}\ k_{53}$

同理可以推出$k_{32}\ k_{42}\ k_{52}$，依此可以推出$k_{41}\ k_{51}$ 和$k_{50}$

推导公式：
$$
k_{ij} = k_{i(j+1)} \cdot k_{(i-1)j}^{-1}\ \text{mod}\ p
$$
根据推导公式可以进行求解：

```python
from gmpy2 import invert,gcd
from sympy import isprime
know = b'flag{'

g = []
with open('./output.txt','r') as f:
    for line in f.readlines():
        line = line.strip('\n')
        g.append(int(line[-4:],16))

maxg = max(g)

for p in range(maxg+1, 2**16):
    if isprime(p):
        g04 = g[0]
        g14 = g[1]
        g13 = g14 * invert(g04, p) % p
        g24 = g[2]
        g23 = g24 * invert(g14, p) % p
        g22 = g23 * invert(g13, p) % p
        g34 = g[3]
        g33 = g34 * invert(g24, p) % p
        g32 = g33 * invert(g23, p) % p
        g31 = g32 * invert(g22, p) % p
        g44 = g[4]
        g43 = g44 * invert(g34, p) % p
        g42 = g43 * invert(g33, p) % p
        g41 = g42 * invert(g32, p) % p
        g40 = g41 * invert(g31, p) % p

        k = [g40, g41, g42, g43, g44]
        flag = ''
        for i in range(5,42):
            _k = k[0]*k[1]*k[2]*k[3]*k[4]
            alp = g[i] * invert(_k, p) % p
            flag += chr(alp)
            for j in range(5):
                k[j] = alp = alp * k[j] % p

        if flag.endswith('}'):
            print(f'p = {p}')
            print(b'flag{'+flag.encode())
```

运行代码得到：

```text
p = 59441
b'flag{749d39d4-78db-4c55-b4ff-bca873d0f18e}'
```

这种思路是由局部推断整体的想法，可能将题目的过程复杂化，而显得不是非常优雅。

---

此题的求解脚本还可以使用sagemath进行相应的脚本求解，思路也是大致一样的思路。

构造一个p的整数环，来进行运算求解，这种求解方法反而会更加简单，sagemath求解的脚本：

```python
g = []
with open('./output.txt','r') as f:
    for line in f.readlines():
        line = line.strip('\n')
        g.append(int(line[-4:],16))

maxg = max(g)
for p in range(maxg, 2^16):
    if is_prime(p):
        flag = g
        F = GF(p)
        for _ in range(5):
            data = []
            for j in range(len(flag)-1):
                data.append(F(flag[j+1])/F(flag[j]))
            flag = data
            try:
                print(b'flag{'+bytes(data))
            except:
                pass
```

运行脚本就可以得到flag，`b'flag{749d39d4-78db-4c55-b4ff-bca873d0f18e}'`

这种脚本的思路就是直接进行整体运算来求解。



### david_homework

>  这道题目是当时考虑是将递归转循环来进行求解，但是时间复杂度还是很高，不能正常进行求解。想到线性代数的提示，时间已经所剩无几，故未能完成求解

题目说到是一个线性代数的作业，应该是一个线性代码相关的问题求解。

赛题给到的代码是一个含有递归的函数：

![image-20220829095725043](/images/2022_WDB_QL-writeup/image-20220829095725043.png)

通过搜索引擎搜索关键信息

1. 搜索 `递归函数转线代` : [(191条消息) 线性代数求解递推形式数列的通项公式_wdq347的博客-CSDN博客](https://blog.csdn.net/wdq347/article/details/8919645)
2. 搜索 `递推 线代` : [线性代数-递推法解“数字型”行列式_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV18p411o7vc/)
3. 搜索 `线性代数-递推法解“数字型”行列式` : [递推公式法在行列式计算中的证明与应用 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/37683681)
4. 搜索 `递推公式法 行列式 `

找到文章：[怎么用递归法计算行列式? - 知乎 (zhihu.com)](https://www.zhihu.com/question/426892843)

根据文章：

![image-20220829101251970](/images/2022_WDB_QL-writeup/image-20220829101251970.png)

显然这道题目是要求求解出逆过程，首先需要抽象出递推函数的递归公式：

$$
f(x,cof) = \left\\\{
\begin{array}{l}
x+1 & & {x < 3} \\\\
cof_2\cdot f(x-3) + cof_1\cdot f(x-2) + cof_0\cdot f(x-1) & & {x \geq 3}
\end{array}
\right.
$$


根据查找到的一系列文章中的[(191条消息) 线性代数 —— 线性递推关系_Alex_McAvoy的博客-CSDN博客_线性递推关系](https://blog.csdn.net/u011815404/article/details/99621516)，可以看到：

![image-20220829102307637](/images/2022_WDB_QL-writeup/image-20220829102307637.png)

根据关系可以得到

$$
f(n) = A * f(n-1) 
$$

$$
A = \left[\begin{array}{ccc}
1 & 0 & 0 \\\\
0 & 1 & 0 \\\\
0 & 0 & 1 \\\\
cof_2 & cof_1 & cof_0 
\end{array} \right]
$$

$$
f(n-1) = \left[
\begin{array}{l}
f(n-3) \\\\
f(n-2) \\\\
f(n-1) \\\\
\end{array}
\right]
$$

紧接着根据这个：

![image-20220829104119031](/images/2022_WDB_QL-writeup/image-20220829104119031.png)

可以得到：
$$
F(n) = A^{n-2}\cdot f(2)
$$

$$
f(2) = \left[
\begin{array}{l}
f(0)\\\\
f(1) \\\\
f(2) \\\\
\end{array}
\right]
= \left[
\begin{array}{l}
1\\\\
2 \\\\
3 \\\\
\end{array}
\right]
$$

尝试使用sagemath求解，出现报错

![image-20220829113116331](/images/2022_WDB_QL-writeup/image-20220829113116331.png)

进行次方运算必须是一个方块矩阵，也就是矩阵的维度必须是2x2, 3x3 … n x n 的矩阵，进行运算推导下：
$$
f(n) = A * f(n-1) = \left[
\begin{array}{c}
f(n-3) \\\\
f(n-2) \\\\
f(n-1) \\\\
cof_2\cdot f(n-3) + cof_1\cdot f(n-2) + cof_0\cdot f(n-1)
\end{array}
\right]
$$


继续进行推导：
$$
f(n-1) = A * f(n-2) = \left[
\begin{array}{c}
f(n-4) \\\\
f(n-3) \\\\
f(n-2) \\\\
cof_2\cdot f(n-4) + cof_1\cdot f(n-3) + cof_0\cdot f(n-2)
\end{array}
\right]
$$

$$
A * f(n-1) \\\\
= \left[\begin{array}{l}
1 & 0 & 0 \\\\
0 & 1 & 0 \\\\
0 & 0 & 1 \\\\
cof_2 & cof_1 & cof_0 
\end{array} \right] \cdot \left[
\begin{array}{c}
f(n-4) \\\\
f(n-3) \\\\
f(n-2) \\\\
cof_2\cdot f(n-4) + cof_1\cdot f(n-3) + cof_0\cdot f(n-2)
\end{array}
\right]
$$

此时无法进行运算，进行推导发现将A修改成如下形式运算结果不变：

$$
A = \left[
\begin{array}{ccc}
0 & 1 & 0 \\\\
0 & 0 & 1 \\\\
cof_2 & cof_1 & cof_0
\end{array}
\right]
$$

再次尝试编写sage代码进行运算：

```python
# sagemath
from hashlib import md5, sha256
from binascii import unhexlify

cof_t = [
    [353, -1162, 32767], [206, -8021, 42110], [262, -7088, 31882],
    [388, -6394, 21225], [295, -9469, 44468], [749, -3501, 40559],
    [528, -2690, 10210], [354, -5383, 18437], [491, -8467, 26892],
    [932, -6984, 20447], [731, -6281, 11340], [420, -5392, 44071],
    [685, -6555, 40938], [408, -8070, 47959], [182, -9857, 49477],
    [593, -3584, 49243], [929, -7410, 31929], [970, -4549, 17160],
    [141, -2435, 36408], [344, -3814, 18949], [291, -7457, 40587],
    [765, -7011, 32097], [700, -8534, 18013], [267, -2541, 33488],
    [249, -8934, 12321], [589, -9617, 41998], [840, -1166, 22814],
    [947, -5660, 41003], [206, -7195, 46261], [784, -9270, 28410],
    [338, -3690, 19608], [559, -2078, 44397], [534, -3438, 47830],
    [515, -2139, 39546], [603, -6460, 49953], [234, -6824, 12579],
    [805, -8793, 36465], [245, -5886, 21077], [190, -7658, 20396],
    [392, -7053, 19739], [609, -5399, 39959], [479, -8172, 45734],
    [321, -7102, 41224], [720, -4487, 11055], [208, -1897, 15237],
    [890, -4427, 35168], [513, -5106, 45849], [666, -1137, 23725],
    [755, -6732, 39995], [589, -6421, 43716], [866, -3265, 30017],
    [416, -6540, 34979], [840, -1305, 18242], [731, -6844, 13781],
    [561, -2728, 10298], [863, -5953, 23132], [204, -4208, 27492],
    [158, -8701, 12720], [802, -4740, 16628], [491, -6874, 29057],
    [531, -4829, 29205], [363, -4775, 41711], [319, -9206, 46164],
    [317, -9270, 18290], [680, -5136, 12009], [880, -2940, 34900],
    [162, -2587, 49881], [997, -5265, 20890], [485, -9395, 23048], 
    [867, -1652, 18926], [691, -7844, 11180], [355, -5990, 13172], 
    [923, -2018, 23110], [214, -4719, 23005], [921, -9528, 29351], 
    [349, -7957, 20161], [470, -1889, 46170], [244, -6106, 23879], 
    [419, -5440, 43576], [930, -1123, 29859], [151, -5759, 23405], 
    [843, -6770, 36558], [574, -6171, 33778], [772, -1073, 44718], 
    [932, -4037, 40088], [848, -5813, 27304], [194, -6016, 39770], 
    [966, -6789, 14217], [219, -6849, 40922], [352, -6046, 18558], 
    [794, -8254, 29748], [618, -5887, 15535], [202, -9288, 26590], 
    [611, -4341, 46682], [155, -7909, 16654], [935, -5739, 39342], 
    [998, -6538, 24363], [125, -5679, 36725], [507, -7074, 15475], 
    [699, -5836, 47549]
    ]
B = [[1], [2], [3]]
B = matrix(B)
s = 0
for i in range(100):
    A = [[0,1,0],[0,0,1], cof_t[i][::-1]]
    A = matrix(A)
    C = A ^ (200000-2) * B
    s += C[2][0]
s = str(s)[-2000:-1000]
key = unhexlify(md5(s.encode()).hexdigest())
check = sha256(key).hexdigest()
verify = '2cf44ec396e3bb9ed0f2f3bdbe4fab6325ae9d9ec3107881308156069452a6d5'
print(f's= {s}')
print(f'key = {key}')
print(f'hash(key) = {check}')
print(f'check = {check==verify}')
```

运行sagemath脚本得到：

```text
s= 8365222366127410597598169954399481033882921410074214649102398062373189165630613993923060190128768377015697889610969869189338768501949778819512483009804114510646333513147157016729806311717181191848898389803672575716843797638777123435881498143998689577186959772296072473194533856870919617472555638920296793205581043222881816090693269730028856738454951305575065708823347157677411074157254186955326531403441609073128679935513392779152628590893913048822608749327034655805831509883357484164977115164240733564895591006693108254829407400850621646091808483228634435805213269066211974452289769022399418497986464430356041737753404266468993201044272042844144895601296459104534111416147795404108912440106970848660340526207025880755825643455720871621993251258247195860214917957713359490024807893442884343732717743882154397539800059579470352302688717025991780505564794824908605015195865226780305658376169579983423732703921876787723921599023795922881747318116849413935343800909756656082327558085457335537828343666748
key = b'S29O\x9a\xf3Z\x87\xe5\xbc{?D`xB'
hash(key) = 2cf44ec396e3bb9ed0f2f3bdbe4fab6325ae9d9ec3107881308156069452a6d5
check = True
```

现在已经获取到了密钥key，使用python进行aes解密即可。

> **注意：！！！**
>
> 如果使用的是Python3.10版本，可能会出现报错：
>
> `SystemError: PY_SSIZE_T_CLEAN macro must be defined for '#' formats`
>
> 这种情况是由于pycrypto库导致的问题，进行求解可以尝试安装miniconda创建python虚拟环境，切换到python3.8安装pycrypto库进行求解就可以正常求解

求解python脚本：

```python
from hashlib import md5, sha256
from binascii import unhexlify
from Crypto.Cipher import AES
s = '836522236612741059759816995439948103388292141007421464910239806237318916563061399392306019012876837701569788\
96109698691893387685019497788195124830098041145106463335131471570167298063117171811918488983898036725757168437976\
38777123435881498143998689577186959772296072473194533856870919617472555638920296793205581043222881816090693269730\
02885673845495130557506570882334715767741107415725418695532653140344160907312867993551339277915262859089391304882\
26087493270346558058315098833574841649771151642407335648955910066931082548294074008506216460918084832286344358052\
13269066211974452289769022399418497986464430356041737753404266468993201044272042844144895601296459104534111416147\
79540410891244010697084866034052620702588075582564345572087162199325125824719586021491795771335949002480789344288\
43437327177438821543975398000595794703523026887170259917805055647948249086050151958652267803056583761695799834237\
32703921876787723921599023795922881747318116849413935343800909756656082327558085457335537828343666748'

key = unhexlify(md5(s.encode()).hexdigest())
aes = AES.new(key,AES.MODE_ECB)
c = '4f12b3a3eadc4146386f4732266f02bd03114a404ba4cb2dabae213ecec451c9d52c70dc3d25154b5af8a304afafed87'
print(aes.decrypt(unhexlify(c)).strip(b'\x00'))
```

运行脚本即可得到相应的flag: `b'flag{519427b3-d104-4c34-a29d-5a7c128031ff}'`



### whereiscode

>  这道题目当时是找到文章了但是当时去看密码学题目了，没有细看这道题目

题目给到了一个apk程序文件，但是主程序里面没有程序的源代码：

![image-20220829185549101](/images/2022_WDB_QL-writeup/image-20220829185549101.png)

感觉没有什么思路去处理，对程序的具体结构仔细分析：

![image-20220829185730520](/images/2022_WDB_QL-writeup/image-20220829185730520.png)

发现有一个类似于壳的代码程序，尝试进行搜索找到了壳的相关博客和程序源码。

dpt壳程序文章：[Android函数抽取壳的实现 - luoyesiqiu - 博客园 (cnblogs.com)](https://www.cnblogs.com/luoyesiqiu/p/dpt.html)

dpt壳源码：[luoyesiqiu/dpt-shell: Android函数抽取壳实现 (github.com)](https://github.com/luoyesiqiu/dpt-shell)

下载壳工具的release版本，进行反编译查看dpt.jar java包文件，可以使用jadx进行反编译查看内容，找到dpt函数进行代码逻辑的审计

![image-20220829190248720](/images/2022_WDB_QL-writeup/image-20220829190248720.png)

根据文章，这个壳是一个函数抽取壳会进行函数的抽取壳：

![image-20220829190759843](/images/2022_WDB_QL-writeup/image-20220829190759843.png)

文章也把壳运行的整个流程逻辑写出来了：

![image-20220829190732698](/images/2022_WDB_QL-writeup/image-20220829190732698.png)

根据文章的信息，这道题目首先的一个子任务就是脱壳，而脱壳目前来说的目标就是对整个壳程序的逻辑进行分析。对整个流程大致分析下，发现主要是对于Dex进行操作，首先可以在dpt函数中搜索Dex关键字来定位到函数位置：

![image-20220829191309699](/images/2022_WDB_QL-writeup/image-20220829191309699.png)

找到一个关键的函数，跟进这个函数，可以找到些许信息

![image-20220829191405666](/images/2022_WDB_QL-writeup/image-20220829191405666.png)

这个函数打开了一个`OoooooOooo`的文件，这是一个关键的文件信息，可能依据文章中的逻辑找到原来字节码的存储位置。这个函数不长，简单来说就是对原有的Dex进行提取，然后将提取的Dex写入到这个文件中，主要是写入操作的具体实现

![image-20220829192021242](/images/2022_WDB_QL-writeup/image-20220829192021242.png)

跟进`writeMultiDexCode`查看内部实现情况：

![image-20220829192849282](/images/2022_WDB_QL-writeup/image-20220829192849282.png)

感觉是比较类似于pe结构的实现方式，也就是找到相应的位置进行求解

应该就是类似的一种结构，知道相应的函数偏移位置和相应的数据大小来对dex文件的函数进行抽取，根据这个原理可以进行PE文件结构类似的操作方式进行处理。编写一个小的程序将抽取出来的数据重新填充到相应的位置中。

可以编写一个python脚本进行脱壳处理：

```python
from struct import unpack

with open('./classes2.dex','rb') as f:
    data = f.read()
with open('./OoooooOooo', 'rb') as f:
    raw = f.read()

print(f"version = {unpack('<H',raw[0:2])[0]}")
print(f"dexcount = {unpack('<H',raw[2:4])[0]}")
print(f"dexCodesIndex1 = {unpack('<I',raw[4:8])[0]}")
print(f"dexCodesIndex2 = {unpack('<I',raw[8:12])[0]}")
pos = 0x10
while(True):
    method_index = unpack('<I',raw[pos:pos+4])[0]
    pos += 4
    offset_dex = unpack('<I',raw[pos:pos+4])[0]
    pos += 4
    instruction_data_size = unpack('<I',raw[pos:pos+4])[0]
    pos += 4
    instruction_data = raw[pos:pos+instruction_data_size]
    pos += instruction_data_size
    data = data[:offset_dex]+instruction_data + data[offset_dex+instruction_data_size:]
    if pos == len(raw):
        break

with open('./classes2_dump.dex', 'wb') as f:
    f.write(data)
```

脱壳后的程序，可以使用dex2jar程序进行解包，可以得到一个jar包程序：

![image-20220830160534943](/images/2022_WDB_QL-writeup/image-20220830160534943.png)

可以直接解压得到一个文件夹，文件夹里面含有程序的class文件：

![image-20220830160400390](/images/2022_WDB_QL-writeup/image-20220830160400390.png)

这个文件结构是Android程序包的文件结构，我们关注的点应该就是com文件夹，进入到com\example\nothingcode文件夹中：

![image-20220830161024487](/images/2022_WDB_QL-writeup/image-20220830161024487.png)

这两个核心文件应该就是程序的主要文件，使用jd-gui工具进行java字节码文件的反编译来获取到java源码文件来看到程序的真实代码：

![image-20220830161253724](/images/2022_WDB_QL-writeup/image-20220830161253724.png)

直接将程序保存成一个java源代码文件

![image-20220830161338787](/images/2022_WDB_QL-writeup/image-20220830161338787.png)

点击Save来保存反编译的源码

使用java ide进行源码的审计，打开生成的源代码文件，一看多少会有些惊讶：

![image-20220830161646007](/images/2022_WDB_QL-writeup/image-20220830161646007.png)

有很多混淆和switch，应该是控制流平坦化的方式进行代码混淆的。这种混淆方式主要是由于一个发生器来形成switch结构，来混淆整个程序的主要流程结构。这种混淆的解决方式主要是脚本去混淆和静态代码审计来获取到主要的程序流程。

尝试寻找些未进行混淆的字符串，来确定大致的逻辑特征：

![image-20220830162651236](/images/2022_WDB_QL-writeup/image-20220830162651236.png)

可以看出flag的数据应该是位于password的输入位置，往下继续寻找发现：

![image-20220830224828585](/images/2022_WDB_QL-writeup/image-20220830224828585.png)

发现，check函数对str2和str3进行了处理，而str2和str3就对应的是两个输入框的内容，根据一般输入行的原理，应该是第一个输入行是用户名，而第二个输入行是密码。进入到check函数中一探究竟：

![image-20220830225238183](/images/2022_WDB_QL-writeup/image-20220830225238183.png)

字符串str2有一个等于判断，判断str2是不是等于`“helloctf”` 这显然像是一个用户名，而不是密码。由于密码是flag这个设定，所以这个str2的形式显然不像是flag的形式。下面寻找字符串str3的操作：

![image-20220830230118538](/images/2022_WDB_QL-writeup/image-20220830230118538.png)

找到str3的相关操作，并且找到一组数据，暂时不清楚这组数据有什么用。这个操作显然是把str3转成一个Int数组的操作，跟进函数查看具体操作情况：

![image-20220830230758071](/images/2022_WDB_QL-writeup/image-20220830230758071.png)

函数内部有两个操作，一个是判断长度的操作，另一个是除法操作。应该是进行Int数组的初始化处理，继续探究后续操作：

![image-20220830231328844](/images/2022_WDB_QL-writeup/image-20220830231328844.png)

进行大端序处理的操作，以及后面有迭代的操作，根据目前分析的情况。可以判断这个函数就是将字符串以大端序的方式转成Int数组。

这个函数分析完毕继续探究以下的内容：

![image-20220830231925973](/images/2022_WDB_QL-writeup/image-20220830231925973.png)

发现非常有趣的特征，一个特征是`<32`说明可能有32轮次，后面两个运算方程。这些特征和TEA加密算法的特征类似。尝试进行TEA算法的比对，TEA加密算法有32轮次加密并且每轮加密的算法也是相应的一致。由此可以判断这个加密算法应该就是TEA加密算法。

现在找到了加密算法，一个加密算法应该有相应的密钥和相应的密文以及明文。由于我们输入的内容就是要加密的明文，因此找到密文和密钥就可能求解出此题。进行寻找相关信息：

![image-20220831113843486](/images/2022_WDB_QL-writeup/image-20220831113843486.png)

可以大致看出这个数据像是密文的，之前找到的四个成员的数组数据比较符号TEA加密算法的密钥形式，据此推断那个数据应该就是密钥。而且数据只对Int数组进行了加密前面两个成员。没有加密其他成员。

可以大致总结下，加密的大致逻辑：

![image-20220831115650012](/images/2022_WDB_QL-writeup/image-20220831115650012.png)

根据整合的逻辑可以编写脚本进行求解：

```python
from ctypes import *

def decrypt(v, k):
    x = c_int32(v[0])
    y = c_int32(v[1])
    delta = -1640531527
    total = c_uint32(delta * 32)
    for i in range(32):
        y.value -= ((x.value << 4) + k[2]) ^ (x.value + total.value) ^ ((x.value >> 5) + k[3])
        x.value -= ((y.value << 4) + k[0]) ^ (y.value + total.value) ^ ((y.value >> 5) + k[1])
        total.value -= delta
    return x.value, y.value

c_bytes = [
    -63, -69, -86, 43, 126, 114, 32, -75, 102, 49, 
    65, 103, 121, 107, 111, 99
]
c = []
for i in range(0,len(c_bytes),4):
    num = ''
    for v in range(4):
        num += hex((c_bytes[i+v] + 256)%256)[2:].zfill(2)
    num = int(num,16)
    if num < pow(2,31):
        c.append(num)
    else:
        c.append(num-pow(2,32))

key = [2023708229, -158607964, -2120859654, 1167043672]
m = list(decrypt(c,key)) + c[2:4]
flag = b''
for i in m:
    f = [int(hex(i)[2:][v:v+2],16) for v in range(0,8,2)]
    flag += bytes(f)
print(f'flag{{{flag.decode()}}}')
```

运行得到flag：`flag{Givey0urf1Agykoc}`

这道题目的脱壳方法也很多种，可以使用frida进行hook，然后让程序运行到解密的位置进行内存dump来进行脱壳。当然还有另一个方法也可以进行脱壳，在网上搜索到的一篇博客中有详细说明：[(191条消息) Android脱壳之整体脱壳原理与实践，今天带你详细了解各组件原理_m0_64604636的博客-CSDN博客_android 脱壳](https://blog.csdn.net/m0_64604636/article/details/121885541)
