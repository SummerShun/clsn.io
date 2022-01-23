---
title: keepalived实现服务高可用
author: 惨绿少年
type: post
date: 2017-12-17T01:21:00+00:00
url: /clsn/lx427.html
Baidusubmit:
  - 1
views:
  - 442
specs_zan:
  - 5
zm_favorites:
  - 1
categories:
  - Linux运维
  - Web应用
  - 安全
  - 自动化
  - 运维基本功

---
# <span id="1_keepalived">第1章 keepalived服务说明</span>

## <span id="11_keepalived">1.1 keepalived是什么？</span>

　　<span style="text-decoration: underline;">Keepalived</span><span style="text-decoration: underline;">软件起初是专为LVS</span><span style="text-decoration: underline;">负载均衡软件设计的，用来管理并监控LVS</span><span style="text-decoration: underline;">集群系统中各个服务节点的状态，后来又加入了可以实现高可用的VRRP</span><span style="text-decoration: underline;">功能。</span>因此，Keepalived除了能够管理LVS软件外，还可以作为其他服务（例如：Nginx、Haproxy、MySQL等）的高可用解决方案软件。

　　Keepalived软件主要是通过VRRP协议实现高可用功能的。VRRP是Virtual Router RedundancyProtocol(虚拟路由器冗余协议）的缩写，VRRP出现的目的就是为了解决静态路由单点故障问题的，它能够保证当个别节点宕机时，整个网络可以不间断地运行。

　　<span style="background-color: #ffff00;">所以，Keepalived 一方面具有配置管理LVS的功能，同时还具有对LVS下面节点进行健康检查的功能，另一方面也可实现系统网络服务的高可用功能。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; keepalived官网<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.keepalived.org"  target="_blank">http://www.keepalived.org</a>

## <span id="12_keepalived">1.2 keepalived服务的三个重要功能</span>

　　管理LVS负载均衡软件

　　实现LVS集群节点的健康检查中

　　作为系统网络服务的高可用性（failover）

## <span id="13_Keepalived">1.3 Keepalived高可用故障切换转移原理</span>

　　Keepalived高可用服务对之间的故障切换转移，是通过 VRRP (Virtual Router Redundancy Protocol ,虚拟路由器冗余协议）来实现的。

　　在 Keepalived服务正常工作时，主 Master节点会不断地向备节点发送（多播的方式）心跳消息，用以告诉备Backup节点自己还活看，当主 Master节点发生故障时，就无法发送心跳消息，备节点也就因此无法继续检测到来自主 Master节点的心跳了，于是调用自身的接管程序，接管主Master节点的 IP资源及服务。而当主 Master节点恢复时，备Backup节点又会释放主节点故障时自身接管的IP资源及服务，恢复到原来的备用角色。

　　那么，什么是VRRP呢？

　　VRRP ,全 称 Virtual Router Redundancy Protocol ,中文名为虚拟路由冗余协议 ，VRRP的出现就是为了解决静态踣甶的单点故障问题，VRRP是通过一种竞选机制来将路由的任务交给某台VRRP路由器的。

## <span id="14_keepalived">1.4 keepalived 原理</span>

### <span id="141keepalived">1.4.1keepalived高可用架构示意图</span>

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171217170645561-1428317110.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="keepalived实现服务高可用" alt="" />
</p>

### <span id="142">1.4.2 文字，表述</span>

Keepalived的工作原理：

　　Keepalived高可用对之间是通过VRRP通信的，因此，我们从 VRRP开始了解起：

　　　　1) VRRP,全称 Virtual Router Redundancy Protocol,中文名为虚拟路由冗余协议，VRRP的出现是为了解决静态路由的单点故障。

　　　　2) VRRP是通过一种竟选协议机制来将路由任务交给某台 VRRP路由器的。

　　　　3) VRRP用 IP多播的方式（默认多播地址（224.0_0.18))实现高可用对之间通信。

　　　　4) 工作时主节点发包，备节点接包，当备节点接收不到主节点发的数据包的时候，就启动接管程序接管主节点的开源。备节点可以有多个，通过优先级竞选，但一般 Keepalived系统运维工作中都是一对。

