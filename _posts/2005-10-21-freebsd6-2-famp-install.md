---
title: FreeBSD6.2架设WEB服务器备忘-手动
tags:
- FreeBSD
- FAMP
- LAMP
---
安装mysql5
cd /usr/ports/databases/mysql51-server
#编译安装（FreeBSD默认不带入所有字符集支持，需要声明打开）
make WITH_XCHARSET=all install clean
或自定义参数：
#make WITH_CHARSET=gbk WITH_XCHARSET=all WITH_PROC_SCOPE_PTH=yes BUILD_OPTIMIZED=yes BUILD_STATIC=yes SKIP_DNS_CHECK=yes WITHOUT_INNODB=yes install clean
rehash
#复制配置文件（#服务器内存1G，但是与apache在一起）：
cp /usr/local/share/mysql/my-large.cnf /etc/my.cnf
#设置随机启动：
vi /etc/rc.conf
mysql_enable="YES"
#初始化：
/usr/local/bin/mysql_install_db --user=mysql 
#更改数据库权限（若所有者不是mysql）：
chown -R mysql /var/db/mysql/
#立即启动：
/usr/local/bin/mysqld_safe -user=mysql &
#修改mysql root密码：
/usr/local/bin/mysqladmin -u root password 'newpasswd'
1、创建mysql用户和mysql用户组，并修改目录权限
#pw groupadd mysql -g 88
#pw adduser mysql -u 88 -g 88 -d /nonexistent -s /sbin/nologin
#chown -R mysql:mysql /var/db/mysql
#chown -R mysql:mysql /usr/local/share/mysql
2、创建mysql系统数据库
#/usr/local/bin/mysql_install_db
#/usr/lcoal/bin/mysql_create_system_tables
3、创建mysql自启动脚本
#cp /usr/local/share/mysql/mysql.server /usr/local/etc/rc.d/mysql-server.sh
4、创建mysql配置文件 my.cnf
#cp /usr/local/share/mysql/my-medium.cnf /etc/my.cnf (有my-medium、small、large、huge等可选）
#测试：
mysql -u root -p
mysql> show databases;

#启动/重启/停止mysqld
/usr/local/etc/rc.d/mysql-server start (等于/usr/local/bin/mysqld_safe -user=mysql &)
/usr/local/etc/rc.d/mysql-server restart
/usr/local/etc/rc.d/mysql-server stop
# end

安装 apache 2.0.59
#先确定网路通畅
ping www.apache.org
ping
cd /usr/ports/www/apache20
#确定版本：
less distinfo
#MD5 (apache2/httpd-2.0.59.tar.bz2) = b0200a497d1c89daad680c676d32a6df
#会用到这些库，安装过程会自动下载所需要的库
perl-5.8.8
autoconf-2.59_2
libtool-1.5.22_4
expat-2.0.0_1
libiconv-1.9.2_2
m4-1.4.8_1
help2man-1.36.4_1
gmake-3.81_1
p5-gettext-1.05_1
gettext-0.16.1
apache-2.0.59
#开始安装
make install clean;rehash
# download and waiting...
===>        Vulnerability check disabled, database not found
=> httpd-2.0.59.tar.bz2 doesn't seem to exist in /usr/ports/distfiles/apache2.
=> Attempting to fetch from http://www.apache.org/dist/httpd/.
httpd-2.0.59.tar.bz2                                  0% of 4632 kB 1394        Bps
#若fetch的速度太慢，ctrl+c暂停安装，另用工具下载文件并上传到/usr/ports/distfiles/ 中然后make install clean;rehash可以继续安装
#（按装需要用到的源码包都会存放在这里，可打包备份以便以后用到）
#完毕，留意一下安装信息
#检查是否安装成功
pkg_info
#修改配置
vi /usr/local/etc/apache2/httpd.conf
#立即启动
/usr/local/etc/rc.d/apache2.sh startup
#查看有没监控80端口
netstat -an
#控制：启动/重启/停止
/usr/local/etc/rc.d/apache2.sh startup
/usr/local/etc/rc.d/apache2 restart
/usr/local/etc/rc.d/apache2 stop
#To run apache www server from startup, add apache2_enable="YES"
#以后随机自动启动
vi /etc/rc.conf
#添加
apache2_enable="YES"
#The End
安装php 5.2.1
#热身阅读：
#http://bbs.chinaunix.net/viewthread.php?tid=848816
#http://netflow.kmseh.gov.tw/blog/index.php?op=ViewArticle&articleId=13&blogId=1
#http://www.phoebus.cn/?action=show&id=106
#http://tw.myblog.yahoo.com/bbsfans-blog/article?mid=19&prev=-1&next=12 
#php 5.2.1，http://br.php.net/distributions/php-5.2.1.tar.bz2
#可预先下载放到/usr/ports/distfiles/中
#这里也有：
#http://www.freshports.org/lang/php5/
#进入ports目录：
cd /usr/ports/lang/php5
#检查版本：
cat distinfo
#MD5 (php-5.2.1.tar.bz2) = 261218e3569a777dbd87c16a15f05c8d
#安装
make install clean;rehash
# 出现选择菜单： Options for php5 5.2.1_3 
#选择安装选项：
#选中：CLI,CGI,APACHE,SUHOSIN,MULTIBYTE,IPV6,FASTCGI,PATHINFO
#不要选DEGUG 否则Zend装不上
#SUHOSIN的简单说明：http://linux.chinaunix.net/bbs/thread-886577-1-9.html
#完毕,3种模式的程序:
/usr/local/libexec/apache22/libphp5.so
/usr/local/bin/php
/usr/local/bin/php-cgi
#查看pkg:
pkg_info

