+++
draft = false
date = 2022-10-04T23:18:37+08:00
title = "OtterCTF 2018 内存取证复现"
tags = ["ctf", "Forensics", "writeup"]
categories = [ "challange" ]
toc = true
+++

# OtterCTF 2018 Memory Forensic Reappearance

想学习并练习下电子取证技术，经过网络上的一番搜索发现OtterCTF的取证题目非常有意思，这次尝试练习下内存取证的题目顺便学习下内存取证的相关内容和技能点。内存取证主要使用的工具是Volatility，githu上面有相关项目。Volatility有两个版本分别是用python2和python3进行构建的，目前主要的组件还是以python2为主。
Volatility项目地址：[https://github.com/volatilityfoundation/volatility](https://github.com/volatilityfoundation/volatility)
Volatility3项目地址：[https://github.com/volatilityfoundation/volatility3](https://github.com/volatilityfoundation/volatility3)

环境配置：Kali Linux 2022
工具配置：volatility + mimikatz

## Info

题目附件就一个镜像，先查看下镜像的指纹数据：

```text
5b3d8a9f9c96581a821c19b71dd6aa80dd299fc674b818f443f3a6fb5495a872  OtterCTF.vmem
```

使用vol简单查看下镜像信息数据，查看数据的指令：

```bash
vol.py -f OtterCTF.vmem imageinfo
```

> **说明**
>
> - `vol.py` volatility程序
> - `-f OtterCTF.vmem`  加载`OtterCTF.vmem`内存镜像文件
> - `imageinfo` 查看内存镜像的基本信息

得到如下信息：

![image-20221002204848160](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221002204848160.png)

可以看到大致的镜像系统是Win7系统



## What the password?

查看题目描述：

```text
you got a sample of rick's PC's memory. can you get his user password?
```

要得到用户名的密码，先获取到hash数据，使用获取hash的指令：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 hashdump  
```

> - `--profile=Win7SP1x64` 设置配置为`Win7SP1x64`配置
> - `hashdump` 从内存中dump出密码的hash信息

得到hash信息：

![image-20221002212051508](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221002212051508.png)

对于这种hash信息可以使用mimikatz插件，或者使用hashcat也可以，这里使用mimikatz进行获取

使用如下指令使用mimikatz插件进行获取：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 mimikatz
```

得到如下信息：

![image-20221002214545148](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221002214545148.png)

已经获取到了密码信息，提交flag，攻破此题：

```text
CTF{MortyIsReallyAnOtter}
```



## General Info

查看题目描述：

```text
Let's start easy - whats the PC's name and IP address?
```

需要获取PC’s name和IP， PC的名字已经很明确了，应该就是刚刚获取密码信息得到的`WIN-LO6FAF3DTFE`

下面需要获取的应该就是IP地址，使用网络扫描指令应该可以获取到相应的信息：

![image-20221003001348636](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003001348636.png)

可以看出IP地址应该是：`192.168.202.131`

根据题目要求，flag应该是：

```text
CTF{WIN-LO6FAF3DTFE}
CTF{192.168.202.131}
```



## Play Time

查看题目描述：

```text
Rick just loves to play some good old videogames.
can you tell which game is he playing?
whats the IP address of the server?
```

玩游戏肯定会有进程的存储在内存中，扫描下进程情况，使用如下指令进行查看：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 psscan
```

得到如下信息：

![image-20221003002817020](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003002817020.png)

可以发现到了一个不熟悉的进程，猜测这个进程是游戏进程，也就是游戏名称是`LunarMS.exe`

再使用`netscan`查看下网络通信信息：

![image-20221003003940157](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003003940157.png)

可以发现一个ip地址，应该是游戏下载的ip地址，可以得到flag：

```text
CTF{LunarMS.exe}
CTF{77.102.199.102}
```

## Name Game

查看题目描述：

```text
We know that the account was logged in to a channel called Lunar-3. what is the account name?
```

应该需要找到游戏的用户名，应该存储在游戏的内存中，尝试将游戏进程进行dump来获取到游戏进程的内存信息

vol程序进行内存dump的指令如下：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 memdump -p 708 -D ./
```

> **说明**
>
> - `memdump` 内存dump的命令
> - `-p 708` 指定PID号708的进程
> - `-D ./` 指定内存dump进程文件存储位置

运行指令得到：

![image-20221003014902581](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003014902581.png)

尝试检索dump出来的文件的字符串，使用如下命令：

```bash
strings 708.dmp | grep Lunar-3
```

> **说明**
>
> - `strings 708.dmp` 打印出`708.dmp`二进制文件中的可打印字符
> - `|`  管道符，可以将左边命令输出的数据传递右边的命令处理
> - `grep Lunar-3` 检索有`Lunar`字符串所在的行并打印

运行命令得到：

![image-20221003015212113](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003015212113.png)

发现是存在相关字符串内容，增加点参数来获得字符串的上下文内容，执行如下命令：

```bash
strings 708.dmp | grep Lunar-3 -A 3 -B 3
```

> **说明**
>
> - `-A 3` 检索字符串所在行的前3行
> - `-B 3` 检索字符串所在行的后3行

执行命令得到：

![image-20221003020425640](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003020425640.png)

发现存在可疑字符串：`0tt3r8r33z3`

感觉这个字符串应该就是用户名，尝试提交成功

故flag是：

```text
CTF{0tt3r8r33z3}
```



## Name Game 2

查看题目描述：

```text
From a little research we found that the username of the logged on character is always after this signature: 0x64 0x??{6-8} 0x40 0x06 0x??{18} 0x5a 0x0c 0x00{2}
What's rick's character's name?
```

这道题目应该寻找相关的字符，需要进行模糊搜索

这道题目有两种解题方法：

1. 使用Linux的`hexdump`和`grep`命令来进行联合检索
2. 使用Hexedit类工具进行逐一寻找来确定相应位置

### Method 1

使用如下Linux命令：

```bash
hexdump -C 708.dmp | grep "64" -A 5 -B 5 | grep "40 06" -A 5 -B 5 | grep "5a 0c 00 00" -A 5 -B 5
```

执行命令获得：

![image-20221003113309491](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003113309491.png)

由于dump文件比较大，因此，这个命令可能需要执行4-5min才能执行出来结果，运行命令比较简单，但是运行时间相对比较长。

运行得到的字符串就是`M0rtyL0L`

故此题的flag就是`CTF{M0rtyL0L}`



### Method 2

使用WinHex之类的工具进行查看搜索`5a0c0000`

![image-20221003115058149](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003115058149.png)

进行Hex value的搜索，依次对搜索列表进行比对和寻找：

![image-20221003115148926](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003115148926.png)

最后找到目标位置：

![image-20221003115217012](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003115217012.png)

这种方式要进行查找比对，查找速度快的话是比Linux命令要快些，但是就是需要不断的人工比对和查看

得到字符串`M0rtyL0L`

故此题的flag就是`CTF{M0rtyL0L}`



这两种解题方法各有利弊，可以根据个人的喜好选择求解，个人更倾向于Linux那种解法，输入完命令就可以喝杯茶慢慢等，一会儿结果就出来了

## Silly Rick

查看题目描述

```text
Silly rick always forgets his email's password, so he uses a Stored Password Services online to store his password. He always copy and paste the password so he will not get it wrong. whats rick's email password?
```

根据题目描述，可以确定的关键语句`copy and paste`，说明邮箱地址应该是存储在粘贴板上面，可以查看下内存的粘贴板。

使用vol的如下指令查看粘贴板信息：

![image-20221003131135095](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003131135095.png)

可以发现的粘贴板信息：`M@il_Pr0vid0rs`

故此题的flag就是：

```text
CTF{M@il_Pr0vid0rs}
```



## Hide And Seek

查看题目描述

```text
The reason that we took rick's PC memory dump is because there was a malware infection. Please find the malware process name (including the extension)
```

根据题目描述，应该是需要寻找下恶意软件进程名称，vol这个软件功能非常强大，可以直接使用`malfind`的参数进行扫描，使用如下指令：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 malfind | grep Process
```

得到恶意进程信息：

![image-20221003180849409](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221003180849409.png)

可以简单的归纳下恶意进程的信息：

| Name             | Pid  |
| ---------------- | ---- |
| `WmiPrvSE.exe`   | 2136 |
| `explorer.exe`   | 2728 |
| `BitTorrent.exe` | 2836 |
| `PresentationFo` | 724  |
| `mscorsvw.exe`   | 412  |
| `mscorsvw.exe`   | 3124 |
| `svchost.exe`    | 3196 |
| `chrome.exe`     | 4076 |
| `vmware-tray.ex` | 3720 |
| `WebCompanionIn` | 3880 |
| `Lavasoft.WCAss` | 3496 |
| `WebCompanion.e` | 3856 |

这些是可能的恶意进程，需要找到题目要求的恶意进程，看看进程的cmd指令，使用如下指令：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 cmdline
```

得到：

![image-20221004150006413](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004150006413.png)

可以看到有一个进程是在Temp目录下进行执行，这个进程应该就是恶意进程了，一般恶意进程会在Temp目录中运行。

故恶意进程就是`vmware-tray.exe`

故此题的flag就是`CTF{vmware-tray.exe}`



## Path To Glory

查看题目描述：

```text
How did the malware got to rick's PC? It must be one of rick old illegal habits...
```

应该是要寻找的Rick的不良习惯，进行文件扫描来尝试确定可疑的文件

文件扫描使用`filescan`指令，由于是要寻找Rick的文件需要进行简单过滤下，使用如下命令：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 filescan | grep -i "Rick"
```

> **说明**
>
> - `grep -i "Rick"` 忽略大小写检索`Rick`

得到：

![image-20221004163351355](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004163351355.png)

发现一个torrent文件，可以猜测Rick应该平时会进行torrent种子文件的下载，而且这个种子文件的位置一般是在`Rick And Morty`文件夹下面。因此，进一步过滤查看相关信息，使用如下指令：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 filescan | grep -i "Rick And Morty"
```

得到：

![image-20221004164710378](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004164710378.png)

发现有一个文件的权限比较可疑，尝试将这个文件进行dump，来查看文件具体信息

使用如下命令进行dump：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -Q 0x000000007dae9350 -D ./
```

> **说明**
>
> - `dumpfiles` dump文件指令
> - `-Q 0x000000007dae9350 ` 以物理偏移为`0x000000007dae9350`的文件作为对象
> - `-D ./` dump文件目录在`./`目录下

得到：

![image-20221004170421613](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004170421613.png)

查看下dump文件中的可打印字符串数据

执行如下命令：

```bash
strings file.None.0xfffffa801b42c9e0.dat
```

得到可打印字符串信息：

![image-20221004170725684](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004170725684.png)

故此题的flag应该是藏在`e7:website19:M3an_T0rren7_4_R!cke`这段字符串中，对这段字符串仔细观察，可以发现这段字符串是有一些明显特征的

可以发现`e`是作为开头结尾的引用，中间的数据就是`7:website19:M3an_T0rren7_4_R!ck`，应该是三个对象显然最后一个对象应该是就是flag

故此题的flag就是`CTF{M3an_T0rren7_4_R!ck}`



## Path To Glory 2

查看题目描述：

```text
Continue the search after the way that malware got in.
```

需要继续寻找关于恶意软件的信息，要往深入探索应该需要探索浏览器的相关信息，由于Rick被恶意攻击是由于Rick在浏览器上下载种子文件

把浏览器的进程内存dump出来查看关键的信息

使用如下指令来dump相关信息：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 memdump -n chrome -D ./f/
```

> **说明**
>
> - `memdump` dump内存命令
> - `-n chrome` 以进程名称作为内存对象
> - `-D ./f/` dump内存文件`./f/`目录

![image-20221004201432915](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004201432915.png)

使用`strings`命令来获取内存文件的可打印字符串，并添加`download.exe.torrent`作为过滤，由于恶意文件名是`download.exe.torrent`，因此采用这种方式进行过滤

使用如下指令：

```bash
strings ./f/* | grep "download\.exe\.torrent" -A 10 -B 10
```

得到：

![image-20221004204205771](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004204205771.png)

这串字符看样子是挺奇怪的，应该就是flag信息

尝试发现真正的flag数据是`Hum@n_I5_Th3_Weak3s7_Link_In_Th3_Ch@in`

故此题的flag是`CTF{Hum@n_I5_Th3_Weak3s7_Link_In_Th3_Ch@in}`



## Bit 4 Bit

查看题目描述：

```text
We've found out that the malware is a ransomware. Find the attacker's bitcoin address.
```

恶意软件是一个勒索软件，需要将那个恶意软件进行分析

首先需要dump出恶意软件的进程，使用`procdump`将进程dump作为一个可执行程序

使用如下命令进行dump：

```text
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 procdump -p 3720 -D ./
```

> **说明**
>
> - `procdump` dump进程作为一个可执行文件
> - `-p 3720`  以PID作为进程对象
> - `-D ./`  将可执行文件存储在`./`目录

得到：

![image-20221004210324575](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004210324575.png)

需要对文件进行分析，首先查看软件的相关信息：

![image-20221004210448989](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004210448989.png)

发现恶意软件是一个.NET的文件，可以使用dnSpy进行查看，拖进dnSpy进行寻找发现：

![image-20221004210559036](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004210559036.png)

一个GUI绘制流，根据GUI的代码信息，使用PS绘制出GUI：

![image-20221004211105657](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004211105657.png)

比特币地址显而易见就是：`1MmpEmebJkqXG8nQv4cjJSmxZQFVmFo63M`

故此题的flag就是`CTF{1MmpEmebJkqXG8nQv4cjJSmxZQFVmFo63M}`



## Graphic’s For The Weak

查看题目描述：

```text
There's something fishy in the malware's graphics.
```

使用dnSpy加载恶意软件，可以在资源目录中找到：

![image-20221004212217979](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004212217979.png)

故本题的flag就是`CTF{S0_Just_M0v3_Socy}`



## Recovery

查看题目描述：

```text
Rick got to have his files recovered! What is the random password used to encrypt the files?
```

对恶意软件进行审计，找到关键的密码发送函数：

![image-20221004214117780](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004214117780.png)

发现密码是根据计算机名称和用户名以及密码一起发送的，要dump恶意软件的内存信息，使用如下指令进行dump：

```bash
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 memdump -p 3720 -D ./
```

执行命令得到：

![image-20221004214626524](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004214626524.png)

使用`strings`命令进行检索，由于程序的数据是16字节小端序数据，.NET程序的用2字节表示一个字符串，所以是16字节小端序数据，因此使用如下命令进行过滤检索：

```bash
strings -el 3720.dmp | grep WIN-LO6FAF3DTFE-Rick
```

运行得到：

![image-20221004215044513](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004215044513.png)

故此，密码就是`aDOBofVYUNVnmp7`

故此题的flag就是`CTF{aDOBofVYUNVnmp7}`



## Closure

查看题目描述：

```text
Now that you extracted the password from the memory, could you decrypt rick's files?
```

要找文件并且解密文件。猜测存在一个flag文件，使用如下指令进行文件扫描：

```
vol.py -f OtterCTF.vmem --profile=Win7SP1x64 filescan | grep -i "flag"
```

运行命令得到：

![image-20221004215615853](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004215615853.png)

应该是桌面上的`Flag.txt`文件

将文件进行dump，使用如下命令进行文件dump：
```bash
ol.py -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -Q 0x000000007e410890 -D ./
```
运行得到：

![image-20221004220208957](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004220208957.png)

下面就需要对文件进行解密了

首先对dnSpy反编译的代码进行审计可以看到是：

![image-20221004225734265](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004225734265.png)

hidden tear的程序算法

继续进行分析，可以发现一个AES的加密算法：

![image-20221004230040181](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004230040181.png)

进行跟进分析可以发现一个加密文件的函数：

![image-20221004230149524](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004230149524.png)

对这两个关键函数进行代码审计发现，核心的加密算法还是AES的算法

根据目前的分析情况，如果要解密这个文件主要有两种思路

- 根据加密算法写解密脚本进行解密
- 使用HiddenTearDecrypt工具进行解密

### HiddenTearDecrypt工具解密

由于在文件中发现了HiddenTear的关键信息，可以直接去检索HiddenTearDecrypt:

![image-20221004230418792](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004230418792.png)

HiddenTearDecrypt工具在网络上都是可以进行下载的

尝试使用工具进行解密发现不能成功求解。由于加密代码是采用CBC模式进行加密，会对文件进行填充，只要删除掉文件的填充应该就可以正常解密，使用winhex打开文件：

![image-20221004230652451](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004230652451.png)

可以发现有很多`00`字节的填充，只要删除`00`字节的填充就可以进行正常的解密，使用工具进行解密

![image-20221004230849821](/images/OtterCTF_2018_Memory_Forensic_writeup/image-20221004230849821.png)

解密成功，打开文件得到flag：

```text
CTF{Im_Th@_B3S7_RicK_0f_Th3m_4ll}
```



### 解密脚本解密

由于加密代码已经进行分析，对于加密的情况已经基本清晰

Python编写解密脚本的难点应该是在于生成器的替代，CS中的`Rfc2898DeriveBytes`可以使用Python中的`PBKDF2`进行替代

解密脚本主要难度其实已经克服，根据加密写解密脚本：

```python
from Crypto.Protocol.KDF import PBKDF2
from Crypto.Cipher import AES
from hashlib import sha256
def AES_Decrypt(c,password):
    salt = bytes([1, 2, 3, 4, 5, 6, 7, 8])
    kdf = PBKDF2(password, salt, 48, count = 1000)
    key = kdf[:32]
    iv = kdf[32:]
    aes = AES.new(key=key,iv=iv,mode=AES.MODE_CBC)
    m = aes.decrypt(c)
    return m

password = b"aDOBofVYUNVnmp7"
p = sha256(password).digest()

with open("./file.dat","rb") as f:
    data = f.read()
    raw = AES_Decrypt(data,p)
    with open("./file.txt","wb") as new:
        new.write(raw)
```

运行后得到解密文件，打开文件得到flag：

```text
CTF{Im_Th@_B3S7_RicK_0f_Th3m_4ll}
```



两种解法都可以正常进行求解，求解得到flag。

用工具求解可能比较快，但是存在一定的局限性，需要对加密文件进行一定的处理

用脚本求解可能相对较慢，但是比较灵活可以灵活地处理文件，对解密脚本编写熟练的，可以尝试写脚本求解，速度也不慢
