---
title: kickstart模式实现批量安装centos7.x系统
author: 惨绿少年
type: post
date: 2017-11-14T00:44:00+00:00
url: /clsn/lx797.html
Baidusubmit:
  - 1
views:
  - 156
categories:
  - Linux运维
  - 玩转Linux
  - 自动化
  - 运维基本功

---
## <span id="11">1.1 安装系统的方法</span>

　　l&nbsp; 光盘（ISO文件，光盘的镜像文件）===>>每一台物理机都得给一个光驱，如果用外置光驱的话，是不是每台机器都需要插一下

　　l&nbsp; U盘：ISO镜像刻录到U盘==>>需要每台机器都需要插一下

　　l&nbsp; 并行安装==>>网络安装

　　l&nbsp; 自动化安装

## <span id="12_linux">1.2 linux下批量安装系统</span>

kickstart是RedHat公司开源的软件，所以对CentOS兼容性最好。

**原理：**

　　我们将手动安装的所有的详细步骤记录到一个文件中，然后kickstart通过读取这个文件就可以实现自动化安装系统。

　　kickstart是一个项目的名称。没有这个软件。使用者水平是高中以上

　　cobbler是对kickstart的所有组件的封装。使用者水平是初中以上。本质上就是网页版本的kickstart。

### <span id="121_PXE">1.2.1 PXE说明</span>

　　PXE，全名Pre-boot Execution Environment，预启动执行环境；

　　通过网络接口启动计算机，不依赖本地存储设备（如硬盘）或本地已安装的操作系统；

　　由Intel和Systemsoft公司于1999年9月20日公布的技术；

　　客户端/Server的工作模式；

　　PXE客户端会调用网际协议(IP)、用户数据报协议(UDP)、动态主机设定协议(DHCP)、小型文件传输协议(TFTP)等网络协议；

　　PXE客户端(客户端)这个术语是指机器在PXE启动过程中的角色。一个PXE客户端可以是一台服务器、笔记本电脑或者其他装有PXE启动代码的机器（我们电脑的网卡）

### <span id="122_kickstart">1.2.2 kickstart原理</span>

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163420781-1228474973.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />&nbsp;
</p>

### <span id="123_PXE">1.2.3 PXE请求顺序说明</span>

**①&nbsp;** **PXE** **客户端发送UDP广播请求** 

　　PXE 客户端从自己的PXE网卡启动，通过PXE BootROM(自启动芯片)会以UDP(简单用户数据报协议)发送一个广播请求，向本网络中的DHCP服务器索取IP。

**②&nbsp;** **DHCP****服务器提供信息** 

　　DHCP服务器收到客户端的请求，验证是否来至合法的PXE 客户端的请求，验证通过它将给客户端一个&ldquo;提供&rdquo;响应，这个&ldquo;提供&rdquo;响应中包含了为客户端分配的IP地址、pxelinux启动程序(TFTP)位置，以及配置文件所在位置。

**③&nbsp;** **PXE****客户端请求下载启动文件** 

　　客户端收到服务器的&ldquo;回应&rdquo;后，会回应一个帧，以请求传送启动所需文件。这些启动文件包括：pxelinux.0、pxelinux.cfg/default、vmlinuz、initrd.img等文件。

**④&nbsp;** **TETP****服务器响应客户端请求并传送文件** 

　　当服务器收到客户端的请求后，他们之间之后将有更多的信息在客户端与服务器之间作应答, 用以决定启动参数。BootROM由TFTP通讯协议从tftp服务器 下载启动安装程序所必须的文件(pxelinux.0、pxelinux.cfg/default)。default文件下载完成后，会根据该文件中定义的引导顺序，启动Linux安装程序的引导内核。

**⑤&nbsp;** **请求下载自动应答文件** 

　　客户端通过pxelinux.cfg/default文件成功的引导Linux安装内核后，安装程序首先必须确定你通过什么安装介质来安装linux，如果是通过网络安装(NFS, FTP, HTTP)，则会在这个时候初始化网络，并定位安装源位置。接着会读取default文件中指定的自动应答文件ks.cfg所在位置，根据该位置请求下载该文件。

