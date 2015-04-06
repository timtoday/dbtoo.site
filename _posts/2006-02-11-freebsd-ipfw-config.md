---
title: freebsd下ipfw详细配置说明
tags:
- FreeBSD
- Firewall
- Kernel
---
# ee /etc/rc.conf
复制内容到剪贴板代码:
firewall_enable="YES"
firewall_type="open"
firewall_script="/etc/ipfw.rules"
firewall_logging="YES"
# ee /etc/sysctl.conf
复制内容到剪贴板代码:
net.inet.ip.fw.verbose=1
net.inet.ip.fw.verbose_limit=5
编辑防火墙规则
复制内容到剪贴板代码:
# ee /etc/ipfw.rules
复制内容到剪贴板代码:
# 具体语法请参考http://cnsnap.cn.freebsd.org/doc/zh_CN.GB2312/books/handbook/firewalls-ipfw.html
#
##################
#启动时重载规则列表#
##################
ipfw -q -f flush
#
#############
#设置命令前缀#
#############
cmd="ipfw -q add"
#
#############
#设置DNS地址#
#############
dns="192.168.163.2"
#
################
#公网网卡界面名称#
################
pif="lnc0"
#
################
#不限制loopback#
################
$cmd 00100 allow all from any to any via lo0
#
###############
#允许自定义规则#
###############
$cmd 00200 check-state
#
###############
#允许与DNS通讯#
###############
$cmd 00300 allow tcp from any to $dns 53 out via $pif setup keep-state
$cmd 00400 allow udp from any to $dns 53 out via $pif keep-state
#
#####################################################
#允许http连接（limit src-addr意为限制同一地址连接数量）#
#####################################################
$cmd 00500 allow tcp from any to any 80 out via $pif setup keep-state
$cmd 00600 allow tcp from any to me 80 in via $pif setup limit src-addr 10
#
######################################################
#允许https连接（limit src-addr意为限制同一地址连接数量）#
######################################################
$cmd 00700 allow tcp from any to any 443 out via $pif setup keep-state
$cmd 00800 allow tcp from any to me 443 in via $pif setup limit src-addr 10
#
#######################################################
#允许收发电子邮件（limit src-addr意为限制同一地址连接数量）#
#######################################################
$cmd 00900 allow tcp from any to any 25 out via $pif setup keep-state
#$cmd 01000 allow tcp from any to me 25 in via $pif setup limit src-addr 1
#
$cmd 01100 allow tcp from any to any 110 out via $pif setup keep-state
#$cmd 01100 allow tcp from any to me 110 in via $pif setup limit src-addr 1
#
#########################
#允许CVSP和PORT安装/更新#
#########################
$cmd 01200 allow tcp from any to any via $pif setup keep-state uid root
#
##########
#允许ping#
##########
$cmd 01300 allow icmp from any to any out via $pif keep-state
#$cmd 01300 allow icmp from any to any in via $pif keep-state
#
####################################################
#允许FTP连接（limit src-addr意为限制同一地址连接数量）#
####################################################
$cmd 01400 allow tcp from any to any 21 out via $pif setup keep-state
$cmd 01500 allow tcp from any to any 21 in via $pif setup limit src-addr 2
#
########################################################
#允许SSH远程连接（limit src-addr意为限制同一地址连接数量）#
########################################################
$cmd 01600 allow tcp from any to any 22 out via $pif setup keep-state
$cmd 01700 allow tcp from any to any 22 in via $pif setup limit src-addr 2
#
######################
#禁止此规则以外的所有连接#
######################
$cmd 60000 deny log all from any to any