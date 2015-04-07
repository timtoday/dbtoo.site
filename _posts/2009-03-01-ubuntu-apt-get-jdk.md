---
title : ubuntu 10.0.4 apt-get安装jdk
layout :  post
tags : 
- Linux
- ubuntu
- Java
---
<div> ubuntu  10.04已经去掉 sun-java6-jdk 和 sun-java6- jre的软件包，ubuntu  官方声明：建议安装 openjdk-6 以取代 sun-java6-jre ，但如果你不能转换到openjdk-6下，仍可以继续使用 sun-java6- jre。小弟对java不是很熟悉，不过据说 openjdk和 sun-jdk还是有很多差别的。<br/>安装办法：<br/>1，编辑源列表：~$  sudo vim /etc/apt/sources.list ，在最后 添加一行：<br/><br/>deb http : //archive.canonical.com/ lucid partner<br/>2，更新 : ~$ sudo  apt-get update<br/><br/>3，安装java： ~$ sudo apt-get install sun-java6-jre <br/><br/>如果需要也可以直接安装jdk： ~$ sudo apt-get install sun-java6-jdk </div>