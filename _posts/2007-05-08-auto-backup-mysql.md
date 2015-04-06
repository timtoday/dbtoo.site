---
title: 自动备份mysql
tags:
- Linux
- Mysql
- Shell
---

{% highlight bash linenos %}

#!/usr/local/bin/bash
#1. mkdir /home/dbackup
#2. chmod 700 backup.sh
#3. vi /etc/crontab , 30 03 * * * root /home/dbackup/mysql_backup_db.sh
#everyday 03:30 the backup.sh will work
#System Setup
#ftp Info
host=192.168.2.248 #ftp host
UserName=wwwftp #ftp user name
Passwd=www!^#!^*ftp #fto user password
#MySQL Info
SQL_host=localhost #MySQL host
SQL_User=root #MySQL UserName
SQL_Passwd=163!^*147!@# #User Password
SQL_db=all #database name
#set saved backup path
backup_path=/home/dbackup
file=$SQL_db-$(date +%Y%m%d).tar.gz
#set the mysql database bin path
MySQL_path=/usr/website/mysql/bin/
cd $backup_path
#export database
$MySQL_path/mysqldump --opt -h $SQL_host -u$SQL_User -p$SQL_Passwd -A > $SQL_db.sql
sleep 5s
#file tar
tar -czf $backup_path/$file $SQL_db.sql
sleep 10s
cd $backup_path
sleep 1s
#file ftp
ftp -i -n <<!
open $host
user $UserName $Passwd
cd backup
put $file
bye
!
sleep 10s
rm -rf $backup_path/$file
rm -rf $backup_path/$SQL_db.sql

{% endhighlight %}