　　　　5) VRRP使用了加密协议加密数据，但Keepalived官方目前还是推荐用明文的方式配置认证类型和密码。

　　介绍完 VRRP,接下来我再介绍一下 Keepalived服务的工作原理：

　　Keepalived高可用对之间是通过 VRRP进行通信的， VRRP是遑过竞选机制来确定主备的，主的优先级高于备，因此，工作时主会优先获得所有的资源，备节点处于等待状态，当主挂了的时候，备节点就会接管主节点的资源，然后顶替主节点对外提供服务。

　　在 Keepalived服务对之间，只有作为主的服务器会一直发送 VRRP广播包,告诉备它还活着，此时备不会枪占主，当主不可用时，即备监听不到主发送的广播包时，就会启动相关服务接管资源，保证业务的连续性.接管速度最快可以小于1秒。

# <span id="2_keepalived">第2章 keepalived软件使用</span>

## <span id="21">2.1 软件的部署</span>

### <span id="211_keepalived">2.1.1 第一个里程碑 keepalived软件安装</span>

<div>
  <p class="a1">
    &nbsp;<span class="cnblogs_code"><span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> keepalived -y</span>&nbsp;
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>/etc/<span style="color: #000000;">keepalived
</span>/etc/keepalived/<span style="color: #000000;">keepalived.conf     #keepalived服务主配置文件
</span>/etc/rc.d/init.d/<span style="color: #000000;">keepalived         #服务启动脚本
</span>/etc/sysconfig/<span style="color: #000000;">keepalived
</span>/usr/bin/<span style="color: #000000;">genhash
</span>/usr/libexec/<span style="color: #000000;">keepalived
</span>/usr/sbin/keepalived</pre>
  </div>
</div>

第二个里程碑： 进行默认配置测试

### <span id="212">2.1.2 配置文件说明</span>

1-13行表示全局配置

<div>
  <div class="cnblogs_code">
    <pre> global_defs {    <span style="color: #008000;">#</span><span style="color: #008000;">全局配置</span>
<span style="color: #000000;">    notification_email {   定义报警邮件地址
      acassen@firewall.loc
      failover@firewall.loc
      sysadmin@firewall.loc
    } 
    notification_email_from Alexandre.Cassen@firewall.loc  </span><span style="color: #008000;">#</span><span style="color: #008000;">定义发送邮件的地址</span>
    smtp_server 192.168.200.1   <span style="color: #008000;">#</span><span style="color: #008000;">邮箱服务器 </span>
    smtp_connect_timeout 30      <span style="color: #008000;">#</span><span style="color: #008000;">定义超时时间</span>
    router_id LVS_DEVEL        <span style="color: #008000;">#</span><span style="color: #008000;">定义路由标识信息，相同局域网唯一</span>
 }  </pre>
  </div>
</div>

15-30行 虚拟ip配置 brrp

<div>
  <div class="cnblogs_code">
    <pre>vrrp_instance VI_1 {   <span style="color: #008000;">#</span><span style="color: #008000;">定义实例</span>
    state MASTER         <span style="color: #008000;">#</span><span style="color: #008000;">状态参数 master/backup 只是说明</span>
    interface eth0       <span style="color: #008000;">#</span><span style="color: #008000;">虚IP地址放置的网卡位置</span>
    virtual_router_id 51 <span style="color: #008000;">#</span><span style="color: #008000;">同一家族要一直，同一个集群id一致</span>
    priority 100         <span style="color: #008000;">#</span><span style="color: #008000;"> 优先级决定是主还是备    越大越优先</span>
    advert_int 1        <span style="color: #008000;">#</span><span style="color: #008000;">主备通讯时间间隔</span>
    authentication {     <span style="color: #008000;">#</span><span style="color: #008000;"> &darr;</span>
        auth_type PASS    <span style="color: #008000;">#</span><span style="color: #008000;">&darr;</span>
        auth_pass 1111    <span style="color: #008000;">#</span><span style="color: #008000;">认证</span>
    }                        <span style="color: #008000;">#</span><span style="color: #008000;">&uarr; </span>
    virtual_ipaddress {  <span style="color: #008000;">#</span><span style="color: #008000;">&darr;</span>
        192.168.200.16<span style="color: #000000;">    设备之间使用的虚拟ip地址
        </span>192.168.200.17
        192.168.200.18<span style="color: #000000;">
    }
}</span></pre>
  </div>
