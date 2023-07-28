<div align="center"><h1>Danm Vulnerable Router Firmware</div>
<p align="center"><img src="./images/brand.png" alt="DVRF" width="300"  /></p>

# 介绍

DVRF 的全称是 Danm Vulnerable Router Firmware，该项目是一个基于 [OpenWrt](https://openwrt.org/) 改造的漏洞固件。用 CTF 模式来帮助安全专业人员测试物联网设备中常见的漏洞，其中部分漏洞题基于公开的 CVE 漏洞。

| DVRF | 描述 |
| :--- | :--- |
| L1 Brute Login |  |
| L2 Damn XSS |(CVE-2019-18993) |
| L3 What‘s your bandwidth? | (CVE-2019-12272) |
| L4 Untar | 
| L5 Easy serialize 
| L6 Try Opkg | (CVE-2020-7982) |
| L7 Baby & Big Pwn | (CVE-2018-1160) |

## L3 What's your bandwidth?

本题是基于公开漏洞 [CVE-2019-12272](https://www.cvedetails.com/cve/CVE-2019-12272/) 设计而成，旨在让用户了解有关 **URL 接口的命令注入漏洞**。该接口可用于获取网卡 eth0 的宽带数据，但是由于处理参数时未检查和处理命令拼接而导致命令注入漏洞。在设计该题目时，为增加题目难度对参数进行一定的过滤，如禁用 `cat`,`less`,`more` 等命令，同时该命令无法直接执行与根目录操作有关的命令，如 `cd /`,`tail /flag` 等命令。本题的解题方法不止一种，对于该命令注入漏洞接口，攻击者可以构造各种各样的 payload 以获取服务器信息。

## L4 Untar

本题旨在让用户了解有关**文件上传的漏洞利用**。该题目设计了上传文件并解压文件到固定目录的功能，在网页端上传选择好的 tar 文件后，服务器就会将上传的文件解压存储在固定目录。由于服务器在解压文件后，未对文件进行检查和处理导致攻击者可以上传木马进行攻击。在设计题目时，对上传的文件进行固定重命名而避免了用户利用文件名进行命令拼接。解题的关键是用户要将木马放在服务器某目录并执行该目录，想要攻击成功攻击者需要了解 openwrt web 端的整体架构与逻辑，主要掌握 lua 语言和 luci 架构。本题的解题方法不止一种，构造怎样的 payload，如何执行木马，木马实现怎样的功能都可以由攻击者设计决定，木马可以简单的执行函数寻找服务器内的 flag 并返回 json 字段在网页端；也可以 socket 连接，让用户直接访问服务器 shell。

## L5 Easy serialize

本题改编自第十六届全国大学生信息安全创新实践能力赛-华北赛区赛题 ez_date 旨在让用户了解有关**反序列化的漏洞利用**。该题目会为用户提供反序列化函数源码及相关会用到函数源码，用户根据源码了解反序列化函数的运行逻辑，自行构造序列化值发送到相应接口获取 flag。在反序列化函数中有有关 md5 值的比较，需要攻击者可以绕开 md5 值比较，使得两个变量值不同但 md5 值相同；除此之外还需要绕过一些过滤规则。
