---
title: freebsd 双线策略路由(pf方法)
tags:
- FreeBSD
- Firewall
- Kernel
---
 
device pf
options ALTQ
options ALTQ_CBQ
options ALTQ_RED
options ALTQ_RIO
options ALTQ_HFSC
options ALTQ_CDNR
options ALTQ_PRIQ
Note: If you are installing altq on a multiprocessor system, add options ALTQ_NOPPC to your configuration before you recompile your kernel.
 


# vi /etc/pf.conf
=================================================
 
if_isp1="fxp0"
if_isp2="ne3"
gw_isp1="192.168.0.1"
gw_isp2="192.168.1.10"
block all
pass quick on lo0 all
pass in quick on $if_isp1 reply-to ( $if_isp1 $gw_isp1 ) proto {tcp,udp,icmp} to any keep state
pass in quick on $if_isp2 reply-to ( $if_isp2 $gw_isp2 ) proto {tcp,udp,icmp} to any keep state
pass out keep state
 
=================================================
为了试验方便，以上PF规则没有对TCP/UDP等协议的端口进行限制。大家根据自己的实际情况修改一下即可。为了方便控制PF的启动和关闭，下面列出我使用的一个SHELL脚步：
# vi /etc/rc.d/pf.sh
=================================================
 
#!/bin/sh
# made by llzqq
# pf startup scripts
#
case "$1" in
start)
        if [ -f /etc/pf.conf ]; then
                /sbin/pfctl -e -f /etc/pf.conf
        fi
        ;;
stop)
        /sbin/pfctl -F all
        /sbin/pfctl -d
        ;;
*)
        echo "$0 start | stop"
        ;;
esac
exit 0
 