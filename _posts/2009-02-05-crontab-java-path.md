---
title:crontab 运行java程序 路径问题解决方法
layout: post
tags:
- Linux
- java
---
<div> crontab里运行java程序需要把所有库,都加到环境变量里,自己写了个简单的shell方便使用:<br/>-------------------------------<br/>#!/bin/sh <br/><br/>export   JAVA_HOME=/usr/local/jdk1.6.0_20<br/>export   BCRAWLER_DIR=/home/spider/bcrawler<br/><br/><br/>CLASSPATH=$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar<br/><br/>for jar in `ls $BCRAWLER_DIR/BCrawler_lib/*.jar`<br/>do<br/>CLASSPATH="$CLASSPATH:""$jar"<br/>done<br/><br/>cd $BCRAWLER_DIR<br/>$JAVA_HOME/bin/java -jar -Xmx800m $BCRAWLER_DIR/BCrawler.jar </div>