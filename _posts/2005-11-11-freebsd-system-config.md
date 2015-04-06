---
title: freebsd 常用系统配置
tags:
- FreeBSD
- Linux
---
how to 支持4G以存
options PAE
或者
修改/boot/loader.conf.local
添加： 
kern.maxdsiz="1700000000" #每个进程最大使用的内存数，默认是512M，这里修改成1.7G
kernel="/kernel.PAE" #设置内核启动模式，用PAE模式支持4G以上内存
kern.maxswzone="18352640"
kern.nswapdev=1
查找目录里的大文件
du -s * | sorht -nr | head
连接数排序
netstat -nat|awk '{print awk $NF}'|sort|uniq -c|sort -n
查看当前的连接数可以用：
ps aux | grep httpd | wc -l
pgrep httpd|wc -l
计算httpd占用内存的平均数:
ps aux|grep -v grep|awk '/httpd/{sum+=$6;n++};END{print sum/n}'
------------------------------------------
定时/etc/crontab 
f1 f2 f3 f4 f5 program
其中 f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。 
------------------------------------
用户管理
1 groups     查看使用者是哪个群组
groups jackpig
1增加用户
adduser
2删除用户
rmuser
3更改用户的详细信息
chpass
4显示系统全部用户
pw usershow -a
5增加一个用户组
pw groupadd 组名
6显示系统全部用户组
pw groupshow -a
7查看用户id组id所属组
id 用户名
8　finger 显示用户详细信息