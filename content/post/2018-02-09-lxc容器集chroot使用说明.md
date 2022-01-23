---
title: LXC 容器集chroot使用说明
author: 惨绿少年
type: post
date: 2018-02-08T16:42:00+00:00
url: /clsn/lx75.html
Baidusubmit:
  - 1
views:
  - 161
zm_favorites:
  - 1
categories:
  - Linux运维
  - 云计算
  - 玩转Linux
  - 虚拟化

---
## <span id="11_LXC">1.1 LXC<span style="font-family: '微软雅黑',sans-serif;">是什么？</span></span>

### <span id="111_LXC">1.1.1 <span style="font-family: '微软雅黑',sans-serif;">关于</span>LXC</span>

<p style="text-indent: 21.0pt;">
  LXC<span style="font-family: '微软雅黑',sans-serif;">，其名称来自</span>Linux<span style="font-family: '微软雅黑',sans-serif;">软件容器（</span><em>Linux Containers</em><span style="font-family: '微软雅黑',sans-serif;">）的缩写，一种<em><span style="text-decoration: underline;">操作系统层虚拟化（</span></em></span><em><span style="text-decoration: underline;">Operating system</span></em><em><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">&ndash;</span>level virtualization</span></em><em><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">）技术</span></span></em><span style="font-family: '微软雅黑',sans-serif;">，为</span>Linux<span style="font-family: '微软雅黑',sans-serif;">内核容器功能的一个用户空间接口。它将应用软件系统打包成一个软件<span style="background: yellow;">容器（</span></span><span style="background: yellow;">Container</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">）</span><span style="font-family: '微软雅黑',sans-serif;">，内含应用软件本身的代码，以及所需要的操作系统核心和库。通过统一的名字空间和共用</span>API<span style="font-family: '微软雅黑',sans-serif;">来分配不同软件容器的可用硬件资源，创造出应用程序的<span style="text-decoration: underline;">独立沙箱运行环境</span>，使得</span>Linux<span style="font-family: '微软雅黑',sans-serif;">用户可以容易的创建和管理系统或应用容器。</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180203180641203-214434730.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="LXC 容器集chroot使用说明" alt="" />
</p>

<p style="text-align: center;" align="center">
  <span style="font-family: '微软雅黑',sans-serif;">图</span> - lxc<span style="font-family: '微软雅黑',sans-serif;">官方</span>logo
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>Linux<span style="font-family: '微软雅黑',sans-serif;">内核中，提供了</span>cgroups<span style="font-family: '微软雅黑',sans-serif;">功能，来达成资源的区隔化。它同时也提供了名称空间区隔化的功能，使应用程序看到的操作系统环境被区隔成<span style="text-decoration: underline;">独立区间，包括进程树，网络，用户</span></span><span style="text-decoration: underline;">id</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">，以及挂载的文件系统。</span></span><span style="font-family: '微软雅黑',sans-serif;">但是</span><strong>cgroups</strong><span style="font-family: '微软雅黑',sans-serif;">并不一定需要引导任何虚拟机。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="background: yellow;">LXC</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">利用</span><span style="background: yellow;">cgroups</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">与名称空间的功能</span><span style="font-family: '微软雅黑',sans-serif;">，提供应用软件一个独立的操作系统环境。</span>LXC<span style="font-family: '微软雅黑',sans-serif;">不需要</span>Hypervisor<span style="font-family: '微软雅黑',sans-serif;">这个软件层，软件容器（</span>Container<span style="font-family: '微软雅黑',sans-serif;">）本身极为轻量化，提升了创建虚拟机的速度。软件</span>Docker<span style="font-family: '微软雅黑',sans-serif;">被用来管理</span>LXC<span style="font-family: '微软雅黑',sans-serif;">的环境。</span>
</p>

### <span id="112">1.1.2 <span style="font-family: '微软雅黑',sans-serif;">关于操作系统层虚拟化</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">操作系统层虚拟化（英语：</span><em>Operating system</em><em><span style="font-family: '微软雅黑',sans-serif;">&ndash;</span>level virtualization</em><span style="font-family: '微软雅黑',sans-serif;">），一种虚拟化技术，这种技术将操作系统内核虚拟化，可以允许使用者空间软件物件（</span>instances<span style="font-family: '微软雅黑',sans-serif;">）被分割成几个独立的单元，在内核中运行，而不是只有一个单一物件运行。这个软件物件，也被称为是一个<span style="text-decoration: underline;">容器（</span></span><span style="text-decoration: underline;">containers</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">），虚拟引擎（</span>Virtualization engine</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">），虚拟专用服务器（</span>virtual private servers</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">）或是</span> jails</span><span style="font-family: '微软雅黑',sans-serif;">。对每个行程的拥有者与使用者来说，他们使用的服务器程式，看起来就像是自己专用的。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">操作系统层虚拟化之后，可以实现<span style="background: lime;">软件的即时迁移（</span></span><span style="background: lime;">Live migration</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">）</span><span style="font-family: '微软雅黑',sans-serif;">，使一个软件容器中的物件，即时移动到另一个操作系统下，再重新执行起来。但是在这种技术下，软件即时迁移，只能在同样的操作系统下进行。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在类</span>Unix<span style="font-family: '微软雅黑',sans-serif;">操作系统中，这个技术最早起源于<strong><span style="color: red;">标准的</span></strong></span><strong><span style="color: red;">chroot</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">机制</span></strong><span style="font-family: '微软雅黑',sans-serif;">，再进一步演化而成。除了将软件独立化的机制之外，内核通常也提供资源管理功能，使得单一软件容器在运作时，对于其他软件容器的造成的交互影响最小化。</span>
</p>

### <span id="113_LXC">1.1.3 LXC<span style="font-family: '微软雅黑',sans-serif;">的特点</span></span>