**⑥&nbsp;** **客户端安装操作系统** 

　　将ks.cfg文件下载回来后，通过该文件找到http镜像，并按照该文件的配置请求下载安装过程需要的软件包。

　　http镜像和客户端建立连接后，将开始传输软件包，客户端将开始安装操作系统。

<div>
  <p class="a1">
    　　安装完成后，将提示重新引导计算机。
  </p>
</div>

## <span id="13_kickstart">1.3 kickstart批量安装系统实践</span>

　　一般批量安装操作系统最好**一次安装****23****台机器最佳**，主要因为大部分交换机为24口，安装23台服务器没有太大的压力

### <span id="131">1.3.1 环境说明</span>

**<span style="text-decoration: underline;">防火墙与selinux</span>****<span style="text-decoration: underline;">必须关闭。</span>**

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~]# <span style="color: #0000ff;">cat</span> /etc/redhat-<span style="color: #000000;">release
CentOS Linux release </span><span style="color: #800080;">7.4</span>.<span style="color: #800080;">1708</span><span style="color: #000000;"> (Core)

[root@kickstart </span>~]# <span style="color: #0000ff;">uname</span> -<span style="color: #000000;">r
</span><span style="color: #800080;">3.10</span>.<span style="color: #800080;"></span>-<span style="color: #800080;">693</span><span style="color: #000000;">.el7.x86_64

[root@kickstart </span>~<span style="color: #000000;">]# getenforce
Disabled

[root@kickstart </span>~<span style="color: #000000;">]# systemctl status firewalld.service
● firewalld.service </span>- firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: </span><span style="color: #0000ff;">man</span>:firewalld(<span style="color: #800080;">1</span><span style="color: #000000;">)

[root@kickstart </span>~]# <span style="color: #0000ff;">hostname</span> -<span style="color: #000000;">I
</span><span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.201</span> <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.201</span></pre>
  </div>
</div>

### <span id="132_dhcp">1.3.2 第一个里程碑：安装dhcp服务</span>

_<span style="text-decoration: underline;">安装</span>_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> dhcp -y</pre>
  </div>
</div>

_<span style="text-decoration: underline;">配置DHCP</span>__<span style="text-decoration: underline;">服务</span>_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">cat</span> >>/etc/dhcp/dhcpd.conf&lt;&lt;<span style="color: #000000;">EOF
subnet </span><span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.0</span> netmask <span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span><span style="color: #000000;"> {
range </span><span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.100</span> <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.199</span><span style="color: #000000;">;
option subnet</span>-mask <span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span><span style="color: #000000;">;
default</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">21600</span><span style="color: #000000;">;
max</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">43200</span><span style="color: #000000;">;
next</span>-server <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.201</span><span style="color: #000000;">;
filename </span><span style="color: #800000;">"</span><span style="color: #800000;">/pxelinux.0</span><span style="color: #800000;">"</span><span style="color: #000000;">;
}
EOF</span></pre>
  </div>
</div>

**_<span style="text-decoration: underline;">配置文件说明：</span>_**

<div>
  <div class="cnblogs_code">
    <pre>range <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.100</span> <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.199</span>;     # 可分配的起始IP-<span style="color: #000000;">结束IP
option subnet</span>-mask <span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span><span style="color: #000000;">;    # 设定netmask
default</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">21600</span><span style="color: #000000;">;            # 设置默认的IP租用期限
max</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">43200</span><span style="color: #000000;">;                # 设置最大的IP租用期限
next</span>-server <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.201</span><span style="color: #000000;">;            # 告知客户端TFTP服务器的ip
filename </span><span style="color: #800000;">"</span><span style="color: #800000;">/pxelinux.0</span><span style="color: #800000;">"</span>;              # 告知客户端从TFTP根目录下载pxelinux.0文件</pre>
  </div>
</div>

