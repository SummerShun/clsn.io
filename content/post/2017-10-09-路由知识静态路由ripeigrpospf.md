---
title: '路由知识  静态路由 rip eigrp ospf'
author: 惨绿少年
type: post
date: 2017-10-08T18:35:00+00:00
url: /clsn/lx943.html
Baidusubmit:
  - 1
views:
  - 123
categories:
  - Linux运维
  - 网络技术
  - 运维基本功

---
# <span id="1">第1章 <span style="font-family: '微软雅黑',sans-serif;">路由选择原理</span></span>

## <span id="11">1.1 <span style="font-family: '微软雅黑',sans-serif;">几个概念</span></span>

### <span id="111">1.1.1 <span style="font-family: '微软雅黑',sans-serif;">被动路由协议</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">用来在路由之间传递用户信息</span>
</p>

### <span id="112">1.1.2 <span style="font-family: '微软雅黑',sans-serif;">主动路由协议</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">用于维护路由器的路由表</span>
</p>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R2#show ip route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * - candidate default, U - per-user static route, o - ODR
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; P - periodic downloaded static route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.2.0/24 is directly connected, FastEthernet0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.3.0/24 is directly connected, Serial0/0/1
  </p>
</div>

## <span id="12">1.2 <span style="font-family: '微软雅黑',sans-serif;">路由的来源</span></span>

### <span id="121">1.2.1 <span style="font-family: '微软雅黑',sans-serif;">直连路由</span></span>

<p style="margin-left: 42.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">直接连接到路由器上的网络</span>
</p>

### <span id="122">1.2.2 <span style="font-family: '微软雅黑',sans-serif;">静态路由</span></span>

<p style="margin-left: 42.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">管理员<span style="background: lime;">手工构建</span>路由表</span>
</p>

### <span id="123">1.2.3 <span style="font-family: '微软雅黑',sans-serif;">动态路由</span></span>

<p style="margin-left: 42.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">路由器之间动态学习到的路由表</span>
</p>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R2#show&nbsp; ip interface brief
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Interface&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; IP-Address&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; OK? Method Status&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Protocol
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    FastEthernet0/0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 192.168.2.2&nbsp;&nbsp;&nbsp;&nbsp; YES manual up&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; up
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    FastEthernet0/1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; unassigned&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; YES unset&nbsp; administratively down down
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Serial0/0/0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; unassigned&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; YES unset&nbsp; down&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; down
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Serial0/0/1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 192.168.3.1&nbsp;&nbsp;&nbsp;&nbsp; YES manual up&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; up
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Vlan1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; unassigned&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; YES unset&nbsp; administratively down down
  </p>
</div>

## <span id="13">1.3 <span style="font-family: '微软雅黑',sans-serif;">静态路由的配置命令</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    ip route network-address subnet-mask {ip-add | exit-initerface}
  </p>
</div>

### <span id="131">1.3.1 <span style="font-family: '微软雅黑',sans-serif;">实例</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    ip route 192.168.1.0 255.255.255.0 192.168.12.2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    ip route 192.168.1.0 255.255.255.0 serial 0
  </p>
</div>

### <span id="132">1.3.2 <span style="font-family: '微软雅黑',sans-serif;">拓扑图</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

### <span id="133_R2">1.3.3 R2<span style="font-family: '微软雅黑',sans-serif;">路由信息</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R2#show ip route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: '微软雅黑',sans-serif;">&hellip;&hellip;</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.12.0/24 is directly connected, Serial0/0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.23.0/24 is directly connected, Serial0/0/1
  </p>
</div>

### <span id="134_R1R3">1.3.4 <span style="font-family: '微软雅黑',sans-serif;">添加路由，让</span>R1<span style="font-family: '微软雅黑',sans-serif;">与</span>R3<span style="font-family: '微软雅黑',sans-serif;">通讯</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R1(config)#ip route 192.168.23.0 255.255.255.0 192.168.12.2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R3(config)#ip route 192.168.12.0 255.255.255.0 serial 0/0/1
  </p>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">出口是本地端的出口端口，下一跳是对端路由的入口</span>ip

## <span id="14">1.4 <span style="font-family: '微软雅黑',sans-serif;">默认路由</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: yellow;">配置方式</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    ip route 0.0.0.0 0.0.0.0. 192.168.12.2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: yellow;">下一跳的入口</span><span style="color: yellow;">IP</span>
  </p>
</div>

## <span id="15_lookback">1.5 lookback <span style="font-family: '微软雅黑',sans-serif;">环回接口配置</span></span>

<p style="margin-left: 21.0pt;">
  lookback<span style="font-family: '微软雅黑',sans-serif;">就是路由器上一个<span style="background: lime;">环回地址</span>，它是一个虚拟的接口。</span> <span style="font-family: '微软雅黑',sans-serif;">可以确保路由</span>ID<span style="font-family: '微软雅黑',sans-serif;">的稳定性，该接口不会出现链路失效的情况。</span>
</p>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R1(config)#interface loopback 0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R1(config-if)#
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    %LINK-5-CHANGED: Interface Loopback0, changed state to up
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up
  </p>
</div>

### <span id="151">1.5.1 <span style="font-family: '微软雅黑',sans-serif;">练习</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>R1<span style="font-family: '微软雅黑',sans-serif;">上配置环回接口</span>1.1.1.1
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>R2<span style="font-family: '微软雅黑',sans-serif;">上配置环回接口</span>2.2.2.2
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>R3<span style="font-family: '微软雅黑',sans-serif;">上配置环回接口</span>3.3.3.3
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">实现全网互通。</span>
</p>

&nbsp;

<p style="text-align: center;" align="center">
  &nbsp;
</p>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

#### <span id="1511nbspR1">1.5.1.1&nbsp;R1<span style="font-family: '微软雅黑',sans-serif;">路由表</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R1#show&nbsp; ip route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * - candidate default, U - per-user static route, o - ODR
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; P - periodic downloaded static route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 1.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1.1.1.0 is directly connected, Loopback0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 2.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2.2.2.0 is directly connected, Serial0/0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 3.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3.3.3.0 is directly connected, Serial0/0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.12.0/24 is directly connected, Serial0/0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp; 192.168.23.0/24 [1/0] via 192.168.12.2
  </p>
</div>

#### <span id="1512nbspR2">1.5.1.2&nbsp;R2<span style="font-family: '微软雅黑',sans-serif;">路由表</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 1.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1.1.1.0 is directly connected, Serial0/0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 2.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2.2.2.0 is directly connected, Loopback0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 3.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3.3.3.0 is directly connected, Serial0/0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.12.0/24 is directly connected, Serial0/0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.23.0/24 is directly connected, Serial0/0/1
  </p>
