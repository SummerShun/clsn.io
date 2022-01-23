---
title: KICKSTART无人值守安装
author: 惨绿少年
type: post
date: 2017-10-24T21:13:00+00:00
url: /clsn/lx1124.html
views:
  - 2
categories:
  - Linux运维
  - 玩转Linux
  - 自动化
  - 运维基本功

---
## <span id="11">1.1 环境说明</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# <span style="color: #0000ff;">cat</span> /etc/redhat-<span style="color: #000000;">release
CentOS release </span><span style="color: #800080;">6.9</span><span style="color: #000000;"> (Final)

[root@test </span>~]# <span style="color: #0000ff;">uname</span> -<span style="color: #000000;">r
</span><span style="color: #800080;">2.6</span>.<span style="color: #800080;">32</span>-<span style="color: #800080;">696</span><span style="color: #000000;">.el6.x86_64

[root@test </span>~<span style="color: #000000;">]# getenforce
Disabled

[root@test </span>~]# /etc/init.d/<span style="color: #000000;">iptables status
iptables: Firewall is not running.

[root@test </span>~]# <span style="color: #0000ff;">ifconfig</span> eth0|<span style="color: #0000ff;">awk</span> -F <span style="color: #800000;">"</span><span style="color: #800000;">[ :]+</span><span style="color: #800000;">"</span> <span style="color: #800000;">'</span><span style="color: #800000;">NR==2 {print $4}</span><span style="color: #800000;">'</span>
<span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.250</span><span style="color: #000000;">

[root@test </span>~]# <span style="color: #0000ff;">hostname</span><span style="color: #000000;">
test</span></pre>
  </div>
</div>

## <span id="12_DHCP">1.2 配置DHCP</span>

### <span id="121_dhcp">1.2.1 安装dhcp</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> -y <span style="color: #0000ff;">install</span><span style="color: #000000;"> dhcp
rpm </span>-ql dhcp |<span style="color: #0000ff;">grep</span> <span style="color: #800000;">"</span><span style="color: #800000;">dhcpd.conf</span><span style="color: #800000;">"</span></pre>
  </div>
</div>

### <span id="122">1.2.2 编写配置文件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# <span style="color: #0000ff;">cat</span> /etc/dhcp/<span style="color: #000000;">dhcpd.conf
#
# DHCP Server Configuration </span><span style="color: #0000ff;">file</span><span style="color: #000000;">.
#   see </span>/usr/share/doc/dhcp*/<span style="color: #000000;">dhcpd.conf.sample
#   see </span><span style="color: #800000;">'</span><span style="color: #800000;">man 5 dhcpd.conf</span><span style="color: #800000;">'</span><span style="color: #000000;">
#
subnet </span><span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.0</span> netmask <span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span><span style="color: #000000;"> {

        range </span><span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.100</span> <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.200</span><span style="color: #000000;">;

        option subnet</span>-mask <span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span><span style="color: #000000;">;

        default</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">21600</span><span style="color: #000000;">;

        max</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">43200</span><span style="color: #000000;">;

        next</span>-server <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.250</span><span style="color: #000000;">;

        filename </span><span style="color: #800000;">"</span><span style="color: #800000;">/pxelinux.0</span><span style="color: #800000;">"</span><span style="color: #000000;">;

}</span></pre>
  </div>
  
  <p>
    ----------------------------------------------------------------
  </p>
  
  <p class="a1">
    &nbsp;# 注释
  </p>
  
  <div class="cnblogs_code">
    <pre>range <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.100</span> <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.200</span>;         # 可分配的起始IP-<span style="color: #000000;">结束IP
option subnet</span>-mask <span style="color: #800080;">255.255</span>.<span style="color: #800080;">255.0</span><span style="color: #000000;">;    # 设定netmask
default</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">21600</span><span style="color: #000000;">;            # 设置默认的IP租用期限
max</span>-lease-<span style="color: #0000ff;">time</span> <span style="color: #800080;">43200</span><span style="color: #000000;">;                # 设置最大的IP租用期限
next</span>-server <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.250</span><span style="color: #000000;">;                # 告知客户端TFTP服务器的ip
filename </span><span style="color: #800000;">"</span><span style="color: #800000;">/pxelinux.0</span><span style="color: #800000;">"</span>;              # 告知客户端从TFTP根目录下载pxelinux.0文件</pre>
  </div>
</div>

### <span id="123">1.2.3 启动服务</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# /etc/init.d/<span style="color: #000000;">dhcpd start
Starting dhcpd:                                            [  OK  ]
[root@test </span>~]# netstat -tunlp|<span style="color: #0000ff;">grep</span><span style="color: #000000;"> dhcp
udp        </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">67</span>                  <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*                               <span style="color: #800080;">4578</span>/dhcpd   </pre>
  </div>
</div>

## <span id="13_TFTP">1.3 安装TFTP服务</span>

### <span id="131_tftp">1.3.1 安装tftp服务</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@linux-node1 ~]# <span style="color: #0000ff;">yum</span> -y <span style="color: #0000ff;">install</span> tftp-server</pre>
  </div>
</div>

### <span id="132_xindetd">1.3.2 编写xindetd下的配置文件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@linux-node1 ~]# vim /etc/xinetd.d/<span style="color: #000000;">tftp
# default: off
# description: The tftp server serves files using the trivial </span><span style="color: #0000ff;">file</span><span style="color: #000000;"> transfer \
#       protocol.  The tftp protocol is often used to boot diskless \
#       workstations, download configuration files to network</span>-<span style="color: #000000;">aware printers, \
#       and to start the installation process </span><span style="color: #0000ff;">for</span><span style="color: #000000;"> some operating systems.
service tftp
{
        socket_type             </span>=<span style="color: #000000;"> dgram
        protocol                </span>=<span style="color: #000000;"> udp
        </span><span style="color: #0000ff;">wait</span>                    =<span style="color: #000000;"> yes
        user                    </span>=<span style="color: #000000;"> root
        server                  </span>= /usr/sbin/<span style="color: #0000ff;">in</span><span style="color: #000000;">.tftpd
        server_args             </span>= -s /var/lib/<span style="color: #000000;">tftpboot # 指定目录，保持默认，不用修改
        disable                 </span>=<span style="color: #000000;"> no # 由原来的yes改为no
        per_source              </span>= <span style="color: #800080;">11</span><span style="color: #000000;">
        cps                     </span>= <span style="color: #800080;">100</span> <span style="color: #800080;">2</span><span style="color: #000000;">
        flags                   </span>=<span style="color: #000000;"> IPv4
}</span></pre>
  </div>