</div>

配置管理LVS

<div>
  <p class="a1">
    　　关于 LVS 详情参考<a href="/wp-content/themes/clsn-003/inc/go.php?url=%20http://www.cnblogs.com/clsn/p/7920637.html#_label7"  target="_blank"> http://www.cnblogs.com/clsn/p/7920637.html#_label7</a>
  </p>
</div>

### <span id="213">2.1.3 最终配置文件</span>

**主负载均衡服务器配置**

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 conf]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  /etc/keepalived/keepalived.conf </span>
! Configuration File <span style="color: #0000ff;">for</span><span style="color: #000000;"> keepalived

global_defs {
   router_id lb01
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id </span>51<span style="color: #000000;">
    priority </span>150<span style="color: #000000;">
    advert_int </span>1<span style="color: #000000;">
    authentication {
        auth_type PASS
        auth_pass </span>1111<span style="color: #000000;">
    }
    virtual_ipaddress {
        </span>10.0.0.3<span style="color: #000000;">
    }
}</span></pre>
  </div>
</div>

**备负载均衡服务器配置**

<div>
  <div class="cnblogs_code">
    <pre>[root@lb02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/keepalived/keepalived.conf </span>
! Configuration File <span style="color: #0000ff;">for</span><span style="color: #000000;"> keepalived

global_defs {
   router_id lb02
}

vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    virtual_router_id </span>51<span style="color: #000000;">
    priority </span>100<span style="color: #000000;">
    advert_int </span>1<span style="color: #000000;">
    authentication {
        auth_type PASS
        auth_pass </span>1111<span style="color: #000000;">
    }
    virtual_ipaddress {
     </span>10.0.0.3<span style="color: #000000;">
    }
}</span></pre>
  </div>
</div>

### <span id="214_keepalived">2.1.4 启动keepalived</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@lb02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> /etc/init.d/keepalived start</span>
Starting keepalived:                                       [  OK  ]</pre>
  </div>
</div>

### <span id="215">2.1.5 【说明】在进行访问测试之前要保证后端的节点都能够单独的访问。</span>

测试连通性.&nbsp; &nbsp;&nbsp;后端节点

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 conf]<span style="color: #008000;">#</span><span style="color: #008000;"> curl -H host:www.etiantian.org  10.0.0.8</span>
<span style="color: #000000;">web01 www
[root@lb01 conf]</span><span style="color: #008000;">#</span><span style="color: #008000;"> curl -H host:www.etiantian.org  10.0.0.7</span>
<span style="color: #000000;">web02 www
[root@lb01 conf]</span><span style="color: #008000;">#</span><span style="color: #008000;"> curl -H host:www.etiantian.org  10.0.0.9</span>
<span style="color: #000000;">web03 www
[root@lb01 conf]</span><span style="color: #008000;">#</span><span style="color: #008000;"> curl -H host:bbs.etiantian.org  10.0.0.9</span>
<span style="color: #000000;">web03 bbs
[root@lb01 conf]</span><span style="color: #008000;">#</span><span style="color: #008000;"> curl -H host:bbs.etiantian.org  10.0.0.8</span>
<span style="color: #000000;">web01 bbs
[root@lb01 conf]</span><span style="color: #008000;">#</span><span style="color: #008000;"> curl -H host:bbs.etiantian.org  10.0.0.7</span>
web02 bbs</pre>
  </div>
</div>

### <span id="216_ip">2.1.6 查看虚拟ip状态</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 conf]<span style="color: #008000;">#</span><span style="color: #008000;"> ip a</span>
1: lo: &lt;LOOPBACK,UP,LOWER_UP> mtu 65536<span style="color: #000000;"> qdisc noqueue state UNKNOWN 
    link</span>/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00<span style="color: #000000;">
    inet </span>127.0.0.1/8<span style="color: #000000;"> scope host lo
    inet6 ::</span>1/128<span style="color: #000000;"> scope host 
       valid_lft forever preferred_lft forever
