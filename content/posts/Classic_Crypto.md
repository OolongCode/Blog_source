---
title: "古典密码-探寻曾经的故事"
date: 2022-01-29T11:28:09+08:00
draft: false
math: true
tags: ["crypto"]
categories: ["wiki"]
toc: true
---

# 古典密码

> 密码和编码最大的区别就是密码多了一个很关键的信息：密钥。

密码(Cryptology)是一种用来混淆的技术,它希望将正常的、可识别的信息转变为无法识别的信息。密码学是一个即古老又新兴的学科,密码学一词源自希腊文“krypto's”及“logos”两字,直译即为“隐藏”及“讯息”之意。

密码学是一门拥有几千年历史的学科。密码学的发展大概经历了三个阶段:古典密码阶段、近代密码阶段、现代密码阶段。下面我们一起了解古典密码阶段。

古典密码阶段是指从密码的产生到发展成为近代密码之间的这段时期密码的发展历史。

> 值得一提的是，在古典密码学中，设计者主要考虑消息的保密性，使得只有相关密钥的人才可以解密密文获得消息的内容，对于消息的完整性和不可否认性则并没有进行太多的考虑。(1)

## 历史

古代中国:从古到今,军队历来是使用密码最频繁的地方,因为保护己方秘密并洞悉敌方秘密是克敌制胜的重要条件。正如中国古代军事著作《孙子兵法》中所说:知己知彼,百战不殆;不知彼而知己,一胜一负;不知彼不知己,每战必败。中国古代有着丰富的军事实践和发达的军事理论,其中不乏巧妙、规范和系统的保密通信和身份认证方法。

中国古代兵书《六韬》中的阴符和阴书:《六韬》又称《太公六韬》或《太公兵法》,据说是由西周的开国功臣太公望(又名吕尚或姜子牙,约公元前1128—公元前1015)所著。书中以周文王和周武王与太公问答的形式阐述军事理论,其中《龙韬•阴符》篇和《龙韬•阴书》篇,讲述了君主如何在战争中与在外的将领进行保密通信。

以下是关于“阴符”使用方法对话的译文。

武王问太公说:领兵深入敌国境内,军队突然遇到紧急情况,战事或有利,或失利。我要与各军远近相通,内外相应,保持密切的联系,以便及时应对战场上军队的需求,应该怎么办呢?

太公回答说:国君与主将之间用阴符秘密联络。阴符共有八种:一种长一尺,表示大获全胜,摧毁敌人;一种长九寸,表示攻破敌军,杀敌主将;一种长八寸,表示守城的敌人已投降,我军已占领该城;一种长七寸,表示敌军已败退,远传捷报;一种长六寸,表示我军将誓死坚守城邑;一种长五寸,表示请拨运军粮,增派援军;一种长四寸,表示军队战败,主将阵亡;一种长三寸,表示战事失利,全军伤亡惨重。如奉命传递阴符的使者延误传递,则处死;如阴符的秘密被泄露,则无论无意泄密者或有意传告者也处死。只有国君和主将知道这八种阴符的秘密。这就是不会泄露朝廷与军队之间相互联系内容的秘密通信语言。敌人再聪明也不能识破它。

以下是关于“阴书”使用方法对话的译文。

武王问太公说:领兵深入敌国境内,君主和将帅各率一军,要使两支军队配合作战,实施变化无穷的作战方法,谋取敌人意想不到的胜利。但需要联络的事情很多,使用阴符难以说明,而两军之间又距离遥远,言语不能通达,应该怎么办呢?

太公回答说:如果有军机大事需要联络,应该用书信而不用符。君主通过书信向主将指示,主将则通过书信向君主请示。书信都要拆分成三部分,并分派三人发出,每人拿一部分。只有这三部分合在一起才能读懂信的内容。这就是所谓的阴书(机密信),敌人再聪明,也看不懂这种书信。

中国宋代兵书《武经总要》是北宋仁宗时期官修的一部兵书,成书于1040年—1044年,作者是天章阁待制曾公亮和工部侍郎丁度。该书前集第15卷中有“符契”、“信牌”和“字验”三节,专门讲述军队中秘密通信和身份验证的方法。