<span style="font-family: '微软雅黑',sans-serif;">目前的</span>LXC<span style="font-family: '微软雅黑',sans-serif;">使用下列内核功能来控制进程：</span>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span><span style="font-family: 'MS Gothic';">&zwj;</span><span style="font-family: 'Segoe UI Emoji',sans-serif;">♂</span>️ <span style="font-family: '微软雅黑',sans-serif;">内核命名空间（进程间通信、</span>uts<span style="font-family: '微软雅黑',sans-serif;">、</span>mount<span style="font-family: '微软雅黑',sans-serif;">、</span>pid<span style="font-family: '微软雅黑',sans-serif;">、</span>network<span style="font-family: '微软雅黑',sans-serif;">和</span>user<span style="font-family: '微软雅黑',sans-serif;">）</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span><span style="font-family: 'MS Gothic';">&zwj;</span><span style="font-family: 'Segoe UI Emoji',sans-serif;">♂</span>️ AppArmor<span style="font-family: '微软雅黑',sans-serif;">和</span>SELinux<span style="font-family: '微软雅黑',sans-serif;">配置</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span><span style="font-family: 'MS Gothic';">&zwj;</span><span style="font-family: 'Segoe UI Emoji',sans-serif;">♂</span>️ Seccomp<span style="font-family: '微软雅黑',sans-serif;">策略</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span><span style="font-family: 'MS Gothic';">&zwj;</span><span style="font-family: 'Segoe UI Emoji',sans-serif;">♂</span>️ chroot<span style="font-family: '微软雅黑',sans-serif;">（使用</span>pivot_root<span style="font-family: '微软雅黑',sans-serif;">）</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span><span style="font-family: 'MS Gothic';">&zwj;</span><span style="font-family: 'Segoe UI Emoji',sans-serif;">♂</span>️ Kernel Capibilities
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span><span style="font-family: 'MS Gothic';">&zwj;</span><span style="font-family: 'Segoe UI Emoji',sans-serif;">♂</span>️ <span style="font-family: '微软雅黑',sans-serif;">控制组（</span>cgroups<span style="font-family: '微软雅黑',sans-serif;">）</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">因此，</span>LXC<span style="font-family: '微软雅黑',sans-serif;">通常被认为<span style="text-decoration: underline;">介于&ldquo;加强版&rdquo;的</span></span><span style="text-decoration: underline;">chroot</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">和完全成熟的虚拟机之间的技术</span></span><span style="font-family: '微软雅黑',sans-serif;">。</span>LXC<span style="font-family: '微软雅黑',sans-serif;">的目标是创建一个尽可能与标准安装的</span>Linux<span style="font-family: '微软雅黑',sans-serif;">相同但又不需要分离内核的环境。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">关于</span>chroot<span style="font-family: '微软雅黑',sans-serif;">可以参考：</span><a href="https://www.ibm.com/developerworks/cn/linux/l-cn-chroot/">https://www.ibm.com/developerworks/cn/linux/l-cn-chroot/</a>
</p>

### <span id="114_LXC">1.1.4 LXC<span style="font-family: '微软雅黑',sans-serif;">的应用</span></span>

**<span style="background: lime;">Docker</span>****<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">：</span>**

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">它在</span>0.9<span style="font-family: '微软雅黑',sans-serif;">版之前都是使用</span>LXC<span style="font-family: '微软雅黑',sans-serif;">技术，但在</span>0.9<span style="font-family: '微软雅黑',sans-serif;">版之后，已不再是唯一且默认的运行环境。</span>
</p>

**<span style="background: lime;">Proxmox VE</span>****<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">：</span>**

<p style="margin-left: 21.0pt;">
  Proxmox VE<span style="font-family: '微软雅黑',sans-serif;">是一个开源的服务器虚拟化环境</span>Linux<span style="font-family: '微软雅黑',sans-serif;">发行版。基于</span>Debian<span style="font-family: '微软雅黑',sans-serif;">，使用基于</span>Ubuntu<span style="font-family: '微软雅黑',sans-serif;">的定制内核。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">它直到</span>4.0<span style="font-family: '微软雅黑',sans-serif;">版才使用</span>LXC<span style="font-family: '微软雅黑',sans-serif;">技术，在此之前的版本都是使用</span>OpenVZ<span style="font-family: '微软雅黑',sans-serif;">技术。</span>
</p>

## <span id="12_LXC">1.2 <span style="font-family: '微软雅黑',sans-serif;">安装</span>LXC</span>

### <span id="121">1.2.1 <span style="font-family: '微软雅黑',sans-serif;">环境说明</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span>https://linuxcontainers.org/lxc/getting-started/
</p>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">安装</span>LXC<span style="font-family: '微软雅黑',sans-serif;">内核版本不能低于</span>2.6.32<span style="font-family: '微软雅黑',sans-serif;">，对</span>lxc<span style="font-family: '微软雅黑',sans-serif;">至此最佳的为</span>Ubuntu<span style="font-family: '微软雅黑',sans-serif;">系统。</span>

