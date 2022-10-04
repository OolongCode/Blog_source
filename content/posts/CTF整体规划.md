---
title: "CTF整体规划"
date: 2022-01-29T09:29:15+08:00
draft: false
math: false
tags: ["ctf"]
toc: true
---

# CTF整体规划

**CTF**（Capture The Flag，夺旗赛）起源于 1996 年 **DEFCON** 全球黑客大会，是网络安全爱好者之间的竞技游戏。

**CTF** 竞赛涉及众多领域，内容繁杂。与此同时，安全技术的发展速度越来越快，**CTF** 题目的难度越来越高，初学者面对的门槛越来越高。
## 概述^1^
### CTF 的起源 

CTF 的前身是传统黑客之间的网络技术比拼游戏，起源于 1996 年第四届 DEFCON。

### 早期 CTF 竞赛

第一个 CTF 比赛（1996 年 - 2001 年），没有明确的比赛规则，没有专业搭建的比赛平台与环境。由参数队伍各自准备比赛目标（自行准备与防守比赛目标，并要尝试攻破对方的比赛目标）。而组织者大都只是一些非专业的志愿者，接受参赛队伍手工计分的请求。

没有后台自动系统支持和裁判技术能力认定，计分延迟和误差以及不可靠的网络和不当的配置，导致比赛带来了极大的争论与不满。

### 「现代」CTF 竞赛 

由专业队伍承担比赛平台、命题、赛事组织以及自动化积分系统。参赛队伍需提交参赛申请，由 DEFCON 会议组织者们进行评选。

就 LegitBS 组织的三年 DEFCON CTF 比赛而言，有以下突出特点：

- 比赛侧重于对计算机底层和系统安全的核心能力，Web 漏洞攻防技巧完全被忽略。
- 竞赛环境趋向多 CPU 指令架构集，多操作系统，多编程语言。
- 采用「零和」计分规则。
- 团队综合能力考验：逆向分析、漏洞挖掘、漏洞利用、漏洞修补加固、网络流量分析、系统安全运行维护以及安全方面的编程调试。

## 二级制安全规划

### Hacking 三部曲

- 理解系统（Understanding）
  - 系统性地基础课程学习，深入理解计算机系统运作机制
- 破坏系统（Breaking）
  - 学习与创造漏洞挖掘与利用技巧
- 重构系统（Reconstruction）
  - 设计与构建系统防护

### 基础课程学习

核心基础课程 - 计算工作原理

- 体系结构 

  CPU的设计与实现 [CMU 18-477](https://course.ece.cmu.edu/~ece447/s15/doku.php)

  - 机器指令与汇编语言
  - 指令的解码、执行
  - 内存管理

- 编译原理

  编译器的设计与实现 [Stanford CS143](http://web.stanford.edu/class/cs143/)

  - 自动机、词法分析、语法分析
  - 运行时
  - 程序静态分析

- 操作系统

  操作系统的设计与实现 [MIT 6.828](https://pdos.csail.mit.edu/6.828/2018/schedule.html)

  - 系统的加载与引导
  - 用户态与内核态、系统调用、中断和驱动
  - 进程与内存管理、文件系统
  - 虚拟机

其他基础课程 - 系统软件开发基础

- 编程语言
- 网络协议
- 算法与数据结构

### 漏洞挖掘与利用快速入门 - CTF

- 蓝莲花战队CTF成长秘诀——坚持超过1年的以赛代练
- CTF历史资料库：[CTFs · GitHub](https://github.com/ctfs/)
- Wargames
  - [https://pwnable.kr](https://pwnable.kr/)
  - [SmashTheStack Wargaming Network](http://smashthestack.org/)

### 漏洞挖掘与利用实战

#### 如何从CTF赛棍转型

CTF

- 短时间
- 目标代码量小
- 漏洞容易发现
- 利用技巧千奇百怪

实战 - 长期做一道很难的CTF题

- 长期
- 目标代码量大
- 漏洞难以发现
- 利用技术有套路可寻

#### 目标

- 网络协议的实现
- 脚本引擎
- 内核

#### 准备

- 学习历史漏洞 - CVEs
- 挖掘新漏洞
  - 逆向分析 + 代码审计
    - 快速逆向与理解
    - 对漏洞的感觉
  - 模糊测试
    - 测试框架
    - 样例生成想法

### 构建系统防护

漏洞自动挖掘技术

- 静态程序分析
- 符号执行
- 机器学习

漏洞利用防护机制

- Intel SGX
- 控制流完整
- 拟态

## WEB安全规划

### 漏洞类型

注入类

- SQL注入
- XSS
- XEE
- 命令执行，命令注入
- 文件上传，文件下载

信息泄露

- 源码泄露
- 敏感信息泄露
- 员工资料泄露
- 服务器信息泄露

逻辑类

- 权限绕过
- 条件竞争
- 数据篡改

### 基础课程学习

#### 核心基础课程 - 网站工作原理

HTTP协议

- http-header构成
- http-body构成

- http方法

Webserver

- Webserver分类
- Webserver解析流程
- Webserver基础安全

#### 其他基础课程 - 软件开发基础

编程语言

- 前端：html、js、css
- 后端、脚本语言：php、java、python

数据库原理

- 关系型数据库
- 非关系型数据库

### 漏洞挖掘与利用

#### 准备

信息收集工具

- 端口
- 子域名
- 代码泄露
- 员工字典

数据包抓取修改重放工具

顺手的浏览器以及插件

VPS，漏洞验证

#### 挖掘

分析业务功能

分析web架构

针对性罗列可能的漏洞类型

详细测试：不要放过任何一个数据包

#### 利用举例

单一利用

- Getshell
- 敏感信息接口

组合利用

- Xss+csrf

## 相关资料

CTF比赛详情

- [CTFtime.org / All about CTF (Capture The Flag)](https://ctftime.org/)

CTF历史资料库

- [CTFs · GitHub](https://github.com/ctfs/)

Wargames & Labs

- [https://pwnable.kr](https://pwnable.kr/)
- [SmashTheStack Wargaming Network](http://smashthestack.org/)
- [Wargame.kr - 2.1](http://wargame.kr/)
- [PentesterLab: Learn Web Penetration Testing: The Right Way](https://www.pentesterlab.com/)
- [OverTheWire: Wargames](https://overthewire.org/wargames/)
- [Homepage One - exploit-exercises.com](https://exploit-exercises.com/)

## 参考

1. [CTF 历史 - CTF Wiki (ctf-wiki.org)](https://ctf-wiki.org/introduction/history/)
