---
title:ORACLE 10G-XE-CLIENT for debian的bug
layout: post
tags:
- Linux
- Oracle
---
<div> .安装如果出现下列错误信息：<br/>/usr/lib/oracle/xe/app/oracle/product/10.2.0/server/bin/nls_lang.sh: 112: [[: not found<br/>/usr/lib/oracle/xe/app/oracle/product/10.2.0/server/bin/nls_lang.sh: 112: [[: not found<br/>这时候修改/usr/lib/oracle/xe/app/oracle/product/10.2.0/client/bin/nls_lang.sh，将<br/>If[ [ -n "$LC_ALL" ]]; then<br/>   locale=$LC_ALL<br/>elif[ [ -n "$LANG" ]]; then<br/>   locale=$LANG<br/>else<br/>   locale=<br/>fi去掉多余的一个“[”和“]” </div>