_<span style="text-decoration: underline;">启动DHCP</span>__<span style="text-decoration: underline;">服务</span>_

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~<span style="color: #000000;">]# systemctl start dhcpd.service
[root@kickstart </span>~<span style="color: #000000;">]# systemctl status dhcpd.service
● dhcpd.service </span>- DHCPv4 Server Daemon</pre>
  </div>
</div>

_<span style="text-decoration: underline;">检测端口信息</span>_

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~]# netstat -lntup |<span style="color: #0000ff;">grep</span><span style="color: #000000;"> dhcpd
udp        </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">43679</span>           <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*           <span style="color: #800080;">2090</span>/<span style="color: #000000;">dhcpd         
udp        </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">67</span>              <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*             <span style="color: #800080;">2090</span>/<span style="color: #000000;">dhcpd         
udp6       </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> :::<span style="color: #800080;">58101</span>                :::*                     <span style="color: #800080;">2090</span>/dhcpd    </pre>
  </div>
</div>

_<span style="text-decoration: underline;">查看其日志信息： </span>(__日志位置为/var/log/messages)_

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~]# tailf /var/log/<span style="color: #000000;">messages
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd: Listening on PF/eth1/<span style="color: #800080;">00</span>:0c:<span style="color: #800080;">29</span>:a8:<span style="color: #800080;">59</span>:b2/<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.0</span>/<span style="color: #800080;">24</span><span style="color: #000000;">
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd: Sending on   LPF/eth1/<span style="color: #800080;">00</span>:0c:<span style="color: #800080;">29</span>:a8:<span style="color: #800080;">59</span>:b2/<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.0</span>/<span style="color: #800080;">24</span><span style="color: #000000;">
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span><span style="color: #000000;"> clsn dhcpd:
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd: No subnet declaration <span style="color: #0000ff;">for</span> eth0 (<span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.201</span><span style="color: #000000;">).
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd: **<span style="color: #000000;"> Ignoring requests on eth0.  If this is not what
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd:   you want, please <span style="color: #0000ff;">write</span><span style="color: #000000;"> a subnet declaration
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd:   <span style="color: #0000ff;">in</span> your dhcpd.conf <span style="color: #0000ff;">file</span> <span style="color: #0000ff;">for</span><span style="color: #000000;"> the network segment
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd:   to <span style="color: #0000ff;">which</span> interface eth0 is attached. **</pre>
  </div>
</div>

注：配置dhcp的时候是可以配置监听的网卡，指定监听网卡

<div>
  <p class="a3">
    DHCPDARGS=eth1&nbsp; # 指定监听网卡
  </p>
</div>

### <span id="133">1.3.3 创建一个新的空白虚拟机</span>

**说明：Centos7.3****之后安装系统至少需要2G****内存，低于2G****将无法安装**

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163630843-1334194351.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />&nbsp;
</p>

&nbsp;&nbsp; 开启虚拟机

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163641812-473530751.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />&nbsp;
</p>

日志输出

<div>
  <div class="cnblogs_code">
    <pre>Nov <span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span> clsn dhcpd: Sending on   Socket/fallback/fallback-<span style="color: #000000;">net
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">01</span>:<span style="color: #800080;">40</span><span style="color: #000000;"> clsn systemd: Started DHCPv4 Server Daemon.
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">12</span>:<span style="color: #800080;">16</span> clsn dhcpd: DHCPDISCOVER from <span style="color: #800080;">00</span>:0c:<span style="color: #800080;">29</span>:<span style="color: #800080;">64</span>:7b:<span style="color: #800080;">09</span><span style="color: #000000;"> via eth1
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">12</span>:<span style="color: #800080;">17</span> clsn dhcpd: DHCPOFFER on <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.120</span> to <span style="color: #800080;">00</span>:0c:<span style="color: #800080;">29</span>:<span style="color: #800080;">64</span>:7b:<span style="color: #800080;">09</span><span style="color: #000000;"> via eth1
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">12</span>:<span style="color: #800080;">18</span> clsn dhcpd: DHCPREQUEST <span style="color: #0000ff;">for</span> <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.120</span> (<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.201</span>) from <span style="color: #800080;">00</span>:0c:<span style="color: #800080;">29</span>:<span style="color: #800080;">64</span>:7b:<span style="color: #800080;">09</span><span style="color: #000000;"> via eth1
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">12</span>:<span style="color: #800080;">18</span> clsn dhcpd: DHCPACK on <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.120</span> to <span style="color: #800080;">00</span>:0c:<span style="color: #800080;">29</span>:<span style="color: #800080;">64</span>:7b:<span style="color: #800080;">09</span><span style="color: #000000;"> via eth1
Nov </span><span style="color: #800080;">13</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">12</span>:<span style="color: #800080;">54</span> clsn kernel: perf: interrupt took too <span style="color: #0000ff;">long</span> (<span style="color: #800080;">6342</span> > <span style="color: #800080;">6262</span>), lowering kernel.perf_event_max_sample_rate to <span style="color: #800080;">31000</span></pre>
  </div>