</div>

#### <span id="1513nbspR3">1.5.1.3&nbsp;R3<span style="font-family: '微软雅黑',sans-serif;">路由表</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 1.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1.1.1.0 is directly connected, Serial0/0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 2.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2.2.2.0 is directly connected, Serial0/0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp; 3.0.0.0/24 is subnetted, 1 subnets
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3.3.3.0 is directly connected, Loopback0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S&nbsp;&nbsp;&nbsp; 192.168.12.0/24 is directly connected, Serial0/0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.23.0/24 is directly connected, Serial0/0/1
  </p>
</div>

# <span id="2_rip">第2章 <span style="font-family: '微软雅黑',sans-serif;">动态路由协议</span> rip</span>

## <span id="21">2.1 <span style="font-family: '微软雅黑',sans-serif;">什么是路由</span></span>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">根据目的地址查找路由表，依据<span style="background: lime;">路由表</span></span>
</p>

### <span id="211">2.1.1 <span style="font-family: '微软雅黑',sans-serif;">路由表方法</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R1#show&nbsp; ip route
  </p>
</div>

## <span id="22">2.2 <span style="font-family: '微软雅黑',sans-serif;">路由协议的分类</span></span>

### <span id="221">2.2.1 <span style="font-family: '微软雅黑',sans-serif;">静态路由</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">由管理员根据数据访问需求手工在每台设备上进行添加和维护</span>
</p>

### <span id="222">2.2.2 <span style="font-family: '微软雅黑',sans-serif;">动态路由</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">路由器<span style="background: lime;">自动进行路由信息的更新</span>和同步，并且当网络拓扑变更时，能够自动收敛。&lsquo;</span>
</p>

## <span id="23">2.3 <span style="font-family: '微软雅黑',sans-serif;">动态路由协议</span></span>

### <span id="231">2.3.1 <span style="font-family: '微软雅黑',sans-serif;">内部网关协议</span></span>

<span style="font-family: '微软雅黑',sans-serif;">距离矢量路由协议</span> <span style="background: lime;">rip</span>

<span style="font-family: '微软雅黑',sans-serif;">链路状态路由协议</span> <span style="background: lime;">ospf</span>

### <span id="232">2.3.2 <span style="font-family: '微软雅黑',sans-serif;">外部网关协议</span></span>

<p style="margin-left: 7.1pt;">
  BGP
</p>

## <span id="24">2.4 <span style="font-family: '微软雅黑',sans-serif;">距离矢量路由协议</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">使用<span style="background: lime;">距离矢量路由协议</span>的路由器并不了解到达目的网络的整条路径。</span>
</p>

<p style="margin-left: 21.0pt;">
  &nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">改路由器只知道：</span>
</p>

<p style="margin-left: 21.0pt;">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">自身与目的</span><span style="font-family: '微软雅黑',sans-serif;">网络之间的距离</span>
</p>

<p style="margin-left: 21.0pt;">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">应该往哪个方向或使用哪个接口转发数据包</span>
</p>

### <span id="241">2.4.1 <span style="font-family: '微软雅黑',sans-serif;">距离矢量路由协议特点：</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">周期性的更新（<span style="background: lime;">广播</span>）整张路由表</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

### <span id="242">2.4.2 <span style="font-family: '微软雅黑',sans-serif;">初次路由信息交换</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

### <span id="243">2.4.3 <span style="font-family: '微软雅黑',sans-serif;">收敛完成</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">当所有路由表包含<span style="background: lime;">相同路由表</span></span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

## <span id="25_Metric">2.5 <span style="font-family: '微软雅黑',sans-serif;">度量值</span><span style="color: red;"> Metric</span></span>

<p style="margin-left: 25.1pt;">
  <span style="background: lime;">rip </span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">以跳数</span><span style="font-family: '微软雅黑',sans-serif;">作为</span>metric
</p>

<p style="margin-left: 25.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">同一个路由器</span> <span style="font-family: '微软雅黑',sans-serif;">收到<span style="background: yellow;">多条去往同一目的地的路由</span>会比较</span> Metric
</p>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">会放在路由表当中。</span>

## <span id="26_AD">2.6 <span style="font-family: '微软雅黑',sans-serif;">管理距离<span style="color: red;">（</span></span><span style="color: red;">AD</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">值）</span> <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">总结</span></span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">一台路由器，当它从<span style="background: lime;">两种不同</span>的动态路由选择协议中，学习到去往同一个目的地的路由，这个时候<span style="background: lime;">比较</span></span><span style="background: lime;">AD</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">值</span> <span style="font-family: '微软雅黑',sans-serif;">取信小的，将路由装入路由表进行数据转发，另一条路径，只有当优选的路径</span>down<span style="font-family: '微软雅黑',sans-serif;">掉的时候，才能出现和使用</span>
</p>

<p style="margin-left: 25.1pt;">
  &nbsp;
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">一台路由器，当它从<span style="background: lime;">同种</span>动态路由选择协议中，但不同邻居学习去往同一个目的地的路由，<span style="background: lime;">比较</span></span><span style="background: lime;">metric </span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">度量值，</span><span style="font-family: '微软雅黑',sans-serif;">选择优的装进路由表，进行数据转发</span>
</p>

&nbsp;

## <span id="27">2.7 <span style="font-family: '微软雅黑',sans-serif;">依据传闻的更新</span></span>

### <span id="271">2.7.1 <span style="font-family: '微软雅黑',sans-serif;">环路的产生</span></span>

### <span id="272">2.7.2 <span style="font-family: '微软雅黑',sans-serif;">消除环路</span></span>

<span style="font-family: '微软雅黑',sans-serif;">定义最大度量值以防止计数至无穷大（定义最大跳数）：<span style="color: #0070c0;">超过以后不可达</span></span>

<span style="font-family: '微软雅黑',sans-serif;">水平分割：<span style="color: #0070c0;">收到的接口不再发出</span></span>

<span style="font-family: '微软雅黑',sans-serif;">抑制计数器：<span style="color: #0070c0;">为正在重新收敛的网络增加了应变能力，引入了某种程度的怀疑量</span></span>

<span style="font-family: '微软雅黑',sans-serif;">路由毒化或毒性反转：<span style="color: #0070c0;">将坏掉的网段信息变得条数无穷大，让路由不可达</span></span>

<span style="font-family: '微软雅黑',sans-serif;">触发更新：</span>

