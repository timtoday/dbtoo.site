---
title: freebsd 常用系统配置
tags:
- FreeBSD
- Linux
---
<div> <p>how to 支持4G以存</p><p>options PAE</p><p>或者<br>修改/boot/loader.conf.local<br>添加： <br>kern.maxdsiz="1700000000" #每个进程最大使用的内存数，默认是512M，这里修改成1.7G<br>kernel="/kernel.PAE" #设置内核启动模式，用PAE模式支持4G以上内存<br>kern.maxswzone="18352640"<br>kern.nswapdev=1</p><p>查找目录里的大文件<br>du -s * | sorht -nr | head</p><p>连接数排序<br>netstat -nat|awk '{print awk $NF}'|sort|uniq -c|sort -n</p><p>查看当前的连接数可以用：<br>ps aux | grep httpd | wc -l<br>pgrep httpd|wc -l</p><p>计算httpd占用内存的平均数:<br>ps aux|grep -v grep|awk '/httpd/{sum+=$6;n++};END{print sum/n}'<br>------------------------------------------<br>定时/etc/crontab <br>f1 f2 f3 f4 f5 program</p><p>其中 f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。 <br>------------------------------------<br>用户管理<br>1 groups&nbsp;&nbsp;&nbsp;&nbsp;  查看使用者是哪个群组</p><p>groups jackpig</p><p>1增加用户</p><p>adduser</p><p>2删除用户</p><p>rmuser</p><p>3更改用户的详细信息<br>chpass</p><p>4显示系统全部用户<br>pw usershow -a</p><p>5增加一个用户组<br>pw groupadd 组名</p><p>6显示系统全部用户组<br>pw groupshow -a</p><p>7查看用户id组id所属组<br>id 用户名</p><p>8　finger 显示用户详细信息</p> </div>