</span>2: eth0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000<span style="color: #000000;">
    link</span>/ether 00:0c:29:90<span style="color: #000000;">:7f:0d brd ff:ff:ff:ff:ff:ff
    inet </span>10.0.0.5/24 brd 10.0.0.255 scope <span style="color: #0000ff;">global</span><span style="color: #000000;"> eth0
   <span style="background-color: #ffff00;"> inet </span></span><span style="background-color: #ffff00;">10.0.0.3/24 scope <span style="color: #0000ff;">global</span> secondary eth0:1</span><span style="color: #000000;">
    inet6 fe80::20c:29ff:fe90:7f0d</span>/64<span style="color: #000000;"> scope link 
       valid_lft forever preferred_lft forever
</span>3: eth1: &lt;BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000<span style="color: #000000;">
    link</span>/ether 00:0c:29:90:7f:17<span style="color: #000000;"> brd ff:ff:ff:ff:ff:ff
    inet </span>172.16.1.5/24 brd 172.16.1.255 scope <span style="color: #0000ff;">global</span><span style="color: #000000;"> eth1
    inet6 fe80::20c:29ff:fe90:7f17</span>/64<span style="color: #000000;"> scope link 
       valid_lft forever preferred_lft forever</span></pre>
  </div>
</div>

### <span id="217">2.1.7 【总结】配置文件修改</span>

　　Keepalived主备配置文件区别：

　　　　01. router_id 信息不一致

　　　　02. state 状态描述信息不一致

　　　　03. priority 主备竞选优先级数值不一致

## <span id="22">2.2 脑裂</span>

&nbsp;&nbsp;&nbsp; 　　在高可用（HA）系统中，当联系2个节点的&ldquo;心跳线&rdquo;断开时，本来为一整体、动作协调的HA系统，就分裂成为2个独立的个体。由于相互失去了联系，都以为是对方出了故障。两个节点上的HA软件像&ldquo;裂脑人&rdquo;一样，争抢&ldquo;共享资源&rdquo;、争起&ldquo;应用服务&rdquo;，就会发生严重后果&mdash;&mdash;或者共享资源被瓜分、2边&ldquo;服务&rdquo;都起不来了；或者2边&ldquo;服务&rdquo;都起来了，但同时读写&ldquo;共享存储&rdquo;，导致数据损坏（常见如数据库轮询着的联机日志出错）。

&nbsp;　　对付HA系统&ldquo;裂脑&rdquo;的对策，目前达成共识的的大概有以下几条：

　　　　1）添加冗余的心跳线，例如：双线条线（心跳线也HA），尽量减少&ldquo;裂脑&rdquo;发生几率；

　　　　2）启用磁盘锁。正在服务一方锁住共享磁盘，&ldquo;裂脑&rdquo;发生时，让对方完全&ldquo;抢不走&rdquo;共享磁盘资源。但使用锁磁盘也会有一个不小的问题，如果占用共享盘的一方不主动&ldquo;解锁&rdquo;，另一方就永远得不到共享磁盘。现实中假如服务节点突然死机或崩溃，就不可能执行解锁命令。后备节点也就接管不了共享资源和应用服务。于是有人在HA中设计了&ldquo;智能&rdquo;锁。即：正在服务的一方只在发现心跳线全部断开（察觉不到对端）时才启用磁盘锁。平时就不上锁了。

　　　　3）设置仲裁机制。例如设置参考IP（如网关IP），当心跳线完全断开时，2个节点都各自ping一下参考IP，不通则表明断点就出在本端。不仅&ldquo;心跳&rdquo;、还兼对外&ldquo;服务&rdquo;的本端网络链路断了，即使启动（或继续）应用服务也没有用了，那就主动放弃竞争，让能够ping通参考IP的一端去起服务。更保险一些，ping不通参考IP的一方干脆就自我重启，以彻底释放有可能还占用着的那些共享资源。

### <span id="221">2.2.1 脑裂产生的原因</span>

　　一般来说，裂脑的发生，有以下几种原因：