## <span id="28_rip">2.8 rip<span style="font-family: '微软雅黑',sans-serif;">协议概述</span></span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">路由信息协议</span>RIP<span style="font-family: '微软雅黑',sans-serif;">（</span>Routing Information Protocol<span style="font-family: '微软雅黑',sans-serif;">）是基于距离矢量算法的路由协议，利用跳数来作为计量标准。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  RIP<span style="font-family: '微软雅黑',sans-serif;">只能应用于小规模网络；</span>
</p>

&nbsp;&nbsp; &nbsp;&nbsp; RIP <span style="font-family: '微软雅黑',sans-serif;">时基于</span>udp <span style="font-family: '微软雅黑',sans-serif;">，端口是</span>520 <span style="font-family: '微软雅黑',sans-serif;">的应用层协议</span>

&nbsp;&nbsp; &nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">管理距离</span>120

<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: aqua;">支持主类网络通告</span>

### <span id="281_rip">2.8.1 rip<span style="font-family: '微软雅黑',sans-serif;">配置</span></span>

<span style="font-family: '微软雅黑',sans-serif;">启动</span>rip<span style="font-family: '微软雅黑',sans-serif;">路由选择进程</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    router rip
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">宣告指定的直连网络（直连接口的网段）</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    network network-number
  </p>
</div>

### <span id="282">2.8.2 <span style="font-family: '微软雅黑',sans-serif;">配置实例</span></span>

#### <span id="2821nbsp">2.8.2.1&nbsp;<span style="font-family: '微软雅黑',sans-serif;">拓扑图</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

#### <span id="2822nbsp">2.8.2.2&nbsp;<span style="font-family: '微软雅黑',sans-serif;">配置结果</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 0cm 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    R01 (config)#router rip
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    R01 (config-router)#network 192.168.12.0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    R01#show ip route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * - candidate default, U - per-user static route, o - ODR
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; P - periodic downloaded static route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    C&nbsp;&nbsp;&nbsp; 192.168.12.0/24 is directly connected, Serial0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    <span style="color: #ffc000;">R </span>&nbsp;&nbsp;&nbsp;192.168.23.0/24 [120/1] via 192.168.12.2, 00:00:10, Serial0/0
  </p>
</div>

# <span id="3_EIGRP">第3章 EIGRP<span style="font-family: '微软雅黑',sans-serif;">路由选择协议（增强型内部网关）</span></span>

<p style="margin-left: 7.1pt; text-indent: 13.9pt;">
  <span style="font-family: '微软雅黑',sans-serif;">收敛速度快</span>
</p>

<p style="margin-left: 7.1pt; text-indent: 13.9pt;">
  <span style="font-family: '微软雅黑',sans-serif;">带宽浪费少</span> keeplive
</p>

## <span id="31_eigrp">3.1 eigrp<span style="font-family: '微软雅黑',sans-serif;">的特点</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    1)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">高级距离矢量协议</span>--<span style="font-family: '微软雅黑',sans-serif;">具有距离矢量性和链路状态协议特征</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    2)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">无类路由协议</span>--<span style="font-family: '微软雅黑',sans-serif;">可划分子网、可聚合子网路由</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    3)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">支持</span>VLSM <span style="font-family: '微软雅黑',sans-serif;">（可变长子网掩码）与不连续子网</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    4)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span>100%<span style="font-family: '微软雅黑',sans-serif;">无环路</span>--DUAL<span style="font-family: '微软雅黑',sans-serif;">算法</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    5)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">快速收敛</span>--<span style="font-family: '微软雅黑',sans-serif;">路由条目不过期，拥有备份路由</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    6)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">触发更新</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    7)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">低路由更新信息开销</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    8)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">配置简单</span>
  </p>
  
  <p style="margin-left: 21pt; text-indent: -21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    9)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif;">支持多种网络层协议（</span>IP<span style="font-family: '微软雅黑',sans-serif;">、</span>ipx<span style="font-family: '微软雅黑',sans-serif;">、</span>appletalk<span style="font-family: '微软雅黑',sans-serif;">、</span>etc<span style="font-family: '微软雅黑',sans-serif;">）</span>
  </p>
</div>

## <span id="32_EIGRP">3.2 EIGRP<span style="font-family: '微软雅黑',sans-serif;">的三张表</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

## <span id="33_EIGRP">3.3 EIGRP<span style="font-family: '微软雅黑',sans-serif;">数据包</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 134.45pt; border-top: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-right: none; background: #4f81bd; padding: 0cm 5.4pt;" width="179">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">报文类型</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: 1pt solid white; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: none; background: #4f81bd; padding: 0cm 5.4pt;" width="518">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">含义</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="color: white;">HELLO</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #B8CCE4; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">用于发现邻居和维护邻居关系，使用组播</span>224.0.0.10<span style="font-family: '微软雅黑',sans-serif;">每</span>5<span style="font-family: '微软雅黑',sans-serif;">秒发送一次。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">更新（</span><span style="color: white;">UPDATE</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: white;">）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #DBE5F1; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">以单播（点对点网络）或组播（多路访问网络）方式可靠地发送其认为已经收敛的路由。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">查询（</span><span style="color: white;">QUERY</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: white;">）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #B8CCE4; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">以单播（点对点网络）或组播（多路访问网络）方式可靠地向邻居查询到达某目的地路由时使用的数据包，有时也以单播方式重传</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">应答（</span><span style="color: white;">REPLY</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: white;">）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #DBE5F1; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">以单播方式可靠地应答查询数据包</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">确认（</span><span style="color: white;">ACK</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: white;">）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #B8CCE4; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">以单播方式发送。用来确认</span>UPDATE<span style="font-family: '微软雅黑',sans-serif;">、</span>QUERY<span style="font-family: '微软雅黑',sans-serif;">以及</span>REPLY<span style="font-family: '微软雅黑',sans-serif;">数据包。</span>ACK<span style="font-family: '微软雅黑',sans-serif;">分组是上面</span>3<span style="font-family: '微软雅黑',sans-serif;">种数据包可靠传输的保障。</span>ACK<span style="font-family: '微软雅黑',sans-serif;">本身不需要确认</span>
      </p>
    </td>
  </tr>
</table>

## <span id="34_EIGRPMetric">3.4 EIGRP<span style="font-family: '微软雅黑',sans-serif;">度量值</span><span style="color: red;">Metric</span></span>

<p style="margin-left: 7.1pt; text-indent: 21.0pt;">
  EIGRP<span style="font-family: '微软雅黑',sans-serif;">使用度量值来确定到目的地的最佳路径。对于每一个子网，</span>EIGRP<span style="font-family: '微软雅黑',sans-serif;">拓扑表包含一条或者多条可能的路由。每条可能的路由都包含各种度量值：<span style="background: lime;">带宽，延迟、可靠性</span></span> <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">、负载、</span><span style="background: lime;">MTU</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">等</span><span style="font-family: '微软雅黑',sans-serif;">。</span>EIGRP<span style="font-family: '微软雅黑',sans-serif;">路由器根据度量值计算一个整数度量值，来选择前往目的地的最佳路由。</span>