“符契”是《六韬》中“阴符”方法的改进。其中的“符”是皇帝派人向军队调兵的凭证,共有5种符,各种符的组合表示调用兵力的多少,每符分左右两段,右段留京师,左段由各路军队的主将收掌。使者将带着皇帝的命令和由枢密院封印的相应的右符,前往军队调兵;主将听完使者宣读皇帝的命令后,须启封使者带来的右符,并与所藏的左符验合,才能接受命令;然后用本将军的印重封右符,交由使者带回京师。

“契”是主将派人向镇守各方的下属调兵的凭证,共有三种契,每契都是鱼形,可分为上下两段。上段留主将收掌,下段交各处下属收掌,使用方法类似于上述的符。

“信牌”是两军阵前交战时,派人传送紧急命令的信物和文件。北宋初期使用的信物是一分两半的铜钱,后来改用木牌,上面可以写字。

“字验”则是秘密传送军情的一套方法。先约定40种不同的军情,然后用一首含有40个不同字的诗,令其中每一个字对应一种军情。传送军情时,写一封普通的书信或文件,在其中的关键字旁加印记。军使在送信途中,不怕被敌方截获并破解信中内容。将军们收到信后,找出其中加印记的关键字,然后根据约定的40字诗来查出该字所告知的情况,还可以在这些字上再加印记,以表示对有关情况的处理,并令军使带回。

我们看到,宋代的“字验”方法与近代以来军队、外交官和间谍们常用的借助密码字典进行秘密通信和联络的原理相同。

古代中国的君王常以虎符作为调用军队的凭证。如在春秋战国时期,就有魏信陵君使如姬窃取魏王的虎符,并以此夺取大将晋鄙的兵权,然后率兵大破秦军,以解赵国之围的故事。虎符一般由铜、银等金属制成,背面刻有铭文,以示级别、身份、调用军队的对象和范围等;虎符分为两半,一半放在朝廷,另一半由在外的将帅保管。朝廷派来的使者,需携虎符验合,才可调兵遣将。

顺便解释“符”字:其本义是指古代朝廷下命令的凭证;部首的“竹”表明最早的“符”是用竹子做的;“符”通常做成两部分,使用时一分为二,验证时合二为一;只有同一符的两部分才能完美地合在一起;这就是常用词“符合”的来历。近代间谍史上,常有人把纸币钞票一撕为二,作为接头联络的工具,其原理同“符”。现代密码学中,运用公钥—私钥体系进行身份认证的方法也与“符”相通。

我国明末清初著名的军事理论家揭暄(1613—1695)所著的《兵经百言》用100个字条系统阐述了中国古代的军事理论。其中的“传”字诀则是古代军队通信方法的总结,其解释如下:

军队分开行动后,如相互之间不能通信,就要打败仗;如果能通信但不保密,则也要被敌人暗算。所以除了用锣鼓、旌旗、骑马送信、燃火、烽烟等联系外,两军相遇,还要对暗号(口令)。当军队分开有千里之远时,宜用机密信(素书)进行通信。机密信分为三种:改变字的通常书写或阅读方式(“不成字”,如传统密码学的文字替换或移位方法);隐写术(“无形文”,用含有某种化学物质的液体来书写,收信者用特殊方法使文字显现出来,如矾书);不是把书信写在常用的纸上(“非纸简”),而是写在特殊的、不引人注意的载体上(如服饰,甚至人体上等)。这些通信方式连送信的使者都不知道信中的内容,但收信人却可以接收到信息。

古埃及:公元前2000年人类文明刚刚形成,大约就在那个时候古埃及就拥有了密码。贵族克努姆霍特普二世的墓碑上记载了在阿梅连希第二法老王朝供职期间它所建立的功勋。上面的象形文字与我们已知的埃及象形文字有所不同,那是由一位擅长书写的人对普通象形文字经过处理之后刻录的,但是具体的方法尚未可知。民众们推测这可能是庄严和权威的象征。

古印度:印度公元前三百年写成的《经济论》旨在描述当时密探充斥全国时特务机关的官员用密写的方式给密探下达任务。

