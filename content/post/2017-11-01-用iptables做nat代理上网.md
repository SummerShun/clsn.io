---
title: 用iptables 做NAT代理上网
author: 惨绿少年
type: post
date: 2017-11-01T06:17:00+00:00
url: /clsn/lx886.html
Baidusubmit:
  - 1
views:
  - 154
categories:
  - Linux运维
  - 安全
  - 运维基本功

---
背景：  
有一台A服务器不能上网，和B服务器通过内网来连接，B服务器可以上网，要实现A服务器也可以上网。

<div class="cnblogs_Highlighter">
  <pre class="brush:bash;gutter:true;">内网主机： A eth1:172.16.1.8
外网主机： B eth0:10.0.0.6<br />外网主机： B eth1:172.16.1.6</pre>
</div>

SNAT:改变数据包的源地址。防火墙会使用外部地址，替换数据包的本地网络地址。这样使网络内部主机能够与网络外部通信。

1.在可以上网那台服务器B上，开启内核路由转发功能

<div class="cnblogs_Highlighter">
  <pre class="brush:bash;gutter:true;">#临时
echo 1 > /proc/sys/net/ipv4/ip_forward
sysctl -p
#永久
echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
sysctl -p</pre>
</div>

2.在需要通过代理上网服务器A上，查看路由表。并添加默认网关。route add default gw 172.16.1.6

<div class="cnblogs_Highlighter">
  <pre class="brush:bash;gutter:true;">[root@localhost ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
172.16.1.0      0.0.0.0         255.255.255.0   U     0      0        0 eth1
169.254.0.0     0.0.0.0         255.255.0.0     U     0      0        0 eth1
0.0.0.0         172.16.1.6      0.0.0.0         UG    0      0        0 eth1</pre>
</div>

3.可以上网那台服务器B上添加SNAT规则

<div class="cnblogs_Highlighter">
  <pre class="brush:bash;gutter:true;">iptables -t nat -A POSTROUTING -o eth0 -s 172.16.1.0/24 -j SNAT --to 10.0.0.6</pre>
</div>

4.保存

<div class="cnblogs_Highlighter">
  <pre class="brush:bash;gutter:true;">service iptables save
#重启iptables服务
/etc/init.d/iptables restart</pre>
</div>

5.验证是否可以正常上网。

<div class="cnblogs_Highlighter">
  <pre class="brush:bash;gutter:true;">将iptables设置为开机自启动
[root@lb02 ~]# chkconfig |grep iptables 
iptables       	0:off	1:off	2:on	3:on	4:on	5:on	6:off
</pre>
</div>

　　