　　　　? 高可用服务器对之间心跳线链路发生故障，导致无法正常通信。

　　　　　　　　因心跳线坏了（包括断了，老化）。

　　　　　　　　因网卡及相关驱动坏了，ip配置及冲突问题（网卡直连）。

　　　　　　　　因心跳线间连接的设备故障（网卡及交换机）。

　　　　　　　　因仲裁的机器出问题（采用仲裁的方案）。

　　　　?&nbsp;&nbsp;高可用服务器上开启了 iptables防火墙阻挡了心跳消息传输。

　　　　?&nbsp;高可用服务器上心跳网卡地址等信息配置不正确，导致发送心跳失败。

　　　　?&nbsp;其他服务配置不当等原因，如心跳方式不同，心跳广插冲突、软件Bug等。

<div>
  <p class="a">
    　　　　<span style="background-color: #ffff00;"><strong>提示：</strong></span> Keepalived配置里同一 VRRP实例如果 virtual_router_id两端参数配置不一致也会导致裂脑问题发生。
  </p>
</div>

&nbsp;

### <span id="222">2.2.2 常见的解决方案</span>

　　在实际生产环境中，我们可以从以下几个方面来防止裂脑问题的发生：

　　? 同时使用串行电缆和以太网电缆连接，同时用两条心跳线路，这样一条线路坏了，另一个还是好的，依然能传送心跳消息。

　　?&nbsp;当检测到裂脑时强行关闭一个心跳节点（这个功能需特殊设备支持，如Stonith、feyce）。相当于备节点接收不到心跳消患，通过单独的线路发送关机命令关闭主节点的电源。

　　?&nbsp; 做好对裂脑的监控报警（如邮件及手机短信等或值班）.在问题发生时人为第一时间介入仲裁，降低损失。例如，百度的监控报警短倍就有上行和下行的区别。报警消息发送到管理员手机上，管理员可以通过手机回复对应数字或简单的字符串操作返回给服务器.让服务器根据指令自动处理相应故障，这样解决故障的时间更短.

　　当然，在实施高可用方案时，要根据业务实际需求确定是否能容忍这样的损失。对于一般的网站常规业务.这个损失是可容忍的。

## <span id="23">2.3 如何进行脑裂情况监控</span>

### <span id="231">2.3.1 在什么服务器上进行监控？</span>

　　在备服务器上进行监控，可以使用zabbix监控，参考<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/7885990.html"  target="_blank">http://www.cnblogs.com/clsn/p/7885990.html</a>

### <span id="232">2.3.2 监控什么信息？</span>

_　　备上面出现vip__情况：_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 　　1）脑裂情况出现

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 　　2）正常主备切换也会出现

### <span id="233">2.3.3 编写监控脑裂脚本</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@lb02 scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> vim check_keepalived.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash</span>

<span style="color: #0000ff;">while</span><span style="color: #000000;"> true
do
</span><span style="color: #0000ff;">if</span> [ `ip a show eth0 |grep 10.0.0.3|wc -l` -<span style="color: #000000;">ne 0 ]
then
    echo </span><span style="color: #800000;">"</span><span style="color: #800000;">keepalived is error!</span><span style="color: #800000;">"</span>
<span style="color: #0000ff;">else</span><span style="color: #000000;">
    echo </span><span style="color: #800000;">"</span><span style="color: #800000;">keepalived is OK !</span><span style="color: #800000;">"</span><span style="color: #000000;">
fi
done</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background-color: #ffff00;">编写完脚本后要给脚本赋予执行权限</span>