</div>

### <span id="133_xinetd">1.3.3 启动服务，让xinetd 管理</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@linux-node1 ~]# /etc/init.d/<span style="color: #000000;">xinetd restart
Stopping xinetd:                                           [FAILED]
Starting xinetd:                                           [  OK  ]</span></pre>
  </div>
</div>

### <span id="134">1.3.4 检查端口</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@linux-node1 ~]# netstat -tunlp|<span style="color: #0000ff;">grep</span> <span style="color: #800080;">69</span><span style="color: #000000;">
udp        </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">69</span>                  <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*                               <span style="color: #800080;">1106</span>/xinetd</pre>
  </div>
</div>

## <span id="14_HTTP">1.4 配置HTTP服务</span>

### <span id="141_nginxpcre-devel_openssl-devel">1.4.1 安装nginx的依赖包（pcre-devel openssl-devel）</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> -y pcre-devel openssl-devel</pre>
  </div>
  
  <p>
    <span style="font-size: 1.17em;">1.4.2 下载nginx软件</span>
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">wget</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">nginx.org/download/nginx-1.10.3.tar.gz</span></pre>
  </div>
  
  <p>
    解压软件
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">tar</span> xf nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>.<span style="color: #0000ff;">tar</span>.gz</pre>
  </div>
  
  <p>
    <span style="font-size: 1.17em;">1.4.3 创建管理用户 www</span>
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>useradd -M -s /sbin/nologin www</pre>
  </div>
</div>

### <span id="144_nbspnginx">1.4.4 &nbsp;nginx软件编译安装过程</span>

1、配置软件，在软件的解压目录中

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>]# ./configure --prefix=/application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span> --user=www --group=www --with-http_stub_status_module --with-http_ssl_module</pre>
  </div>
  
  <p>
    &nbsp; &nbsp;通过软件编译过程中的返回值是否正确，确认配置是否正确
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>]# <span style="color: #0000ff;">echo</span> $?
<span style="color: #800080;"></span></pre>
  </div>
  
  <p>
    &nbsp; &nbsp;2、编译软件
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>]# <span style="color: #0000ff;">make</span></pre>
  </div>
  
  <p>
    &nbsp; &nbsp;3、编译安装
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>]# <span style="color: #0000ff;">make</span> <span style="color: #0000ff;">install</span></pre>
  </div>
  
  <p>
    <span style="font-size: 1.17em;">1.4.5 创建软连接</span>
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# <span style="color: #0000ff;">ln</span> -s /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/ /application/nginx</pre>
  </div>
  
  <p>
    <span style="font-size: 1.17em;">1.4.6 修改nginx配置文件</span>
  </p>
</div>

添加一行配置，作用是显示目录里的所文件

<div>
  <div class="cnblogs_code">
    <pre>[root@test html]# vim ../conf/<span style="color: #000000;">nginx.conf
worker_processes  </span><span style="color: #800080;">1</span><span style="color: #000000;">;
events {
    worker_connections  </span><span style="color: #800080;">1024</span><span style="color: #000000;">;
}
http {
    include       mime.types;
    default_type  application</span>/octet-<span style="color: #000000;">stream;
    sendfile        on;
    keepalive_timeout  </span><span style="color: #800080;">65</span><span style="color: #000000;">;
    server {
        listen       </span><span style="color: #800080;">80</span><span style="color: #000000;">;
        server_name  localhost;
        location </span>/<span style="color: #000000;"> {
            autoindex on;
            root   html;
            index  index.html index.htm;
        }
        error_page   </span><span style="color: #800080;">500</span> <span style="color: #800080;">502</span> <span style="color: #800080;">503</span> <span style="color: #800080;">504</span>  /<span style="color: #000000;">50x.html;
        location </span>= /<span style="color: #000000;">50x.html {
            root   html;
        }
    }
}</span></pre>
  </div>
</div>

### <span id="147">1.4.7 启动程序</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# /application/nginx/sbin/<span style="color: #000000;">nginx
[root@web01 application]#</span></pre>
  </div>
  
  <p>
    检查是否启动
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# <span style="color: #0000ff;">ps</span> -ef |<span style="color: #0000ff;">grep</span><span style="color: #000000;"> nginx
root      </span><span style="color: #800080;">26548</span>      <span style="color: #800080;">1</span>  <span style="color: #800080;"></span> <span style="color: #800080;">20</span>:<span style="color: #800080;">13</span> ?        <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span> nginx: master process /application/nginx/sbin/<span style="color: #000000;">nginx
www       </span><span style="color: #800080;">26549</span>  <span style="color: #800080;">26548</span>  <span style="color: #800080;"></span> <span style="color: #800080;">20</span>:<span style="color: #800080;">13</span> ?        <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span><span style="color: #000000;"> nginx: worker process        
root      </span><span style="color: #800080;">26551</span>  <span style="color: #800080;">23431</span>  <span style="color: #800080;">3</span> <span style="color: #800080;">20</span>:<span style="color: #800080;">13</span> pts/<span style="color: #800080;"></span>    <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span> <span style="color: #0000ff;">grep</span> --color=auto nginx</pre>
  </div>
  
  <p>
    检查端口信息
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# netstat -lntup |<span style="color: #0000ff;">grep</span> <span style="color: #800080;">80</span><span style="color: #000000;">
tcp        </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">80</span>                  <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*                   LISTEN      <span style="color: #800080;">26548</span>/nginx  </pre>
  </div>
  
  <p>
    <span style="font-size: 1.5em;">1.5 挂载光盘</span>
  </p>
</div>

### <span id="151">1.5.1 删除默认的主页文件，创建挂载目录</span>

<div>
  <div class="cnblogs_code">
    <pre>cd /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html && \<span style="color: #0000ff;">rm</span> *<span style="color: #000000;">.html
