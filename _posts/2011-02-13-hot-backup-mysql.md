---
title : 不关闭服务器备份mysql数据文件
layout :  post
tags : 
- Linux
- Mysql
---
<div> <br/>需要打安装一个补丁，就可以实现 : <br/><a href="http : //code.google.com/p/google-mysql-tools/wiki/InnodbFreeze" target="_blank">http : //code.google.com/p/google-mysql-tools/wiki/InnodbFreeze</a><br/><br/>不关闭mysql服务器的情况下，可以禁止掉所有的innodb文件的写，但不阻塞读,这个时候可以利用操作系统的文件拷贝命令进行在线备份 </div>