古希腊:大约在公元前700年,古希腊军队用一种叫做Scytale的圆木棍来进行保密通信。其使用方法是:把长带子状羊皮纸缠绕在圆木棍上,然后在上面写字;解下羊皮纸后,上面只有杂乱无章的字符,只有再次以同样的方式缠绕到同样粗细的棍子上,才能看出所写的内容。

这种Scytale圆木棍也许是人类最早使用的文字加密解密工具,据说主要是古希腊城邦中的斯巴达人(Sparta)在使用它,所以又被叫做“斯巴达棒”。

斯巴达棒的加密原理属于密码学中的“换位法”(Transition)加密,因为它通过改变文本中字母的阅读顺序来达到加密的目的。(2)

## 类型

古典密码在形式上可分成移位密码和替代密码两类，其中替代密码又可分为单表替代密码和多表替代。

### 移位密码

#### 曲路密码

曲路密码是一种置换密码，其中密钥是从明文创建的块中读取密文时要遵循的路径,该密钥需双方事先约定好（曲路路径）。

下面给出一个例子：

```text
明文：The quick brown fox jumps over the lazy dog
```

填入填入 5 行 7 列表（事先约定填充的行列数）

![image-20210719200647994](/images/Classic_Crypto/image-20210719200647994.png)

加密的回路线（事先约定填充的行列数）

![image-20210719200710292](/images/Classic_Crypto/image-20210719200710292.png)

```text
密文：gesfc inpho dtmwu qoury zejre hbxva lookT
```



#### 云影密码

该密码又称为01248，使用 0，1，2，4，8 四个数字，其中 0 用来表示间隔，其他数字以加法可以表示出 如：28=10，124=7，18=9，再用 1->26 表示 A->Z。

可以看出该密码有以下特点：

- 只有 0，1，2，4，8

这里举个栗子

```text
密文：WELLDONE
```

采用0进行分割，得到云影密码加密结果：

```text
8842101220480224404014224202480122
```



#### 栅栏密码

栅栏密码把要加密的明文分成 N 个一组，然后把每组的第 1 个字连起来，形成一段无规律的话。栅栏密码分为两种类型，一种是传统的栅栏密码，另一种是W型的栅栏密码。

##### 传统栅栏密码

```text
明文：THEREISACIPHER
```

分成三栏，三个一组得到

```text
THE REI SAC IPH ER
```

先取出第一个字母，再取出第二个字母

```
TRSIE
HEAPR
EICH
```

连在一起就是密文

```
TRSIEHEAPREICH
```



##### W型栅栏密码

W型栅栏密码和传统的栅栏密码原理类似，就排列方式有所差异

```text
明文：THEREISACIPHER
```

分成三栏，三个一组得到

```text
THE REI SAC IPH ER
```

进行W型栅栏排列

```text
T   E   C   E
 H R I A I H R
  E   S   P
```

连在一起就是密文

```text
TECEHRIAIHRESP
```



### 替换密码

#### 单表替换密码

在单表替换加密中，所有的加密方式几乎都有一个共性，那就是明密文一一对应。所以说，一般有以下两种方式来进行破解 (4)

- 在密钥空间较小的情况下，采用暴力破解方式
- 在密文长度足够长的时候，使用词频分析，http://quipqiup.com/

当密钥空间足够大，而密文长度足够短的情况下，破解较为困难。

##### 凯撒密码

凯撒密码是非常著名的古典密码，也是非常经典的古典密码。

凯撒密码（Caesar）加密时会将明文中的 **每个字母** 都按照其在字母表中的顺序向后（或向前）移动固定数目（**循环移动**）作为密文。例如，当偏移量是左移 3 的时候（解密时的密钥就是 3）：^4^

```text
明文字母表：ABCDEFGHIJKLMNOPQRSTUVWXYZ
密文字母表：DEFGHIJKLMNOPQRSTUVWXYZABC
```

使用时，加密者查找明文字母表中需要加密的消息中的每一个字母所在位置，并且写下密文字母表中对应的字母。需要解密的人则根据事先已知的密钥反过来操作，得到原来的明文。例如：

