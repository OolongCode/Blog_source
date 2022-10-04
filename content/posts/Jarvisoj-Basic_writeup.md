---
title: "Jarvisoj Basic_writeup"
date: 2022-01-29T10:49:20+08:00
draft: false
math: true
tags: ["ctf","writeup"]
categories: ["practice"]
toc: true
---

# Jarvisoj-Basic writeup

>  JarvisOJ是浙江大学系统安全实验室(USS  Lab.)学生Jarvis所开发的一个CTF在线答题系统。题目形式与各大CTF比赛相同。目的主要是为自己整理历次比赛题目供以后查阅以及测试的作用，同时开放给广大爱好信息安全的朋友，可以在这里一起学习，一起进步。

![image-20211018145837413](/images/Jarvisoj-Basic_writeup/image-20211018145837413.png)

浙江大学的CTF刷题平台，使用起来还是比较不错的。界面简洁易用。

Basic模块的题目都是非常简单的练手题，刷一刷，玩一玩。顺便学习一些奇淫技巧，:smirk:

## 0x00 base64?

```text
GUYDIMZVGQ2DMN3CGRQTONJXGM3TINLGG42DGMZXGM3TINLGGY4DGNBXGYZTGNLGGY3DGNBWMU3WI===
```

看样子像是Base32编码，直接解编码得到：

```text
504354467b4a7573745f743373745f683476335f66346e7d
```

hex编码解编码得到：

```text
PCTF{Just_t3st_h4v3_f4n}
```



## 0x01 关于USS Lab

```text
USS的英文全称是什么，请全部小写并使用下划线连接_，并在外面加上PCTF{}之后提交
```

USS Lab是Jarvis OJ的主要承办单位，在刷题平台就能找到相关的信息

![image-20211018160449136](/images/Jarvisoj-Basic_writeup/image-20211018160449136.png)

flag已经显而易见了，非常简单

```text
PCTF{UBIQUITOUS_SYSTEM_SECURITY}
```

## 0x02 veryeasy

```text
使用基本命令获取flag
```

直接使用cat命令查看附件内容，或者strings命令查看附件内容

就能得到flag：

```text
PCTF{strings_i5_3asy_isnt_i7}
```



## 0x03 段子

```text
程序猿圈子里有个非常著名的段子：
手持两把锟斤拷，口中疾呼烫烫烫。
请提交其中"锟斤拷"的十六进制编码。(大写)
FLAG: PCTF{你的答案}
```

锟斤拷，是一串经常在搜索引擎页面和其他网站上看到的乱码字符。乱码源于GBK字符集和Unicode字符集之间的转换问题。

可以直接在python解释器上进行调试得到：

```python
"锟斤拷".encode('gbk').hex().upper()
# 'EFBFBDEFBFBD'
```

故flag：

```text
PCTF{EFBFBDEFBFBD}
```



## 0x04 手贱

```text
某天A君的网站被日，管理员密码被改，死活登不上，去数据库一看，啥，这密码md5不是和原来一样吗？为啥登不上咧？

d78b6f302l25cdc811adfe8d4e7c9fd34

请提交PCTF{原来的管理员密码}
```

挺无聊的题目的，题目给出的md5的长度是33位，需要删去一位来进行爆破

使用python脚本迭代出可能的md5数值：

```python
# coding:utf8

myMd5 = ""
for i in range(len(myMd5)):
    for j in range(len(myMd5)):
        if i == j:
            pass
        else:
            print myMd5[j],
    print ""
```

然后依次在cmd5网站进行解密，直到解出flag

当然也可以使用python结合批量工具进行MD5求解，这就需要使用到彩虹表的工具进行求解。

这里就不深入说明这个工具的使用，批量脚本确实很快，但是编写和生成彩虹表比较麻烦。

经测试，flag：

```text
PCTF{hack}
```

## 0x05 美丽的实验室logo

```text
出题人丢下个logo就走了，大家自己看着办吧
```

附件是个好康的图片：

![logo](/images/Jarvisoj-Basic_writeup/logo.jpg)

使用`Stegsolve.jar`工具，使用Analyse的Frame Browsers直接看

![image-20211018184321692](/images/Jarvisoj-Basic_writeup/image-20211018184321692.png)

直接就是flag了：

```text
PCTF{You_are_R3ally_Car3ful}
```