</div>

### <span id="134_tftp">1.3.4 安装tftp服务</span>

_<span style="text-decoration: underline;">安装tftp</span>__<span style="text-decoration: underline;">软件</span>_

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> tftp-server -y</pre>
  </div>
</div>

_<span style="text-decoration: underline;">启动服务</span>_

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~<span style="color: #000000;">]# systemctl start tftp.socket
[root@kickstart </span>~]# systemctl status tftp.socket</pre>
  </div>
</div>

**tftp****根目录**

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~]# cd /var/lib/tftpboot/</pre>
  </div>
</div>

&nbsp;&nbsp; 再次启动虚拟机

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163726374-1592633776.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />
</p>

启动发现又tftp但是没找到要加载的系统。上面报错是在TFTP服务的根目录找不到启动文件pxelinux.0

### <span id="135_pxelinux0">1.3.5 获取pxelinux.0系统</span>

通过安装软件获得

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart tftpboot]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> -y syslinux</pre>
  </div>
</div>

_<span style="text-decoration: underline;">复制pxelinux.0</span>__<span style="text-decoration: underline;">文件到tftp</span>__<span style="text-decoration: underline;">根目录</span>_

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart tftpboot]# <span style="color: #0000ff;">cp</span> /usr/share/syslinux/pxelinux.<span style="color: #800080;"></span>  /var/lib/tftpboot/<span style="color: #000000;">
[root@kickstart tftpboot]# </span><span style="color: #0000ff;">ls</span><span style="color: #000000;">
pxelinux.</span><span style="color: #800080;"></span>&nbsp;</pre>
  </div>
</div>

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163752359-867489723.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />
</p>

&nbsp;

<p align="center">
  &nbsp;
</p>

　　　　首先排除最简单故障原因：selinux是否关闭，防火墙是否关闭

　　　　上面的错误是因为pxelinux.0这个小系统的**配置文件（****default****）**不存在，或者文件名不对

### <span id="136_pxelinux0">1.3.6 添加pxelinux.0配置文件</span>

1）给虚拟机添加上centos7的镜像，注意实在kickstart服务端添加

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163817327-892449848.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />&nbsp;
</p>

2）将镜像挂载上

**特别说明：**由于这是测试环境可以使用挂载，在生产环境中必须把镜像中的文件复制到硬盘上，光驱速度太慢。

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">mkdir</span> -p /var/www/html/<span style="color: #000000;">CentOS7
</span><span style="color: #0000ff;">mount</span> /dev/cdrom /var/www/html/CentOS7</pre>
  </div>
</div>

&nbsp;&nbsp; 3）将镜像中的相关文件复制到tftp根目录

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">cp</span> -a /var/www/html/CentOS7/isolinux<span style="color: #008000;">/*</span><span style="color: #008000;"> /var/lib/tftpboot/
mkdir -p /var/lib/tftpboot/pxelinux.cfg
cp /var/www/html/CentOS7/isolinux/isolinux.cfg /var/lib/tftpboot/pxelinux.cfg/default</span></pre>
  </div>