</p>

<p style="margin-left: 7.1pt; text-indent: 21.0pt;">
  D <span style="font-family: '微软雅黑',sans-serif;">表示</span>EIGRP<span style="font-family: '微软雅黑',sans-serif;">协议</span>
</p>

### <span id="341_EIGRP">3.4.1 EIGRP<span style="font-family: '微软雅黑',sans-serif;">度量值计算公式</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

<p style="text-indent: 21.0pt;">
  EIGRP<span style="font-family: '微软雅黑',sans-serif;">度量值</span> = IGRP<span style="font-family: '微软雅黑',sans-serif;">的度量值</span>*256
</p>

<p style="text-indent: 21.0pt;">
  EIGRP<span style="font-family: '微软雅黑',sans-serif;">度量值的计算公式为：</span>256*{K1<span style="font-family: '微软雅黑',sans-serif;">（</span>10^7/<span style="font-family: '微软雅黑',sans-serif;">带宽）</span>+K2<span style="font-family: '微软雅黑',sans-serif;">（</span>10^7/<span style="font-family: '微软雅黑',sans-serif;">带宽）</span>/<span style="font-family: '微软雅黑',sans-serif;">（</span>256-<span style="font-family: '微软雅黑',sans-serif;">负载）</span>+K3<span style="font-family: '微软雅黑',sans-serif;">（延迟）</span>+K5/(<span style="font-family: '微软雅黑',sans-serif;">可靠性</span>+K4)}
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">默认情况下，</span>K1<span style="font-family: '微软雅黑',sans-serif;">和</span>K3<span style="font-family: '微软雅黑',sans-serif;">是</span>1<span style="font-family: '微软雅黑',sans-serif;">，<span style="background: lime;">其他的</span></span><span style="background: lime;">K</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">值都是</span><span style="background: lime;">0.</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">所以通常情况下，度量值</span>=256<span style="font-family: '微软雅黑',sans-serif;">&times;（</span>10^7/<span style="font-family: '微软雅黑',sans-serif;">最小带宽</span>+<span style="font-family: '微软雅黑',sans-serif;">累积延时）</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: yellow;">默认为</span> <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: yellow;">：</span> <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">带宽</span><span style="background: lime;">+</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">延迟</span>
</p>

### <span id="342_DUAL">3.4.2 DUAL<span style="font-family: '微软雅黑',sans-serif;">算法</span></span>

<p style="margin-left: 7.1pt; text-indent: 13.9pt;">
  <span style="font-family: '微软雅黑',sans-serif;">用于计算最佳无环路径和备用路径</span>
</p>

#### <span id="3421nbsp"><span style="courier new"4courier new"; background: aqua;">3.4.2.1&nbsp;</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: aqua;">特点</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">无环拓扑</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">可立即使用的无环备用路径</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">快速收敛</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">低带宽利用率（通过限定更新实现）</span>
</p>

#### <span id="3422nbsp">3.4.2.2&nbsp;<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: aqua;">几个术语</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">后继路由器</span>&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: '微软雅黑',sans-serif;">最终使用的路由器</span>
</p>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">可行距离（</span><span style="background: darkcyan;">FD</span><span style="font-family: '微软雅黑',sans-serif;">）</span>&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: '微软雅黑',sans-serif;">本地到达目的网络的距离，度量值</span>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">可行后继路由器（</span><span style="background: darkcyan;">FS</span><span style="font-family: '微软雅黑',sans-serif;">）</span> <span style="font-family: '微软雅黑',sans-serif;">备份</span>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">通告距离（</span><span style="background: darkcyan;">AD</span><span style="font-family: '微软雅黑',sans-serif;">）</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">某一个邻居去往目的地的距离</span>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">可行条件，或称可行性条件（</span><span style="background: darkcyan;">FC</span><span style="font-family: '微软雅黑',sans-serif;">）</span>

## <span id="35">3.5 <span style="font-family: '微软雅黑',sans-serif;">基本配置</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    route rigrp autonomous-ysytem
  </p>
</div>

&nbsp;&nbsp; autonomous-ysytem <span style="font-family: '微软雅黑',sans-serif;">自治系统</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    network network-number
  </p>
</div>

## <span id="36">3.6 <span style="font-family: '微软雅黑',sans-serif;">实例</span></span>

### <span id="361">3.6.1 <span style="font-family: '微软雅黑',sans-serif;">拓扑图</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

### <span id="362">3.6.2 <span style="font-family: '微软雅黑',sans-serif;">路由器配置</span></span>

R0_1<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_1(config)#router eigrp 100
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_1(config-router)#network 192.168.12.0 0.0.0.255
  </p>
</div>

R0_2<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_2(config)#router eigrp 100
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_2(config-router)#network 192.168.12.0 0.0.0.255
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_2(config-router)#network 192.168.1.0 0.0.0.255
  </p>
</div>

### <span id="363">3.6.3 <span style="font-family: '微软雅黑',sans-serif;">检查结果</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_1#show ip route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * - candidate default, U - per-user static route, o - ODR
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; P - periodic downloaded static route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #e36c0a;">D&nbsp;&nbsp;&nbsp; 192.168.1.0/24 [90/2297856] via 192.168.12.2, 00:01:23, Serial0/0</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.12.0/24 is directly connected, Serial0/0
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看详细的信息</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_1#<span style="color: #95b3d7;">show ip eigrp neighbors </span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    IP-EIGRP neighbors for process 100
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    H&nbsp;&nbsp; Address&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Interface&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Hold Uptime&nbsp;&nbsp;&nbsp; SRTT&nbsp;&nbsp; RTO&nbsp;&nbsp; Q&nbsp;&nbsp; Seq
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (sec)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (ms)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Cnt&nbsp; Num
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    0&nbsp;&nbsp; 192.168.12.2&nbsp;&nbsp;&nbsp; Se0/0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 13&nbsp;&nbsp; 00:02:47&nbsp; 40&nbsp;&nbsp;&nbsp;&nbsp; 1000&nbsp; 0&nbsp;&nbsp; 2
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看</span>eigrp<span style="font-family: '微软雅黑',sans-serif;">拓扑</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R0_1#<span style="color: #95b3d7;">show ip eigrp topology</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    IP-EIGRP Topology Table for AS 100/ID(192.168.12.1)
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; r - Reply status
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    P 192.168.1.0/24, 1 successors, FD is 2297856
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; via 192.168.12.2 (2297856/128256), Serial0/0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    P 192.168.12.0/24, 1 successors, FD is 2169856
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; via Connected, Serial0/0
  </p>
