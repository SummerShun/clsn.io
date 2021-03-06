---
title: keepalived中的脑裂
author: 惨绿少年
type: post
date: 2017-10-28T07:24:00+00:00
url: /clsn/lx893.html
Baidusubmit:
  - 1
views:
  - 189
zm_like:
  - 2
categories:
  - Linux运维
  - 玩转Linux

---
&nbsp; &nbsp; 在高可用（HA）系统中，当联系2个节点的&ldquo;心跳线&rdquo;断开时，本来为一整体、动作协调的HA系统，就分裂成为2个独立的个体。由于相互失去了联系，都以为是对方出了故障。两个节点上的HA软件像&ldquo;裂脑人&rdquo;一样，争抢&ldquo;共享资源&rdquo;、争起&ldquo;应用服务&rdquo;，就会发生严重后果&mdash;&mdash;或者共享资源被瓜分、2边&ldquo;服务&rdquo;都起不来了；或者2边&ldquo;服务&rdquo;都起来了，但同时读写&ldquo;共享存储&rdquo;，导致数据损坏（常见如数据库轮询着的联机日志出错）。  
对付HA系统&ldquo;裂脑&rdquo;的对策，目前达成共识的的大概有以下几条：  
　　1）添加冗余的心跳线，例如：双线条线（心跳线也HA），尽量减少&ldquo;裂脑&rdquo;发生几率；  
　　2）启用磁盘锁。正在服务一方锁住共享磁盘，&ldquo;裂脑&rdquo;发生时，让对方完全&ldquo;抢不走&rdquo;共享磁盘资源。但使用锁磁盘也会有一个不小的问题，如果占用共享盘的一方不主动&ldquo;解锁&rdquo;，另一方就永远得不到共享磁盘。现实中假如服务节点突然死机或崩溃，就不可能执行解锁命令。后备节点也就接管不了共享资源和应用服务。于是有人在HA中设计了&ldquo;智能&rdquo;锁。即：正在服务的一方只在发现心跳线全部断开（察觉不到对端）时才启用磁盘锁。平时就不上锁了。  
　　3）设置仲裁机制。例如设置参考IP（如网关IP），当心跳线完全断开时，2个节点都各自ping一下参考IP，不通则表明断点就出在本端。不仅&ldquo;心跳&rdquo;、还兼对外&ldquo;服务&rdquo;的本端网络链路断了，即使启动（或继续）应用服务也没有用了，那就主动放弃竞争，让能够ping通参考IP的一端去起服务。更保险一些，ping不通参考IP的一方干脆就自我重启，以彻底释放有可能还占用着的那些共享资源。