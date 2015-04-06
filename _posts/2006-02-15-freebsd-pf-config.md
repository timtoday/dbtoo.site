---
title: freebsd 下pf详细配置
tags:
- FreeBSD
- Firewall
- Kernel
---
要在 FreeBSD 6.2 上使用 PF 防火墙，有二个方式，一个是编译进入核心，另外是以动态模块方式加载。
编译进入核心的方式
#FreeBSD log traffic，如果有使用 pflog，就要编译进核心
device bpf
#启动 PF Firewall
device pf
#启动虚拟网络设备来记录流量(经由 bpf)
device pflog
#启动虚拟网络设备来监视网络状态
device pfsync
以动态模块加载
vi /etc/rc.conf
加入下面四行
#启用 PF
pf_enable="YES"
#PF 防火墙规则的设定文件
pf_rules="/etc/pf.conf"
#启用 inetd 服务
inetd_enable="YES"
#启动 pflogd
pflog_enable="YES"
#pflogd 储存记录档案的地方
pflog_logfile="/var/log/pflog"
#转送封包
gateway_enable="YES"
#开启 ftp-proxy 功能
vi /etc/inetd.conf
把下面这一行最前面的 # 删除
ftp-proxy stream tcp nowait root /usr/libexec/ftp-proxy ftp-proxy
使用 sysctl 做设定(也可以重新开机让设定生效)
sysctl -w net.inet.ip.forwarding=1
vi /etc/pf.conf
#对外的网络卡
ext_if = "sis0"
#对内的网络卡
int_if = "rl0"
#频宽控管
#定义 std_out 总频宽 512Kb
#altq on $ext_if cbq bandwidth 512Kb queue { std_out }
#定义 std_out 队列频宽 256Kb，使用预设队列
#queue std_out bandwith 256Kb cbq (default)
#定义 std_in 总频宽 2Mb
#altq on $int_if cbq bandwidth 2Mb queue { std_in }
#假设频宽足够的话，可以从父队列借用额外的频宽
#queue std_in bandwidth 768Kb cbq (brrrow)
#对外开放的服务
open_services = "{80, 443}"
#内部私有的 IP
priv_nets = "{ 127.0.0.0/8, 192.168.0.0/16, 172.16.0.0/12, 10.0.0.0/8 }"
# options
#设定拒绝联机封包的处理方式
set block-policy return
#
set optimization aggressive
#纪录 $ext_if
set loginterface $ext_if
# scrub
#整理封包
scrub in all
#nat
#NAT 地址转译处理
nat on $ext_if from $int_if:network to any -> $ext_if
#ftp-proxy
#ftp-proxy 重新导向
rdr on $int_if proto tcp from any to any port 21 -> 127.0.0.1 port 8021
#rdr on $ext_if proto tcp from any to 140.111.152.13 port 21 -> 192.168.13.253 port 21
#Transparent Proxy Server
rdr on rl0 proto tcp from 192.168.13.0/24 to any 80 -> 127.0.0.1 port 3128
#阻挡可疑封包在 $ext_if 网卡进出
antispoof log quick for $ext_if
#阻挡所有进出的封包
block all
#开放 loopback
pass quick on lo0 all
#拒绝内部私有 IP 对 $ext_if 网络卡联机
block drop in quick on $ext_if from $priv_nets to any
block drop out quick on $ext_if from any to $priv_nets
#开放对外的 80, 443 埠
pass in on $ext_if inet proto tcp from any to $ext_if port $open_services flags S/SA keep state
#只容许 140.111.152.0/24 网段对本机做 22 埠联机
pass in on $ext_if inet proto tcp from 140.111.152.0/24 to $ext_if port 22 flags S/SA keep state
#开放内部网络对外联机
#pass in on $inf_if proto rcp from any to any queue std_in
pass in on $int_if from $int_if:network to any keep state
pass out on $int_if from any to $int_if:network keep state
#开放对外网络的联机
#pass out $ext_if proto tcp from any to any queue std_out
pass out on $ext_if proto tcp all modulate state flags S/SA
pass out on $ext_if proto { udp, icmp } all keep state
启动 PF，并读取 pf 规则
pfctl -e;pfctl -f /etc/pf.conf
PF 指令的用法
#启动 PF
pfctl -e
#加载 PF 规则
pfctl -f /etc/pf.conf
#检查 PF 语法是否正确 (未加载)
pfctl -nf /etc/pf.conf
#停用 PF
pfctl -d
#重读 PF 设定档中的 NAT 部分
pfctl -f /etc/pf.conf -N
#重读 PF 设定档中的 filter rules
pfctl -f /etc/pf.conf -R
#重读 PF 设定文件中的选项规则
pfctl -f /etc/pf.conf -O
#查看 PF 信息
#显示现阶段过滤封包的统计资料
pfctl -s info
pfctl -si
pfctl -s memory
#显示现阶段过滤的规则
pfctl -s rules
pfctl -sr
pfctl -vs rules
#显示现阶段过滤封包的统计资料
pfctl -vsr
#显示现阶段 NAT 的规则
pfctl -s nat
pfctl -sn
#检视目前队列
pfctl -s queue
#显示现阶段所有统计的数据
pfctl -s all
pfctl -sa
#清除 PF 规则
#清空 NAT 规则
pfctl -F nat
#清空队列
pfctl -F queue
#清空封包过滤规则
pfctl -F rules
#清空计数器
pfctl -F info
pfctl -F Tables
#清空所有的规则
pfctl -F all
#PF Tables 的使用
#显示 table 内数据
pfctl -t ssh-bruteforce -Tshow
pfctl -t table_name -T add spammers.org
pfctl -t table_name -T delete spammers.org
pfctl -t table_name -T flush
pfctl -t table_name -T show
pfctl -t table_name -T zero
过滤扫描侦测软件
block in quick proto tcp all flags SF/SFRA
block in quick proto tcp all flags SFUP/SFRAU
block in quick proto tcp all flags FPU/SFRAUP
block in quick proto tcp all flags /SFRA
block in quick proto tcp all flags F/SFRA
block in quick proto tcp all flags U/SFRAU
block in quick proto tcp all flags P
如果防火墙和 Proxy Server 不在同一台主机
Proxy Server：192.168.13.250
no rdr on rl0 proto tcp from 192.168.13.250 to any port 80
rdr on rl0 proto tcp from 192.168.13.0/24 to any port 80 -> 192.168.13.250 port 3128