</div>

4）修改default配置文件实现通过网络安装操作系统

**CentOS7.X** **网络安装的关键点,****修改default****文件**

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163830843-742934555.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />&nbsp;
</p>

&nbsp;&nbsp; 修改完成后，重启pxe客户端，就会有安装界面，与使用光盘安装一致，这里就不是做详细的记录了。注意实现网路安装需要将下一步完成_<span style="text-decoration: underline;">(</span>__<span style="text-decoration: underline;">配置http</span>__<span style="text-decoration: underline;">服务</span>_)。

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114163902968-1255566686.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />
</p>

5）CentOS7实现自动化安装的default文件

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart ~]# <span style="color: #0000ff;">cat</span> /var/lib/tftpboot/pxelinux.cfg/<span style="color: #000000;">default
default ks
prompt </span><span style="color: #800080;"></span><span style="color: #000000;">

label ks
  kernel vmlinuz
  append initrd</span>=initrd.img ks=http:<span style="color: #008000;">//</span><span style="color: #008000;">172.16.1.201/ks_config/CentOS7-ks.cfg net.ifnames=0 biosdevname=0 ksdevice=eth1</span></pre>
  </div>
</div>

### <span id="137_httpd">1.3.7 配置httpd服务</span>

_<span style="text-decoration: underline;">安装apache</span>__<span style="text-decoration: underline;">服务</span>_

<div>
  <div class="cnblogs_code">
    <pre>[root@kickstart tftpboot]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> httpd -y</pre>
  </div>
</div>

_<span style="text-decoration: underline;">启动apache</span>__<span style="text-decoration: underline;">服务</span>_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">[root@kickstart tftpboot]# systemctl start httpd.service
[root@kickstart tftpboot]# systemctl status httpd.service
● httpd.service </span>- The Apache HTTP Server</pre>
  </div>
</div>

_<span style="text-decoration: underline;">通过浏览器访问，使用curl</span>__<span style="text-decoration: underline;">命令镜像访问。</span>_

<div>
  <div class="cnblogs_code">
    <pre>http:<span style="color: #008000;">//</span><span style="color: #008000;">10.0.0.201/CentOS7/</span>
curl http:<span style="color: #008000;">//</span><span style="color: #008000;">172.16.1.201/CentOS7/</span></pre>
  </div>
</div>

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114164017484-900195888.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />&nbsp;
</p>

## <span id="14_ks">1.4 ks文件说明</span>

### <span id="141_ks">1.4.1 ks文件的作用</span>

　　　通常，我们在安装操作系统的过程中，需要大量的和服务器交互操作，为了减少这个交互过程，kickstart就诞生了。使用这种kickstart，只需事先定义好一个Kickstart自动应答配置文件ks.cfg（通常存放在安装服务器上），并让安装程序知道该配置文件的位置，在安装过程中安装程序就可以自己从该文件中读取安装配置，这样就避免了在安装过程中多次的人机交互，从而实现无人值守的自动化安装。

### <span id="142_ks">1.4.2 ks文件生成的三种方式</span>

**方法1** 

　　每安装好一台Centos机器，Centos安装程序都会创建一个kickstart配置文件，记录你的真实安装配置。如果你希望实现和某系统类似的安装，可以基于该系统的kickstart配置文件来生成你自己的kickstart配置文件。（生成的文件名字叫anaconda-ks.cfg位于/root/anaconda-ks.cfg）　　

**方法2** 

　　Centos提供了一个图形化的kickstart配置工具。在任何一个安装好的Linux系统上运行该工具，就可以很容易地创建你自己的kickstart配置文件。kickstart配置工具命令为redhat-config-kickstart（RHEL3）或system-config-kickstart（RHEL4，RHEL5）.网上有很多用CentOS桌面版生成ks文件的文章，如果有现成的系统就没什么可说。但没有现成的，也没有必要去用桌面版，命令行也很简单。

**方法3**

