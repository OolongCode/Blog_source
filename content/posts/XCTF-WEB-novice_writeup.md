---
title: "XCTF WEB novice Writeup"
date: 2021-06-25T21:08:13+08:00
draft: false
toc: true
tags: ["ctf","writeup","web"]
categories: ["practice"]
---

来点时效性的文章，不能总闲聊吧？

[XCTF](https://adworld.xctf.org.cn/)是一个国内比较常用的CTF的刷题网站，网站页面如下：

![image-1](/images/XCTF-WEB-novice_writeup/image-1.png)XCTF攻防世界页面

初次写writeup，解题思路可能不是很明确。

本次要解决的题目如下：

![image-2](/images/XCTF-WEB-novice_writeup/image-2.png)XCTF WEB新手区题目

- view source
- robots
- backup
- cookie
- disabled button
- weak auth
- simple php
- get post
- xff referer
- webshell
- command execution
- simple js



## view_source

进入到题目页面中，获取与解题相关的信息

![image-3](/images/XCTF-WEB-novice_writeup/image-3.png)view_source题目

根据题目要求可知，鼠标右键不可用了。

我们进入环境来一探究竟

![image-4](/images/XCTF-WEB-novice_writeup/image-4.png)靶机环境

靶机展示的页面非常简单，说flag不在这儿，我不大相信，尝试使用右键查看源代码

发现右键不能使用。看来靶机的代码把浏览器的右键给禁用了，解决方法有两个：

1. 使用F12进行检查源代码
2. 开启浏览器禁用js模式

这里使用F12进行查看源代码（开启禁用js模式比较麻烦）

![image-5](/images/XCTF-WEB-novice_writeup/image-5.png)

页面源代码

F12成功打开页面源代码调试，可以看到flag就在源代码的注释中，简单题

本题主要考察对浏览器调试器的使用技巧，没有什么难度。



## robots

进入到题目页面环境中，查看题目信息和相关描述。

![image-6](/images/XCTF-WEB-novice_writeup/image-6.png)robots题目

题目描述中提到了robots协议，本菜鸡不知道什么是robots协议，但是可以肯定robots协议就是本题的突破点，我去搜索查找一下有关robots协议的相关信息。

![image-7](/images/XCTF-WEB-novice_writeup/image-7.png)robots协议相关信息

根据百度百科的说明，其实robots协议就是网站目录下的robots.txt文件

预备的知识信息获取到了，下面进入到靶机环境，去拿flag

![image-8](/images/XCTF-WEB-novice_writeup/image-8.png)靶机页面

靶机页面是个空白页面，在靶机地址后面输入/robots.txt尝试找到flag

![image-9](/images/XCTF-WEB-novice_writeup/image-9.png)

进入到robots.txt页面寻找有关flag的相关信息

![image-10](/images/XCTF-WEB-novice_writeup/image-10.png)

robots.txt页面信息

根据robots.txt展示的页面信息，可知flag就在flag_1s_h3re.php文件中

那就进入到这个文件中

![image-11](/images/XCTF-WEB-novice_writeup/image-11.png)flag_1s_h3re.php文件页面

成功拿到flag数据信息，题目也是简单题

这道题目主要考察robots协议的相关知识以及网站目录的部分知识，也是简单题，思路非常明确



## backup

进入到题目页面中，寻找有用的题目突破信息

![image-12-1024x292](/images/XCTF-WEB-novice_writeup/image-12-1024x292.png)backup题目

根据题目描述，这道题目是在考察备份文件，备份文件是解题的关键

根据备份文件的相关信息可知，备份文件通常都是后缀名.bak的文件

已有知识准备好了，现在进入到靶机环境中拿flag

![image-13](/images/XCTF-WEB-novice_writeup/image-13.png)靶机页面

靶机直接就把提示摆到页面上面了，直接访问index.php.bak文件就可以了

一般来说index.php的备份文件就是index.php.bak文件

访问url/index.php.bak，备份文件成功被下载下来

![image-14](/images/XCTF-WEB-novice_writeup/image-14.png)

备份文件

打开备份文件寻找信息

![image-15](/images/XCTF-WEB-novice_writeup/image-15.png)备份文件信息

发现flag数据就在备份文件中，题目解决，也是一道简单题目

题目主要考察的就是备份文件的相关知识，简单题，思路非常明确



## cookie

进入到题目页面，寻找与解题相关的信息

![image-16-1024x282](/images/XCTF-WEB-novice_writeup/image-16-1024x282.png)cookie题目

题目描述和题目明显提示是cookie相关的知识，cookie的知识一般做web安全都是必须知道且需要了解的一个重要的知识点。这里搬出MDN上面对于[cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)的说明，cookie知识不清楚的可以去MDN页面中了解

![image-17](/images/XCTF-WEB-novice_writeup/image-17.png)MDN cookie

简单来说，cookie就是存储在用户服务器上的一段信息内容

可以使用浏览器的调试器查看该页面的cookie数据，准备知识现在已经完备。

进入到靶机环境，拿flag！

![image-18](/images/XCTF-WEB-novice_writeup/image-18.png)

靶机页面

靶机页面信息展示的很明确，就是cookie

打开F12调试器查看cookie信息

![image-19-1024x234](/images/XCTF-WEB-novice_writeup/image-19-1024x234.png)cookie数据

发现有很多条cookie数据，不知道该选择哪一条cookie数据，我发现这些cookie的domain信息不太一样。有四条的domain信息是baidu.com，只有一条的domain信息是靶机的ip地址，看来需要的cookie信息就是domain信息是靶机ip地址的那条cookie

![image-20](/images/XCTF-WEB-novice_writeup/image-20.png)靶机cookie数据

cookie的键值对是look-here:cookie.php，cookie.php显然不是flag数据，估计是想让我们访问这个文件，我们来访问一下这个文件

![image-21](/images/XCTF-WEB-novice_writeup/image-21.png)

cookie.php页面信息

这个页面展示的内容也是非常简单的，让我们去看看response信息。

可能有人会问response是什么？response就是http头部信息的响应信息，在调试器的网络那一栏可以查看到页面的http头部信息。关于http头部信息的更多内容，可以访问[MDN的HTTP头部列表](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

打开浏览器的调试器

![image-22-1024x140](/images/XCTF-WEB-novice_writeup/image-22-1024x140.png)网络响应数据

如果响应数据中没有任何信息，可以刷新一下页面来找到响应数据

然后我们选择响应数据中的cookie.php的数据信息

![image-23](/images/XCTF-WEB-novice_writeup/image-23.png)cookie.php的响应信息

发现响应标头的信息中有flag数据，本题也就成功解出，也是简单题目，跟着引导走，很快就能拿到flag

题目主要考察cookie相关知识和htpp头的相关知识内容，这里也可以使用burp suite来抓包获取http信息，简单题，思路也相对比较流畅。



## disabled button

看到这个题目就大致知道这道题目的考察目标了，应该是一道非常简单的题目

来看看题目内容有什么具体的描述信息

![img](/images/XCTF-WEB-novice_writeup/image-24-1024x287.png)disabled_button题目

根据题目描述可以知道，这道题目是考察前端知识的。根据题目disabled_button，这道题目很可能是在考察html标签属性的，下面我们进入到题目中一探究竟

![img](/images/XCTF-WEB-novice_writeup/image-25.png)

靶机页面

页面展示的信息非常清晰不能按的按钮，而且flag信息就藏在这个按钮后面

直接点开F12查看源代码

![img](/images/XCTF-WEB-novice_writeup/image-26.png)源代码检查

发现input标签有关disabled属性，我们将disabled属性删除，按钮就可以按了

![img](/images/XCTF-WEB-novice_writeup/image-27.png)

源代码修改

然后返回到页面上去，发现按钮可以被按下

![img](/images/XCTF-WEB-novice_writeup/image-28.png)

按钮页面

按下按钮，查看可以获取到的信息

![img](/images/XCTF-WEB-novice_writeup/image-29.png)

flag信息

成功获取到flag信息，题目解决，这道题目非常简单，应该是道签到题

题目主要考察前端的html知识，签到题



## weak auth

进入到题目页面，查看可以利用的信息及提示

![img](/images/XCTF-WEB-novice_writeup/image-30-1024x277.png)weak_auth

根据题目和题目描述信息可以知道，这道题目是一个登录页面，而且采用的是弱口令进行认证的页面。

考察的信息应该是暴力破解的知识以及暴力破解的工具使用。

这里说一下暴力破解的内容知识：

暴力破解也叫蛮力攻击，是一种非常无脑的攻击手段，经常会和社会工程学一起采用来达到成功破解用户密码的效果。

蛮力攻击（英语：Brute-force attack），又称为穷举攻击（英语：Exhaustive attack）或暴力破解，是一种密码分析的方法，即将密码进行逐个推算直到找出真正的密码为止。例如：一个已知是四位数并且全部由阿拉伯数字组成的密码，其可能共有10000种组合，因此最多尝试9999次就能找到正确的密码。理论上除了具有完善保密性的密码以外，利用这种方法可以破解任何一种密码，问题只在于如何缩短试误时间。有些人运用计算机来增加效率，有些人透过字典攻击（英语：Dictionary attack）来缩小密码组合的范围。[1]

如果要解决这道题目，就必不可少一些暴力破解会使用的工具来进行暴力破解。暴力破解工具有很多，一般来说，web安全会有一些比较常用的暴力破解工具，这道题目可能需要使用到kali操作系统来辅助解题。常用的暴力破解工具一般有：Hydra，Medusa，Burp suite。

这里可能使用到Hydra进行暴力破解，这里说明一些Hydra的暴力破解的方法和相关参数

**hydra** 是一个支持众多协议的爆破工具，已经集成到KaliLinux中，直接在终端打开即可。[2]

常用的hydra的暴力破解命令：

```bash
1、破解ssh： 
hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns ip ssh 
hydra -l 用户名 -p 密码字典 -t 线程 -o save.log -vV ip ssh 


2、破解ftp： 
hydra ip ftp -l 用户名 -P 密码字典 -t 线程(默认16) -vV 
hydra ip ftp -l 用户名 -P 密码字典 -e ns -vV 


3、get方式提交，破解web登录： 
hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns ip http-get /admin/ 
hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns -f ip http-get /admin/index.php


4、post方式提交，破解web登录： 
hydra -l 用户名 -P 密码字典 -s 80 ip http-post-form "/admin/login.php:username=^USER^&password=^PASS^&submit=login:sorry password" 
hydra -t 3 -l admin -P pass.txt -o out.txt -f 10.36.16.18 http-post-form "login.php:id=^USER^&passwd=^PASS^:<title>wrong username or password</title>" 
（参数说明：-t同时线程数3，-l用户名是admin，字典pass.txt，保存为out.txt，-f 当破解了一个密码就停止， 10.36.16.18目标ip，http-post-form表示破解是采用http的post方式提交的表单密码破解,<title>中 的内容是表示错误猜解的返回信息提示。） 


5、破解https： 
hydra -m /index.php -l muts -P pass.txt 10.36.16.18 https 

6、破解teamspeak： 
hydra -l 用户名 -P 密码字典 -s 端口号 -vV ip teamspeak 

7、破解cisco： 
hydra -P pass.txt 10.36.16.18 cisco 
hydra -m cloud -P pass.txt 10.36.16.18 cisco-enable 

8、破解smb： 
hydra -l administrator -P pass.txt 10.36.16.18 smb 

9、破解pop3： 
hydra -l muts -P pass.txt my.pop3.mail pop3 

10、破解rdp： 
hydra ip rdp -l administrator -P pass.txt -V 

11、破解http-proxy： 
hydra -l admin -P pass.txt http-proxy://10.36.16.18 

12、破解imap： 
hydra -L user.txt -p secret 10.36.16.18 imap PLAIN 
hydra -C defaults.txt -6 imap://[fe80::2c:31ff:fe12:ac11]:143/PLAIN
```

这些常用的命令解决这道题目应该是足够的。

现在工具和知识都已经准备完毕了，进入靶机来一探究竟

![img](/images/XCTF-WEB-novice_writeup/image-31.png)

weak_auth页面

非常简单的一个登录认证页面，先进行简单的密码猜测

使用root:root进行登录尝试

![img](/images/XCTF-WEB-novice_writeup/image-32.png)

弹出提示，please login as admin，说明登录的用户名必须要素admin

下面试试admin:admin进行登录尝试

![img](/images/XCTF-WEB-novice_writeup/image-33.png)

弹出提示，password error，登录错误的提示，然后点击确定查看一下页面的源代码

![img](/images/XCTF-WEB-novice_writeup/image-34.png)靶机error页面源代码

发现登录错误关键字error，然后查看页面的响应标头确定传输方式

![img](/images/XCTF-WEB-novice_writeup/image-35.png)

发现页面数据的传输方式是post方式进行传输的。而且页面存在着跳转，hydra的易用性相对较差，这里需要选用burpsuite进行暴力破解

进入到kali系统中，抓取页面信息进行暴力破解攻击

![img](/images/XCTF-WEB-novice_writeup/image-36.png)bp抓到的数据包

右键将页面发送到intruder页面中

![img](/images/XCTF-WEB-novice_writeup/image-37.png)

然后点击intruder页面进行暴力破解的设置

![img](/images/XCTF-WEB-novice_writeup/image-38-1024x287.png)

调整好参数，然后进入到option的配置页面中进行攻击，（最好找一个弱口令字典）

![img](/images/XCTF-WEB-novice_writeup/image-41-1024x490.png)

简单设置进行暴力破解的字典，然后设置匹配项。由于我们知道页面登录失败的时候会出现password error的选项，因此进入到option页面中设置匹配。

![img](/images/XCTF-WEB-novice_writeup/image-42.png)

点击clear将所有的匹配规则清除

![img](/images/XCTF-WEB-novice_writeup/image-43.png)

点击add将error的匹配规则添加进去

![img](/images/XCTF-WEB-novice_writeup/image-44.png)

然后点击start attack开始攻击

![img](/images/XCTF-WEB-novice_writeup/image-45.png)

稍微等一下，等攻击结果出现

![img](/images/XCTF-WEB-novice_writeup/image-46.png)

发现123456这个密码没有匹配到error的规则，然后进入到页面中将123456密码输入进去

![img](/images/XCTF-WEB-novice_writeup/image-47.png)

进入到跳转页面中，成功找到flag数据。

本题也成功解决，题目的思路也是比较简单的。考察的要点就是暴力破解，通过暴力破解解决问题。

题目属于简单题。



## simple php

这道题的题目是simple_php，应该是一道考察php代码的简单题目

进入到题目页面，来获取到更多信息

![img](/images/XCTF-WEB-novice_writeup/image-48-1024x287.png)simple_php题目

页面中的题目描述信息也是在说php代码的问题，这道题目应该是在考察php代码的简单使用情况

进入到靶机环境来一探究竟

![img](/images/XCTF-WEB-novice_writeup/image-49.png)

直接展示出源代码，这应该是一道简单的php代码审计题目，本菜鸡的php基础还可以，这道题目主要是考察php代码的特性和缺陷。

这道题目中有三个特性进行了考察：

1. php中的字符串在进行比较的时候都会被当作0来处理
2. php中的变量如果被赋值了数字加字符，在进行数值判断的时候，字符会被忽略
3. php中的`is_numeric()`函数会判断变量是否是纯数字，如果是纯数字就返回true，如果不是纯数字就返回false

有时候php代码审计的题目遇到不认识的代码或者函数，可以进行搜索引擎的使用和查找

代码可控的地方是get传输的数据，a和b参数作为数据接收并进行传递的

由于这道题目比较简单，直接在url上面进行构造

构造payload：`url?a=Flag&b=1235s`

![img](/images/XCTF-WEB-novice_writeup/image-50.png)

成功获取到flag数据，题目解决。

题目主要考察get传输方式和php代码的特性，思路也比较简单，分析代码的逻辑进行简单的注入就可以解决问题。属于简单题目。



## get post

看题目，这道题目应该是考察http的传输数据的方式，get传输方式和post传输数据的方式

点开题目，希望可以从题目页面中获取到更多的信息

![img](/images/XCTF-WEB-novice_writeup/image-51-1024x307.png)get_post题目

题目描述也是说用get和post方式，看来这道题目的关键点就是get和post传输数据的方式。

由于这里涉及的post方式的传输，这里需要使用一个Hackbar的插件攻击来辅助进行注入进攻来获取到flag数据。这里给出hackbar的GitHub地址：https://github.com/Hack-Free/HackBar，如果没有这个工具可以进行下载使用。

现在工具齐全了，可以进行尝试去拿flag了，打开靶机进入到环境中



![img](/images/XCTF-WEB-novice_writeup/image-52.png)

页面中展示的信息非常明确，而且比较明了。为了方便操作，进入到kali系统中的已装好hackbar的firefox浏览器中进行操作。

![img](/images/XCTF-WEB-novice_writeup/image-53-1024x222.png)

首先使用get方法进行提交，点击execut进行传输

![img](/images/XCTF-WEB-novice_writeup/image-54.png)

页面内容发生了变化，这次使用post数据进行传输

![img](/images/XCTF-WEB-novice_writeup/image-55-1024x333.png)

点击execute进行传输数据

![img](/images/XCTF-WEB-novice_writeup/image-56.png)

数据传输过去后，页面发生变化，然后flag数据就展示在眼前，题目解决。

这道题目的思路非常清晰，就是引导性的题目，没有什么难度，应该是道签到题目。

题目考察的知识点是http传输数据的方式，属于签到题。



## xff referer

刚开始看到这个题目标题的时候还是有点懵逼的，因为本菜鸡并不知道什么是xff和referer

于是使用搜索引擎解决一下问题

**`X-Forwarded-For`** (XFF) 在客户端访问服务器的过程中如果需要经过HTTP代理或者负载均衡服务器，可以被用来获取最初发起请求的客户端的IP地址，这个消息首部成为事实上的标准。在消息流从客户端流向服务器的过程中被拦截的情况下，服务器端的访问日志只能记录代理服务器或者负载均衡服务器的IP地址。如果想要获得最初发起请求的客户端的IP地址的话，那么 X-Forwarded-For 就派上了用场。[3]

`**Referer**` 请求头包含了当前请求页面的来源页面的地址，即表示当前页面是通过此来源页面里的链接进入的。服务端一般使用 `Referer` 请求头识别访问来源，可能会以此进行统计分析、日志记录以及缓存优化等。[4]

发现Xff和Referer就是一个可以进行IP代理的东西和一个可以进行来源记录的东西

再去查一下Xff和Referer的语法格式，确保对于Xff和Referer的知识掌握的比较完善。

于是再去MDN上查看一波：

```http
X-Forwarded-For: <client>, <proxy1>, <proxy2>

# 示例
X-Forwarded-For: 2001:db8:85a3:8d3:1319:8a2e:370:7348

X-Forwarded-For: 203.0.113.195

X-Forwarded-For: 203.0.113.195, 70.41.3.18, 150.172.238.178
Referer: <url>

# 示例
Referer: https://developer.mozilla.org/en-US/docs/Web/JavaScript
```

点开题目查看，题目中又有些什么信息

![img](/images/XCTF-WEB-novice_writeup/image-57-1024x287.png)xff_referer题目

根据题目描述，xff和referer是可以伪造的，可以知道这道题目应该是伪造xff和referer的题目，由于xff和referer都是http头部的信息，所以需要使用burp suite进行抓包来伪造xff和referer信息，需要先启动一下kali操作系统。

目前，知识基础和工具基础都准备好了，进入到靶机环境

![img](/images/XCTF-WEB-novice_writeup/image-58.png)靶机环境

要求ip必须为123.123.123.123，用burp suite抓到数据包，修改xff数据来进行伪造

![img](/images/XCTF-WEB-novice_writeup/image-59.png)

然后进行放行来查看页面情况

![img](/images/XCTF-WEB-novice_writeup/image-60.png)

页面返回了一个必须来自https://www.google.com

再次抓包，设置一下referer和xff的信息

![img](/images/XCTF-WEB-novice_writeup/image-61.png)

将数据包放行，然后查看页面信息

![img](/images/XCTF-WEB-novice_writeup/image-62.png)

最后，页面成功出现flag信息，题目成功解决，题目比较简单，具有引导性

题目属于简单的题目，应该算是签到题，题目主要考察对于xff和referer的http头部信息的了解和掌握，思路比较流程，具有引导性。



## webshell

看到这个·题目，首先第一反应是上传php一句话木马拿webshell。可能有人不解，什么是webshell？什么是一句话木马？这里搬出百度百科的解释，对webshell简单说明：

webshell就是以asp、php、jsp或者cgi等网页文件形式存在的一种代码执行环境，也可以将其称做为一种网页后门。黑客在入侵了一个网站后，通常会将asp或php后门文件与网站服务器WEB目录下正常的网页文件混在一起，然后就可以使用浏览器来访问asp或者php后门，得到一个命令执行环境，以达到控制网站服务器的目的。[5]

webshell简单来说就是命令执行的环境，而一句话木马就是在创建一个可以连接到网站的命令执行环境的一个后门程序，这个后门程序通常都是比较简单，比较小的文件。可以通过网站的文件上传漏洞进行文件上传，创建后门木马。

进入到题目页面，看看可以获取到什么额外的信息：

![img](/images/XCTF-WEB-novice_writeup/image-63-1024x274.png)webshell题目

根据题目描述，这道题目应是考察一句话木马的题目，而且是php一句话木马的题目。

根据目前的推出和知识分析，进入环境来看看怎么拿flag

![img](/images/XCTF-WEB-novice_writeup/image-64.png)靶机环境

靶机环境中的页面直接把页面中写入的php一句话木马展示出来了，是通过post方式进行参数传递的。

这道题目可以使用hackbar插件进行post数据的传输，首先进行hello world输出来测试webshell的稳定性，根据页面回显情况来进行下一步操作。

![img](/images/XCTF-WEB-novice_writeup/image-66-1024x352.png)

查看一下，页面的回显情况

![img](/images/XCTF-WEB-novice_writeup/image-71.png)

页面将hello world成功输出到页面上面，说明页面会直接将代码执行结果回显到页面上面，回显效果良好。

接下来，讲一个的php的小技巧：

> php代码中的反引号```可以直接执行终端shell命令.并返回输出

```php
<?php 
    echo `ls`; #会将ls命令的输出结果输出到php页面上面
?>
```

下面我们就可以根据这个小技巧来构造payload：`shell=echo `ls`;`

![img](/images/XCTF-WEB-novice_writeup/image-67-1024x340.png)

查看页面返回的结果

![img](/images/XCTF-WEB-novice_writeup/image-68.png)

发现网站的站点目录下有两个文件，一个是index.php文件，一个是flag.txt文件

显然flag文件肯定就是目标文件，需要查看到flag.txt文件中究竟写了些什么样的内容，flag.txt文件很可能藏着flag文件

构造payload：`shell=echo `cat flag.txt`;`

![img](/images/XCTF-WEB-novice_writeup/image-69-1024x363.png)

查看页面显示的结果

![img](/images/XCTF-WEB-novice_writeup/image-70.png)

发现flag.txt文件中写的就是flag数据，题目解决

题目主要考察php一句话木马，php特性和linux命令的简单使用，整体思路还是比较流畅的，题目难度比较简单，顺着思路就可以解决了。当然此题有多种解法。



## command execution

看到题目，可能是考察命令执行漏洞的题目，从题目也获取不到太多信息

直接点开题目页面，来看看有没有更多的信息

![img](/images/XCTF-WEB-novice_writeup/image-72-1024x290.png)command_execution题目

题目描述说是ping功能，题目可能于ping功能有些出入，进入靶场环境看看情况

![img](/images/XCTF-WEB-novice_writeup/image-73.png)

靶机环境

页面非常简单，好像就是一个ping功能的页面，首先试试使用127.0.0.1地址进行测试

![img](/images/XCTF-WEB-novice_writeup/image-74.png)

发现这是一个命令执行环境，可控的地方就是输入框

来分析一下输入框的输入模式：

> 输入框可以输入ip地址和url地址
>
> 输入的内容前方会被增加`ping -c 3 `的代码
>
> 输入内容的后面不会增加任何额外代码
>
> 页面输出内容会把终端输出内容返回

因此，这里可以使用一点shell的语法技巧来构造payload

`&&`在shell语法中是前面的命令执行成功后继续执行后面的代码

```
ping -c 3 127.0.0.1 && ls # 会先执行ping命令，ping命令执行成功会再执行ls命令
```

于是构造payload：`127.0.0.1 && ls` ，并输入到输入框中来执行

![img](/images/XCTF-WEB-novice_writeup/image-75.png)

发现网站页面下没有藏有flag文件，下一个可能的目录是home目录或是root目录

访问root目录需要权限，于是先查看一下ping功能的用户权限

构造payload：`127.0.0.1 && id` ，并输入到输入框中来执行

![img](/images/XCTF-WEB-novice_writeup/image-76.png)

发现ping的权限仅仅知识apache的权限，使用的服务器很可能是Ubuntu服务

构造payload：`127.0.0.1 && uname -a` ，并输入到输入框中执行

![img](/images/XCTF-WEB-novice_writeup/image-77.png)

发现服务器确实是Ubuntu服务器

根据目前收集到的信息，可能只能访问到home目录下，那就先尝试查看到home目录，如果home目录下没有再尝试提权进入到root目录下

构造查看home目录的payload：`127.0.0.1 && ls /home `，输入到输入框中执行

![img](/images/XCTF-WEB-novice_writeup/image-78.png)

发现home目录下存在有flag文件，让本菜鸡来瞧瞧这个flag.txt里面写的啥

构造payload：`127.0.0.1 && cat /home/flag.txt `，输入到输入框中执行

![img](/images/XCTF-WEB-novice_writeup/image-79.png)

发现flag.txt里面写的就是flag数据，题目解决

题目主要考察linux命令行的使用以及对于命令执行漏洞的觉察，题目的解题思路还是比较流畅的，题目应该也属于简单题。难度并不是很高。



## simple js

看到题目，应该是一个简单JavaScript代码审计的题目

进入到题目页面中，希望可以获取到更多相关的数据

![img](/images/XCTF-WEB-novice_writeup/image-80-1024x304.png)simple_js题目

看到题目的难度系数，可知这道题目应该不简单，网页一直输入不对密码，这应该是一个提示

下面就直接进入到靶机环境来看看情况

![img](/images/XCTF-WEB-novice_writeup/image-81.png)

页面直接就是一个提示框，先随便输入点内容

![img](/images/XCTF-WEB-novice_writeup/image-82.png)

就报出了另一个提示框，然后页面内容是空白的

![img](/images/XCTF-WEB-novice_writeup/image-83.png)

这种情况下，只能尝试从F12检查源代码中找到一些有用的信息

![img](/images/XCTF-WEB-novice_writeup/image-84.png)

在源代码检查的过程中找到了js的代码，这道题目应该是对js源代码的审计

```javascript
function dechiffre(pass_enc) {
    var pass = "70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65";
    var tab = pass_enc.split(',');
    var tab2 = pass.split(',');
    var i, j, k, l = 0,
        m, n, o, p = "";
    i = 0;
    j = tab.length;
    k = j + (l) + (n = 0);
    n = tab2.length;
    for (i = (o = 0); i < (k = j = n); i++) {
        o = tab[i - l];
        p += String.fromCharCode((o = tab2[i]));
        if (i == 5) break;
    }
    for (i = (o = 0); i < (k = j = n); i++) {
        o = tab[i - l];
        if (i > 5 && i < k - 1)
            p += String.fromCharCode((o = tab2[i]));
    }
    p += String.fromCharCode(tab2[17]);
    pass = p;
    return pass;
}
String["fromCharCode"](dechiffre("\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30"));

h = window.prompt('Enter password');
alert(dechiffre(h));
```

然后对这段js代码进行简单分析：

> 1. 从js代码整体来看，代码先定义了一个dechiffre的函数，然后定义了一个字符串数组，然后使用了两个功能性函数进行弹窗。
>
> 2. 整段js代码的核心应该是应该是定义的dechiffre的函数，对于dechiffre函数的分析应该就是解决这道题目的关键性问题

下面对JS源代码中的dechiffre函数进行分析：

首先将dechiffre函数内部进行划分

```javascript
function dechiffre(pass_enc) {

    // 变量定义区
    var pass = "70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65";
    var tab = pass_enc.split(',');
    var tab2 = pass.split(',');
    var i, j, k, l = 0,
        m, n, o, p = "";

    // 变量处理区
    i = 0;
    j = tab.length;
    k = j + (l) + (n = 0);
    n = tab2.length;

    // 逻辑处理区
    for (i = (o = 0); i < (k = j = n); i++) {
        o = tab[i - l];
        p += String.fromCharCode((o = tab2[i]));
        if (i == 5) break;
    }
    for (i = (o = 0); i < (k = j = n); i++) {
        o = tab[i - l];
        if (i > 5 && i < k - 1)
            p += String.fromCharCode((o = tab2[i]));
    }

    // 最终输出区
    p += String.fromCharCode(tab2[17]);
    pass = p;
    return pass;
}
```

js函数被划分成四个区域：

1. 变量定义区
2. 变量处理区
3. 逻辑处理区
4. 最终输出区

下面对这四个分区进行逐一分析

变量定义区：

```javascript
var pass = "70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65";
    var tab = pass_enc.split(',');
    var tab2 = pass.split(',');
    var i, j, k, l = 0,
        m, n, o, p = "";
```

> 1. 定义了一个pass变量并赋值"70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65"
>
> 2. 定义了一个tab变量并赋值pass_enc参数进行分隔成数组
>
> 3. 定义了一个tab2变量并赋值pass变量进行分隔成数组[70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65]
>
> 4. 定义了变量i，j，k，l并赋值为0，定义了变量m，n，o，p并赋值为“”

变量处理区

```
    i = 0;
    j = tab.length;
    k = j + (l) + (n = 0);
    n = tab2.length;
```

> 1. 将变量i再次赋值为0
>
> 2. 将变量j赋值为tab的长度
>
> 3. 将变量k赋值为j的值加上l和n=0的数值
>
> 4. 将变量n赋值为tab2的长度，即n=18

逻辑处理区

```javascript
    for (i = (o = 0); i < (k = j = n); i++) {
        o = tab[i - l];
        p += String.fromCharCode((o = tab2[i]));
        if (i == 5) break;
    }
    for (i = (o = 0); i < (k = j = n); i++) {
        o = tab[i - l];
        if (i > 5 && i < k - 1)
            p += String.fromCharCode((o = tab2[i]));
    }
```

> 1. 对于第一个循环，初始值i被赋值为0，限制条件是i<18，循环条件是i++
>
>    循环内部是对于o变量的处理，第一个赋值语句是无用的赋值语句，由于下面的语句会对o进行   重新赋值处理。下面`p += String.fromCharCode((o = tab2[i]));`语句涉及了string对象和fromCharCode（）函数。经过搜索和查询，发现fromcharcode函数是将unicode值转换为字符的函数，属于String对象的api。这条语句的作用是对p变量进行累计赋值处理。如果i==5循环就结束。
>
> 2. 对于第二个循环，初始值i被赋值为0，限制条件是i<18，循环条件是i++
>
>    循环内部依旧是对于o变量的处理，还是和第一个循环非常类似的处理，都是最终对于p变量进行累计赋值。
>
> 3. 两个循环都是对于p变量进行累加赋值。

最终输出区

```javascript
    p += String.fromCharCode(tab2[17]);
    pass = p;
    return pass;
```

> 1. 仍然是对p变量进行赋值处理
>
> 2. 将p的值赋值给pass
>
> 3. 将pass变量返回

总体对这个函数进行分析，这个函数根本没有涉及任何传入参数的处理情况，简单来说就是没有tab数组任何事情。无论传入什么变量都只返回tab2数组的数据。

再看看代码最后的调用情况

```javascript
String["fromCharCode"](dechiffre("\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30"));

h = window.prompt('Enter password');
alert(dechiffre(h));
```

> 1. 这个函数被调用两次。
>
> 2. 第一次是调用了dechiffre并传入参数
>
> ```text
> “\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30”
> ```
>
> 作为函数的实参进行传入数据
>
> 3. 第二次是调用了用户输入的数据（无论传入什么数据结果都一样）

所以这个JavaScript的代码中肯定藏有flag，flag可能藏在第一次传入的参数中

```text
\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30
```



编写js文件对第一次传入的参数进行处理

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>test</title>
	<script type="text/javascript">
		var input="\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30";
		var result;
		var o=0;
		var tab=input.split(',');
		document.write(tab);
		for (var i = 0; i<tab.length; i++){
			result += String.fromCharCode((o=tab[i]))
		}
    	document.write(result);
	</script>
</head>
<body>

</body>
</html>
```

在浏览器上运行一下这段代码

![img](/images/XCTF-WEB-novice_writeup/image-85.png)

undefined后面那段字符就是flag数据：786OsErtk12

这道题目也解决了，分析过程比较复杂，需要一定的JavaScript基础。难度其实也应该是一道简单题目，但是思路比较绕，如果比较灵敏可以直接找到关键数据，对关键数据进行unicode解码在进行ascii解码就能得出flag数据。

这里提供几个网址，便于js基础不是非常牢固的人补习一下：

https://javascript.ruanyifeng.com/

https://www.liaoxuefeng.com/wiki/1022910821149312

https://developer.mozilla.org/zh-CN/docs/learn/JavaScript



## 参考

1. [WIKI百科-蛮力攻击](https://wiwiki.kfd.me/wiki/暴力破解)
2. [爆破工具 Hydra 简单使用](https://www.jianshu.com/p/4da49f179cee)
3. [X-Forwarded-For MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Forwarded-For)
4. [Referer MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referer)
5. [webshell 百度百科](https://baike.baidu.com/item/WEBSHELL)



本期wp分享到此为止，有时间再来喝杯茶呀！