<span style="font-family: '微软雅黑',sans-serif;">宿主机环境说明：</span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/redhat-release </span>
CentOS Linux release 7.2.1511<span style="color: #000000;"> (Core) 
[root@lxc </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r </span>
3.10.0-327<span style="color: #000000;">.el7.x86_64
[root@lxc </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> sestatus </span>
<span style="color: #000000;">SELinux status:                 disabled
[root@lxc </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> hostname -I</span>
172.16.1.100 10.0.0.100</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">安装</span>lxc<span style="font-family: '微软雅黑',sans-serif;">需要使用到</span>epel<span style="font-family: '微软雅黑',sans-serif;">源，这里使用的是</span>aliyun<span style="font-family: '微软雅黑',sans-serif;">源站</span>
</p>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install epel-release -y    </span>
[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo</span></pre>
</div>

### <span id="122_LXC">1.2.2 <span style="font-family: '微软雅黑',sans-serif;">安装</span>LXC</span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install lxc-* libcgroup* bridge-utils.x86_64 -y</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">包组说明</span>

> &nbsp;lxc&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;lxc相关软件
> 
> &nbsp;libcgroup&nbsp; &nbsp;&nbsp;资源管理，限制
> 
> &nbsp;bridge-utils&nbsp;管理桥接网卡

### <span id="123">1.2.3 <span style="font-family: '微软雅黑',sans-serif;">修改网卡配置</span></span>

<p style="text-indent: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建桥接网卡</span>
</p>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;">  cat /etc/sysconfig/network-scripts/ifcfg-eth0 </span>
TYPE=<span style="color: #000000;">Ethernet
BOOTPROTO</span>=<span style="color: #000000;">none
NAME</span>=<span style="color: #000000;">eth0
DEVICE</span>=<span style="color: #000000;">eth0
ONBOOT</span>=<span style="color: #000000;">yes
BRIDGE</span>=<span style="color: #000000;">br0
[root@lxc </span>~]<span style="color: #008000;">#</span><span style="color: #008000;">  cat /etc/sysconfig/network-scripts/ifcfg-br0 </span>
TYPE=<span style="color: #000000;">Bridge
BOOTPROTO</span>=<span style="color: #000000;">static
NAME</span>=<span style="color: #000000;">br0
DEVICE</span>=<span style="color: #000000;">br0
ONBOOT</span>=<span style="color: #000000;">yes
IPADDR</span>=10.0.0.100<span style="color: #000000;">
NETMASK</span>=255.255.255.0<span style="color: #000000;">
GATEWAY</span>=10.0.0.254<span style="color: #000000;">
DNS1</span>=223.5.5.5</pre>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">修改</span>lxc<span style="font-family: '微软雅黑',sans-serif;">配置使用桥接网卡</span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vi /etc/lxc/default.conf </span>
lxc.network.type =<span style="color: #000000;"> veth
lxc.network.link </span>=<span style="color: #000000;"> br0
lxc.network.flags </span>= up</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">重启网卡</span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> /etc/init.d/network restart </span>
Restarting network (via systemctl):                        [  确定  ]</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看桥接信息</span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> brctl show</span>
<span style="color: #000000;">bridge name    bridge id        STP enabled    interfaces
br0        </span>8000.000c29c008a6    no        eth0</pre>
</div>

### <span id="124">1.2.4 <span style="font-family: '微软雅黑',sans-serif;">启动服务</span></span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl restart cgconfig.service </span>
[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl enable cgconfig.service </span>
[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ystemctl restart lxc.service</span></pre>
</div>

## <span id="13_LXC">1.3 <span style="font-family: '微软雅黑',sans-serif;">启动第一个</span>LXC<span style="font-family: '微软雅黑',sans-serif;">容器</span></span>

<p style="margin-left: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考方法</span>: <a href="https://mirrors.tuna.tsinghua.edu.cn/help/lxc-images/">https://mirrors.tuna.tsinghua.edu.cn/help/lxc-images/</a>
</p>

<p style="margin-left: 15.75pt;">
  <strong><span style="color: red;">lxc</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">启动容器时，推荐使用</span><span style="color: red;">ubuntu/centos6</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">系统，</span><span style="color: red;">centos7</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">系统使用会有问题。</span></strong>
</p>

<div class="cnblogs_code">
  <pre>[root@docker01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> lxc-create -t download -n centos7 -- --server mirrors.tuna.tsinghua.edu.cn/lxc-images -d centos -r 7 -a amd64</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">参数说明：</span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 141.5pt; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="189">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">参数</span></strong>
      </p>
    </td>
    
    <td style="width: 381.3pt; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="508">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 141.5pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="189">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>-t, --template=t</strong>
      </p>
    </td>
    
    <td style="width: 381.3pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="508">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">设置容器的模板</span>.LXC 1.0 <span style="font-family: '微软雅黑',sans-serif;">以上版本增加了</span> download <span style="font-family: '微软雅黑',sans-serif;">模版，支持下载定义好的系统镜像。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 141.5pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="189">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>-n, --name=NAME</strong>
      </p>
    </td>
    
    <td style="width: 381.3pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="508">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">为容器设置名称</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 141.5pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="189">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>--server</strong>
      </p>
    </td>
    
    <td style="width: 381.3pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="508">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">指定服务器地址</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 141.5pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="189">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>-d <em>DIST</em></strong>
      </p>
    </td>
    
    <td style="width: 381.3pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="508">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">发行版本</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 141.5pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="189">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>-r <em>RELEASE</em></strong>
      </p>
    </td>
    
    <td style="width: 381.3pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="508">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">系统版本</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 141.5pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="189">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>-a <em>ARCH</em></strong>
      </p>
    </td>
    
    <td style="width: 381.3pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="508">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">架构</span>
      </p>
    </td>
  </tr>
</table>

<span style="font-family: '微软雅黑',sans-serif;">查看你</span>lxc<span style="font-family: '微软雅黑',sans-serif;">的缓存文件位置</span>

<div class="cnblogs_code">
  <pre>[root@lxc lxc]<span style="color: #008000;">#</span><span style="color: #008000;"> cd /var/cache/lxc</span>
[root@lxc lxc]<span style="color: #008000;">#</span><span style="color: #008000;"> tree</span>
<span style="color: #000000;">.
└── download
    └── centos
        ├── </span>6<span style="color: #000000;">
        │   └── amd64
        │       └── default
        │           ├── build_id
        │           ├── config
        │           ├── config</span>-<span style="color: #000000;">user
        │           ├── create</span>-<span style="color: #000000;">message
        │           ├── excludes</span>-<span style="color: #000000;">user
        │           ├── expiry
        │           ├── rootfs.tar.xz
        │           └── templates
        └── </span>7<span style="color: #000000;">
            └── amd64
                └── default
                    ├── build_id
                    ├── config
                    ├── config</span>-<span style="color: #000000;">user
                    ├── create</span>-<span style="color: #000000;">message
                    ├── excludes</span>-<span style="color: #000000;">user
                    ├── expiry
                    ├── rootfs.tar.xz
                    └── templates</span></pre>
</div>

### <span id="131">1.3.1 <span style="font-family: '微软雅黑',sans-serif;">查看你当前容器列表</span></span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> lxc-ls </span>
centos6  centos7  </pre>
</div>

### <span id="132">1.3.2 <span style="font-family: '微软雅黑',sans-serif;">修改容器的密码</span></span>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">修改密码使用</span>chroot<span style="font-family: '微软雅黑',sans-serif;">的方式进行修改</span>
</p>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chroot /var/lib/lxc/centos7/rootfs/ passwd</span>
Changing password <span style="color: #0000ff;">for</span><span style="color: #000000;"> user root.
New password: </span>123456<span style="color: #000000;">
BAD PASSWORD: The password </span><span style="color: #0000ff;">is</span> shorter than 8<span style="color: #000000;"> characters
Retype new password: </span>123456<span style="color: #000000;">
passwd: all authentication tokens updated successfully.</span></pre>
</div>

### <span id="133">1.3.3 <span style="font-family: '微软雅黑',sans-serif;">启动容器</span></span>

<p style="text-indent: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在启动容器之前，先修改容器配置文件，添加</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址信息</span>
</p>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /var/lib/lxc/centos7/config</span>
lxc.network.name =<span style="color: #000000;"> eth0
lxc.network.ipv4 </span>= 10.0.0.111/24<span style="color: #000000;">
lxc.network.ipv4.gateway </span>= 10.0.0.254</pre>
</div>

&nbsp;&nbsp; ip<span style="font-family: '微软雅黑',sans-serif;">设置好后可以启动容器</span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> lxc-start -n centos7 </span>
<span style="color: #000000;">
centos7 login: root
Password: 
Last failed login: Wed Jan </span>31 10:12:22 UTC 2018 on lxc/<span style="color: #000000;">console
There was </span>1<span style="color: #000000;"> failed login attempt since the last successful login.
[root@centos7 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install openssh-server -y </span>
[root@centos7 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl start sshd</span></pre>
</div>

### <span id="134">1.3.4 <span style="font-family: '微软雅黑',sans-serif;">启动容器的其他方法</span></span>

<div class="cnblogs_code">
  <pre>lxc-create -t centos -n test</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">这种方法启动的容器会生成默认的密码</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">默认密码位置</span>;
</p>

<div class="cnblogs_code">
  <pre>cat /var/lib/lxc/test/tmp_root_pass</pre>
</div>

## <span id="14_LXC">1.4 LXC<span style="font-family: '微软雅黑',sans-serif;">管理操作</span></span>

<p style="margin-left: 21.0pt;">
  LXC<span style="font-family: '微软雅黑',sans-serif;">官方手册：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://lxc.sourceforge.net/man/" >http://lxc.sourceforge.net/man/</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">克隆一个容器</span>

<div class="cnblogs_code">
  <pre>[root@docker01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> lxc-clone  test  clsn-image</span>
Created container clsn-image as copy of test</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">检查</span>lxc<span style="font-family: '微软雅黑',sans-serif;">配置</span>

<div class="cnblogs_code">
  <pre>[root@docker01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> lxc-checkconfig </span>
--- Namespaces ---<span style="color: #000000;">
Namespaces: enabled
Utsname namespace: enabled
Ipc namespace: enabled
Pid namespace: enabled
User namespace: enabled
Network namespace: enabled
Multiple </span>/dev/<span style="color: #000000;">pts instances: enabled
&middot;&middot;&middot;</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">持续观察容器的状态和优先级变化：</span>

<div class="cnblogs_code">
  <pre>lxc-monitor -n name</pre>
</div>

&nbsp;LXC <span style="font-family: '微软雅黑',sans-serif;">使用</span> cgroup <span style="font-family: '微软雅黑',sans-serif;">文件系统管理容器。</span>

<span style="font-family: '微软雅黑',sans-serif;">可以通过</span> LXC <span style="font-family: '微软雅黑',sans-serif;">读和操纵</span> cgroup <span style="font-family: '微软雅黑',sans-serif;">文件系统的一些部分。要管理每个容器对</span> cpu <span style="font-family: '微软雅黑',sans-serif;">的使用，则可以通过读取和调整容器的</span> cpu.shares <span style="font-family: '微软雅黑',sans-serif;">来进行：</span>

<div class="cnblogs_code">
  <pre>lxc-cgroup -<span style="color: #000000;">n name cpu.shares
lxc</span>-cgroup -n name cpu.shares howmany</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">暂停和恢复</span>

<div class="cnblogs_code">
  <pre>lxc-freeze -<span style="color: #000000;">n name
lxc</span>-unfreeze -n name</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">停止，停止一个容器将导致该容器中启动的所有进程全体死亡，并且清理容器：</span>

<div class="cnblogs_code">
  <p>
    lxc-stop -n name
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">销毁，销毁容器是指删除通过</span> lxc-create <span style="font-family: '微软雅黑',sans-serif;">步骤与名称关联的配置文件和元数据：</span>

<div class="cnblogs_code">
  <p>
    lxc-destroy -n name
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看容器当前状态</span>

<div class="cnblogs_code">
  <pre>[root@lxc ~]<span style="color: #008000;">#</span><span style="color: #008000;"> lxc-info --name centos6</span>
<span style="color: #000000;">Name:           centos6
State:          STOPPED</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看</span>lxc<span style="font-family: '微软雅黑',sans-serif;">使用进程情况</span>

<div class="cnblogs_code">
  <p>
    lxc-top
  </p>
</div>

## <span id="15_chroot_SSH">1.5 <span style="font-family: '微软雅黑',sans-serif;">使用</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱限制</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户访问指定目录</span></span>

### <span id="151">1.5.1 <span style="font-family: '微软雅黑',sans-serif;">当前网络威胁</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">暴露在互联网上的系统要将坏人隔离在外是一个很大的挑战，而且一直更新最新的安全补丁也不太容易。因此，一些聪明的管理员们尝试采用系统的方法来限制可能发生的入侵，这其中一个绝佳的方法就是使用</span>chroot<span style="font-family: '微软雅黑',sans-serif;">监狱（</span>jail<span style="font-family: '微软雅黑',sans-serif;">）。</span>
</p>

<p style="text-indent: 21.0pt;">
  chroot<span style="font-family: '微软雅黑',sans-serif;">监狱极大地限制了应用程序可查看的文件系统范围，拥有更少的系统权限，这些都是为了限制应用程序误操作或者被坏人利用从而对系统造成损害。</span>
</p>

<span style="font-family: '微软雅黑',sans-serif;">本文简述一下</span>chroot<span style="font-family: '微软雅黑',sans-serif;">是如何工作的，并着重讨论一下开发者和管理员能够用到的一些最佳实践来让系统更加安全。</span>

### <span id="152_chroot">1.5.2 chroot<span style="font-family: '微软雅黑',sans-serif;">做什么</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">将</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户会话限制访问到特定的目录内，特别是在</span> web <span style="font-family: '微软雅黑',sans-serif;">服务器上，这样做有多个原因，但最显而易见的是为了系统安全。为了锁定</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户在某个目录，我们可以使用</span> chroot <span style="font-family: '微软雅黑',sans-serif;">机制。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在诸如</span> Linux <span style="font-family: '微软雅黑',sans-serif;">之类的类</span> Unix <span style="font-family: '微软雅黑',sans-serif;">系统中更改</span> root<span style="font-family: '微软雅黑',sans-serif;">（</span>chroot<span style="font-family: '微软雅黑',sans-serif;">）是将特定用户操作与其他</span> Linux <span style="font-family: '微软雅黑',sans-serif;">系统分离的一种手段；使用称为</span> chrooted <span style="font-family: '微软雅黑',sans-serif;">监狱</span> <span style="font-family: '微软雅黑',sans-serif;">的新根目录更改当前运行的用户进程及其子进程的明显根目录。</span>
</p>

<p style="text-indent: 21.0pt;">
  chroot<span style="font-family: '微软雅黑',sans-serif;">系统调用将当前进程及其子进程的</span>root<span style="font-family: '微软雅黑',sans-serif;">目录修改到一个特定的路径，通常是在文件系统真正的</span>root<span style="font-family: '微软雅黑',sans-serif;">目录下的一些受限的子目录中。进程认为新的路径就是系统的&ldquo;</span>/<span style="font-family: '微软雅黑',sans-serif;">&rdquo;，因此我们将这个受限制的环境称为&ldquo;监狱&rdquo;。除非一些特殊手段，要从监狱里面逃出来是根本不可能的。</span>
</p>

<p style="text-indent: 21.0pt;">
  chroot<span style="font-family: '微软雅黑',sans-serif;">系统调用存在于所有已知的</span>UNIX<span style="font-family: '微软雅黑',sans-serif;">版本中，它能够为运行的进程创建一个临时根目录，这种方法将一个受限制的文件系统（比如，</span>/chroot/named)<span style="font-family: '微软雅黑',sans-serif;">作为进程可见的最上层目录。</span>
</p>

### <span id="153_SSH_chroot">1.5.3 <span style="font-family: '微软雅黑',sans-serif;">创建</span> SSH chroot <span style="font-family: '微软雅黑',sans-serif;">监狱</span></span>

1<span style="font-family: '微软雅黑',sans-serif;">、使用</span> mkdir <span style="font-family: '微软雅黑',sans-serif;">命令开始创建</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱</span>

<div class="cnblogs_code">
  <pre>[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /opt/clsn</span></pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、根据</span> sshd_config <span style="font-family: '微软雅黑',sans-serif;">手册找到所需的文件，</span>ChrootDirectory <span style="font-family: '微软雅黑',sans-serif;">选项指定在身份验证后要</span> chroot <span style="font-family: '微软雅黑',sans-serif;">到的目录的路径名。该目录必须包含支持用户会话所必需的文件和目录。</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">对于交互式会话，这需要至少一个</span> shell<span style="font-family: '微软雅黑',sans-serif;">，通常为</span> sh <span style="font-family: '微软雅黑',sans-serif;">和基本的</span> /dev <span style="font-family: '微软雅黑',sans-serif;">节点，例如</span> <em>null</em><em><span style="font-family: '微软雅黑',sans-serif;">、</span>zero</em><em><span style="font-family: '微软雅黑',sans-serif;">、</span>stdin</em><em><span style="font-family: '微软雅黑',sans-serif;">、</span>stdout</em><em><span style="font-family: '微软雅黑',sans-serif;">、</span>stderr </em><span style="font-family: '微软雅黑',sans-serif;">和</span> <em>tty</em> <span style="font-family: '微软雅黑',sans-serif;">设备</span>
</p>

<div class="cnblogs_code">
  <pre>[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ls -l /dev/{null,zero,stdin,stdout,stderr,random,tty}</span>
crw-rw-rw- 1 root root 1, 3 Jan 31 13:24 /dev/<span style="color: #000000;">null
crw</span>-rw-rw- 1 root root 1, 8 Jan 31 13:24 /dev/<span style="color: #000000;">random
lrwxrwxrwx </span>1 root root   15 Jan 31  2018 /dev/stderr -> /proc/self/fd/2<span style="color: #000000;">
lrwxrwxrwx </span>1 root root   15 Jan 31  2018 /dev/stdin -> /proc/self/fd/<span style="color: #000000;">0
lrwxrwxrwx </span>1 root root   15 Jan 31  2018 /dev/stdout -> /proc/self/fd/1<span style="color: #000000;">
crw</span>-rw-rw- 1 root tty  5, 0 Jan 31 13:24 /dev/<span style="color: #000000;">tty
crw</span>-rw-rw- 1 root root 1, 5 Jan 31 13:24 /dev/zero</pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">、使用</span> mknod <span style="font-family: '微软雅黑',sans-serif;">命令创建</span> /dev <span style="font-family: '微软雅黑',sans-serif;">下的文件。在下面的命令中，</span>-m <span style="font-family: '微软雅黑',sans-serif;">标志用来指定文件权限位，</span>c <span style="font-family: '微软雅黑',sans-serif;">意思是字符文件，两个数字分别是文件指向的主要号和次要号。</span>

<div class="cnblogs_code">
  <pre>[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /opt/clsn/dev</span>
[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd /opt/clsn/dev</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> mknod -m 666 null c 1 3</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> mknod -m 666 tty c 5 0</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> mknod -m 666 zero c 1 5</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> mknod -m 666 random c 1 8</span></pre>
</div>

4<span style="font-family: '微软雅黑',sans-serif;">、在</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱中设置合适的权限。注意</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱和它的子目录以及子文件必须被</span> root <span style="font-family: '微软雅黑',sans-serif;">用户所有，并且对普通用户或用户组不可写</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> chown root.root /opt/clsn</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> chmod 0755 /opt/clsn</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> ls -ld /opt/clsn</span>
drwxr-xr-x 3 root root 4096 Jan 31 13:36 /opt/clsn</pre>
</div>

### <span id="154_SSH_chroot_shell">1.5.4 <span style="font-family: '微软雅黑',sans-serif;">为</span> SSH chroot <span style="font-family: '微软雅黑',sans-serif;">监狱设置交互式</span> shell</span>

1<span style="font-family: '微软雅黑',sans-serif;">、创建</span> bin <span style="font-family: '微软雅黑',sans-serif;">目录并复制</span> /bin/bash <span style="font-family: '微软雅黑',sans-serif;">到</span> bin <span style="font-family: '微软雅黑',sans-serif;">中</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /opt/clsn/bin</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> cp -v /bin/bash  /opt/clsn/bin/</span>
`/bin/bash<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/bin/bash</span><span style="color: #800000;">'</span></pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、识别</span> bash <span style="font-family: '微软雅黑',sans-serif;">所需的共享库，如下所示复制它们到</span> lib64 <span style="font-family: '微软雅黑',sans-serif;">中</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> ldd /bin/bash </span>
    linux-vdso.so.1 =>  (0x00007fffb68fb000<span style="color: #000000;">)
    libtinfo.so.</span>5 => /lib64/libtinfo.so.5 (0x0000003600a00000<span style="color: #000000;">)
    libdl.so.</span>2 => /lib64/libdl.so.2 (0x00000035fee00000<span style="color: #000000;">)
    libc.so.</span>6 => /lib64/libc.so.6 (0x00000035ff200000<span style="color: #000000;">)
    </span>/lib64/ld-linux-x86-64.so.2 (0x00000035fea00000<span style="color: #000000;">)
[root@clsn dev]</span><span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /opt/clsn/lib64</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> cp -v /lib64/{libtinfo.so.5,libdl.so.2,libc.so.6,ld-linux-x86-64.so.2} /opt/clsn/lib64/</span>
`/lib64/libtinfo.so.5<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libtinfo.so.5</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/libdl.so.2<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libdl.so.2</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/libc.so.6<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libc.so.6</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/ld-linux-x86-64.so.2<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/ld-linux-x86-64.so.2</span><span style="color: #800000;">'</span></pre>
</div>

### <span id="155_SSH">1.5.5 <span style="font-family: '微软雅黑',sans-serif;">创建并配置</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户</span></span>

1<span style="font-family: '微软雅黑',sans-serif;">、使用</span> useradd <span style="font-family: '微软雅黑',sans-serif;">命令创建</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户，并设置安全密码</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> useradd  clsn</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> echo 123456|passwd --stdin clsn </span>
Changing password <span style="color: #0000ff;">for</span><span style="color: #000000;"> user clsn.
passwd: all authentication tokens updated successfully.</span></pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、创建</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱通用配置目录</span> /opt/etc <span style="font-family: '微软雅黑',sans-serif;">并复制已更新的账号文件（</span>/etc/passwd <span style="font-family: '微软雅黑',sans-serif;">和</span> /etc/group<span style="font-family: '微软雅黑',sans-serif;">）到这个目录中</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /opt/clsn/etc</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> cp -vf /etc/{passwd,group} /opt/clsn/etc/</span>
`/etc/passwd<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/etc/passwd</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/etc/group<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/etc/group</span><span style="color: #800000;">'</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意：每次向系统添加更多</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户时，都需要将更新的帐户文件复制到</span> /opt/clsn/etc/ <span style="font-family: '微软雅黑',sans-serif;">目录中。</span>
</p>

### <span id="156_SSH_chroot">1.5.6 <span style="font-family: '微软雅黑',sans-serif;">配置</span> SSH <span style="font-family: '微软雅黑',sans-serif;">来使用</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱</span></span>

1<span style="font-family: '微软雅黑',sans-serif;">、配置</span>sshd<span style="font-family: '微软雅黑',sans-serif;">配置文件，</span>/etc/ssh/sshd_config

<div class="cnblogs_code">
  <pre>cat >>/etc/ssh/sshd_config &lt;&lt;<span style="color: #800000;">'</span><span style="color: #800000;">EOF</span><span style="color: #800000;">'</span><span style="color: #000000;">
Match User clsn
ChrootDirectory </span>/opt/<span style="color: #000000;">clsn
EOF</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置参数说明</span>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 定义要使用 chroot 监狱的用户</span>
<span style="color: #000000;">Match User clsn
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 指定 chroot 监狱</span>
ChrootDirectory /opt/clsn</pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、重新启动</span>sshd<span style="font-family: '微软雅黑',sans-serif;">服务</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> /etc/init.d/sshd restart </span>
<span style="color: #000000;">Stopping sshd:                                             [  OK  ]
Starting sshd:                                             [  OK  ]</span></pre>
</div>

### <span id="157_SSH_chroot">1.5.7 <span style="font-family: '微软雅黑',sans-serif;">测试</span> SSH <span style="font-family: '微软雅黑',sans-serif;">的</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱</span></span>

<div class="cnblogs_code">
  <pre>[C:\]$ ssh clsn@10.0.0.188<span style="color: #000000;">
Connecting to </span>10.0.0.188:22<span style="color: #000000;">...
Connection established.
To escape to local shell, press </span><span style="color: #800000;">'</span><span style="color: #800000;">Ctrl+Alt+]</span><span style="color: #800000;">'</span><span style="color: #000000;">.
Last login: Wed Jan </span>31 13:49:31 2018 <span style="color: #0000ff;">from</span><span style="color: #000000;"> mirrors.aliyuncs.com
</span>-bash-4.1<span style="color: #000000;">$
</span>-bash-4.1<span style="color: #000000;">$ ls
</span>-bash: ls: command <span style="color: #0000ff;">not</span><span style="color: #000000;"> found
</span>-bash-4.1<span style="color: #000000;">$ date
</span>-bash: date: command <span style="color: #0000ff;">not</span> found</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">从上面可以看出</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户被锁定在了</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱中，并且不能使用任何外部命令如（</span>ls<span style="font-family: '微软雅黑',sans-serif;">、</span>date<span style="font-family: '微软雅黑',sans-serif;">等等）。</span>
</p>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">用户只可以执行</span> bash <span style="font-family: '微软雅黑',sans-serif;">以及它内置的命令（比如：</span>pwd<span style="font-family: '微软雅黑',sans-serif;">、</span>history<span style="font-family: '微软雅黑',sans-serif;">、</span>echo <span style="font-family: '微软雅黑',sans-serif;">等等）：</span>

<div class="cnblogs_code">
  <pre>[C:\]$ ssh clsn@10.0.0.188<span style="color: #000000;">
Connecting to </span>10.0.0.188:22<span style="color: #000000;">...
Connection established.
To escape to local shell, press </span><span style="color: #800000;">'</span><span style="color: #800000;">Ctrl+Alt+]</span><span style="color: #800000;">'</span><span style="color: #000000;">.
Last login: Wed Jan </span>31 13:49:31 2018 <span style="color: #0000ff;">from</span><span style="color: #000000;"> mirrors.aliyuncs.com
</span>-bash-4.1<span style="color: #000000;">$
</span>-bash-4.1<span style="color: #000000;">$ ls
</span>-bash: ls: command <span style="color: #0000ff;">not</span><span style="color: #000000;"> found
</span>-bash-4.1<span style="color: #000000;">$ date
</span>-bash: date: command <span style="color: #0000ff;">not</span> found</pre>
</div>

### <span id="158_Linux">1.5.8 <span style="font-family: '微软雅黑',sans-serif;">创建用户的主目录并添加</span> Linux <span style="font-family: '微软雅黑',sans-serif;">命令</span></span>

<p style="text-indent: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">、从前面的测试中，我们可以看到用户被锁定在了<strong><span style="text-decoration: underline;">根目录</span></strong>，我们可以为</span> SSH <span style="font-family: '微软雅黑',sans-serif;">用户创建一个主目录（为所有将来的用户可以这么做）</span>
</p>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /opt/clsn/home/clsn</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> chown -R clsn.clsn /opt/clsn/home/clsn</span>
[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> chmod -R 0700 /opt/clsn/home/clsn</span></pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、将在</span> bin <span style="font-family: '微软雅黑',sans-serif;">目录中复制进几个用户命令，如</span> ls<span style="font-family: '微软雅黑',sans-serif;">、</span>date<span style="font-family: '微软雅黑',sans-serif;">、</span>mkdir

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> cp -v /bin/ls /opt/clsn/bin/</span>
`/bin/ls<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/bin/ls</span><span style="color: #800000;">'</span><span style="color: #000000;">
[root@clsn dev]</span><span style="color: #008000;">#</span><span style="color: #008000;"> cp -v /bin/date /opt/clsn/bin/</span>
`/bin/date<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/bin/date</span><span style="color: #800000;">'</span><span style="color: #000000;">
[root@clsn dev]</span><span style="color: #008000;">#</span><span style="color: #008000;"> cp -v /bin/mkdir /opt/clsn/bin/</span>
`/bin/mkdir<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/bin/mkdir</span><span style="color: #800000;">'</span></pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">、接下来，检查上面命令的共享库并将它们移到</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱的库目录中</span>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">查看</span>ls<span style="font-family: '微软雅黑',sans-serif;">命令共享库</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> ldd /bin/ls</span>
    linux-vdso.so.1 =>  (0x00007ffe47abf000<span style="color: #000000;">)
    libselinux.so.</span>1 => /lib64/libselinux.so.1 (0x0000003600600000<span style="color: #000000;">)
    librt.so.</span>1 => /lib64/librt.so.1 (0x00000035ffa00000<span style="color: #000000;">)
    libcap.so.</span>2 => /lib64/libcap.so.2 (0x0000003605200000<span style="color: #000000;">)
    libacl.so.</span>1 => /lib64/libacl.so.1 (0x0000003604600000<span style="color: #000000;">)
    libc.so.</span>6 => /lib64/libc.so.6 (0x00000035ff200000<span style="color: #000000;">)
    libdl.so.</span>2 => /lib64/libdl.so.2 (0x00000035fee00000<span style="color: #000000;">)
    </span>/lib64/ld-linux-x86-64.so.2 (0x00000035fea00000<span style="color: #000000;">)
    libpthread.so.0 </span>=> /lib64/libpthread.so.0 (0x00000035ff600000<span style="color: #000000;">)
    libattr.so.</span>1 => /lib64/libattr.so.1 (0x0000003602e00000)</pre>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">复制共享库文件</span>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> \cp -v /lib64/{libselinux.so.1,libcap.so.2,libacl.so.1,libc.so.6,libdl.so.2,ld-linux-x86-64.so.2,libattr.so.1,libpthread.so.0,librt.so.1} /opt/clsn/lib64/ </span>
`/lib64/libselinux.so.1<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libselinux.so.1</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/libcap.so.2<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libcap.so.2</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/libacl.so.1<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libacl.so.1</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/libc.so.6<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libc.so.6</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/libdl.so.2<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libdl.so.2</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/ld-linux-x86-64.so.2<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/ld-linux-x86-64.so.2</span><span style="color: #800000;">'</span><span style="color: #000000;">
cp: cannot create regular file `</span>/opt/clsn/lib64/ld-linux-x86-64.so.2<span style="color: #800000;">'</span><span style="color: #800000;">: Text file busy</span>
`/lib64/libattr.so.1<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libattr.so.1</span><span style="color: #800000;">'</span><span style="color: #000000;">
`</span>/lib64/libpthread.so.0<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/lib64/libpthread.so.0</span><span style="color: #800000;">'</span></pre>
</div>

### <span id="159_sftp__chroot">1.5.9 <span style="font-family: '微软雅黑',sans-serif;">测试</span> sftp <span style="font-family: '微软雅黑',sans-serif;">的</span> <span style="font-family: '微软雅黑',sans-serif;">用</span> chroot <span style="font-family: '微软雅黑',sans-serif;">监狱</span></span>

<p style="text-indent: 7.1pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">、用</span> sftp <span style="font-family: '微软雅黑',sans-serif;">做一个测试；测试你先前安装的命令是否可用。</span>
</p>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">在</span> /etc/ssh/sshd_config <span style="font-family: '微软雅黑',sans-serif;">中添加下面的行</span>

<div class="cnblogs_code">
  <p>
    echo 'ForceCommand internal-sftp'&nbsp; >> /etc/ssh/sshd_config
  </p>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: 微软雅黑, sans-serif; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">重启 sshd 服务</span>
</p>

<div class="cnblogs_code">
  <pre>[root@clsn dev]<span style="color: #008000;">#</span><span style="color: #008000;"> /etc/init.d/sshd restart </span>
<span style="color: #000000;">Stopping sshd:                                             [  OK  ]
Starting sshd:                                             [  OK  ]</span></pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、现在使用</span> ssh <span style="font-family: '微软雅黑',sans-serif;">测试，你会得到下面的错误：</span>

<div class="cnblogs_code">
  <pre>[root@lx ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ssh clsn@10.0.0.188</span>
clsn@10.0.0.188<span style="color: #800000;">'</span><span style="color: #800000;">s password: </span>
<span style="color: #000000;">This service allows sftp connections only.
Connection to </span>10.0.0.188 closed.</pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">、试下使用</span> sftp

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">复制</span>sftp<span style="font-family: '微软雅黑',sans-serif;">程序</span>

<div class="cnblogs_code">
  <pre>[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cp -v  /usr/libexec/openssh/sftp-server /opt/clsn/usr/libexec/openssh/ </span>
`/usr/libexec/openssh/sftp-server<span style="color: #800000;">'</span><span style="color: #800000;"> -> `/opt/clsn/usr/libexec/openssh/sftp-server</span><span style="color: #800000;">'</span></pre>
</div>

&nbsp;&nbsp; sftp<span style="font-family: '微软雅黑',sans-serif;">传输文件测试</span>

<div class="cnblogs_code">
  <pre>[root@lx ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sftp clsn@10.0.0.188</span>
clsn@10.0.0.188<span style="color: #800000;">'</span><span style="color: #800000;">s password: </span>
Connected to 10.0.0.188<span style="color: #000000;">.
sftp</span>><span style="color: #000000;"> pwd
Remote working directory: </span>/home/<span style="color: #000000;">clsn
sftp</span>> mkdir -<span style="color: #000000;">p nmtui
sftp</span>> cd nmtui/<span style="color: #000000;">
sftp</span>> put  anaconda-<span style="color: #000000;">ks.cfg  
Uploading anaconda</span>-ks.cfg to /home/clsn/nmtui/anaconda-<span style="color: #000000;">ks.cfg
anaconda</span>-ks.cfg                                   100% 1207     1.2KB/s   00:00</pre>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">到远端进行查看</span>

<div class="cnblogs_code">
  <pre>[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /opt/clsn/home/clsn/nmtui/</span>
total 4
-rw------- 1 clsn clsn 1207 Jan 31 14:21 anaconda-ks.cfg</pre>
</div>

&nbsp;&nbsp; **<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red; background: yellow;">至此</span>****<span style="font-family: '微软雅黑',sans-serif; color: red; background: yellow;">chroot </span>****<span style="font-family: '微软雅黑',sans-serif; color: red; background: yellow;">监狱就配置完成</span>**

## <span id="16">1.6 <span style="font-family: '微软雅黑',sans-serif;">参考文献</span></span>

> &nbsp;[1]&nbsp;[https://linuxcontainers.org][1]
> 
> [2]&nbsp;<https://zh.wikipedia.org/wiki/LXC>
> 
> [3]&nbsp;<https://www.ibm.com/developerworks/cn/linux/l-lxc-containers/>
> 
> [4]&nbsp;<https://www.tecmint.com/restrict-ssh-user-to-directory-using-chrooted-jail/>
> 
> [5]&nbsp;<https://linux.cn/article-8313-1.html>
> 
> [6]&nbsp;[http://blog.csdn.net/napolunyishi/article/details/21078799][2]

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11_LXC">1.1 LXC是什么？</a><ul>
        <li>
          <a href="#111_LXC">1.1.1 关于LXC</a>
        </li>
        <li>
          <a href="#112">1.1.2 关于操作系统层虚拟化</a>
        </li>
        <li>
          <a href="#113_LXC">1.1.3 LXC的特点</a>
        </li>
        <li>
          <a href="#114_LXC">1.1.4 LXC的应用</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_LXC">1.2 安装LXC</a><ul>
        <li>
          <a href="#121">1.2.1 环境说明</a>
        </li>
        <li>
          <a href="#122_LXC">1.2.2 安装LXC</a>
        </li>
        <li>
          <a href="#123">1.2.3 修改网卡配置</a>
        </li>
        <li>
          <a href="#124">1.2.4 启动服务</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_LXC">1.3 启动第一个LXC容器</a><ul>
        <li>
          <a href="#131">1.3.1 查看你当前容器列表</a>
        </li>
        <li>
          <a href="#132">1.3.2 修改容器的密码</a>
        </li>
        <li>
          <a href="#133">1.3.3 启动容器</a>
        </li>
        <li>
          <a href="#134">1.3.4 启动容器的其他方法</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14_LXC">1.4 LXC管理操作</a>
    </li>
    <li>
      <a href="#15_chroot_SSH">1.5 使用 chroot 监狱限制 SSH 用户访问指定目录</a><ul>
        <li>
          <a href="#151">1.5.1 当前网络威胁</a>
        </li>
        <li>
          <a href="#152_chroot">1.5.2 chroot做什么</a>
        </li>
        <li>
          <a href="#153_SSH_chroot">1.5.3 创建 SSH chroot 监狱</a>
        </li>
        <li>
          <a href="#154_SSH_chroot_shell">1.5.4 为 SSH chroot 监狱设置交互式 shell</a>
        </li>
        <li>
          <a href="#155_SSH">1.5.5 创建并配置 SSH 用户</a>
        </li>
        <li>
          <a href="#156_SSH_chroot">1.5.6 配置 SSH 来使用 chroot 监狱</a>
        </li>
        <li>
          <a href="#157_SSH_chroot">1.5.7 测试 SSH 的 chroot 监狱</a>
        </li>
        <li>
          <a href="#158_Linux">1.5.8 创建用户的主目录并添加 Linux 命令</a>
        </li>
        <li>
          <a href="#159_sftp__chroot">1.5.9 测试 sftp 的 用 chroot 监狱</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#16">1.6 参考文献</a>
    </li>
  </ul>
</div>

 [1]: https://linuxcontainers.org/
 [2]: /wp-content/themes/clsn-003/inc/go.php?url=http://blog.csdn.net/napolunyishi/article/details/21078799