</span><span style="color: #0000ff;">mkdir</span> -p /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/ios</pre>
  </div>
</div>

### <span id="152">1.5.2 挂载光盘</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">mount</span> /dev/cdrom /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/ios/</pre>
  </div>
</div>

### <span id="153">1.5.3 检查挂载信息</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test html]# <span style="color: #0000ff;">df</span> -<span style="color: #000000;">h
Filesystem      Size  Used Avail Use</span>%<span style="color: #000000;"> Mounted on
</span>/dev/sda3        19G  <span style="color: #800080;">1</span>.8G   16G  <span style="color: #800080;">10</span>% /<span style="color: #000000;">
tmpfs           238M     </span><span style="color: #800080;"></span>  238M   <span style="color: #800080;"></span>% /dev/<span style="color: #000000;">shm
</span>/dev/sda1       190M   40M  141M  <span style="color: #800080;">22</span>% /<span style="color: #000000;">boot
</span>/dev/sr0        <span style="color: #800080;">3</span>.7G  <span style="color: #800080;">3</span>.7G     <span style="color: #800080;"></span> <span style="color: #800080;">100</span>% /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/ios/</pre>
  </div>
</div>

&nbsp;

## <span id="16_PXE">1.6 配置支持PXE的启动程序</span>

安装syslinux

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> -y <span style="color: #0000ff;">install</span> syslinux</pre>
  </div>
</div>

复制启动菜单程序文件

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# <span style="color: #0000ff;">cp</span> /usr/share/syslinux/pxelinux.<span style="color: #800080;"></span> /var/lib/tftpboot/<span style="color: #000000;">
[root@test </span>~]# <span style="color: #0000ff;">cp</span> -a /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/isolinux<span style="color: #008000;">/*</span><span style="color: #008000;"> /var/lib/tftpboot/
[root@test ~]#  ls /var/lib/tftpboot/
boot.cat  grub.conf   isolinux.bin  memtest     splash.jpg  vesamenu.c32
boot.msg  initrd.img  isolinux.cfg  pxelinux.0  TRANS.TBL   vmlinuz</span></pre>
  </div>
</div>

新建一个pxelinux.cfg目录，存放客户端的配置文件。

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">mkdir</span> -p /var/lib/tftpboot/<span style="color: #000000;">pxelinux.cfg
</span><span style="color: #0000ff;">cp</span> -a /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/isolinux/isolinux.cfg /var/lib/tftpboot/pxelinux.cfg/default</pre>
  </div>
</div>

## <span id="17">1.7 创建一个新的虚拟机</span>

<p style="text-align: left;">
  　　　　不要使用光盘，然后开机
</p>

<div style="text-align: center;">
  &nbsp;<img data-original="/wp-content/uploads/2018/09/1190037-20171025130643504-1159432220.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="KICKSTART无人值守安装" alt="" />
</div>

<div style="text-align: center;">
  <strong>　　出现此界面说明上面的步骤都配置正确</strong>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# <span style="color: #0000ff;">cat</span>  /var/lib/tftpboot/pxelinux.cfg/<span style="color: #000000;">default
default vesamenu.c32    # 默认加载一个菜单
#prompt </span><span style="color: #800080;">1</span>     # 开启会显示命令行<span style="color: #800000;">'</span><span style="color: #800000;">boot: </span><span style="color: #800000;">'</span>提示符。prompt值为0时则不提示，将会直接启动<span style="color: #800000;">'</span><span style="color: #800000;">default</span><span style="color: #800000;">'</span><span style="color: #000000;">参数中指定的内容。
timeout </span><span style="color: #800080;">600</span>   # timeout时间是引导时等待用户手动选择的时间，设为1可直接引导，单位为1/<span style="color: #000000;">10秒。

display boot.msg
# 菜单背景图片、标题、颜色。
menu background splash.jpg
menu title Welcome to CentOS </span><span style="color: #800080;">6.9</span>!<span style="color: #000000;">
menu color border </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color sel </span><span style="color: #800080;">7</span><span style="color: #000000;"> #ffffffff #ff000000
menu color title </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color tabmsg </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color unsel </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color hotsel </span><span style="color: #800080;"></span><span style="color: #000000;"> #ff000000 #ffffffff
menu color hotkey </span><span style="color: #800080;">7</span><span style="color: #000000;"> #ffffffff #ff000000
menu color scrollbar </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
# label指定在boot:提示符下输入的关键字，比如boot:linux[ENTER]，这个会启动label linux下标记的kernel和initrd.img文件。
label linux    # 一个标签就是前面图片的一行选项
  menu label </span>^<span style="color: #000000;">Install or upgrade an existing system
  menu default
  kernel vmlinuz   # 指定要启动的内核。同样要注意路径，默认是</span>/<span style="color: #000000;">tftpboot目录
  append initrd</span>=<span style="color: #000000;">initrd.img   # 指定追加给内核的参数，initrd.img是一个最小的linux系统
label vesa
  menu label Install system with </span>^<span style="color: #000000;">basic video driver
  kernel vmlinuz
  append initrd</span>=<span style="color: #000000;">initrd.img nomodeset
label rescue
  menu label </span>^<span style="color: #000000;">Rescue installed system
  kernel vmlinuz
  append initrd</span>=<span style="color: #000000;">initrd.img rescue
label local
  menu label Boot from </span>^<span style="color: #000000;">local drive
  localboot </span><span style="color: #800080;">0xffff</span><span style="color: #000000;">
label memtest86
  menu label </span>^<span style="color: #000000;">Memory test
  kernel memtest
  append </span>-</pre>
  </div>
</div>

## <span id="18_kscfg">1.8 创建ks.cfg文件</span>

通常，我们在安装操作系统的过程中，需要大量的和服务器交互操作，为了减少这个交互过程，kickstart就诞生了。使用这种kickstart，只需事先定义好一个Kickstart自动应答配置文件ks.cfg（通常存放在安装服务器上），并让安装程序知道该配置文件的位置，在安装过程中安装程序就可以自己从该文件中读取安装配置，这样就避免了在安装过程中多次的人机交互，从而实现无人值守的自动化安装。

<span style="background-color: #ffff00;"><strong>生成kickstart</strong><strong>配置文件的三种方法：</strong></span>

