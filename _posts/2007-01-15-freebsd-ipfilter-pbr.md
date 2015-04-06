---
title: freebsd 双线策略路由(ipfilter方法)
tags:
- FreeBSD
- Firewall
- Kernel
---

{% highlight bash linenos %}

echo ipfilter_enable="YES" >>/etc/rc.conf
cat >>/etc/ipf.rules <<EOF
pass out quick on fxp0 to fxp1:192.168.200.1 from 192.168.200.200 to any
pass out quick on fxp0 to fxp2:192.168.100.1 from 192.168.100.200 to any
EOF

{% endhighlight %}