#安装扩展：
cd /usr/ports/lang/php5-extensions
#编译安装:
make install clean;rehash
#选择扩展：
#选中(除默认之外)：
[X] CURL             CURL support
[X] GD               GD library support
[X] MBSTRING         multibyte string support
[X] GETTEXT          gettext library support
[X] MYSQL            MySQL database support
[X] MYSQLI           MySQLi database support
[X] PCRE             Perl Compatible Regular Expression support
[X] SOCKETS          sockets support
[X] ZLIB             ZLIB support

#查看已安装包信息:
pkg_info
#配置php：
cd /usr/local/etc
cp php.ini-recommended php.ini

#调整apache(2.2.4)设置:
vi /usr/local/etc/apache22/Includes/php5.conf
#修改内容：
AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .phps
DirectoryIndex index.php
#测试apache配置：
/usr/local/etc/rc.d/apache22 configtest
#OK就重读设置：
/usr/local/etc/rc.d/apache22 graceful
#建立测试页面：
vi /usr/local/www/vhosts/default/htdocs/test.php
#修改内容：
<?php
phpinfo();
#访问：
http://hostname/test.php
#The End

mysql5 字符集问题
这篇是目前我所能找到最详细的了：
http://www.discuz.net/viewthread.php?action=printable&tid=201826
在freebsd中的
http://www.freebsdchina.org/forum/viewtopic.php?p=179463
http://blog.chinaunix.net/u/25477/showart_248080.html
结论：
1.若新数据库服务(msql4.1+)的默认字符集是latin1的，直接复制旧数据库来使用没有问题，但不能用2.6+版的phpmyadmin来管理。
freebsd6.2中，用ports默认安装的mysql5不支持GBx系列的字符集，需要重新编译使mysql5支持所有字符集合：
cd /usr/ports/databases/mysql51-server
#先停止服务
/usr/local/etc/rc.d/mysql-server stop
#卸载已安装的版本（不会伤害已有数据，过程很快，似乎只是删除一些管理脚本）
make deinstall
#加上包含进所有字符集（过程较久，还好是重编译，不用再去fetch源码包）
make WITH_XCHARSET=all install clean;rehash
#重新启动服务
/usr/local/etc/rc.d/mysql-server start