## 0x06 veryeasyRSA

```text
已知RSA公钥生成参数：

p = 3487583947589437589237958723892346254777 q = 8767867843568934765983476584376578389

e = 65537

求d = 

请提交PCTF{d}
```

非常简单的RSA题目，一个脚本：

```python
import gmpy2
p = 3487583947589437589237958723892346254777
q = 8767867843568934765983476584376578389
e = 65537
phi = (p-1)*(q-1)
d = gmpy2.invert(e, phi)
print(d)
# 19178568796155560423675975774142829153827883709027717723363077606260717434369
```

根据题目要求稍加处理就是flag：

```text
PCTF{19178568796155560423675975774142829153827883709027717723363077606260717434369}
```



## 0x07 神秘的文件

```python
出题人太懒，还是就丢了个文件就走了，你能发现里面的秘密吗？
```

可能是文件隐写，查看一下文件格式

```bash
binwalk haha
```

发现是一个磁盘文件，装载一下磁盘

```bash
mount haha mnt
```

然后进入到磁盘，发现磁盘里面有很多文件，而且每个文件里面都是单个字符

可以使用cat命令把所有文件的字符拼接

```bash
cat *
```

得到输出结果：

```text
Haha ext2 file system is easy, and I know you can easily decompress of it and find the content in it.But the content is spilted in pieces can you make the pieces together. Now this is the flag PCTF{P13c3_7oghter_i7}. The rest is up to you. Cheer up, boy.cat: lost+found: Permission denied
```

可以清晰地看到flag：

```text
PCTF{P13c3_7oghter_i7}
```

思路和考察的角度还是比较简单，虽然只是记录下简单的题目



## 0x08 公倍数

```text
请计算1000000000以内3或5的倍数之和。

如：10以内这样的数有3,5,6,9，和是23

请提交PCTF{你的答案}
```

算法题目，非常简单就是有些吃内存

```c++
#include <iostream>
using namespace std;
typedef long long ll;
int main()
{
    ll a=1000000000;
    ll sum=0;
    for(ll i=1;i<a;i++)
    {
        if(i%3==0||i%5==0)
        {
            sum+=i;
        }
    }
    cout<<sum<<endl;
    return 0;
}
```

运行程序，就可以得到：

```text
233333333166666668
```

故本题的flag：

```text
flag{233333333166666668}
```



## 0x09 Easy Crackme

应该是简单的逆向的题目，走下逆向的流程，使用DIE进行查看：

![image-20211108143211427](/images/Jarvisoj-Basic_writeup/image-20211108143211427.png)

64位ELF，先进行静态分析查看：

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char v3; // cl
  char *v4; // rsi
  int v5; // edx
  int v6; // ecx
  int v7; // er8
  int v8; // er9
  char *v9; // rdx
  int v10; // ecx
  unsigned int v11; // eax
  bool v12; // zf
  int v13; // ecx
  __int64 v14; // rdx
  __int64 v15; // rdi
  char v17; // [rsp+0h] [rbp-38h]
  unsigned __int8 v18; // [rsp+10h] [rbp-28h] BYREF
  _BYTE v19[39]; // [rsp+11h] [rbp-27h] BYREF

  printf((unsigned int)"Input your password:", (_DWORD)argv, (_DWORD)envp, v3);
  v4 = (char *)&v18;
  _isoc99_scanf((unsigned int)"%s", (unsigned int)&v18, v5, v6, v7, v8, 171);
  v9 = (char *)&v18;
  do
  {
    v10 = *(_DWORD *)v9;
    v9 += 4;
    v11 = ~v10 & (v10 - 16843009) & 0x80808080;
  }
  while ( !v11 );
  v12 = (~v10 & (v10 - 16843009) & 0x8080) == 0;
  if ( (~v10 & (v10 - 16843009) & 0x8080) == 0 )
    LOBYTE(v11) = (~v10 & (v10 - 16843009) & 0x80808080) >> 16;
  LOBYTE(v13) = (_BYTE)v9 + 2;
  if ( v12 )
    v9 += 2;
  v14 = &v9[-__CFADD__((_BYTE)v11, (_BYTE)v11) - 3] - (char *)&v18;
  if ( v14 == 26 )
  {
    v15 = 0LL;
    v4 = v19;
    if ( (v18 ^ 0xAB) == list1 )
    {
      while ( 1 )
      {
        LODWORD(v14) = ((int)v15 + 1) / 6;
        v13 = ((int)v15 + 1) % 6;
        if ( (v19[v15] ^ (unsigned __int8)*(&v17 + v13)) != byte_6B41D1[v15] )
          break;
        if ( ++v15 == 25 )
        {
          printf((unsigned int)"Congratulations!", (unsigned int)v19, v14, v13);
          return 0;
        }
      }
    }
  }
  printf((unsigned int)"Password Wrong!! Please try again.", (_DWORD)v4, v14, v13);
  return 0;
}
```

进行代码审计可以轻松手撸出解题程序：

```c++
#include <iostream>