### <span id="234">2.3.4 测试 确保两台负载均衡能够正常负载</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> curl -H Host:www.etiantian.org 10.0.0.5</span>
<span style="color: #000000;">web01 www
[root@lb01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> curl -H Host:www.etiantian.org 10.0.0.6</span>
<span style="color: #000000;">web01 www
[root@lb01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> curl -H Host:bbs.etiantian.org 10.0.0.6</span>
<span style="color: #000000;">web02 bbs
 [root@lb01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> curl -H Host:www.etiantian.org 10.0.0.5</span>
web03 www&nbsp;</pre>
  </div>
</div>

## <span id="24">2.4 排错过程</span>

　　1）利用负载均衡服务器，在服务器上curl所有的节点信息（web服务器配置有问题）

　　2）curl 负载均衡服务器地址，可以实现负载均衡

　　3）windows上绑定虚拟IP，浏览器上进行测试

**　　　　<span style="background-color: #ffff00;">keepalived</span>**<span style="background-color: #ffff00;"><strong>日志文件位置 /var/log/messages</strong></span>

## <span id="25_nginx_vip">2.5 更改nginx反向代理配置 只监听vip地址</span>

修改nginx监听参数&nbsp;&nbsp;<span class="cnblogs_code">listen 10.0.0.3:80;</span>&nbsp;

修改内核参数，实现监听本地不存在的ip

<div>
  <div class="cnblogs_code">
    <pre>echo <span style="color: #800000;">'</span><span style="color: #800000;">net.ipv4.ip_nonlocal_bind = 1</span><span style="color: #800000;">'</span> >>/etc/<span style="color: #000000;">sysctl.conf
sysctl </span>-<span style="color: #000000;">p

[root@lb02 conf]</span><span style="color: #008000;">#</span><span style="color: #008000;"> cat /proc/sys/net/ipv4/ip_nonlocal_bind</span>
&nbsp;</pre>
  </div>
</div>

## <span id="26_keepalivednginx">2.6 让keepalived监控nginx</span>

<div>
  <div class="cnblogs_code">
    <pre>ps -ef |grep nginx |grep -v grep |wc -l</pre>
  </div>
</div>

　　编写执行脚本

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008000;">#</span><span style="color: #008000;">!/bin/bash</span>

<span style="color: #0000ff;">while</span><span style="color: #000000;"> true
do
</span><span style="color: #0000ff;">if</span> [ `ps -ef |grep nginx |grep -v grep |wc -l` -lt 2<span style="color: #000000;"> ]
then
   </span>/etc/init.d/<span style="color: #000000;">keepalived stop
   exit
fi
done</span></pre>
  </div>
</div>

<div>
  <p class="a">
    <strong>注意脚本的授权</strong>
  </p>
  
  <div class="cnblogs_code">
    <pre>[root@lb01 scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> chmod +x check_www.sh</span></pre>
  </div>
</div>

### <span id="261_keepalived">2.6.1 使用keepalived的监控脚本</span>

　　<span style="background-color: #00ff00;">说明 执行的脚本名称尽量不要和服务名称相同或相似</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/keepalived/keepalived.conf</span>
! Configuration File <span style="color: #0000ff;">for</span><span style="color: #000000;"> keepalived

global_defs {
   router_id lb01
}

vrrp_script check {     </span><span style="color: #008000;">#</span><span style="color: #008000;">定义脚本</span>
   script <span style="color: #800000;">"</span><span style="color: #800000;">&ldquo;/server/scripts/check_web.sh</span><span style="color: #800000;">"</span>  ---<span style="color: #000000;"> 表示将一个脚本信息赋值给变量check_web
   interval </span>2    ---<span style="color: #000000;"> 执行监控脚本的间隔时间
   weight </span>2  ---<span style="color: #000000;">利用权重值和优先级进行运算，从而降低主服务优先级使之变为备服务器（建议先忽略）
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id </span>51<span style="color: #000000;">
    priority </span>150<span style="color: #000000;">
    advert_int </span>1<span style="color: #000000;">
    authentication {
        auth_type PASS
        auth_pass </span>1111<span style="color: #000000;">
    }
    virtual_ipaddress {
        </span>10.0.0.3/24 dev eth0 label eth0:1<span style="color: #000000;">
    }
    track_script {     </span><span style="color: #008000;">#</span><span style="color: #008000;">调用脚本</span>
<span style="color: #000000;">       check
    }
}</span></pre>
  </div>
</div>

## <span id="27">2.7 多实例的配置</span>

### <span id="271_lb01keepalived">2.7.1 lb01的keepalived配置文件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  /etc/keepalived/keepalived.conf </span>
! Configuration File <span style="color: #0000ff;">for</span><span style="color: #000000;"> keepalived

global_defs {
   router_id lb01
}

vrrp_script check {
   script </span><span style="color: #800000;">"</span><span style="color: #800000;">/server/scripts/check_www.sh</span><span style="color: #800000;">"</span><span style="color: #000000;">
   interval </span>2<span style="color: #000000;"> 
   weight </span>2<span style="color: #000000;">
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id </span>51<span style="color: #000000;">
    priority </span>150<span style="color: #000000;">
    advert_int </span>1<span style="color: #000000;">
    authentication {
        auth_type PASS
        auth_pass </span>1111<span style="color: #000000;">
    }
    virtual_ipaddress {
        </span>10.0.0.3/24 dev eth0 label eth0:1<span style="color: #000000;">
    }
    track_script {
       check
    }
}
vrrp_instance VI_2 {
    state BACKUP
    interface eth0
    virtual_router_id </span>52<span style="color: #000000;">
    priority </span>100<span style="color: #000000;">
    advert_int </span>1<span style="color: #000000;">
    authentication {
        auth_type PASS
        auth_pass </span>1111<span style="color: #000000;">
    }
    virtual_ipaddress {
        </span>10.0.0.4/24 dev eth0 label eth0:2<span style="color: #000000;">
    }
}</span></pre>
  </div>
</div>

### <span id="272_lb02keepalived">2.7.2 修改lb02的keepalived配置文件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@lb02 conf]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/keepalived/keepalived.conf </span>
! Configuration File <span style="color: #0000ff;">for</span><span style="color: #000000;"> keepalived

global_defs {
   router_id lb02
}

vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    virtual_router_id </span>51<span style="color: #000000;">
    priority </span>100<span style="color: #000000;">
    advert_int </span>1<span style="color: #000000;">
    authentication {
        auth_type PASS
        auth_pass </span>1111<span style="color: #000000;">
    }
    virtual_ipaddress {
     </span>10.0.0.3 dev eth0 label eth0:1<span style="color: #000000;">
    }
}
vrrp_instance VI_2 {
    state MASTER
    interface eth0
    virtual_router_id </span>52<span style="color: #000000;">
    priority </span>150<span style="color: #000000;">
    advert_int </span>1<span style="color: #000000;">
    authentication {
        auth_type PASS
        auth_pass </span>1111<span style="color: #000000;">
    }
    virtual_ipaddress {
     </span>10.0.0.4 dev eth0 label eth0:2<span style="color: #000000;">
    }
}</span></pre>
  </div>
</div>

修改nginx配置文件，让bbs 与www分别监听不同的ip地址

<div>
  <div class="cnblogs_code">
    <pre>worker_processes  1<span style="color: #000000;">;
events {
    worker_connections  </span>1024<span style="color: #000000;">;
}
http {
    include       mime.types;
    default_type  application</span>/octet-<span style="color: #000000;">stream;
    sendfile        on;
    keepalive_timeout  </span>65<span style="color: #000000;">;                           
    upstream server_pools {
      server </span>10.0.0.7:80<span style="color: #000000;">;
      server </span>10.0.0.8:80<span style="color: #000000;">;
      server </span>10.0.0.9:80<span style="color: #000000;">;
    }
    server {
        listen </span>10.0.0.3:80<span style="color: #000000;">;
        server_name www.etiantian.org;
        location </span>/<span style="color: #000000;"> {
            proxy_pass http:</span>//<span style="color: #000000;">server_pools;
            proxy_set_header Host $host;
            proxy_set_header X</span>-Forwarded-<span style="color: #000000;">For $remote_addr;
        }
    } 
    server {
        listen </span>10.0.0.4:80<span style="color: #000000;">;
        server_name bbs.etiantian.org;
        location </span>/<span style="color: #000000;"> {
            proxy_pass http:</span>//<span style="color: #000000;">server_pools;
            proxy_set_header Host $host;
            proxy_set_header X</span>-Forwarded-<span style="color: #000000;">For $remote_addr;
        }
    } 
}</span></pre>
  </div>
</div>

lb01

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> netstat -lntup |grep nginx</span>
tcp        0      0 10.0.0.3:80                 0.0.0.0:*                   LISTEN      84907/<span style="color: #000000;">nginx         
tcp        0      0 </span>10.0.0.4:80                 0.0.0.0:*                   LISTEN      84907/nginx         </pre>
  </div>
</div>

lb02

<div>
  <div class="cnblogs_code">
    <pre>[root@lb02 conf]<span style="color: #008000;">#</span><span style="color: #008000;"> netstat -lntup |grep nginx</span>
tcp        0      0 10.0.0.3:80                 0.0.0.0:*                   LISTEN      12258/<span style="color: #000000;">nginx         
tcp        0      0 </span>10.0.0.4:80                 0.0.0.0:*                   LISTEN      12258/nginx  </pre>
  </div>
</div>

<h2 style="text-align: left;">
  <span id="nbsp28_keepalivednbsp">&nbsp;2.8 keepalived双主模式示意图&nbsp;</span>
</h2>

&nbsp;

<p style="text-align: right;">
  &nbsp;<strong>本文出自&ldquo;惨绿少年&rdquo;，欢迎转载，转载请注明出处！http://blog.nmtui.com</strong>
</p>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1_keepalived">第1章 keepalived服务说明</a><ul>
        <li>
          <a href="#11_keepalived">1.1 keepalived是什么？</a>
        </li>
        <li>
          <a href="#12_keepalived">1.2 keepalived服务的三个重要功能</a>
        </li>
        <li>
          <a href="#13_Keepalived">1.3 Keepalived高可用故障切换转移原理</a>
        </li>
        <li>
          <a href="#14_keepalived">1.4 keepalived 原理</a><ul>
            <li>
              <a href="#141keepalived">1.4.1keepalived高可用架构示意图</a>
            </li>
            <li>
              <a href="#142">1.4.2 文字，表述</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2_keepalived">第2章 keepalived软件使用</a><ul>
        <li>
          <a href="#21">2.1 软件的部署</a><ul>
            <li>
              <a href="#211_keepalived">2.1.1 第一个里程碑 keepalived软件安装</a>
            </li>
            <li>
              <a href="#212">2.1.2 配置文件说明</a>
            </li>
            <li>
              <a href="#213">2.1.3 最终配置文件</a>
            </li>
            <li>
              <a href="#214_keepalived">2.1.4 启动keepalived</a>
            </li>
            <li>
              <a href="#215">2.1.5 【说明】在进行访问测试之前要保证后端的节点都能够单独的访问。</a>
            </li>
            <li>
              <a href="#216_ip">2.1.6 查看虚拟ip状态</a>
            </li>
            <li>
              <a href="#217">2.1.7 【总结】配置文件修改</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#22">2.2 脑裂</a><ul>
            <li>
              <a href="#221">2.2.1 脑裂产生的原因</a>
            </li>
            <li>
              <a href="#222">2.2.2 常见的解决方案</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#23">2.3 如何进行脑裂情况监控</a><ul>
            <li>
              <a href="#231">2.3.1 在什么服务器上进行监控？</a>
            </li>
            <li>
              <a href="#232">2.3.2 监控什么信息？</a>
            </li>
            <li>
              <a href="#233">2.3.3 编写监控脑裂脚本</a>
            </li>
            <li>
              <a href="#234">2.3.4 测试 确保两台负载均衡能够正常负载</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#24">2.4 排错过程</a>
        </li>
        <li>
          <a href="#25_nginx_vip">2.5 更改nginx反向代理配置 只监听vip地址</a>
        </li>
        <li>
          <a href="#26_keepalivednginx">2.6 让keepalived监控nginx</a><ul>
            <li>
              <a href="#261_keepalived">2.6.1 使用keepalived的监控脚本</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#27">2.7 多实例的配置</a><ul>
            <li>
              <a href="#271_lb01keepalived">2.7.1 lb01的keepalived配置文件</a>
            </li>
            <li>
              <a href="#272_lb02keepalived">2.7.2 修改lb02的keepalived配置文件</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#nbsp28_keepalivednbsp">&nbsp;2.8 keepalived双主模式示意图&nbsp;</a>
        </li>
      </ul>
    </li>
  </ul>
</div>