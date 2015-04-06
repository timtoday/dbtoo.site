---
title: freebsd 双线策略路由(ipfilter方法)
tags:
- FreeBSD
- Firewall
- Kernel
---

<div> echo ipfilter_enable="YES" &gt;&gt;/etc/rc.conf<br>cat &gt;&gt;/etc/ipf.rules &lt;&lt;EOF<br>pass out quick on fxp0 to fxp1:192.168.200.1 from 192.168.200.200 to any<br>pass out quick on fxp0 to fxp2:192.168.100.1 from 192.168.100.200 to any<br>EOF </div>
 