int main()
{
  int n[] = {251, 158, 103, 18, 78, 157, 152, 171, 0, 6, 70, 138, 244,
     180, 6, 11, 67, 220, 217, 164, 108, 49, 116, 156, 210, 160};
  int key[] = { 0xab, 0xdd, 0x33, 0x54, 0x35, 0xef };
  for(size_t i = 0; i < 26; i++)
          putchar(n[i] ^ key[i % 6]);
  return 0;
}
```

编译并运行程序可以得到：

```c++
PCTF{r3v3Rse_i5_v3ry_eAsy}
```



## 0x0A Secret

web题目，可是这个平台的web题目炸了，不能继续开心地刷题了

进入网站F12 然后使用F5查看请求头来拿到flag

flag为：

```text
PCTF{Welcome_to_phrackCTF_2016}
```



## 0x0B 爱吃培根的出题人

看题目应该是培根密码，看下题目

```text
听说你也喜欢吃培根？那我们一起来欣赏一段培根的介绍吧：


bacoN is one of aMerICa'S sWEethEartS. it's A dARlinG, SuCCulEnt fOoD tHAt PaIRs FlawLE


什么，不知道要干什么？上面这段巨丑无比的文字，为什么会有大小写呢？你能发现其中的玄机吗？


提交格式：PCTF{你发现的玄机}
```

看文段应该就是培根密码，直接进行解密：

```text
BACONISNOTFOOD
```



## 0x0C Easy RSA

```text
还记得veryeasy RSA吗？是不是不难？那继续来看看这题吧，这题也不难。


已知一段RSA加密的信息为：0xdc2eeeb2782c且已知加密所用的公钥：

(N=322831561921859 e = 23)

请解密出明文，提交时请将数字转化为ascii码提交

比如你解出的明文是0x6162，那么请提交字符串ab


提交格式:PCTF{明文字符串}
```

N的数值非常小，直接对N进行yafu大数分解可以得到：

```text
p = 23781539
q = 13574881
```

知道p和q，直接一个脚本求解

```python
import gmpy2
from Crypto.Util.number import long_to_bytes
p = 23781539
q = 13574881
e = 23
n = 322831561921859
c = 0xdc2eeeb2782c
phi = (p-1)*(q-1)
d = gmpy2.invert(e, phi)
m = pow(c,d,n)
flag = long_to_bytes(m)
print(flag)
# b'3a5Y'
```

根据题目要求，flag：

```text
PCTF{3a5Y}
```



## 0x0D ROPGadget

```text
都说学好汇编是学习PWN的基础，以下有一段ROPGadget的汇编指令序列，请提交其十六进制机器码(大写，不要有空格)


XCHG EAX,ESP

RET

MOV ECX,[EAX]

MOV [EDX],ECX

POP EBX

RET


提交格式：PCTF{你的答案}
```

非常简单，使用python的pwntools模块就可以进行求解

```python
from pwn import *
print asm('XCHG EAX,ESP\nRET\nMOV ECX,[EAX]\nMOV [EDX],ECX\nPOP EBX\nRET'.lower()).encode('hex').upper()
```

运行得到：

```text
94C38B08890A5BC3
```



## 0x0E 取证

```text
有一款取证神器如下图所示，可以从内存dump里分析出TureCrypt的密钥，你能找出这款软件的名字吗？名称请全部小写。
```

![img](/images/Jarvisoj-Basic_writeup/28601465358215259.JPG)

```text
提交格式：PCTF{软件名字}
```

常识性题目，可以直接作答

flag为：

```text
PCTF{volatility}
```



## 0x0F 熟悉的声音

```text
两种不同的元素，如果是声音的话，听起来是不是很熟悉呢，据说前不久神盾局某位特工领便当了大家都很惋惜哦