```text
明文：THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
密文：WKH TXLFN EURZQ IRA MXPSV RYHU WKH ODCB GRJ
```

根据偏移量的不同，还存在**若干特定的恺撒密码名称**：

- 偏移量为10：Avocat (A–>K)
- 偏移量为13：ROT13
- 偏移量为-5：Cassis (K 6)
- 偏移量为-6：Cassette (K 7)

举个栗子：

```text
明文：This is a fake flag
```

经过凯撒密码，位移3进行加密得到

```text
密文：Wklv lv d idnh iodj
```



##### 埃特巴什码

与凯撒密码不同的是，埃特巴什码的提代表不是通过移位获得的，而是通过对称获得的。其通过将字母表的位置完全镜面对称后获得字母的替代表，然后进行加密。

埃特巴什码（Atbash Cipher）其实可以视为下面要介绍的简单替换密码的特例，它使用字母表中的最后一个字母代表第一个字母，倒数第二个字母代表第二个字母。在罗马字母表中，它是这样出现的：

```text
明文：A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
密文：Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
```

给出一个栗子：

```text
明文：the quick brown fox jumps over the lazy dog
密文：gsv jfrxp yildm ulc qfnkh levi gsv ozab wlt
```



##### 培根密码

培根密码一般使用两种不同的字体表示密文，密文的内容不是关键所在，关键是字体。使用AB代表两种字体，五个一组，表示密文，明密问对应如表：(5)

| 明文 | 密文  |
| ---- | ----- |
| a    | AAAAA |
| b    | AAAAB |
| c    | AAABA |
| d    | AAABB |
| e    | AABAA |
| f    | AABAB |
| g    | AABBA |
| h    | AABBB |
| i-j  | ABAAA |
| k    | ABAAB |
| l    | ABABA |
| m    | ABABB |
| n    | ABBAA |
| o    | ABBAB |
| p    | ABBBA |
| q    | ABBBB |
| r    | BAAAA |
| s    | BAAAB |
| t    | BAABA |
| u-v  | BAABB |
| w    | BABAA |
| x    | BABAB |
| y    | BABBA |
| z    | BABBB |

上面的是常用的加密表。还有另外的一种加密表，可认为是将 26 个字母从 0 到 25 排序，以二进制表示，A 代表 0，B 代表 1。

下面这一段内容就是明文 steganography 加密后的内容，正常字体是 A，粗体是 B：

**T**o en**co**de **a** mes**s**age e**ac**h letter **of** the **pl**a**i**nt**ex**t **i**s replaced b**y a g**rou**p of f**i**ve** of **th**e lett**ers** **'A'** o**r 'B'**.

可以看到，培根密码主要有以下特点:

- 只有两种字符
- 每一段的长度为5
- 加密内容会有特殊的字体之分，亦或者大小写之分

##### 图形替代密码

猪圈密码和跳舞的小人都是典型的图形替代密码，图形替代密码是通过将明文用图形进行替代以实现加密。解密时进行一样对应就可以进行解密。这里列出几种经典的图形替代密码：

1. 猪圈密码

   ![image-20210723143749253](/images/Classic_Crypto/image-20210723143749253.png)

2. 标准银河字母

   ![image-20210723143804055](/images/Classic_Crypto/image-20210723143804055.png)

3. 圣堂武士密码

   ![image-20210723143823252](/images/Classic_Crypto/image-20210723143823252.png)

4. 跳舞的小人

   ![image-20210723170216677](/images/Classic_Crypto/image-20210723170216677.png)

5. 古埃及象形密码

   ![image-20210723143640427](/images/Classic_Crypto/image-20210723143640427.png)

6. Wingdings

   ![image-20210723170304399](/images/Classic_Crypto/image-20210723170304399.png)

##### 仿射密码

仿射密码的替代表生成方式依据：
$$
c = am+b\ \text{mod}\ n
$$

- $m$ 为明文对应字母得到的数字 
- $a$  和 $n$ 互质
- $n$ 是编码系统中字母的数目

