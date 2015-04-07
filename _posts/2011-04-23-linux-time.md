---
title:linux修改服务器时间
layout: post
tags:
- Linux
---
<div> 修改linux的时间可以使用date指令<br/><br/>在命令行输入:<br/><br/>date<br/>显示当前时间 11月 27日 10:03:16 CST 2008<br/><br/>date -s<br/>按字符串方式修改时间<br/>可以只修改日期,不修改时间,输入: date -s 2008-11-27<br/>只修改时间,输入:date -s 10:03:00<br/>同时修改日期时间,注意要加双引号,日期与时间之间有一空格,输入:date -s "2008-11-27 10:03:00"<br/><br/>修改完后,记得输入:hwclock -w<br/>把系统时间写入CMOS </div>