</div>

## <span id="37">3.7 <span style="font-family: '微软雅黑',sans-serif;">不等价负载均衡</span></span>

<p style="margin-left: 7.1pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">所谓不等价负载均衡指的是，到达目的地有多条路径，但是每条路径开销都不一样。并且能够保证在</span>eigrp<span style="font-family: '微软雅黑',sans-serif;">拓扑表中有路由条目。在路由表中只出现最有条目</span>
</p>

# <span id="4_OSPF">第4章 OSPF<span style="font-family: '微软雅黑',sans-serif;">路由选择协议</span></span>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">链路状态路由协议：</span>
</p>

<p style="margin-left: 7.1pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">开放式短路径优先（最英语：</span>Open Shortest Path First<span style="font-family: '微软雅黑',sans-serif;">，缩写为</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">）是对链路状态路由协议的一种实现，隶属内部网关协议（</span>IGP<span style="font-family: '微软雅黑',sans-serif;">），故运作于自治系统内部。采用戴克斯特拉算法（</span>Dijkstra's algorithm<span style="font-family: '微软雅黑',sans-serif;">）被用来计算最短路径树。它使用&ldquo;<span style="background: lime;">开销（</span></span><span style="background: lime;">Cost</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">）</span><span style="font-family: '微软雅黑',sans-serif;">&rdquo;作为路由度量。链路状态数据库（</span>LSDB<span style="font-family: '微软雅黑',sans-serif;">）用来保存当前网络拓扑结构，路由器上属于同一区域的链路状态数据库是相同的（属于多个区域的路由器会为每个区域维护一份链路状态数据库）。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: yellow;">接口敏感型协议</span>
</p>

## <span id="41_Open_Shortest_Path_First">4.1 Open Shortest Path First<span style="font-family: '微软雅黑',sans-serif;">开放式短路径优先</span></span>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">&Oslash;&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">是一种链路状态路由协议，无路由循环（全局拓扑），</span>PFC232
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">&Oslash;&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">开放意味着非私有</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">&Oslash;&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">管理性距离：</span>110
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">&Oslash;&nbsp;</span>ospf<span style="font-family: '微软雅黑',sans-serif;">采用</span>spf<span style="font-family: '微软雅黑',sans-serif;">算法计算到达目的地对的最短路径</span>
</p>

<p style="margin-left: 24.0pt;">
  &nbsp;&nbsp; --<span style="font-family: '微软雅黑',sans-serif;">链路（</span>link<span style="font-family: '微软雅黑',sans-serif;">）</span> =<span style="font-family: '微软雅黑',sans-serif;">路由器接口</span>
</p>

<p style="margin-left: 24.0pt;">
  &nbsp;&nbsp; --<span style="font-family: '微软雅黑',sans-serif;">状态（</span>state<span style="font-family: '微软雅黑',sans-serif;">）</span>=<span style="font-family: '微软雅黑',sans-serif;">描述接口以及与邻居路由器之间的关系</span>
</p>

## <span id="42_ospf_Metric">4.2 ospf <span style="font-family: '微软雅黑',sans-serif;">度量值</span> <span style="color: red;">Metric</span></span>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">每个路由都把自己当作根，并且给予累积成本（</span>cost <span style="font-family: '微软雅黑',sans-serif;">开销）来计算到达目的地的最短路径。</span>
</p>

### <span id="421_Cost">4.2.1 Cost<span style="font-family: '微软雅黑',sans-serif;">的计算</span></span>

<p style="margin-left: 28.1pt; text-indent: 13.9pt;">
  <span style="color: #002060;">Cost=</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: #002060;">参考带宽（</span><span style="color: #002060;">10^8</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: #002060;">）</span><span style="color: #002060;">/</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: #002060;">接口带宽（</span><span style="color: #002060;">b/s</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: #002060;">）</span>
</p>

## <span id="43_ospf">4.3 ospf<span style="font-family: '微软雅黑',sans-serif;">报文类型</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 155.7pt; border-top: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" valign="top" width="208">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">报文类型</span></strong>
      </p>
    </td>
    
    <td style="width: 367.1pt; border-top: 1pt solid white; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" valign="top" width="489">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">作用</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 155.7pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;" width="208">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="color: white;">Hello</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">包</span><span style="color: white;"> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HELLO</span></strong>
      </p>
    </td>
    
    <td style="width: 367.1pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #D6E3BC; padding: 0cm 5.4pt 0cm 5.4pt;" width="489">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">建立和维护</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">邻居关系</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 155.7pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;" width="208">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">数据库描述包</span><span style="color: white;">&nbsp; &nbsp;DBD</span></strong>
      </p>
    </td>
    
    <td style="width: 367.1pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="489">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">链路状态数据库描述信息（描述</span>LSDB<span style="font-family: '微软雅黑',sans-serif;">中</span>LSA<span style="font-family: '微软雅黑',sans-serif;">头部信息）</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 155.7pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;" width="208">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">链路状态请求包</span><span style="color: white;"> LSR</span></strong>
      </p>
    </td>
    
    <td style="width: 367.1pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #D6E3BC; padding: 0cm 5.4pt 0cm 5.4pt;" width="489">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">链路状态请求，向</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">邻居请求链路状态信息</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 155.7pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;" width="208">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">链路状态更新包</span><span style="color: white;"> LSU</span></strong>
      </p>
    </td>
    
    <td style="width: 367.1pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="489">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">链路状态更新</span>(<span style="font-family: '微软雅黑',sans-serif;">包含一条或多条</span>LSA)
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 155.7pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;" width="208">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">链路状态确认包</span><span style="color: white;"> LSAck</span></strong>
      </p>
    </td>
    
    <td style="width: 367.1pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #D6E3BC; padding: 0cm 5.4pt 0cm 5.4pt;" width="489">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">对</span>LSU<span style="font-family: '微软雅黑',sans-serif;">中的</span>LSA <span style="font-family: '微软雅黑',sans-serif;">进行确认</span>
      </p>
    </td>
  </tr>
</table>

### <span id="431_OSPF">4.3.1 OSPF<span style="font-family: '微软雅黑',sans-serif;">协议三张表</span></span>

<span style="font-family: '微软雅黑',sans-serif;">邻接表（</span>neighbor table<span style="font-family: '微软雅黑',sans-serif;">）：主要是与邻居间发送链路状态的依据；</span>

<span style="font-family: '微软雅黑',sans-serif;">链路状态表（</span>topology table<span style="font-family: '微软雅黑',sans-serif;">）：存储本网络所有路由器的链路状态；</span>