　　阅读kickstart配置文件的手册。用任何一个文本编辑器都可以创建你自己的kickstart配置文件。

### <span id="143_centos74ks">1.4.3 centos7.4自动应答文件(ks)</span>

<div>
  <div class="cnblogs_code">
    <pre># Kickstart Configurator <span style="color: #0000ff;">for</span> CentOS <span style="color: #800080;">7</span><span style="color: #000000;"> by yao zhang #命令段
</span><span style="color: #0000ff;">install</span><span style="color: #000000;">   #告知安装程序，这是一次全新安装，而不是升级
url </span>--url=<span style="color: #800000;">"</span><span style="color: #800000;">http://172.16.1.201/CentOS7/</span><span style="color: #800000;">"</span><span style="color: #000000;">  #通过http下载安装镜像
text     #以文本格式安装
lang en_US.UTF</span>-<span style="color: #800080;">8</span><span style="color: #000000;">   #设置字符集格式
keyboard us  #设置键盘类型
zerombr   #清除mbr引导
bootloader </span>--location=mbr --driveorder=sda --append=<span style="color: #800000;">"</span><span style="color: #800000;">crashkernel=auto rhgb quiet</span><span style="color: #800000;">"</span><span style="color: #000000;">    #指定引导记录被写入的位置
network  </span>--bootproto=static --device=eth0 --gateway=<span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.254</span> --ip=<span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.202</span> --nameserver=<span style="color: #800080;">223.5</span>.<span style="color: #800080;">5.5</span> --netmask=<span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span> --<span style="color: #000000;">activate  #配置eth0网卡
network  </span>--bootproto=static --device=eth1 --ip=<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.202</span> --netmask=<span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span> --<span style="color: #000000;">activate   #配置eth1网卡
network  </span>--<span style="color: #0000ff;">hostname</span>=<span style="color: #000000;">Cobbler  #设置主机名
#network </span>--bootproto=dhcp --device=eth1 --onboot=yes --noipv6 --<span style="color: #0000ff;">hostname</span>=<span style="color: #000000;">CentOS7
timezone </span>--utc Asia/<span style="color: #000000;">Shanghai  可以使用dhcp方式设置网络
authconfig </span>--enableshadow --passalgo=<span style="color: #000000;">sha512  #设置密码格式
rootpw  </span>--iscrypted $<span style="color: #800080;">6</span>$X20eRtuZhkHznTb4$dK0BJByOSAWSDD8jccLVFz0CscijS9ldMWwpoCw/ZEjYw2BTQYGWlgKsn945fFTjRC658UXjuocwJbAjVI5D6/<span style="color: #000000;">     #密文密码
clearpart </span>--all --<span style="color: #000000;">initlabel  #清空分区
part </span>/boot --fstype xfs --size <span style="color: #800080;">1024</span>   #/<span style="color: #000000;">boot分区
part swap </span>--size <span style="color: #800080;">1024</span><span style="color: #000000;">                    #swap分区
part </span>/ --fstype xfs --size <span style="color: #800080;">1</span> --grow   #/<span style="color: #000000;">分区
firstboot </span>--<span style="color: #000000;">disable       #负责协助配置redhat一些重要的信息
selinux </span>--<span style="color: #000000;">disabled        #关闭selinux
firewall </span>--<span style="color: #000000;">disabled       #关闭防火墙
logging </span>--level=<span style="color: #0000ff;">info</span><span style="color: #000000;">      #设置日志级别
reboot                       #安装完成重启

</span>%<span style="color: #000000;">packages #包组段   @表示包组
@</span>^<span style="color: #000000;">minimal
@compat</span>-<span style="color: #000000;">libraries
@debugging
@development
tree
nmap
sysstat
lrzsz
dos2unix
telnet
</span><span style="color: #0000ff;">wget</span><span style="color: #000000;">
vim
bash</span>-<span style="color: #000000;">completion
</span>%<span style="color: #000000;">end

</span>%<span style="color: #000000;">post #脚本段，可以放脚本或命令
systemctl disable postfix.service   #关闭邮件服务开机自启动
</span>%end</pre>
  </div>