方法1、

　　&nbsp;每安装好一台Centos机器，Centos安装程序都会创建一个kickstart配置文件，记录你的真实安装配置。

　　如果你希望实现和某系统类似的安装，可以基于该系统的kickstart配置文件来生成你自己的kickstart配置文件。（生成的文件名字叫anaconda-ks.cfg位于/root/anaconda-ks.cfg）

方法2、

　　Centos提供了一个图形化的kickstart配置工具。在任何一个安装好的Linux系统上运行该工具，就可以很容易地创建你自己的kickstart配置文件。

　　kickstart配置工具命令为redhat-config-kickstart（RHEL3）或system-config-kickstart（RHEL4，RHEL5）.网上有很多用CentOS桌面版生成ks文件的文章，如果有现成的系统就没什么可说。但没有现成的，也没有必要去用桌面版，命令行也很简单。

方法3、

　　阅读kickstart配置文件的手册。用任何一个文本编辑器都可以创建你自己的kickstart配置文件。

## <span id="kscfg">ks.cfg文件详解</span>

<div class="cnblogs_Highlighter">
  <pre class="brush:php;gutter:true;">官网文档
CentOS5 : http://www.centos.org/docs/5/html/Installation_Guide-en-US/s1-kickstart2-options.html
CentOS6 : https://access.redhat.com/knowledge/docs/en-US/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-kickstart2-options.html</pre>
</div>

官网自带中文版，选一下语言即可  
`ks.cfg`文件组成大致分为3段

  * 命令段  
    键盘类型，语言，安装方式等系统的配置，有必选项和可选项，如果缺少某项必选项，安装时会中断并提示用户选择此项的选项

  * 软件包段
    
    <ol class="linenums">
      <li class="L0">
        <code>%packages</code>
      </li>
      <li class="L1">
        <code>@groupname：指定安装的包组</code>
      </li>
      <li class="L2">
        <code>package_name：指定安装的包</code>
      </li>
      <li class="L3">
        <code>-package_name：指定不安装的包</code>
      </li>
    </ol>

在安装过程中默认安装的软件包，安装软件时会自动分析依赖关系。

  * 脚本段(可选) <ol class="linenums">
      <li class="L0">
        <code>%pre:安装系统前执行的命令或脚本(由于只依赖于启动镜像，支持的命令很少)</code>
      </li>
      <li class="L1">
        <code>%post:安装系统后执行的命令或脚本(基本支持所有命令)</code>
      </li>
    </ol>

<table>
  <tr>
    <th>
      关键字
    </th>
    
    <th>
      含义
    </th>
  </tr>
  
  <tr>
    <td>
      <code>install</code>
    </td>
    
    <td>
      告知安装程序，这是一次全新安装，而不是升级<code>upgrade</code>。
    </td>
  </tr>
  
  <tr>
    <td>
      <code>url --url=" "</code>
    </td>
    
    <td>
      通过<code>FTP</code>或<code>HTTP</code>从远程服务器上的安装树中安装。<br /><code>url --url="http://10.0.0.7/CentOS-6.7/"</code><br /><code>url --url ftp://&lt;username>:&lt;password>@&lt;server>/&lt;dir></code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>nfs</code>
    </td>
    
    <td>
      从指定的<code>NFS</code>服务器安装。<br /><code>nfs --server=nfsserver.example.com --dir=/tmp/install-tree</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>text</code>
    </td>
    
    <td>
      使用文本模式安装。
    </td>
  </tr>
  
  <tr>
    <td>
      <code>lang</code>
    </td>
    
    <td>
      设置在安装过程中使用的语言以及系统的缺省语言。<code>lang en_US.UTF-8</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>keyboard</code>
    </td>
    
    <td>
      设置系统键盘类型。<code>keyboard us</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>zerombr</code>
    </td>
    
    <td>
      清除<code>mbr</code>引导信息。
    </td>
  </tr>
  
  <tr>
    <td>
      <code>bootloader</code>
    </td>
    
    <td>
      系统引导相关配置。<br /><code>bootloader --location=mbr --driveorder=sda --append="crashkernel=auto rhgb quiet"</code><br /><code>--location=</code>,指定引导记录被写入的位置.有效的值如下:<code>mbr</code>(缺省),<code>partition</code>(在包含内核的分区的第一个扇区安装引导装载程序)或<code>none</code>(不安装引导装载程序)。<br /><code>--driveorder</code>,指定在<code>BIOS</code>引导顺序中居首的驱动器。<br /><code>--append=</code>,指定内核参数.要指定多个参数,使用空格分隔它们。
    </td>
  </tr>
  
  <tr>
    <td>
      <code>network</code>
    </td>
    
    <td>
      为通过网络的<code>kickstart</code>安装以及所安装的系统配置联网信息。<br /><code>network --bootproto=dhcp --device=eth0 --onboot=yes --noipv6 --hostname=CentOS6</code><br /><code>--bootproto=[dhcp/bootp/static]</code>中的一种，缺省值是<code>dhcp</code>。<code>bootp</code>和<code>dhcp</code>被认为是相同的。<br /><code>static</code>方法要求在<code>kickstart</code>文件里输入所有的网络信息。<br /><code>network --bootproto=static --ip=10.0.0.100 --netmask=255.255.255.0 --gateway=10.0.0.2 --nameserver=10.0.0.2</code><br />请注意所有配置信息都必须在一行上指定,不能使用反斜线来换行。<br /><code>--ip=</code>,要安装的机器的<code>IP</code>地址.<br /><code>--gateway=</code>,IP地址格式的默认网关.<br /><code>--netmask=</code>,安装的系统的子网掩码.<br /><code>--hostname=</code>,安装的系统的主机名.<br /><code>--onboot=</code>,是否在引导时启用该设备.<br /><code>--noipv6=</code>,禁用此设备的<code>IPv6</code>.<br /><code>--nameserver=</code>,配置<code>dns</code>解析.
    </td>
  </tr>
  
  <tr>
    <td>
      <code>timezone</code>
    </td>
    
    <td>
      设置系统时区。<code>timezone --utc Asia/Shanghai</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>authconfig</code>
    </td>
    
    <td>
      系统认证信息。<code>authconfig --enableshadow --passalgo=sha512</code><br />设置密码加密方式为<code>sha512</code>&nbsp;启用<code>shadow</code>文件。
    </td>
  </tr>
  
  <tr>
    <td>
      <code>rootpw</code>
    </td>
    
    <td>
      <code>root</code>密码
    </td>
  </tr>
  
  <tr>
    <td>
      <code>clearpart</code>
    </td>
    
    <td>
      清空分区。<code>clearpart --all --initlabel</code><br /><code>--all</code>&nbsp;从系统中清除所有分区，<code>--initlable</code>&nbsp;初始化磁盘标签
    </td>
  </tr>
  
  <tr>
    <td>
      <code>part</code>
    </td>
    
    <td>
      磁盘分区。<br /><code>part /boot --fstype=ext4 --asprimary --size=200</code><br /><code>part swap --size=1024</code><br /><code>part / --fstype=ext4 --grow --asprimary --size=200</code><br /><code>--fstype=</code>,为分区设置文件系统类型.有效的类型为<code>ext2</code>,<code>ext3</code>,<code>swap</code>和<code>vfat</code>。<br /><code>--asprimary</code>,强迫把分区分配为主分区,否则提示分区失败。<br /><code>--size=</code>,以<code>MB</code>为单位的分区最小值.在此处指定一个整数值,如<code>500</code>.不要在数字后面加<code>MB</code>。<br /><code>--grow</code>,告诉分区使用所有可用空间(若有),或使用设置的最大值。
    </td>
  </tr>
  
  <tr>
    <td>
      <code>firstboot</code>
    </td>
    
    <td>
      负责协助配置redhat一些重要的信息。<br /><code>firstboot --disable</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>selinux</code>
    </td>
    
    <td>
      关闭<code>selinux</code>。<code>selinux --disabled</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>firewall</code>
    </td>
    
    <td>
      关闭防火墙。<code>firewall --disabled</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>logging</code>
    </td>
    
    <td>
      设置日志级别。<code>logging --level=info</code>
    </td>
  </tr>
  
  <tr>
    <td>
      <code>reboot</code>
    </td>
    
    <td>
      设定安装完成后重启,此选项必须存在，不然kickstart显示一条消息，并等待用户按任意键后才重新引导，也可以选择<code>halt</code>关机。
    </td>
  </tr>