XYYY YXXX XYXX XXY XYY X XYY YX YYXX

请提交PCTF{你的答案}
```

看样子像是，莫尔斯电码，转换成`.-`，使用Morse电码求解

```text
.--- -... .-.. ..- .-- . .-- -. --..
```

使用Morse求解得到：

```text
JBLUWEWNZ
```

然后发现并不是有意义的字符串，这时候可能需要进行凯撒密码的处理就可以得到flag：

```text
PHRACKCTF
```



## 0x10 Baby’s Crack

应该是道逆向题目，看下题目：

```text
既然是逆向题，我废话就不多说了，自己看着办吧。
```

先使用DIE进行探测吧：

![image-20211109214508584](/images/Jarvisoj-Basic_writeup/image-20211109214508584.png)

64位PE程序，使用x64 IDA pro的静态分析

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int result; // eax
  char Buffer[104]; // [rsp+20h] [rbp-80h] BYREF
  FILE *v5; // [rsp+88h] [rbp-18h]
  FILE *Stream; // [rsp+90h] [rbp-10h]
  char v7; // [rsp+9Fh] [rbp-1h]

  _main();
  if ( argc <= 1 )
  {
    printf("Usage: %s [FileName]\n", *argv);
    printf(aFilename);
    exit(1);
  }
  Stream = fopen(argv[1], "rb+");
  if ( Stream )
  {
    v5 = fopen("tmp", "wb+");
    while ( !feof(Stream) )
    {
      v7 = fgetc(Stream);
      if ( v7 != -1 && v7 )
      {
        if ( v7 > 47 && v7 <= 96 )
        {
          v7 += 53;
        }
        else if ( v7 <= 46 )
        {
          v7 += v7 % 11;
        }
        else
        {
          v7 -= v7 % 61;
        }
        fputc(v7, v5);
      }
    }
    fclose(v5);
    fclose(Stream);
    sprintf(Buffer, "del %s", argv[1]);
    system(Buffer);
    sprintf(Buffer, "ren tmp %s", argv[1]);
    system(Buffer);
    result = 0;
  }
  else
  {
    printf(&byte_40404B, argv[1]);
    result = -1;
  }
  return result;
}
```

应该是一个非常简单的文件加密程序，看一下加密文件：

```text
jeihjiiklwjnk{ljj{kflghhj{ilk{k{kij{ihlgkfkhkwhhjgly
```

加密后的字符串都是大于100的字符，可以根据代码：

```c
if ( v7 > 47 && v7 <= 96 )
{
    v7 += 53;
}
```

直接写个小程序进行破解：

```c++
#include <iostream>
#include <string>

int main()
{
  std::string c = "jeihjiiklwjnk{ljj{kflghhj{ilk{k{kij{ihlgkfkhkwhhjgly";
  for(auto s : c){
    putchar(s-53);
  }
  return 0;
}
```

编译并运行得到：

```text
504354467B596F755F6172335F476F6F645F437261636B33527D
```

然后对这段hex字符串进行解码：

```text
PCTF{You_ar3_Good_Crack3R}
```



## 0x11 Help!!

```text
出题人硬盘上找到一个神秘的压缩包，里面有个word文档，可是好像加密了呢~让我们一起分析一下吧！
```

应该是一个MISC的题目，看看吧

下载附件得到了一个zip加密的压缩文件，可能是伪加密的zip文件，使用 010 editor来进行修改下解伪加密就可以正常解压了

得到一个word文件，下面就是丢到binwalk进行查看喽

![image-20211110081259691](/images/Jarvisoj-Basic_writeup/image-20211110081259691.png)

发现存在有两个图片文件，直接修改后缀名继续解压查看图片：

![image-20211110081457331](/images/Jarvisoj-Basic_writeup/image-20211110081457331.png)

flag就在图片里面



## 0x12 Shellcode

使用github上的工具：https://github.com/bdamele/shellcodeexec

执行附件中的Shellcode就可以了

![image-20211110162145491](/images/Jarvisoj-Basic_writeup/image-20211110162145491.png)

成功拿到flag：

```text
PCTF{Begin_4_good_pwnn3r}
```



## 0x13 A Piece Of Cake