<span style="font-family: '微软雅黑',sans-serif;">路由表（</span>routing table<span style="font-family: '微软雅黑',sans-serif;">）：对链路状态数据库使用最短路径优先算法形成到达所有网段的最短路径。</span>

## <span id="44_OSPF">4.4 OSPF<span style="font-family: '微软雅黑',sans-serif;">区域</span></span>

<p style="margin-left: 7.1pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">一个</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">网络被分区成多个区域。区域将网络中的路由器在逻辑上分组并以区域为单位向网络的其余部分发送汇总路由信息。</span>
</p>

<p style="margin-left: 49.1pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">u&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">在区域边界可以做路由汇总，减小了路由表</span>
</p>

<p style="margin-left: 49.1pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">u&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">减少了</span>LSA<span style="font-family: '微软雅黑',sans-serif;">泛洪的范围，有效的把拓扑变化控制在区域内，提高了网络的稳定性</span>
</p>

<p style="margin-left: 49.1pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">u&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">拓扑的变化影响可以只涉及本区域</span>
</p>

<p style="margin-left: 49.1pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">u&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">多区域提高了网络的扩展性，有利于组建大规模的网络</span>
</p>

<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: #00B050;">至少有一个骨干区域</span> <span style="color: #00b050;">area 0</span>

## <span id="45_OSPF">4.5 OSPF<span style="font-family: '微软雅黑',sans-serif;">的基本运行步骤</span></span>

<p style="margin-left: 46.1pt; text-indent: -21.0pt;">
  1)&nbsp;<span style="font-family: '微软雅黑',sans-serif;">建立邻接关系</span>
</p>

<p style="margin-left: 46.1pt; text-indent: -21.0pt;">
  2)&nbsp;<span style="font-family: '微软雅黑',sans-serif;">必要的时候进行</span>DR<span style="font-family: '微软雅黑',sans-serif;">的选举</span>
</p>

<p style="margin-left: 46.1pt; text-indent: -21.0pt;">
  3)&nbsp;<span style="font-family: '微软雅黑',sans-serif;">发现路由</span>
</p>

<p style="margin-left: 46.1pt; text-indent: -21.0pt;">
  4)&nbsp;<span style="font-family: '微软雅黑',sans-serif;">选择合适的路由器</span>
</p>

<p style="margin-left: 46.1pt; text-indent: -21.0pt;">
  5)&nbsp;<span style="font-family: '微软雅黑',sans-serif;">维护路由信息</span>
</p>

### <span id="451_-hello">4.5.1 <span style="font-family: '微软雅黑',sans-serif;">建立邻接关系</span>-<span style="background: lime;">hello</span><span style="font-family: '微软雅黑',sans-serif;">包</span></span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  Hello<span style="font-family: '微软雅黑',sans-serif;">包的</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">包类型为</span>1<span style="font-family: '微软雅黑',sans-serif;">。这些包被周期性的从各个接口（包括虚链路）发出，用来创建和维护邻居关系。另外，在支持组播或广播的物理网络上，</span>Hello<span style="font-family: '微软雅黑',sans-serif;">包使用组播地址（通常为</span><span style="color: red;">224.0.0.5</span>)<span style="font-family: '微软雅黑',sans-serif;">发送。</span>
</p>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">通告两台路由器建立邻接关系所必要同一的参数</span>

## <span id="46_ospf">4.6 ospf<span style="font-family: '微软雅黑',sans-serif;">网络类型</span></span>

<span style="font-family: '微软雅黑',sans-serif;">点到点网络（</span>point-to-point<span style="font-family: '微软雅黑',sans-serif;">）</span>

<span style="font-family: '微软雅黑',sans-serif;">广播网络（</span>broadcast&nbsp; BMA<span style="font-family: '微软雅黑',sans-serif;">）</span> <span style="font-family: '微软雅黑',sans-serif;">多路访问</span>

<span style="font-family: '微软雅黑',sans-serif;">非广播多路访问网络（</span>non-broadcast multi-access<span style="font-family: '微软雅黑',sans-serif;">，</span>NBMA<span style="font-family: '微软雅黑',sans-serif;">）</span>

<span style="font-family: '微软雅黑',sans-serif;">点到多点网络（</span>Point-to-MultiPoint<span style="font-family: '微软雅黑',sans-serif;">）</span>

## <span id="47_LSA">4.7 LSA<span style="font-family: '微软雅黑',sans-serif;">的泛洪</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">必要时选举</span>DR<span style="font-family: '微软雅黑',sans-serif;">和</span>BDR
</p>

<p style="margin-left: 21.0pt;">
  DR<span style="font-family: '微软雅黑',sans-serif;">指定路由器</span>
</p>

<p style="margin-left: 21.0pt;">
  BDR<span style="font-family: '微软雅黑',sans-serif;">备份指定路由器</span>
</p>

### <span id="471_DRBDR">4.7.1 <span style="background: lime;">DR</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">与</span><span style="background: lime;">BDR</span></span>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">n&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">为减少多路访问网络中的</span>ospf<span style="font-family: '微软雅黑',sans-serif;">流量，</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">会选举一个指定路由（</span>DR<span style="font-family: '微软雅黑',sans-serif;">）和一个备用指定路由器（</span>BDR<span style="font-family: '微软雅黑',sans-serif;">）</span>
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">n&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">选举规则：最高接口优先级被选作</span>DR<span style="font-family: '微软雅黑',sans-serif;">，<span style="color: #0070c0;">如果优先级相等</span>（默认为</span>1<span style="font-family: '微软雅黑',sans-serif;">），具有最高的路由器</span>ID<span style="font-family: '微软雅黑',sans-serif;">（</span><span style="color: #0070c0;">Route-ID</span><span style="font-family: '微软雅黑',sans-serif;">）的路由器选举成</span>DR<span style="font-family: '微软雅黑',sans-serif;">，并且</span>DR<span style="font-family: '微软雅黑',sans-serif;">具有非抢占性</span>
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">n&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">指定路由器（</span>DR<span style="font-family: '微软雅黑',sans-serif;">）：</span>DR<span style="font-family: '微软雅黑',sans-serif;">负责使用该变化信息更新其它所有</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">路由器（</span>DRother<span style="font-family: '微软雅黑',sans-serif;">）</span>
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">n&nbsp;</span><span style="font-family: '微软雅黑',sans-serif;">备用指定路由器（</span>BDR<span style="font-family: '微软雅黑',sans-serif;">）：</span>BDR<span style="font-family: '微软雅黑',sans-serif;">会监控</span>DR<span style="font-family: '微软雅黑',sans-serif;">的状态，并在当前</span>DR<span style="font-family: '微软雅黑',sans-serif;">发生故障时接替其角色。</span>
</p>