</div>

### <span id="144_PXEdefault">1.4.4 PXE配置文件default</span>

由于多个客户端可以从一个PXE服务器引导，PXE引导映像使用了一个复杂的配置文件搜索方式来查找针对客户机的配置文件。如果客户机的网卡的MAC地址为8F：3H：AA：6B：CC：5D，对应的IP地址为10.0.0.195，那么客户机首先尝试以MAC地址为文件名匹配的配置文件，如果不存在就以IP地址来查找。根据上述环境针对这台主机要查找的以一个配置文件就是 /tftpboot/pxelinux.cfg/01-8F：3H：AA：6B：CC：5D。如果该文件不存在，就会根据IP地址来查找配置文件了，这个算法更复杂些，PXE映像查找会根据IP地址16进制命名的客户机配置文件。例如：10.0.0.195对应的16进制的形式为C0A801C3。（可以通过syslinux软件包提供的gethostip命令将10进制的IP转换为16进制）

如果C0A801C3文件不存在，就尝试查找C0A801C文件，如果C0A801C也不存在，那么就尝试C0A801文件，依次类推，直到查找C文件，如果C也不存在的话，那么最后尝试default文件。

总体来说，pxelinux搜索的文件的顺序是：

<div>
  <div class="cnblogs_code">
    <pre>/tftpboot/pxelinux.cfg/<span style="color: #800080;">01</span>-<span style="color: #800080;">88</span>-<span style="color: #800080;">99</span>-aa-bb-<span style="color: #0000ff;">cc</span>-<span style="color: #0000ff;">dd</span>
/tftpboot/pxelinux.cfg/<span style="color: #000000;">C0A801C3
</span>/tftpboot/pxelinux.cfg/<span style="color: #000000;">C0A801C
</span>/tftpboot/pxelinux.cfg/<span style="color: #000000;">C0A801
</span>/tftpboot/pxelinux.cfg/<span style="color: #000000;">C0A80
</span>/tftpboot/pxelinux.cfg/<span style="color: #000000;">C0A8
</span>/tftpboot/pxelinux.cfg/<span style="color: #000000;">C0A
</span>/tftpboot/pxelinux.cfg/<span style="color: #000000;">C0
</span>/tftpboot/pxelinux.cfg/<span style="color: #000000;">C
</span>/tftpboot/pxelinux.cfg/default</pre>
  </div>
</div>

### <span id="145">1.4.5 上面的事情做完以后就可以批量安装系统了</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@Cobbler ~]# <span style="color: #0000ff;">df</span> -<span style="color: #000000;">h
Filesystem      Size  Used Avail Use</span>%<span style="color: #000000;"> Mounted on
</span>/dev/sda3        98G  <span style="color: #800080;">1</span>.5G   97G   <span style="color: #800080;">2</span>% /<span style="color: #000000;">
devtmpfs        227M     </span><span style="color: #800080;"></span>  227M   <span style="color: #800080;"></span>% /<span style="color: #000000;">dev
tmpfs           237M     </span><span style="color: #800080;"></span>  237M   <span style="color: #800080;"></span>% /dev/<span style="color: #000000;">shm
tmpfs           237M  </span><span style="color: #800080;">4.6M</span>  232M   <span style="color: #800080;">2</span>% /<span style="color: #000000;">run
tmpfs           237M     </span><span style="color: #800080;"></span>  237M   <span style="color: #800080;"></span>% /sys/fs/<span style="color: #000000;">cgroup
</span>/dev/sda1      1014M  135M  880M  <span style="color: #800080;">14</span>% /<span style="color: #000000;">boot
tmpfs            48M     </span><span style="color: #800080;"></span>   48M   <span style="color: #800080;"></span>% /run/user/<span style="color: #800080;"></span><span style="color: #000000;">

[root@Cobbler </span>~<span style="color: #000000;">]# systemctl status firewalld.service
● firewalld.service </span>- firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: </span><span style="color: #0000ff;">man</span>:firewalld(<span style="color: #800080;">1</span><span style="color: #000000;">)