```text
nit yqmg mqrqn bxw mtjtm nq rqni fiklvbxu mqrqnl xwg dvmnzxu lqjnyxmt xatwnl, rzn nit uxnntm xmt zlzxuuk mtjtmmtg nq xl rqnl. nitmt vl wq bqwltwlzl qw yivbi exbivwtl pzxuvjk xl mqrqnl rzn nitmt vl atwtmxu xamttetwn xeqwa tsftmnl, xwg nit fzruvb, nixn mqrqnl ntwg nq gq lqet qm xuu qj nit jquuqyvwa: xbbtfn tutbnmqwvb fmqamxeevwa, fmqbtll gxnx qm fiklvbxu ftmbtfnvqwl tutbnmqwvbxuuk, qftmxnt xznqwqeqzluk nq lqet gtamtt, eqdt xmqzwg, qftmxnt fiklvbxu fxmnl qj vnltuj qm fiklvbxu fmqbtlltl, ltwlt xwg exwvfzuxnt nitvm twdvmqwetwn, xwg tsivrvn vwntuuvatwn rtixdvqm - tlftbvxuuk rtixdvqm yivbi evevbl izexwl qm qnitm xwvexul. juxa vl lzrlnvnzntfxllvldtmktxlkkqzaqnvn. buqltuk mtuxntg nq nit bqwbtfn qj x mqrqn vl nit jvtug qj lkwnitnvb rvquqak, yivbi lnzgvtl twnvnvtl yiqlt wxnzmt vl eqmt bqefxmxrut nq rtvwal nixw nq exbivwtl.
```

直接丢到词频分析

```text
	the word robot can refer to both physical robots and virtual software agents, but the latter are usually referred to as bots. there is no consensus on which machines qualify as robots but there is general agreement among experts, and the public, that robots tend to do some or all of the following: accept electronic programming, process data or physical perceptions electronically, operate autonomously to some degree, move around, operate physical parts of itself or physical processes, sense and manipulate their environment, and exhibit intelligent behavior - especially behavior which mimics humans or other animals. flag is substitutepassisveryeasyyougotit. closely related to the concept of a robot is the field of synthetic biology, which studies entities whose nature is more comparable to beings than to machines.
```

直接看到flag：

```text
substitutepassisveryeasyyougotit
```



## 0x14 -.-字符串

```text
请选手观察以下密文并转换成flag形式

..-. .-.. .- --. ..... ..--- ..--- ----- .---- ---.. -.. -.... -.... ..... ...-- ---.. --... -.. .---- -.. .- ----. ...-- .---- ---.. .---- ..--- -... --... --... --... -.... ...-- ....- .---- -----

flag形式为32位大写md5


题目来源：CFF2016
```

直接进行Morse解码得到：

```text
FLAG522018D665387D1DA931812B77763410
```

这应该就是flag



## 0x15 德军的密码

```text
已知将一个flag以一种加密形式为使用密钥进行加密，使用密钥WELCOMETOCFF加密后密文为 000000000000000000000000000000000000000000000000000101110000110001000000101000000001 请分析出flag。Flag为12位大写字母


题目来源：CFF2016
```

二战中德国使用过的密码是费娜姆密码

写个小程序进行异或操作就可：

```c++
#include <iostream>
#include <string>
int main()
{
  int data[] {
    0b0000000,0b0000000,0b0000000,0b0000000,0b0000000,0b0000000,0b0000000,0b0010111,
    0b0000110,0b0010000,0b0010100,0b0000001
  };
  char key[] = "WELCOMETOCFF";
  for(size_t i;i<12;i++){
    putchar(key[i]^data[i]);
  }
  return 0;
}
```

编译并运行就可以得到flag：

```text
WELCOMECISRG
```



## 0x16 握手包

```text
给你握手包，flag是Flag_is_here这个AP的密码，自己看着办吧。

提交格式：flag{WIFI密码}
```

把文件直接拖到kali里面，使用kali的工具进行破解

用aircrack-ng命令进行破解

```sh
aircrack-ng  wifi.cap -w /usr/share/wordlists/rockyou.txt
```

很快就可以破解出握手包的密码

![image-20211110085013887](/images/Jarvisoj-Basic_writeup/image-20211110085013887.png)

直接over了，flag即为：

```text
flag{11223344}
```
