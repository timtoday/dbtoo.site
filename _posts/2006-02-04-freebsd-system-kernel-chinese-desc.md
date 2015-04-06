---
title: FreeBSD系统优化部分内核参数调整中文注释
tags:
- FreeBSD
- Linux
- Kernel
---
最大的待发送TCP数据缓冲区空间
net.inet.tcp.sendspace=65536
#最大的接受TCP缓冲区空间
net.inet.tcp.recvspace=65536
#最大的接受UDP缓冲区大小
net.inet.udp.sendspace=65535
#最大的发送UDP数据缓冲区大小
net.inet.udp.maxdgram=65535
#本地套接字连接的数据发送空间
net.local.stream.sendspace=65535
#加快网络性能的协议
net.inet.tcp.rfc1323=1
net.inet.tcp.rfc1644=1
net.inet.tcp.rfc3042=1
net.inet.tcp.rfc3390=1
#最大的套接字缓冲区
kern.ipc.maxsockbuf=2097152
#系统中允许的最多文件数量
kern.maxfiles=65536
#每个进程能够同时打开的最大文件数量
kern.maxfilesperproc=32768
#当一台计算机发起TCP连接请求时,系统会回应ACK应答数据包.
#该选项设置是否延迟ACK应答数据包,把它和包含数据的数据包一起发送,
#在高速网络和低负载的情况下会略微提高性能,但在网络连接较差的时候,
#对方计算机得不到应答会持续发起连接请求,反而会降低性能.
net.inet.tcp.delayed_ack=0
#屏蔽ICMP重定向功能
net.inet.icmp.drop_redirect=1
net.inet.icmp.log_redirect=1
net.inet.ip.redirect=0
net.inet6.ip6.redirect=0
#防止ICMP广播风暴
net.inet.icmp.bmcastecho=0
net.inet.icmp.maskrepl=0
#限制系统发送ICMP速率
net.inet.icmp.icmplim=100
#安全参数,编译内核的时候加了options TCP_DROP_SYNFIN才可以用
net.inet.icmp.icmplim_output=0
net.inet.tcp.drop_synfin=1
#设置为1会帮助系统清除没有正常断开的TCP连接,这增加了一些网络带宽的使用,但是一些死掉的连接最终能被识别并清除.死的TCP连接是被拨号用户存取的系统的一个特别的问题,因为用户经常断开modem而不正确的关闭活动的连接
net.inet.tcp.always_keepalive=1
#若看到net.inet.ip.intr_queue_drops这个在增加,就要调大net.inet.ip.intr_queue_maxlen,为0最好
net.inet.ip.intr_queue_maxlen=1000
#防止DOS攻击,默认为30000
net.inet.tcp.msl=7500
#接收到一个已经关闭的端口发来的所有包,直接drop,如果设置为1则是只针对TCP包
net.inet.tcp.blackhole=2
#接收到一个已经关闭的端口发来的所有UDP包直接drop
net.inet.udp.blackhole=1
#为网络数据连接时提供缓冲
net.inet.tcp.inflight.enable=1
#如果打开的话每个目标地址一次转发成功以后它的数据都将被记录进路由表和arp数据表,节约路由的计算时间,但会需要大量的内核内存空间来保存路由表
net.inet.ip.fastforwarding=0
##kernel编译打开options POLLING功能,高负载情况下使用低负载不推荐
##SMP不能和polling一起用
#kern.polling.enable=1
#并发连接数,默认为128,推荐在1024-4096之间,数字越大占用内存也越大
kern.ipc.somaxconn=32768
#禁止用户查看其他用户的进程
security.bsd.see_other_uids=0
#设置kernel安全级别
kern.securelevel=0
#记录下任何TCP连接
net.inet.tcp.log_in_vain=1
#记录下任何UDP连接
net.inet.udp.log_in_vain=1
#防止不正确的udp包的攻击
net.inet.udp.checksum=1
#防止DOS攻击
net.inet.tcp.syncookies=1
#仅为线程提供物理内存支持,需要256兆以上内存
kern.ipc.shm_use_phys=1
# 线程可使用的最大共享内存
kern.ipc.shmmax=67108864
# 最大线程数量
kern.ipc.shmall=32768
# 程序崩溃时不记录
kern.coredump=0
# lo本地数据流接收和发送空间
net.local.stream.recvspace=65536
net.local.dgram.maxdgram=16384
net.local.dgram.recvspace=65536
# 数据包数据段大小,ADSL为1452.
net.inet.tcp.mssdflt=1460
# 为网络数据连接时提供缓冲
net.inet.tcp.inflight_enable=1
# 数据包数据段最小值,ADSL为1452
net.inet.tcp.minmss=1460
# 本地数据最大数量
net.inet.raw.maxdgram=65536
# 本地数据流接收空间
net.inet.raw.recvspace=65536
#ipfw防火墙动态规则数量,默认为4096,增大该值可以防止某些病毒发送大量TCP连接,导致不能建立正常连接
net.inet.ip.fw.dyn_max=65535
#设置ipf防火墙TCP连接空闲保留时间,默认8640000(120小时)
net.inet.ipf.fr_tcpidletimeout=864000