[root@Cobbler </span>~<span style="color: #000000;">]# getenforce
Disabled

[root@Cobbler </span>~]# <span style="color: #0000ff;">hostname</span> -<span style="color: #000000;">I
</span><span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.202</span> <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.202</span></pre>
  </div>
</div>

## <span id="15">1.5 常见错误</span>

### <span id="151">1.5.1 网卡无法启动解决</span>

解决办法：

&nbsp;&nbsp; 将NetworkManager服务停止，再重新启动network服务

<div>
  <div class="cnblogs_code">
    <pre>[root@CentOS7 ~<span style="color: #000000;">]# systemctl disable NetworkManager
Removed symlink </span>/etc/systemd/system/multi-user.target.wants/<span style="color: #000000;">NetworkManager.service.
Removed symlink </span>/etc/systemd/system/dbus-<span style="color: #000000;">org.freedesktop.NetworkManager.service.
Removed symlink </span>/etc/systemd/system/dbus-org.freedesktop.nm-<span style="color: #000000;">dispatcher.service.

[root@CentOS7 </span>~]# systemctl stop NetworkManager</pre>
  </div>
</div>

### <span id="152_Could_not_find_kernel_image">1.5.2 Could not find kernel image</span>

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171114164237827-576128101.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="kickstart模式实现批量安装centos7.x系统" alt="" />&nbsp;
</p>

<div>
  <p class="a1">
    报错原因：selinux没关
  </p>
</div>

### <span id="153">1.5.3 查考文档</span>

<div>
  <p class="a1">
    http://blog.oldboyedu.com/autoinstall-kickstart/
  </p>
  
  <p class="a1">
    http://www.zyops.com/autoinstall-kickstart
  </p>
</div>

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11">1.1 安装系统的方法</a>
    </li>
    <li>
      <a href="#12_linux">1.2 linux下批量安装系统</a><ul>
        <li>
          <a href="#121_PXE">1.2.1 PXE说明</a>
        </li>
        <li>
          <a href="#122_kickstart">1.2.2 kickstart原理</a>
        </li>
        <li>
          <a href="#123_PXE">1.2.3 PXE请求顺序说明</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_kickstart">1.3 kickstart批量安装系统实践</a><ul>
        <li>
          <a href="#131">1.3.1 环境说明</a>
        </li>
        <li>
          <a href="#132_dhcp">1.3.2 第一个里程碑：安装dhcp服务</a>
        </li>
        <li>
          <a href="#133">1.3.3 创建一个新的空白虚拟机</a>
        </li>
        <li>
          <a href="#134_tftp">1.3.4 安装tftp服务</a>
        </li>
        <li>
          <a href="#135_pxelinux0">1.3.5 获取pxelinux.0系统</a>
        </li>
        <li>
          <a href="#136_pxelinux0">1.3.6 添加pxelinux.0配置文件</a>
        </li>
        <li>
          <a href="#137_httpd">1.3.7 配置httpd服务</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14_ks">1.4 ks文件说明</a><ul>
        <li>
          <a href="#141_ks">1.4.1 ks文件的作用</a>
        </li>
        <li>
          <a href="#142_ks">1.4.2 ks文件生成的三种方式</a>
        </li>
        <li>
          <a href="#143_centos74ks">1.4.3 centos7.4自动应答文件(ks)</a>
        </li>
        <li>
          <a href="#144_PXEdefault">1.4.4 PXE配置文件default</a>
        </li>
        <li>
          <a href="#145">1.4.5 上面的事情做完以后就可以批量安装系统了</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15">1.5 常见错误</a><ul>
        <li>
          <a href="#151">1.5.1 网卡无法启动解决</a>
        </li>
        <li>
          <a href="#152_Could_not_find_kernel_image">1.5.2 Could not find kernel image</a>
        </li>
        <li>
          <a href="#153">1.5.3 查考文档</a>
        </li>
      </ul>
    </li>
  </ul>
</div>