</table>

<div>
  <h2>
    <span id="19_anaconda-kscfg">1.9 查看系统安装完成的anaconda-ks.cfg</span>
  </h2>
</div>

<div>
  <div class="cnblogs_code">
    <pre><br />[root@test ~]#  <span style="color: #0000ff;">cat</span> anaconda-<span style="color: #000000;">ks.cfg
# Kickstart </span><span style="color: #0000ff;">file</span><span style="color: #000000;"> automatically generated by anaconda.

#version</span>=<span style="color: #000000;">DEVEL
</span><span style="color: #0000ff;">install</span><span style="color: #000000;">
cdrom
lang en_US.UTF</span>-<span style="color: #800080;">8</span><span style="color: #000000;">
keyboard us
network </span>--onboot no --device eth0 --bootproto dhcp --<span style="color: #000000;">noipv6
rootpw  </span>--iscrypted $<span style="color: #800080;">6</span>$.8PjXDBfzrUEFZte$IfTqwmdHXTJ6HD5/<span style="color: #000000;">mYOuhuNMhVWaJI0xwyRMvOIrYkaEZduHVYuTEfjbgAqEuEsJii0wkBQvCVgF.KRG9ikwu0
firewall </span>--service=<span style="color: #0000ff;">ssh</span><span style="color: #000000;">
authconfig </span>--enableshadow --passalgo=<span style="color: #000000;">sha512
selinux </span>--<span style="color: #000000;">enforcing
timezone Asia</span>/<span style="color: #000000;">Shanghai
bootloader </span>--location=mbr --driveorder=sda --append=<span style="color: #800000;">"</span><span style="color: #800000;">crashkernel=auto rhgb quiet</span><span style="color: #800000;">"</span><span style="color: #000000;">
# The following is the partition information you requested
# Note that any partitions you deleted are not expressed
# here so unless you </span><span style="color: #0000ff;">clear</span><span style="color: #000000;"> all partitions first, this is
# not guaranteed to work
#clearpart </span>--<span style="color: #000000;">none

#part </span>/boot --fstype=ext4 --asprimary --size=<span style="color: #800080;">200</span><span style="color: #000000;">
#part swap </span>--asprimary --size=<span style="color: #800080;">768</span><span style="color: #000000;">
#part </span>/ --fstype=ext4 --grow --asprimary --size=<span style="color: #800080;">200</span><span style="color: #000000;">


repo </span>--name=<span style="color: #800000;">"</span><span style="color: #800000;">CentOS</span><span style="color: #800000;">"</span>  --baseurl=cdrom:sr0 --cost=<span style="color: #800080;">100</span>

%<span style="color: #000000;">packages
@base
@compat</span>-<span style="color: #000000;">libraries
@core
@debugging
@development
@server</span>-<span style="color: #000000;">policy
@workstation</span>-<span style="color: #000000;">policy
python</span>-<span style="color: #000000;">dmidecode
sgpio
device</span>-mapper-persistent-<span style="color: #000000;">data
systemtap</span>-client</pre>
  </div>
</div>

### <span id="191_ks">1.9.1 编写ks文件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# grub-<span style="color: #000000;">crypt
Password:  </span><span style="color: #800080;">123456</span><span style="color: #000000;">
Retype password:  </span><span style="color: #800080;">123465</span><span style="color: #000000;">
$</span><span style="color: #800080;">6</span>$OH3zrKw7ruG5mtIh$8bV2RhvoB72VCIXYY.2ROFi8AOLdI3lHGB.rkGDEhlqxTZduPE3VoJW2OIZRA1y9Gw4Zka461IBZ9VuIIaNqK.</pre>
  </div>
</div>

创建ks文件存放目录

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# <span style="color: #0000ff;">mkdir</span> /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/ks_config -p</pre>
  </div>
</div>

ks文件内容

