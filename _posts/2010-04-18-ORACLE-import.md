---
title:ORACLE数据导入方法
layout: post
tags:
- Linux
- Oracle
---
<div> 1.<br/>create user 用户名 IDENTIFIED BY 密码 <br/>（如果已经创建过用户，这步可以省略）<br/><br/>2.<br/>GRANT CREATE USER,DROP USER,ALTER USER ,CREATE ANY VIEW ,<br/> DROP ANY VIEW,EXP_FULL_DATABASE,IMP_FULL_DATABASE,<br/> DBA,CONNECT,RESOURCE,CREATE SESSION  TO 用户名字<br/><br/>3.<br/> imp userid=system/manager full=y feedback=1000  file=*.dmp </div>