其他：
查看MYSQL支持的字符集：
SHOW CHARACTER SET
#=SELECT * FROM INFORMATION_SCHEMA.CHARACTER_SETS
SHOW VARIABLES LIKE 'character_set_%';
gbk的Collation=gbk_chinese_ci
mysql的数据库路径
mysql默认的数据目录放在：/var/db/mysql，由于/var分区分的过小，空间满了，需要把数据库转到另一个分区上。
# 默认root也无修改权限，先加上：
chmod +w /usr/local/etc/rc.d/mysql-server
# 修改数据库路径：
vi /usr/local/etc/rc.d/mysql-server
# 找到这行来修改：
: ${mysql_dbdir="/var/db/mysql"}
# The End
apache 2.0.x 升级

#停止httpd:
/usr/local/etc/rc.d/apache2 stop
#卸载2.0.x，安装文件（源码包）还会保留在/usr/ports/distfiles中，需要可以重新编译安装，很方便：
cd /var/db/pkg/
pkg_delete apache-2.0.x
rehash
#安装新版：
cd /usr/ports/www/apache22
#重复以前的安装步骤
#The End
合理设置apache httpd的最大连接数
手头有一个网站在线人数增多，访问时很慢。初步认为是服务器资源不足了，但经反复测试，一旦连接上，不断点击同 一个页面上不同的链接，都能迅速打开，这种现象就是说明apache最大连接数已经满了，新的访客只能排队等待有空闲的链接，而如果一旦连接上，在 keeyalive 的存活时间内（KeepAliveTimeout，默认5秒）都不用重新打开连接，因此解决的方法就是加大apache的最大连接数。
1.在哪里设置？
服务器的为FreeBSD 6.2 ，apache 2.24，使用默认配置（FreeBSD 默认不加载自定义MPM配置），默认最大连接数是250
在/usr/local/etc/apache22/httpd.conf中加载MPM配置（去掉前面的注释）：
# Server-pool management (MPM specific)
Include etc/apache22/extra/httpd-mpm.conf
可见的MPM配置在/usr/local/etc/apache22/extra/httpd-mpm.conf，但里面根据httpd的工作模式分了很多块，哪一部才是当前httpd的工作模式呢？可通过执行 apachectl -l 来查看：
Compiled in modules:
                core.c
                prefork.c
                http_core.c
                mod_so.c
看到prefork 字眼，因此可见当前httpd应该是工作在prefork模式，prefork模式的默认配置是：
<IfModule mpm_prefork_module>
                  StartServers                        5
                  MinSpareServers                     5
                  MaxSpareServers                    10
                  MaxClients                        150
                  MaxRequestsPerChild                 0
</IfModule>
2.要加到多少？
连接数理论上当然是支持越大越好，但要在服务器的能力范围内，这跟服务器的CPU、内存、带宽等都有关系。
查看当前的连接数可以用：
ps aux | grep httpd | wc -l
或：
pgrep httpd|wc -l
计算httpd占用内存的平均数:
ps aux|grep -v grep|awk '/httpd/{sum+=$6;n++};END{print sum/n}'
由于基本都是静态页面，CPU消耗很低，每进程占用内存也不算多，大约200K。
服务器内存有2G，除去常规启动的服务大约需要500M（保守估计），还剩1.5G可用，那么理论上可以支持1.5*1024*1024*1024/200000 = 8053.06368
约8K个进程，支持2W人同时访问应该是没有问题的（能保证其中8K的人访问很快，其他的可能需要等待1、2秒才能连上，而一旦连上就会很流畅）
控制最大连接数的MaxClients ，因此可以尝试配置为：
<IfModule mpm_prefork_module>
                  StartServers                        5
                  MinSpareServers                     5
                  MaxSpareServers                    10
                  ServerLimit                      5500
                  MaxClients                       5000
                  MaxRequestsPerChild                 0
</IfModule>
注意，MaxClients默认最大为250，若要超过这个值就要显示设置ServerLimit，且ServerLimit要放在MaxClients之前，值要不小于MaxClients，不然重启httpd时会有提示。
重启httpd后，通过反复执行pgrep httpd|wc -l 来观察连接数，可以看到连接数在达到MaxClients的设值后不再增加，但此时访问网站也很流畅，那就不用贪心再设置更高的值了，不然以后如果网站访 问突增不小心就会耗光服务器内存，可根据以后访问压力趋势及内存的占用变化再逐渐调整，直到找到一个最优的设置值