<div>
  <div class="cnblogs_code">
    <pre>[root@test ks_config]# <span style="color: #0000ff;">cat</span> /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/ks_config/CentOS-<span style="color: #800080;">6.9</span>-<span style="color: #000000;">ks.cfg
# Kickstart Configurator </span><span style="color: #0000ff;">for</span> CentOS <span style="color: #800080;">6.9</span><span style="color: #000000;"> by hou zhaoshun
</span><span style="color: #0000ff;">install</span><span style="color: #000000;">
url </span>--url=<span style="color: #800000;">"</span><span style="color: #800000;">http://10.0.0.250/ios/</span><span style="color: #800000;">"</span><span style="color: #000000;">
text
lang en_US.UTF</span>-<span style="color: #800080;">8</span><span style="color: #000000;">
keyboard us
zerombr
bootloader </span>--location=mbr --driveorder=sda --append=<span style="color: #800000;">"</span><span style="color: #800000;">crashkernel=auto rhgb quiet</span><span style="color: #800000;">"</span><span style="color: #000000;">
network </span>--bootproto=dhcp --device=eth0 --onboot=yes --noipv6 --<span style="color: #0000ff;">hostname</span>=<span style="color: #000000;">CentOS6
timezone </span>--utc Asia/<span style="color: #000000;">Shanghai
authconfig </span>--enableshadow --passalgo=<span style="color: #000000;">sha512
rootpw  </span>--iscrypted $<span style="color: #800080;">6</span>$X20eRtuZhkHznTb4$dK0BJByOSAWSDD8jccLVFz0CscijS9ldMWwpoCw/ZEjYw2BTQYGWlgKsn945fFTjRC658UXjuocwJbAjVI5D6/<span style="color: #000000;">
clearpart </span>--all --<span style="color: #000000;">initlabel
part </span>/boot --fstype=ext4 --asprimary --size=<span style="color: #800080;">200</span><span style="color: #000000;">
part swap </span>--size=<span style="color: #800080;">768</span><span style="color: #000000;">
part </span>/ --fstype=ext4 --grow --asprimary --size=<span style="color: #800080;">200</span><span style="color: #000000;">
firstboot </span>--<span style="color: #000000;">disable
selinux </span>--<span style="color: #000000;">disabled
firewall </span>--<span style="color: #000000;">disabled
logging </span>--level=<span style="color: #0000ff;">info</span><span style="color: #000000;">
reboot
</span>%<span style="color: #000000;">packages
@base
@compat</span>-<span style="color: #000000;">libraries
@debugging
@development
tree
nmap
sysstat
lrzsz
dos2unix
telnet
</span>%<span style="color: #000000;">post
</span><span style="color: #0000ff;">wget</span> -O /tmp/optimization.<span style="color: #0000ff;">sh</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">10.0.0.250/ks_config/optimization.sh &>/dev/null</span>
/bin/<span style="color: #0000ff;">sh</span> /tmp/optimization.<span style="color: #0000ff;">sh</span>
%end</pre>
  </div>
</div>