### <span id="472_route-id">4.7.2 route-id</span>

<p style="margin-left: 7.1pt; text-indent: 13.9pt;">
  <span style="font-family: '微软雅黑',sans-serif;">每一台</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">路由器都有一个路由器标识符（</span>Identifier<span style="font-family: '微软雅黑',sans-serif;">），一般写作路由器</span>ID<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">可以手工配置，也可以自动获取。</span>

<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: aqua;">自动获取方式：</span>&nbsp;&nbsp;

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">有</span>loopback<span style="font-family: '微软雅黑',sans-serif;">接口时是</span>loopback<span style="font-family: '微软雅黑',sans-serif;">接口中的最大值。没有</span>loopback<span style="font-family: '微软雅黑',sans-serif;">的时候会在活动的物理接口的</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址</span>
</p>

## <span id="48_OSPF">4.8 OSPF<span style="font-family: '微软雅黑',sans-serif;">基本配置</span></span>

<span style="font-family: '微软雅黑',sans-serif;">开启</span>ospf<span style="font-family: '微软雅黑',sans-serif;">进程</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <strong>route ospf</strong> <em>process-id</em>
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">宣告特定的网络到</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">区域</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <strong>network address</strong> <em>witdcard-mask</em> <strong>area</strong> <em>srea-id</em>
  </p>
</div>

### <span id="481">4.8.1 <span style="font-family: '微软雅黑',sans-serif;">通配符掩码</span></span>

<p style="margin-left: 7.1pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">通配符是一个用于决定那些</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址位该精确匹配（</span><span style="font-family: '微软雅黑',sans-serif;">代表精确匹配）那些地址被忽略的</span>32<span style="font-family: '微软雅黑',sans-serif;">位数值，通常用于处理访问控制列表（</span>acl<span style="font-family: '微软雅黑',sans-serif;">），</span>OSPF<span style="font-family: '微软雅黑',sans-serif;">和</span>EIGRP<span style="font-family: '微软雅黑',sans-serif;">等路由协议的网络通告。</span>
</p>

#### <span id="4811nbsp"><span style="courier new"4courier new"; background: lime;">4.8.1.1&nbsp;</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">掩码</span></span>

<p style="margin-left: 7.1pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">表示网络位</span> 0<span style="font-family: '微软雅黑',sans-serif;">表示主机位</span>
</p>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">掩码用于区分</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址中的网络及主机部分。</span>
</p>

#### <span id="4812nbsp"><span style="courier new"4courier new"; background: lime;">4.8.1.2&nbsp;</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">通配符</span> <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: aqua;">反掩码</span></span>

<p style="margin-left: 7.1pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">表示无所谓</span>&nbsp; 0<span style="font-family: '微软雅黑',sans-serif;">表示需要严格匹配</span>
</p>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">通配符用于决定一个</span>IP<span style="font-family: '微软雅黑',sans-serif;">中的那些位该匹配</span>
</p>

## <span id="49_1">4.9 <span style="font-family: '微软雅黑',sans-serif;">【实例</span>1<span style="font-family: '微软雅黑',sans-serif;">】单区域</span></span>

### <span id="491">4.9.1 <span style="font-family: '微软雅黑',sans-serif;">拓扑图</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

&nbsp;

### <span id="492">4.9.2 <span style="font-family: '微软雅黑',sans-serif;">路由器配置</span></span>

R-01<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-01(config)#router ospf 1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-01(config-router)#network 192.168.12.0 0.0.0.255 area 0
  </p>
</div>

R-02<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-02(config)#router ospf 1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-02(config-router)#network 192.168.12.0 0.0.0.255 area 0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-02(config-router)#network 192.168.23.0 0.0.0.255 area 0
  </p>
</div>

R-03<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-03(config)#router ospf 1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-03(config-router)#net 192.168.23.0 0.0.0.255 ar 0
  </p>
</div>

### <span id="493">4.9.3 <span style="font-family: '微软雅黑',sans-serif;">查看路由表</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-03#show ip route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * - candidate default, U - per-user static route, o - ODR
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; P - periodic downloaded static route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #e36c0a;">O&nbsp;&nbsp;&nbsp; 192.168.12.0/24 [110/128] via 192.168.23.1, 00:00:31, Serial0/1</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.23.0/24 is directly connected, Serial0/1
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">检查</span>ospf

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-01#show ip route ospf
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #e36c0a;">O&nbsp;&nbsp;&nbsp; 192.168.23.0 [110/128] via 192.168.12.2, 00:08:48, Serial0/0</span>
  </p>
</div>

## <span id="410_2">4.10 <span style="font-family: '微软雅黑',sans-serif;">【实例</span>2<span style="font-family: '微软雅黑',sans-serif;">】多区域</span></span>

### <span id="4101">4.10.1 <span style="font-family: '微软雅黑',sans-serif;">拓扑图</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

### <span id="4102">4.10.2 <span style="font-family: '微软雅黑',sans-serif;">路由器配置</span></span>

R-ospf-1<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-1(config)#router ospf 1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-1(config-router)#network 192.168.12.0 0.0.0.255 area 0
  </p>
</div>

R-ospf-2<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-2(config)#router ospf 1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-2(config-router)#network 192.168.12.0 0.0.0.255 area 0
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-2(config-router)#network 192.168.23.0 0.0.0.255 area 1
  </p>
</div>

R-ospf-3<span style="font-family: '微软雅黑',sans-serif;">路由器配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-3(config)#router ospf 1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-3(config-router)#network 192.168.23.0 0.0.0.255 area 1
  </p>
</div>

### <span id="4103">4.10.3 <span style="font-family: '微软雅黑',sans-serif;">查看路由信息</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-3#show&nbsp; ip route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * - candidate default, U - per-user static route, o - ODR
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; P - periodic downloaded static route
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    Gateway of last resort is not set
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    &nbsp;
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    O IA 192.168.12.0/24 [110/128] via 192.168.23.1, 00:00:21, Serial0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    C&nbsp;&nbsp;&nbsp; 192.168.23.0/24 is directly connected, Serial0/1
  </p>
</div>

### <span id="4104_ospf">4.10.4 <span style="font-family: '微软雅黑',sans-serif;">查看</span>ospf<span style="font-family: '微软雅黑',sans-serif;">路由信息</span></span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    R-ospf-3#show&nbsp; ip route ospf
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    O IA 192.168.12.0 [110/128] via 192.168.23.1, 00:00:25, Serial0/1
  </p>