解密函数是 $D(x) = a^{-1} (m-b) \ \text{mod}\ n$, 其中$a^{-1}$是$a$在$\mathbb{Z}_m$群的乘法逆元。

#### 多表替换密码

对于多表替换加密来说，加密后的字母几乎不再保持原来的频率，所以我们一般只能通过寻找算法实现对应的弱点进行破解。

##### 棋盘类密码

Playfair、Polybius和Nihilist均属于棋盘类密码。此类密码的密钥为一个5x5的棋盘。棋盘生成符号如下条件：

- 顺序随意
- 不得出现重复字母
- i和j可视为同一个字（也有将q去除的，以保证总数为25个）

生成棋盘后，不同加密方式使用不同的转换方式。

##### 维吉尼亚密码

凯撒密码是单表替代密码，其只使用了一个替代表，维吉尼亚密码则是标准的多表替代密码。

首先，多表替代密码的密钥不再是固定不变的，而是随着位置发生改变的。在维吉尼亚密码中，根据密钥的字母来选择。

使用数学语言进行描述：

维吉尼亚密码是一种简单的多表代换密码(由26个类似的Caesar密码的代换表组成)，

即由**一些**偏移量不同的恺撒密码组成，这些代换在一起组成了密钥。

英文中a\~z，由0\~25表示。

假设串长为：m，明文为P，密文为C，密钥为K则
$$
C = (P_1 +K_1,P_2+K_2,\dots,P_m+K_m)\text{mod}26
$$

$$
P = (C_1 -K_1,C_2-K_2,\dots,C_m-K_m)\text{mod}26
$$

举个栗子：

如果密钥是LOVE，那么明文会四个一组进行循环。明文的第一个位置会使用“L”进行加密，第二个位置会使用“O”进行加密，第四个位置会使用“E”进行加密，到第五个位置时又会回归到使用“L”进行加密。

一般情况下，维吉尼亚密码的破解必须依赖爆破+词频统计的方法来进行

##### 希尔密码

希尔密码（Hill）使用每个字母在字母表中的顺序作为其对应的数字，即 A=0，B=1，C=2 等，然后将明文转化为 n 维向量，跟一个 n × n 的矩阵相乘，再将得出的结果模 26。注意用作加密的矩阵（即密匙）在$\mathbb{Z}^n_{26}$必须是可逆的，否则就不可能解码。只有矩阵的行列式和26互质，才是可逆的。下面举一个例子

```text
明文：ACT
```

将明文化为矩阵
$$
\left[
\begin{matrix}
0 \\\\
2 \\\\
19
\end{matrix}
\right]
$$


假设密钥为：
$$
\left[
\begin{matrix}
6 & 24 & 1\\\\
13 & 16 & 10\\\\
20 & 17 & 15
\end{matrix}
\right]
$$
加密过程为
$$
\left[
\begin{matrix}
6 & 24 & 1\\\\
13 & 16 & 10\\\\
20 & 17 & 15
\end{matrix}
\right]
\left[
\begin{matrix}
0\\\\
2\\\\
19
\end{matrix}
\right]
\equiv
\left[
\begin{matrix}
67\\\\
222\\\\
319
\end{matrix}
\right]
\equiv
\left[
\begin{matrix}
15\\\\
14\\\\
7
\end{matrix}
\right]
\ \text{mod}\ 26
$$

```text
密文：POH
```




## 参考

1. [古典密码简介 - CTF Wiki (ctf-wiki.org)](https://ctf-wiki.org/crypto/classical/introduction/)
2. [密码发展史之古典密码 _国家密码管理局门户 (oscca.gov.cn)](https://oscca.gov.cn/sca/zxfw/2017-04/24/content_1011709.shtml)
3. [古典密码简介 - CTF Wiki (ctf-wiki.org)](https://ctf-wiki.org/crypto/classical/introduction/)
4. [单表代换加密 - CTF Wiki (dyf.ink)](http://www.dyf.ink/crypto/classical/monoalphabetic/)
5. [其它类型加密 - CTF Wiki (ctf-wiki.org)](https://ctf-wiki.org/crypto/classical/others/)