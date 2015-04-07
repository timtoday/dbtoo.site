---
title:shell简单的守护程序
layout: post
tags:
- Linux
- shell
---
<div> <p>偷懒监控mysql  apche  nginx之类的服务是否down掉。</p><p>自动恢复</p><p> </p><p>#!/bin/sh<br/>count=`ps -ef |grep "mysqld"  | wc -l` <br/>if [ $count -lt 3 ] ;then<br/>#       echo "restart"<br/>        /etc/init.d/mysql restart<br/>fi<br/></p> </div>