</div>

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1">第1章 路由选择原理</a><ul>
        <li>
          <a href="#11">1.1 几个概念</a><ul>
            <li>
              <a href="#111">1.1.1 被动路由协议</a>
            </li>
            <li>
              <a href="#112">1.1.2 主动路由协议</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#12">1.2 路由的来源</a><ul>
            <li>
              <a href="#121">1.2.1 直连路由</a>
            </li>
            <li>
              <a href="#122">1.2.2 静态路由</a>
            </li>
            <li>
              <a href="#123">1.2.3 动态路由</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#13">1.3 静态路由的配置命令</a><ul>
            <li>
              <a href="#131">1.3.1 实例</a>
            </li>
            <li>
              <a href="#132">1.3.2 拓扑图</a>
            </li>
            <li>
              <a href="#133_R2">1.3.3 R2路由信息</a>
            </li>
            <li>
              <a href="#134_R1R3">1.3.4 添加路由，让R1与R3通讯</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#14">1.4 默认路由</a>
        </li>
        <li>
          <a href="#15_lookback">1.5 lookback 环回接口配置</a><ul>
            <li>
              <a href="#151">1.5.1 练习</a><ul>
                <li>
                  <a href="#1511nbspR1">1.5.1.1&nbsp;R1路由表</a>
                </li>
                <li>
                  <a href="#1512nbspR2">1.5.1.2&nbsp;R2路由表</a>
                </li>
                <li>
                  <a href="#1513nbspR3">1.5.1.3&nbsp;R3路由表</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2_rip">第2章 动态路由协议 rip</a><ul>
        <li>
          <a href="#21">2.1 什么是路由</a><ul>
            <li>
              <a href="#211">2.1.1 路由表方法</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#22">2.2 路由协议的分类</a><ul>
            <li>
              <a href="#221">2.2.1 静态路由</a>
            </li>
            <li>
              <a href="#222">2.2.2 动态路由</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#23">2.3 动态路由协议</a><ul>
            <li>
              <a href="#231">2.3.1 内部网关协议</a>
            </li>
            <li>
              <a href="#232">2.3.2 外部网关协议</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#24">2.4 距离矢量路由协议</a><ul>
            <li>
              <a href="#241">2.4.1 距离矢量路由协议特点：</a>
            </li>
            <li>
              <a href="#242">2.4.2 初次路由信息交换</a>
            </li>
            <li>
              <a href="#243">2.4.3 收敛完成</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#25_Metric">2.5 度量值 Metric</a>
        </li>
        <li>
          <a href="#26_AD">2.6 管理距离（AD值） 总结</a>
        </li>
        <li>
          <a href="#27">2.7 依据传闻的更新</a><ul>
            <li>
              <a href="#271">2.7.1 环路的产生</a>
            </li>
            <li>
              <a href="#272">2.7.2 消除环路</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#28_rip">2.8 rip协议概述</a><ul>
            <li>
              <a href="#281_rip">2.8.1 rip配置</a>
            </li>
            <li>
              <a href="#282">2.8.2 配置实例</a><ul>
                <li>
                  <a href="#2821nbsp">2.8.2.1&nbsp;拓扑图</a>
                </li>
                <li>
                  <a href="#2822nbsp">2.8.2.2&nbsp;配置结果</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3_EIGRP">第3章 EIGRP路由选择协议（增强型内部网关）</a><ul>
        <li>
          <a href="#31_eigrp">3.1 eigrp的特点</a>
        </li>
        <li>
          <a href="#32_EIGRP">3.2 EIGRP的三张表</a>
        </li>
        <li>
          <a href="#33_EIGRP">3.3 EIGRP数据包</a>
        </li>
        <li>
          <a href="#34_EIGRPMetric">3.4 EIGRP度量值Metric</a><ul>
            <li>
              <a href="#341_EIGRP">3.4.1 EIGRP度量值计算公式</a>
            </li>
            <li>
              <a href="#342_DUAL">3.4.2 DUAL算法</a><ul>
                <li>
                  <a href="#3421nbsp">3.4.2.1&nbsp;特点</a>
                </li>
                <li>
                  <a href="#3422nbsp">3.4.2.2&nbsp;几个术语</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#35">3.5 基本配置</a>
        </li>
        <li>
          <a href="#36">3.6 实例</a><ul>
            <li>
              <a href="#361">3.6.1 拓扑图</a>
            </li>
            <li>
              <a href="#362">3.6.2 路由器配置</a>
            </li>
            <li>
              <a href="#363">3.6.3 检查结果</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#37">3.7 不等价负载均衡</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#4_OSPF">第4章 OSPF路由选择协议</a><ul>
        <li>
          <a href="#41_Open_Shortest_Path_First">4.1 Open Shortest Path First开放式短路径优先</a>
        </li>
        <li>
          <a href="#42_ospf_Metric">4.2 ospf 度量值 Metric</a><ul>
            <li>
              <a href="#421_Cost">4.2.1 Cost的计算</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#43_ospf">4.3 ospf报文类型</a><ul>
            <li>
              <a href="#431_OSPF">4.3.1 OSPF协议三张表</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#44_OSPF">4.4 OSPF区域</a>
        </li>
        <li>
          <a href="#45_OSPF">4.5 OSPF的基本运行步骤</a><ul>
            <li>
              <a href="#451_-hello">4.5.1 建立邻接关系-hello包</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#46_ospf">4.6 ospf网络类型</a>
        </li>
        <li>
          <a href="#47_LSA">4.7 LSA的泛洪</a><ul>
            <li>
              <a href="#471_DRBDR">4.7.1 DR与BDR</a>
            </li>
            <li>
              <a href="#472_route-id">4.7.2 route-id</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#48_OSPF">4.8 OSPF基本配置</a><ul>
            <li>
              <a href="#481">4.8.1 通配符掩码</a><ul>
                <li>
                  <a href="#4811nbsp">4.8.1.1&nbsp;掩码</a>
                </li>
                <li>
                  <a href="#4812nbsp">4.8.1.2&nbsp;通配符 反掩码</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#49_1">4.9 【实例1】单区域</a><ul>
            <li>
              <a href="#491">4.9.1 拓扑图</a>
            </li>
            <li>
              <a href="#492">4.9.2 路由器配置</a>
            </li>
            <li>
              <a href="#493">4.9.3 查看路由表</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#410_2">4.10 【实例2】多区域</a><ul>
            <li>
              <a href="#4101">4.10.1 拓扑图</a>
            </li>
            <li>
              <a href="#4102">4.10.2 路由器配置</a>
            </li>
            <li>
              <a href="#4103">4.10.3 查看路由信息</a>
            </li>
            <li>
              <a href="#4104_ospf">4.10.4 查看ospf路由信息</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</div>