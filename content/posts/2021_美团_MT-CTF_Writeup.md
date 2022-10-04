---
title: "2021_美团_MT CTF_Writeup"
date: 2022-01-29T10:56:47+08:00
draft: false
math: true
tags: ["ctf","writeup","crypto"]
categories: ["challenge"]
toc: true
---

# 2021 美团网络安全 MT-CTF writup

本菜鸡比较菜，有些安详，仅仅只搞出了两道题目。两道简单的密码学题目：

## Symbol

非常奇怪的题目：

![Symbols](/images/2021_美团_MT-CTF_Writeup/Symbols.png)

题目是一堆奇奇怪怪的符号，对字符进行搜索找到其内涵含义后发现找到了`LaTex`关键字

于是想起了写个人简历和公式会经常用到的LaTeX语言，查找一下LaTeX的字符表得到：

```latex
$$
  \flat\lambda\alpha\gamma\{\forall\uplus\nu\_\Lambda\alpha\Tau\epsilon\Xi\_ M \approx\triangleleft\hbar\}
$$
```

根据代码的首字母可以得到：

```text
flag{fun_LaTeX_Math}
```

根据题目要求用md5进行加密，然后套一个flag得到

```text
flag{639220f4b70bb4a3ac80d95efcfb2353}
```



## hamburgerRSA

RSA的题目看下题目源码：

```python
from Crypto.Util.number import *

flag = open('flag.txt').read()
nbit = 64

while True:
    p, q = getPrime(nbit), getPrime(nbit)
    PP = int(str(p) + str(p) + str(q) + str(q))
    QQ = int(str(q) + str(q) + str(p) + str(p))
    if isPrime(PP) and isPrime(QQ):
        break

n = PP * QQ
m = bytes_to_long(flag.encode())
c = pow(m, 65537, n)
print('n =', n)
```

发现生成算法有些奇怪，感觉`p*q`和`PP*QQ`应该有些关系，使用python测试着生成一下，观察一下规律：（不想具体推到关系了）

写一个测试脚本：

```python
from Crypto.Util.number import *

nbit = 64

while True:
    p, q = getPrime(nbit), getPrime(nbit)
    PP = int(str(p) + str(p) + str(q) + str(q))
    QQ = int(str(q) + str(q) + str(p) + str(p))
    if isPrime(PP) and isPrime(QQ):
        break

n = PP * QQ
N = p*q

print("n:{}".format(n))
print("N:{}".format(N))
```

运行下脚本可以发现：

![image-20211217191213473](/images/2021_美团_MT-CTF_Writeup/image-20211217191213473.png)

n的前19位与N的前19位一致，n的后19位与N的后19位一致，可以根据这个特点来进行简单爆破，使用一个sage脚本进行简单爆破：

```python
n = 177269125756508652546242326065138402971542751112423326033880862868822164234452280738170245589798474033047460920552550018968571267978283756742722231922451193

n1 = str(n)[:19]
n2 = str(n)[-19:]
print(n1)
print(n2)
for i in range(10):
  N = int(n1+str(i)+n2)
  result = factor(N)
  if(len(result) == 2):
    print(result)
    break
```

运行得到p和q的数据：

```text
9788542938580474429 * 18109858317913867117
```

然后根据得到的p和q的数据写个脚本：

```python
from Crypto.Util.number import *
from  gmpy2 import invert

n = 177269125756508652546242326065138402971542751112423326033880862868822164234452280738170245589798474033047460920552550018968571267978283756742722231922451193
c = 47718022601324543399078395957095083753201631332808949406927091589044837556469300807728484035581447960954603540348152501053100067139486887367207461593404096

p, q = 9788542938580474429, 18109858317913867117
PP = int(str(p) + str(p) + str(q) + str(q))
QQ = int(str(q) + str(q) + str(p) + str(p))
phi = (PP-1)*(QQ-1)
d = invert(65537,phi)
m = pow(c, d, n)
flag = long_to_bytes(m)
print(flag)
```

运行脚本就可以得到flag：

```text
b'flag{f8d8bfa5-6c7f-14cb-908b-abc1e96946c6}'
```