## <span id="110">1.10 编写开机优化脚本</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ks_config]# <span style="color: #0000ff;">cat</span> /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">3</span>/html/ks_config/optimization.<span style="color: #0000ff;">sh</span><span style="color: #000000;"> 
#</span>!/bin/<span style="color: #000000;">bash
##############################################################
# File Name: </span>/var/www/html/ks_config/optimization.<span style="color: #0000ff;">sh</span><span style="color: #000000;">
# Version: V1.</span><span style="color: #800080;"></span><span style="color: #000000;">
# Author: houzhaoshun
# Organization: blog.znix.top
# Created Time : </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">23</span><span style="color: #000000;"> 
# Description: Linux system initialization
##############################################################
. </span>/etc/init.d/<span style="color: #000000;">functions
Ip</span>=<span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.250</span><span style="color: #000000;">
Port</span>=<span style="color: #800080;">80</span><span style="color: #000000;">
ConfigDir</span>=<span style="color: #000000;">ks_config
# Judge Http server is ok</span>?<span style="color: #000000;">
PortNum</span>=`nmap $Ip  -p $Port <span style="color: #800080;">2</span>>/dev/<span style="color: #0000ff;">null</span>|<span style="color: #0000ff;">grep</span> open|<span style="color: #0000ff;">wc</span> -<span style="color: #000000;">l`
[ $PortNum </span>-lt <span style="color: #800080;">1</span> ] &&<span style="color: #000000;"> {
        </span><span style="color: #0000ff;">echo</span> <span style="color: #800000;">"</span><span style="color: #800000;">Http server is bad!</span><span style="color: #800000;">"</span><span style="color: #000000;">
        exit </span><span style="color: #800080;">1</span><span style="color: #000000;">
}
# Defined result </span><span style="color: #0000ff;">function</span>
<span style="color: #0000ff;">function</span><span style="color: #000000;"> Msg(){
        </span><span style="color: #0000ff;">if</span> [ $? -eq <span style="color: #800080;"></span> ];<span style="color: #0000ff;">then</span><span style="color: #000000;">
          action </span><span style="color: #800000;">"</span><span style="color: #800000;">$1</span><span style="color: #800000;">"</span> /bin/<span style="color: #0000ff;">true</span>
        <span style="color: #0000ff;">else</span><span style="color: #000000;">
          action </span><span style="color: #800000;">"</span><span style="color: #800000;">$1</span><span style="color: #800000;">"</span> /bin/<span style="color: #0000ff;">false</span>
        <span style="color: #0000ff;">fi</span><span style="color: #000000;">
}
# Defined IP </span><span style="color: #0000ff;">function</span>
<span style="color: #0000ff;">function</span><span style="color: #000000;"> ConfigIP(){
Suffix</span>=`<span style="color: #0000ff;">ifconfig</span> eth0|<span style="color: #0000ff;">awk</span> -F <span style="color: #800000;">"</span><span style="color: #800000;">[ .]+</span><span style="color: #800000;">"</span> <span style="color: #800000;">'</span><span style="color: #800000;">NR==2 {print $6}</span><span style="color: #800000;">'</span><span style="color: #000000;">`
</span><span style="color: #0000ff;">cat</span> >/etc/sysconfig/network-scripts/ifcfg-eth0 &lt;&lt;-<span style="color: #000000;">END
DEVICE</span>=<span style="color: #000000;">eth0
TYPE</span>=<span style="color: #000000;">Ethernet
ONBOOT</span>=<span style="color: #000000;">yes
NM_CONTROLLED</span>=<span style="color: #000000;">yes
BOOTPROTO</span>=<span style="color: #000000;">none
IPADDR</span>=<span style="color: #800080;">10.0</span>.<span style="color: #800080;"></span><span style="color: #000000;">.$Suffix
PREFIX</span>=<span style="color: #800080;">24</span><span style="color: #000000;">
GATEWAY</span>=<span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.254</span><span style="color: #000000;">
DNS1</span>=<span style="color: #800080;">223.5</span>.<span style="color: #800080;">5.5</span><span style="color: #000000;">
DEFROUTE</span>=<span style="color: #000000;">yes
IPV4_FAILURE_FATAL</span>=<span style="color: #000000;">yes
IPV6INIT</span>=<span style="color: #000000;">no
NAME</span>=<span style="color: #800000;">"</span><span style="color: #800000;">System eth0</span><span style="color: #800000;">"</span><span style="color: #000000;">
END
Msg </span><span style="color: #800000;">"</span><span style="color: #800000;">config eth0</span><span style="color: #800000;">"</span><span style="color: #000000;">
}
# Defined Yum source Functions
</span><span style="color: #0000ff;">function</span> <span style="color: #0000ff;">yum</span><span style="color: #000000;">(){
        YumDir</span>=/etc/<span style="color: #0000ff;">yum</span><span style="color: #000000;">.repos.d
        [ </span>-f <span style="color: #800000;">"</span><span style="color: #800000;">$YumDir/CentOS-Base.repo</span><span style="color: #800000;">"</span> ] && <span style="color: #0000ff;">cp</span> $YumDir/CentOS-<span style="color: #000000;">Base.repo{,.ori} 
        </span><span style="color: #0000ff;">wget</span> -O $YumDir/CentOS-Base.repo http:<span style="color: #008000;">//</span><span style="color: #008000;">$Ip:$Port/$ConfigDir/CentOS-Base.repo &>/dev/null &&\</span>
        <span style="color: #0000ff;">wget</span> -O $YumDir/epel.repo http:<span style="color: #008000;">//</span><span style="color: #008000;">$Ip:$Port/$ConfigDir/epel.repo &>/dev/null &&\</span>
        Msg <span style="color: #800000;">"</span><span style="color: #800000;">YUM source</span><span style="color: #800000;">"</span><span style="color: #000000;">
}
# Defined Hide the system version number Functions
</span><span style="color: #0000ff;">function</span><span style="color: #000000;"> HideVersion(){
        [ </span>-f <span style="color: #800000;">"</span><span style="color: #800000;">/etc/issue</span><span style="color: #800000;">"</span> ] && >/etc/<span style="color: #000000;">issue
        Msg </span><span style="color: #800000;">"</span><span style="color: #800000;">Hide issue</span><span style="color: #800000;">"</span><span style="color: #000000;"> 
        [ </span>-f <span style="color: #800000;">"</span><span style="color: #800000;">/etc/issue.net</span><span style="color: #800000;">"</span> ] && > /etc/<span style="color: #000000;">issue.net
        Msg </span><span style="color: #800000;">"</span><span style="color: #800000;">Hide issue.net</span><span style="color: #800000;">"</span><span style="color: #000000;">
}
# Defined OPEN FILES Functions
</span><span style="color: #0000ff;">function</span><span style="color: #000000;"> openfiles(){
        [ </span>-f <span style="color: #800000;">"</span><span style="color: #800000;">/etc/security/limits.conf</span><span style="color: #800000;">"</span> ] &&<span style="color: #000000;"> {
        </span><span style="color: #0000ff;">echo</span> <span style="color: #800000;">'</span><span style="color: #800000;">*  -  nofile  65535</span><span style="color: #800000;">'</span> >> /etc/security/<span style="color: #000000;">limits.conf
        Msg </span><span style="color: #800000;">"</span><span style="color: #800000;">open files</span><span style="color: #800000;">"</span><span style="color: #000000;">
        }
}
# Defined Kernel parameters Functions
</span><span style="color: #0000ff;">function</span><span style="color: #000000;"> kernel(){
        KernelDir</span>=/<span style="color: #000000;">etc
        [ </span>-f <span style="color: #800000;">"</span><span style="color: #800000;">$KernelDir/sysctl.conf</span><span style="color: #800000;">"</span> ] && /bin/<span style="color: #0000ff;">mv</span> $KernelDir/<span style="color: #000000;">sysctl.conf{,.ori}
        </span><span style="color: #0000ff;">wget</span> -O $KernelDir/sysctl.conf http:<span style="color: #008000;">//</span><span style="color: #008000;">$Ip:$Port/$ConfigDir/sysctl.conf &>/dev/null</span>
        Msg <span style="color: #800000;">"</span><span style="color: #800000;">Kernel config</span><span style="color: #800000;">"</span><span style="color: #000000;">
}
# Defined System Startup Services Functions
</span><span style="color: #0000ff;">function</span><span style="color: #000000;"> boot(){
        </span><span style="color: #0000ff;">for</span> oldboy <span style="color: #0000ff;">in</span> `chkconfig --list|<span style="color: #0000ff;">grep</span> <span style="color: #800000;">"</span><span style="color: #800000;">3:on</span><span style="color: #800000;">"</span>|<span style="color: #0000ff;">awk</span> <span style="color: #800000;">'</span><span style="color: #800000;">{print $1}</span><span style="color: #800000;">'</span>|<span style="color: #0000ff;">grep</span> -vE <span style="color: #800000;">"</span><span style="color: #800000;">crond|network|rsyslog|sshd|sysstat</span><span style="color: #800000;">"</span><span style="color: #000000;">` 
          </span><span style="color: #0000ff;">do</span><span style="color: #000000;"> 
           chkconfig $oldboy off
        </span><span style="color: #0000ff;">done</span><span style="color: #000000;">
        Msg </span><span style="color: #800000;">"</span><span style="color: #800000;">BOOT config</span><span style="color: #800000;">"</span><span style="color: #000000;">
}
# Defined Time Synchronization Functions
</span><span style="color: #0000ff;">function</span><span style="color: #000000;"> Time(){
        </span><span style="color: #0000ff;">echo</span> <span style="color: #800000;">"</span><span style="color: #800000;">#time sync by houzhaoshun at $(date +%F)</span><span style="color: #800000;">"</span> >>/var/spool/cron/<span style="color: #000000;">root
        </span><span style="color: #0000ff;">echo</span> <span style="color: #800000;">'</span><span style="color: #800000;">*/5 * * * * /usr/sbin/ntpdate time.nist.gov &>/dev/null</span><span style="color: #800000;">'</span> >>/var/spool/cron/<span style="color: #000000;">root
        Msg </span><span style="color: #800000;">"</span><span style="color: #800000;">Time Synchronization</span><span style="color: #800000;">"</span><span style="color: #000000;">
}
# Defined main Functions
</span><span style="color: #0000ff;">function</span><span style="color: #000000;"> main(){
        ConfigIP
        </span><span style="color: #0000ff;">yum</span><span style="color: #000000;">
        HideVersion
        openfiles
        kernel
        boot
        Time
}
main
# rz上传CentOS</span>-Base.repo、epel.repo、sysctl.conf</pre>
  </div>
</div>

## <span id="111_default">1.11 整合编辑default配置文件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ks_config]# <span style="color: #0000ff;">cat</span>  /var/lib/tftpboot/pxelinux.cfg/<span style="color: #000000;">default
default ks
prompt </span><span style="color: #800080;"></span><span style="color: #000000;">
timeout </span><span style="color: #800080;">600</span><span style="color: #000000;">

display boot.msg

menu background splash.jpg
menu title Welcome to CentOS </span><span style="color: #800080;">6.9</span>!<span style="color: #000000;">
menu color border </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color sel </span><span style="color: #800080;">7</span><span style="color: #000000;"> #ffffffff #ff000000
menu color title </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color tabmsg </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color unsel </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">
menu color hotsel </span><span style="color: #800080;"></span><span style="color: #000000;"> #ff000000 #ffffffff
menu color hotkey </span><span style="color: #800080;">7</span><span style="color: #000000;"> #ffffffff #ff000000
menu color scrollbar </span><span style="color: #800080;"></span> #ffffffff #<span style="color: #800080;">00000000</span><span style="color: #000000;">

label linux
  menu label </span>^<span style="color: #000000;">Install or upgrade an existing system
  menu default
  kernel vmlinuz
  append initrd</span>=<span style="color: #000000;">initrd.img
label vesa
  menu label Install system with </span>^<span style="color: #000000;">basic video driver
  kernel vmlinuz
  append initrd</span>=<span style="color: #000000;">initrd.img nomodeset
label rescue
  menu label </span>^<span style="color: #000000;">Rescue installed system
  kernel vmlinuz
  append initrd</span>=<span style="color: #000000;">initrd.img rescue
label local
  menu label Boot from </span>^<span style="color: #000000;">local drive
  localboot </span><span style="color: #800080;">0xffff</span><span style="color: #000000;">
label memtest86
  menu label </span>^<span style="color: #000000;">Memory test
  kernel memtest
  append </span>-<span style="color: #000000;">
label ks
  kernel vmlinuz
  append initrd</span>=initrd.img ks=http:<span style="color: #008000;">//</span><span style="color: #008000;">10.0.0.250/ks_config/CentOS-6.9-ks.cfg</span></pre>
  </div>
  
  <h2>
    <span id="111">1.11 以上操作完成后将之前创建的虚拟机重启</span>
  </h2>
  
  <p>
    　　　　然后你就可以取喝杯茶，等他一会就ok了
  </p>
  
  <p>
    &nbsp;
  </p>
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11">1.1 环境说明</a>
    </li>
    <li>
      <a href="#12_DHCP">1.2 配置DHCP</a><ul>
        <li>
          <a href="#121_dhcp">1.2.1 安装dhcp</a>
        </li>
        <li>
          <a href="#122">1.2.2 编写配置文件</a>
        </li>
        <li>
          <a href="#123">1.2.3 启动服务</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_TFTP">1.3 安装TFTP服务</a><ul>
        <li>
          <a href="#131_tftp">1.3.1 安装tftp服务</a>
        </li>
        <li>
          <a href="#132_xindetd">1.3.2 编写xindetd下的配置文件</a>
        </li>
        <li>
          <a href="#133_xinetd">1.3.3 启动服务，让xinetd 管理</a>
        </li>
        <li>
          <a href="#134">1.3.4 检查端口</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14_HTTP">1.4 配置HTTP服务</a><ul>
        <li>
          <a href="#141_nginxpcre-devel_openssl-devel">1.4.1 安装nginx的依赖包（pcre-devel openssl-devel）</a>
        </li>
        <li>
          <a href="#144_nbspnginx">1.4.4 &nbsp;nginx软件编译安装过程</a>
        </li>
        <li>
          <a href="#147">1.4.7 启动程序</a>
        </li>
        <li>
          <a href="#151">1.5.1 删除默认的主页文件，创建挂载目录</a>
        </li>
        <li>
          <a href="#152">1.5.2 挂载光盘</a>
        </li>
        <li>
          <a href="#153">1.5.3 检查挂载信息</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#16_PXE">1.6 配置支持PXE的启动程序</a>
    </li>
    <li>
      <a href="#17">1.7 创建一个新的虚拟机</a>
    </li>
    <li>
      <a href="#18_kscfg">1.8 创建ks.cfg文件</a>
    </li>
    <li>
      <a href="#kscfg">ks.cfg文件详解</a>
    </li>
    <li>
      <a href="#19_anaconda-kscfg">1.9 查看系统安装完成的anaconda-ks.cfg</a><ul>
        <li>
          <a href="#191_ks">1.9.1 编写ks文件</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#110">1.10 编写开机优化脚本</a>
    </li>
    <li>
      <a href="#111_default">1.11 整合编辑default配置文件</a>
    </li>
    <li>
      <a href="#111">1.11 以上操作完成后将之前创建的虚拟机重启</a>
    </li>
  </ul>
</div>