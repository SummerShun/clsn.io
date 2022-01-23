---
title: Zabbix 3.0 从入门到精通(zabbix使用详解)
author: 惨绿少年
type: post
date: 2017-11-23T01:24:00+00:00
url: /clsn/lx658.html
Baidusubmit:
  - 1
views:
  - 470
specs_zan:
  - 6
zm_favorites:
  - 1
categories:
  - Linux运维
  - 数据库
  - 监控服务
  - 运维基本功

---
# <span id="1_zabbix">第1章 zabbix监控</span>

## <span id="11">1.1 为什么要监控</span>

&nbsp;&nbsp; 　　在需要的时刻，提前提醒我们服务器出问题了

&nbsp;&nbsp;　　 当出问题之后，可以找到问题的根源

&nbsp;　　&nbsp; 网站/服务器 的可用性

### <span id="111">1.1.1 网站可用性</span>

　　在软件系统的高可靠性（也称为可用性，英文描述为HA，High Available）里有个衡量其可靠性的标准&mdash;&mdash;X个9，这个X是代表数字3~5。X个9表示在软件系统1年时间的使用过程中，系统可以正常使用时间与总时间（1年）之比，我们通过下面的计算来感受下X个9在不同级别的可靠性差异。

<div>
  <div class="cnblogs_code">
    <pre>    1个9：(1-90%)*365=36<span style="color: #000000;">.5天，表示该软件系统在连续运行1年时间里最多可能的业务中断时间是36.5天
    2个9：(</span>1-99%)*365=3<span style="color: #000000;">.65天 ， 表示该软件系统在连续运行1年时间里最多可能的业务中断时间是3.65天
    3个9：(</span>1-99.9%)*365*24=8<span style="color: #000000;">.76小时，表示该软件系统在连续运行1年时间里最多可能的业务中断时间是8.76小时。
    4个9：(</span>1-99.99%)*365*24=0.876小时=52<span style="color: #000000;">.6分钟，表示该软件系统在连续运行1年时间里最多可能的业务中断时间是52.6分钟。
    5个9：(</span>1-99.999%)*365*24*60=5<span style="color: #000000;">.26分钟，表示该软件系统在连续运行1年时间里最多可能的业务中断时间是5.26分钟。
    6个9：(</span>1-99.9999%)*365*24*60*60=31秒， 示该软件系统在连续运行1年时间里最多可能的业务中断时间是31秒</pre>
  </div>
</div>

## <span id="12">1.2 监控什么东西</span>

监控一切需要监控的东西，只要能够想到，能够用命令实现的都能用来监控

### <span id="121">1.2.1 监控范畴</span>

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123164550743-1527232078.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

## <span id="13">1.3 怎么来监控</span>

### <span id="131">1.3.1 远程管理服务器</span>

如果想<span style="background-color: #ffff00;"><strong>远程管理服务器</strong></span>就有远程管理卡，比如Dell idRAC，HP ILO，IBM IMM

### <span id="132">1.3.2 监控硬件</span>

查看硬件的温度/风扇转速，电脑有鲁大师，服务器就有ipmitool。

使用ipmitool实现对服务器的命令行远程管理

<div>
  <div class="cnblogs_code">
    <pre>yum -y install OpenIPMI ipmitool  <span style="color: #008000;">#</span><span style="color: #008000;">->IPMI在物理机可以成功，虚拟机不行</span>
<span style="color: #000000;">
[root@KVM </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> ipmitool sdr type Temperature</span>
Temp             | 01h | ns  |  3.1 |<span style="color: #000000;"> Disabled
Temp             </span>| 02h | ns  |  3.2 |<span style="color: #000000;"> Disabled
Temp             </span>| 05h | ns  | 10.1 |<span style="color: #000000;"> Disabled
Temp             </span>| 06h | ns  | 10.2 |<span style="color: #000000;"> Disabled
Ambient Temp     </span>| 0Eh | ok  |  7.1 | 22<span style="color: #000000;"> degrees C
Planar Temp      </span>| 0Fh | ns  |  7.1 |<span style="color: #000000;"> Disabled
IOH THERMTRIP    </span>| 5Dh | ns  |  7.1 |<span style="color: #000000;"> Disabled
CPU Temp Interf  </span>| 76h | ns  |  7.1 |<span style="color: #000000;"> Disabled
Temp             </span>| 0Ah | ns  |  8.1 |<span style="color: #000000;"> Disabled
Temp             </span>| 0Bh | ns  |  8.1 |<span style="color: #000000;"> Disabled
Temp             </span>| 0Ch | ns  |  8.1 | Disabled</pre>
  </div>
</div>

### <span id="133_cpu">1.3.3 查看cpu相关</span>

　　lscpu、uptime、top、htop vmstat mpstat

&nbsp;&nbsp; 其中htop需要安装，安装依赖与epel源。

<div>
  <div class="cnblogs_code">
    <pre>[znix@clsn ~<span style="color: #000000;">]$lscpu
Architecture:          x86_64
CPU op</span>-mode(s):        32-bit, 64-<span style="color: #000000;">bit
Byte Order:            Little Endian
CPU(s):                </span>1<span style="color: #000000;">
On</span>-<span style="color: #000000;">line CPU(s) list:   0
Thread(s) per core:    </span>1<span style="color: #000000;">
Core(s) per socket:    </span>1<span style="color: #000000;">
Socket(s):             </span>1<span style="color: #000000;">
NUMA node(s):          </span>1<span style="color: #000000;">
Vendor ID:             GenuineIntel
CPU family:            </span>6<span style="color: #000000;">
Model:                 </span>85<span style="color: #000000;">
Model name:            Intel(R) Xeon(R) Platinum </span>8163 CPU @ 2<span style="color: #000000;">.50GHz
Stepping:              </span>4<span style="color: #000000;">
CPU MHz:               </span>2494.150<span style="color: #000000;">
BogoMIPS:              </span>4988.30<span style="color: #000000;">
Hypervisor vendor:     KVM
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              33792K
NUMA node0 CPU(s):     0</span></pre>
  </div>
</div>

### <span id="134">1.3.4 内存够不够可以用</span>

　　free

<div>
  <div class="cnblogs_code">
    <pre>[znix@clsn ~]$free -<span style="color: #000000;">h
             total       used       free     shared    buffers     cached
Mem:          996M       867M       128M       712K       145M       450M
</span>-/+ buffers/<span style="color: #000000;">cache:       271M       725M
Swap:         </span>1.0G         0B       1.0G</pre>
  </div>
</div>

### <span id="135">1.3.5 磁盘剩多少写的快不快可以用</span>

　　df、dd、iotop

<div>
  <div class="cnblogs_code">
    <pre>[znix@clsn ~]$df -<span style="color: #000000;">h
Filesystem      Size  Used Avail Use</span>%<span style="color: #000000;"> Mounted on
</span>/dev/vda1        40G   24G   15G  62% /<span style="color: #000000;">
tmpfs           499M   20K  499M   </span>1% /dev/<span style="color: #000000;">shm
</span>/dev/vdb1        20G  4.4G   15G  24% /data</pre>
  </div>
</div>

### <span id="136">1.3.6 监控网络</span>

　　iftop nethogs

<div>
  <div class="cnblogs_code">
    <pre>iftop   监控主机间流量  -<span style="color: #000000;">i 指定监控网卡
nethogs 监控进程流量</span></pre>
  </div>
</div>

## <span id="14">1.4 监控工具总览</span>

　　mrtg 流量监控出图

　　nagios 监控

　　cacti&nbsp; 流量监控出图

**　<span style="background-color: #ffff00;">　zabbix </span>**<span style="background-color: #ffff00;"><strong>监控+</strong><strong>出图</strong></span>

## <span id="15_zabbix">1.5 zabbix介绍</span>

　　Zabbix 是由 Alexei Vladishev 开发的一种网络监视、管理系统，基于 Server-Client 架构。可用于监视各种网络服务、服务器和网络机器等状态。

　　使用各种 Database-end 如 MySQL, PostgreSQL, SQLite, Oracle 或 IBM DB2 储存资料。Server 端基于 C语言、Web 管理端 frontend 则是基于 PHP 所制作的。Zabbix 可以使用多种方式监视。可以只使用 Simple Check 不需要安装 Client 端，亦可基于 SMTP 或 HTTP ... 各种协定做死活监视。

　　在客户端如 UNIX, Windows 中安装 Zabbix Agent 之后，可监视 CPU Load、网络使用状况、硬盘容量等各种状态。而就算没有安装 Agent 在监视对象中，Zabbix 也可以经由 SNMP、TCP、ICMP、利用 IPMI、SSH、telnet 对目标进行监视。

另外，Zabbix 包含 XMPP 等各种 Item 警示功能。

### <span id="151_zabbix">1.5.1 zabbix的组成</span>

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123164838290-262268018.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

zabbix官网: https://www.zabbix.com

zabbix 主要由2部分构成 zabbix server和 zabbix agent

zabbix proxy是用来管理其他的agent，作为代理

### <span id="152_zabbix">1.5.2 zabbix监控范畴</span>

　　&sup2;&nbsp; 硬件监控 ：Zabbix IPMI Interface

　　&sup2;&nbsp; 系统监控 ：Zabbix Agent Interface

　　&sup2;&nbsp; Java 监控：ZabbixJMX Interface

　　&sup2;&nbsp; 网络设备监抟：Zabbix SNMP Interface

　　&sup2;&nbsp; 应用服务监控：Zabbix Agent UserParameter

　　&sup2;&nbsp; MySQL 数据库监控：percona-monitoring-pldlgins

　　&sup2;&nbsp; URL监控：Zabbix Web监控

# <span id="2_zabbix">第2章 安装zabbix</span>

## <span id="21">2.1 环境检查</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/redhat-release</span>
CentOS Linux release 7.4.1708<span style="color: #000000;"> (Core)

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r</span>
3.10.0-693<span style="color: #000000;">.el7.x86_64

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> getenforce</span>
<span style="color: #000000;">Disabled

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status firewalld.service</span>
● firewalld.service - firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(</span>1)</pre>
  </div>
</div>

## <span id="22_zabbix">2.2 安装zabbix过程</span>

### <span id="221">2.2.1 安装方式选择</span>

　　编译安装 （服务较多，环境复杂）

　　yum安装（干净环境）

　　使用yum 需要镜像yum源 http://www.cnblogs.com/clsn/p/7866643.html

### <span id="222">2.2.2 服务端快速安装脚本</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008000;">#</span><span style="color: #008000;">!/bin/bash</span><span style="color: #008000;">
#</span><span style="color: #008000;">clsn</span>

<span style="color: #008000;">#</span><span style="color: #008000;">设置解析 注意：网络条件较好时，可以不用自建yum源</span><span style="color: #008000;">
#</span><span style="color: #008000;"> echo '10.0.0.1 mirrors.aliyuncs.com mirrors.aliyun.com repo.zabbix.com' >> /etc/hosts</span>

<span style="color: #008000;">#</span><span style="color: #008000;">安装zabbix源、aliyun YUM源</span>
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6<span style="color: #000000;">.repo
curl </span>-o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-6<span style="color: #000000;">.repo
rpm </span>-ivh http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1<span style="color: #000000;">.el7.noarch.rpm

</span><span style="color: #008000;">#</span><span style="color: #008000;">安装zabbix </span>
yum install -y zabbix-server-mysql zabbix-web-<span style="color: #000000;">mysql

</span><span style="color: #008000;">#</span><span style="color: #008000;">安装启动 mariadb数据库</span>
yum install -y  mariadb-<span style="color: #000000;">server
systemctl start mariadb.service

</span><span style="color: #008000;">#</span><span style="color: #008000;">创建数据库</span>
mysql -e <span style="color: #800000;">'</span><span style="color: #800000;">create database zabbix character set utf8 collate utf8_bin;</span><span style="color: #800000;">'</span><span style="color: #000000;">
mysql </span>-e <span style="color: #800000;">'</span><span style="color: #800000;">grant all privileges on zabbix.* to zabbix@localhost identified by "zabbix";</span><span style="color: #800000;">'</span>

<span style="color: #008000;">#</span><span style="color: #008000;">导入数据</span>
zcat /usr/share/doc/zabbix-server-mysql-3.0.13/create.sql.gz|mysql -uzabbix -<span style="color: #000000;">pzabbix zabbix

</span><span style="color: #008000;">#</span><span style="color: #008000;">配置zabbixserver连接mysql</span>
sed -i.ori <span style="color: #800000;">'</span><span style="color: #800000;">115a DBPassword=zabbix</span><span style="color: #800000;">'</span> /etc/zabbix/<span style="color: #000000;">zabbix_server.conf

</span><span style="color: #008000;">#</span><span style="color: #008000;">添加时区</span>
sed -i.ori <span style="color: #800000;">'</span><span style="color: #800000;">18a php_value date.timezone  Asia/Shanghai</span><span style="color: #800000;">'</span> /etc/httpd/conf.d/<span style="color: #000000;">zabbix.conf

</span><span style="color: #008000;">#</span><span style="color: #008000;">解决中文乱码</span>
yum -y install wqy-microhei-<span style="color: #000000;">fonts
\cp </span>/usr/share/fonts/wqy-microhei/wqy-microhei.ttc /usr/share/fonts/dejavu/<span style="color: #000000;">DejaVuSans.ttf

</span><span style="color: #008000;">#</span><span style="color: #008000;">启动服务</span>
systemctl start zabbix-<span style="color: #000000;">server
systemctl start httpd

</span><span style="color: #008000;">#</span><span style="color: #008000;">写入开机自启动</span>
chmod +x /etc/rc.d/<span style="color: #000000;">rc.local
cat </span>>>/etc/rc.d/rc.local&lt;&lt;<span style="color: #000000;">EOF
systemctl start mariadb.service
systemctl start httpd
systemctl start zabbix</span>-<span style="color: #000000;">server
EOF

</span><span style="color: #008000;">#</span><span style="color: #008000;">输出信息</span>
echo <span style="color: #800000;">"</span><span style="color: #800000;">浏览器访问 http://`hostname -I|awk '{print $1}'`/zabbix</span><span style="color: #800000;">"</span></pre>
  </div>
</div>

### <span id="223">2.2.3 客户端快速部署脚本</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008000;">#</span><span style="color: #008000;">!/bin/bash</span><span style="color: #008000;">
#</span><span style="color: #008000;">clsn</span>

<span style="color: #008000;">#</span><span style="color: #008000;">设置解析</span>
echo <span style="color: #800000;">'</span><span style="color: #800000;">10.0.0.1 mirrors.aliyuncs.com mirrors.aliyun.com repo.zabbix.com</span><span style="color: #800000;">'</span> >> /etc/<span style="color: #000000;">hosts

</span><span style="color: #008000;">#</span><span style="color: #008000;">安装zabbix源、aliyu nYUM源</span>
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6<span style="color: #000000;">.repo
curl </span>-o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-6<span style="color: #000000;">.repo
rpm </span>-ivh http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1<span style="color: #000000;">.el7.noarch.rpm

</span><span style="color: #008000;">#</span><span style="color: #008000;">安装zabbix客户端</span>
yum install zabbix-agent -<span style="color: #000000;">y
sed </span>-i.ori <span style="color: #800000;">'</span><span style="color: #800000;">s#Server=127.0.0.1#Server=172.16.1.61#</span><span style="color: #800000;">'</span> /etc/zabbix/<span style="color: #000000;">zabbix_agentd.conf
systemctl start  zabbix</span>-<span style="color: #000000;">agent.service

</span><span style="color: #008000;">#</span><span style="color: #008000;">写入开机自启动</span>
chmod +x /etc/rc.d/<span style="color: #000000;">rc.local
cat </span>>>/etc/rc.d/rc.local&lt;&lt;<span style="color: #000000;">EOF
systemctl start  zabbix</span>-<span style="color: #000000;">agent.service
EOF</span></pre>
  </div>
</div>

## <span id="23">2.3 检测连通性</span>

### <span id="231_zabbix-get">2.3.1 服务端安装zabbix-get检测工具</span>

<div>
  <div class="cnblogs_code">
    <pre>yum install zabbix-get</pre>
  </div>
</div>

### <span id="232">2.3.2 在服务端进行测试</span>

注意：只能在服务端进行测试

<div>
  <div class="cnblogs_code">
    <pre>zabbix_get -s 172.16.1.61 -p 10050 -k <span style="color: #800000;">"</span><span style="color: #800000;">system.cpu.load[all,avg1]</span><span style="color: #800000;">"</span><span style="color: #000000;">
zabbix_get </span>-s 172.16.1.21 -p 10050 -k <span style="color: #800000;">"</span><span style="color: #800000;">system.cpu.load[all,avg1]</span><span style="color: #800000;">"</span></pre>
  </div>
</div>

**_<span style="text-decoration: underline;">测试结果</span>_**

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.61 -p 10050 -k "system.cpu.load[all,avg1]"</span>
0.000000<span style="color: #000000;">

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "system.cpu.load[all,avg1]"</span>
0.000000</pre>
  </div>
</div>

# <span id="3_web">第3章 web界面操作</span>

## <span id="31_zabbixweb">3.1 zabbix的web安装</span>

### <span id="311">3.1.1 使用浏览器访问</span>

<div>
  <p class="a3">
    　<em>　http://10.0.0.61/zabbix/setup.php</em>
  </p>
</div>

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165129915-1709791299.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 在检测信息时，可查看具体的报错信息进行不同的解决

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165144836-1283273644.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 选择mysql数据库，输入密码即可

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165152977-895376822.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; host与port不需要修改，name自定义

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165200477-1599173970.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

确认信息,正确点击下一步

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165208680-1427555138.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 安装完成、点击finsh

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165214680-2000908902.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 进入登陆界面&nbsp; 账号**<span style="text-decoration: underline;">Admin</span>**密码**<span style="text-decoration: underline;">zabbix</span>&nbsp;&nbsp;** **注意A****大写**

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165223211-1228760809.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

## <span id="32">3.2 添加监控信息</span>

### <span id="321_zabbix_server">3.2.1 修改监控管理机zabbix server</span>

配置 >> 主机

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165230774-1531918261.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

<div>
  <p class="a3">
    主机名称： 要与主机名相同，这是zabbix server程序用的
  </p>
  
  <p class="a3">
    可见名称： 显示在zabbix网页上的，给我们看的
  </p>
</div>

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165237899-1084816018.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

&nbsp;&nbsp; 修改后，要将下面的已启用要勾上

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165247196-1455730420.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 添加完成就有了管理机的监控主机

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165256383-843667682.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="322">3.2.2 添加新的主机</span>

配置 >> 主机 >> 创建主机

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165304852-1665046481.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

注意勾选以启用

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165312352-145910641.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 然后添加模板，选择linux OS ，先点小添加，再点大添加。

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165322399-2072948610.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 添加完成，将会又两条监控主机信息

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165329915-280518565.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="323">3.2.3 查看监控内容</span>

检测中 &nbsp;>> 最新数据

&nbsp;&nbsp; 在最新数据中需要筛选，

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165409336-1358964269.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 输入ip或者名字都能够搜索出来

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165415821-1695498925.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

在下面就会列出所有的监控项

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165432227-67525405.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="324">3.2.4 查看图像</span>

检测中 >> 图形

&nbsp;&nbsp; 选择正确的主机。选择要查看的图形即可出图

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165443274-1821091326.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

# <span id="4">第4章 自定义监控与监控报警</span>

## <span id="41">4.1 自定义监控</span>

### <span id="411">4.1.1 说明</span>

zabbix自带模板Template OS Linux (Template App Zabbix Agent)提供CPU、内存、磁盘、网卡等常规监控，只要新加主机关联此模板，就可自动添加这些监控项。

**需求：**<span style="text-decoration: underline;">服务器登陆人数不能超过三人，超过三人报警</span>

### <span id="412">4.1.2 预备知识</span>

自定义key能被server和agent认可

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 正确的key</span>
[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "system.uname"</span>
Linux cache01 3.10.0-693.el7.x86_64 <span style="color: #008000;">#</span><span style="color: #008000;">1 SMP Tue Aug 22 21:09:27 UTC 2017 x86_64</span>&nbsp;</pre>
  </div>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 没有登记的，自定义的key</span>
[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "login-user"</span>
ZBX_NOTSUPPORTED: Unsupported item key.&nbsp;</pre>
  </div>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 写错的key</span>
[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "system.uname1"</span>
ZBX_NOTSUPPORTED: Unsupported item key.</pre>
  </div>
</div>

## <span id="42">4.2 实现自定义监控</span>

### <span id="421">4.2.1 自定义语法</span>

<div>
  <div class="cnblogs_code">
    <pre>UserParameter=&lt;key>,&lt;shell command><span style="color: #000000;">
UserParameter</span>=login-user,who|wc -<span style="color: #000000;">l
UserParameter</span>=login-user,/bin/sh /server/scripts/login.sh</pre>
  </div>
</div>

### <span id="422_agent">4.2.2 agent注册</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd /etc/zabbix/zabbix_agentd.d/</span>
<span style="color: #000000;">
[root@cache01 zabbix_agentd.d]</span><span style="color: #008000;">#</span><span style="color: #008000;"> vim userparameter_login.conf</span>
UserParameter=login-user,who|wc -<span style="color: #000000;">l
UserParameter</span>=login-user2,who|wc -<span style="color: #000000;">l
UserParameter</span>=login-user3,who|wc -l</pre>
  </div>
</div>

&nbsp;&nbsp; **注意：**key名字要唯一，多个key以行为分割

<div>
  <p class="ac">
    # 修改完成后重启服务
  </p>
  
  <div class="cnblogs_code">
    <pre>[root@cache01 zabbix_agentd.d]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl restart zabbix-agent.service</span></pre>
  </div>
</div>

&nbsp;&nbsp; 在server端进行get测试

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "login-user"</span>
3<span style="color: #000000;">

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "login-user2"</span>
3<span style="color: #000000;">

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "login-user3"</span>
3<span style="color: #000000;">

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.21 -p 10050 -k "login-user4"</span>
ZBX_NOTSUPPORTED: Unsupported item key.</pre>
  </div>
</div>

### <span id="423_serverweb">4.2.3 在server端注册(web操作)</span>

**①&nbsp;&nbsp;** **创建模板**

<span style="text-decoration: underline;">配置 >> </span><span style="text-decoration: underline;">模板 >> </span><span style="text-decoration: underline;">创建模板</span>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165620586-851212835.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

点击添加，即可创建出来模板

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165631508-690881873.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 查看创建出来的模板。&uarr;

**②&nbsp;&nbsp;** **创建应用集**

应用集类似(目录/文件夹)，其作用是给监控项分类。

<span style="text-decoration: underline;">点击 </span><span style="text-decoration: underline;">应用集 >> </span><span style="text-decoration: underline;">创建应用集</span>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165642571-891321995.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 自定义应用集的名称，然后点击添加

**③&nbsp;&nbsp;** **创建监控项**

<span style="text-decoration: underline;">监控项 >> </span><span style="text-decoration: underline;">创建监控项</span>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165654165-1565485900.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

**键值** -- key,即前面出创建的login-user。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165702071-1170253454.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 注意：创建监控项的时候，注意选择上应用集，即之前创建的安全。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165711852-864078357.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

**④&nbsp;&nbsp;** **创建触发器**

触发器的作用：当监控项获取到的值达到一定条件时就触发报警

_<span style="text-decoration: underline;">(</span>__<span style="text-decoration: underline;">根据需求创建)</span>_

<span style="text-decoration: underline;">触发器 >> </span><span style="text-decoration: underline;">创建触发器</span>

创建触发器，自定义名称，该名称是报警时显示的名称。

&nbsp;&nbsp; 表达式，点击右边的添加，选择**表达式**。&nbsp;

&nbsp;&nbsp; 严重性自定义。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165725977-1129776976.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; **表达式的定义** **&darr;** **，选择**之前创建的监控项，

最新的T值为当前获取到的值。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165735415-365506827.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 添加完成，能够在触发器中看到添加的情况

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165748336-296817387.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

**⑤&nbsp;&nbsp;** **创建图形**

以图形的方式展示出来监控信息

<span style="text-decoration: underline;">图形 >> </span><span style="text-decoration: underline;">创建图形</span>

名称自定义，关联上监控项。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165759430-719975746.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

**⑥&nbsp;&nbsp;** **主机关联模板**

<span style="text-decoration: underline;">配置 >> </span><span style="text-decoration: underline;">主机</span>

&nbsp;&nbsp; 一个主机可以关联多个模板

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165807899-367891158.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="424">4.2.4 查看监控的图形</span>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165818196-1388422323.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

## <span id="43">4.3 监控报警</span>

### <span id="431">4.3.1 第三方报警平台</span>

http://www.**onealert**.com

&nbsp;&nbsp; 　 通过 OneAlert 提供的通知分派与排班策略，以及全方位的短信、微信、QQ、电话提醒服务，您可以在最合适的时间，将最重要的信息推送给最合适的人员。

### <span id="432_onealert">4.3.2 onealert配置</span>

添加应用，注意添加的是zabbix

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165835258-571043802.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 实现微信报警需要关注微信公众号即可。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165843243-961963757.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="433_onealert_Agent">4.3.3 安装 onealert Agent</span>

1.切换到zabbix脚本目录(如何查看zabbix脚本目录)：

<div>
  <div class="cnblogs_code">
    <pre>cd /usr/local/zabbix-server/share/zabbix/<span style="color: #000000;">alertscripts

</span><span style="color: #008000;">#</span><span style="color: #008000;">查看zabbix脚本目录</span>
vi /etc/zabbix/<span style="color: #000000;">zabbix_server.conf
查看AlertScriptsPath</span></pre>
  </div>
</div>

2.获取OneITSM agent包：

<div>
  <div class="cnblogs_code">
    <pre>wget http://www.onealert.com/agent/release/oneitsm_zabbix_release-1.0.1.tar.gz</pre>
  </div>
</div>

3.解压、安装。

<div>
  <div class="cnblogs_code">
    <pre>tar -zxf oneitsm_zabbix_release-1.0.1<span style="color: #000000;">.tar.gz
cd oneitsm</span>/<span style="color: #000000;">bin
bash install.sh </span>--<span style="color: #008000;">#</span><span style="color: #008000;">个人生成的key</span></pre>
  </div>
</div>

注：在安装过程中根据安装提示，**输入****zabbix****管理地址、管理员用户名、密码**。

<div>
  <div class="cnblogs_code">
    <pre>Zabbix管理地址: http://10.0.0.61/zabbix/<span style="color: #000000;">
Zabbix管理员账号: Admin
Zabbix管理员密码:</span></pre>
  </div>
</div>

4.当提示"安装成功"时表示安装成功!

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">验证告警集成

产生新的zabbix告警(problem),动作状态为&ldquo;已送达&rdquo;表示集成成功。</span></pre>
  </div>
</div>

### <span id="431_onealert_Agent">4.3.1 如何删除onealert Agent</span>

①&nbsp; 删除报警媒介类型中的脚本

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165948977-426444787.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

②&nbsp; 删除创建的用户

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123165955852-1528016746.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

③&nbsp; 删除用户群组

<p style="text-align: center;">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170003821-2087815115.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

④&nbsp; 删除创建的动作

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170016071-1437693768.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="432">4.3.2 触发器响应，发送报警信息</span>

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170023977-1957213587.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 在微信和邮件中，均能收到报警信息。

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170030383-1285440863.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; **注意：当状态改变的时候才会发邮件**

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp;　　 <span style="background-color: #00ff00;">好-->坏</span>
  </p>
  
  <p class="a3">
    　　&nbsp; &nbsp; <span style="background-color: #00ff00;">坏-->好</span>
  </p>
</div>

## <span id="44">4.4 监控可视化</span>

### <span id="441">4.4.1 聚合图形</span>

<span style="text-decoration: underline;">最新数据 >> </span><span style="text-decoration: underline;">图形</span>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170055649-256096162.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 自定义名称

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170105915-85092776.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 点击聚合图形的名称，进行更改，添加要显示的图形即可。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170116883-527462565.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="442">4.4.2 幻灯片</span>

添加幻灯片

<span style="text-decoration: underline;">监测中 >> </span><span style="text-decoration: underline;">复合图形 >> </span><span style="text-decoration: underline;">幻灯片演示</span>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170126399-1885224195.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 创建幻灯片，名称自定，选择要显示的

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170138008-232612835.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 幻灯片根据设定的时间自动播放

## <span id="45">4.5 模板的共享</span>

### <span id="451">4.5.1 主机共享</span>

在主机页打开，全选后点击导出

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170147836-1807397032.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 导入

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170154024-1119374996.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="452">4.5.2 模板共享</span>

**https://github.com/zhangyao8/zabbix-community-repos**

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170205430-2041676107.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

# <span id="5">第5章 监控全网服务器</span>

## <span id="51">5.1 需求说明</span>

实际需求：

　　公司已经有了100台服务器，现在需要使用zabbix全部监控起来。

## <span id="52">5.2 规划方案</span>

常规监控：cpu，内存，磁盘，网卡&nbsp; 问题：怎样快速添加100台机器

&nbsp;&nbsp; 　　方法1：使用克隆的方式

&nbsp;　　&nbsp; 方法2：自动注册和自动发现

&nbsp;　　&nbsp; 方法3：调用zabbix api接口&nbsp; curl 、python

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;　　 &nbsp;开发自己的运维平台兼容zabbix的通道

**　　　服务监控，url****监控等特殊监控**：自定义监控

### <span id="521_apicurl">5.2.1 api接口使用（<em>curl</em>）</span>

<div>
  <div class="cnblogs_code">
    <pre>    curl -i -X POST -H <span style="color: #800000;">'</span><span style="color: #800000;">Content-Type:application/json</span><span style="color: #800000;">'</span> -d<span style="color: #800000;">'</span><span style="color: #800000;">{"jsonrpc": "2.0","method":"user.login","params":{"user":"Admin","password":"zabbix"},"auth": null,"id":0}</span><span style="color: #800000;">'</span> <span style="color: #800000;">"</span><span style="color: #800000;">http://10.0.0.61/zabbix/api_jsonrpc.php</span><span style="color: #800000;">"</span><span style="color: #000000;">

    curl </span>-i -X POST -H <span style="color: #800000;">'</span><span style="color: #800000;">Content-Type:application/json</span><span style="color: #800000;">'</span> -d<span style="color: #800000;">'
</span><span style="color: #000000;">    {
        </span><span style="color: #800000;">"</span><span style="color: #800000;">jsonrpc</span><span style="color: #800000;">"</span>: <span style="color: #800000;">"</span><span style="color: #800000;">2.0</span><span style="color: #800000;">"</span><span style="color: #000000;">,
        </span><span style="color: #800000;">"</span><span style="color: #800000;">method</span><span style="color: #800000;">"</span>: <span style="color: #800000;">"</span><span style="color: #800000;">host.get</span><span style="color: #800000;">"</span><span style="color: #000000;">,
        </span><span style="color: #800000;">"</span><span style="color: #800000;">params</span><span style="color: #800000;">"</span><span style="color: #000000;">: {
            </span><span style="color: #800000;">"</span><span style="color: #800000;">output</span><span style="color: #800000;">"</span><span style="color: #000000;">: [
                </span><span style="color: #800000;">"</span><span style="color: #800000;">hostid</span><span style="color: #800000;">"</span><span style="color: #000000;">,
                </span><span style="color: #800000;">"</span><span style="color: #800000;">host</span><span style="color: #800000;">"</span><span style="color: #000000;">
            ],
            </span><span style="color: #800000;">"</span><span style="color: #800000;">selectInterfaces</span><span style="color: #800000;">"</span><span style="color: #000000;">: [
                </span><span style="color: #800000;">"</span><span style="color: #800000;">interfaceid</span><span style="color: #800000;">"</span><span style="color: #000000;">,
                </span><span style="color: #800000;">"</span><span style="color: #800000;">ip</span><span style="color: #800000;">"</span><span style="color: #000000;">
            ]
        },
        </span><span style="color: #800000;">"</span><span style="color: #800000;">id</span><span style="color: #800000;">"</span>: 2<span style="color: #000000;">,
        </span><span style="color: #800000;">"</span><span style="color: #800000;">auth</span><span style="color: #800000;">"</span>: <span style="color: #800000;">"</span><span style="color: #800000;">6a450a8fc3dce71fd310cfe338746578</span><span style="color: #800000;">"</span><span style="color: #000000;">
    }</span><span style="color: #800000;">'</span><span style="color: #800000;"> "http://10.0.0.61/zabbix/api_jsonrpc.php"</span></pre>
  </div>
</div>

## <span id="53">5.3 具体实施规划</span>

### <span id="531">5.3.1 硬件、系统、网络监控</span>

　　所有集群节点（所有虚拟机）都监控上

　　交换机，路由器监控（简单方法：换成端口对应服务器网卡流量监控；标准方法：监控交换机的网卡）

　　snmp监控

### <span id="532">5.3.2 应用服务监控</span>

1. 监控备份服务器，简单方法是监控rsync端口，如果有其他更佳方案可以说明；

<div>
  <div class="cnblogs_code">
    <pre>    方法1：监控873端口net.tcp.port[,873<span style="color: #000000;">]
    方法2：模拟推送拉取文件</span></pre>
  </div>
</div>

2. 监控NFS服务器，使用监控NFS进程来判断NFS服务器正常，如果有其他更佳方案可以说明；

<div>
  <div class="cnblogs_code">
    <pre>    方法1：端口（通过111的rpc端口获取nfs端口） net.tcp.port[,111<span style="color: #000000;">]
    方法2：showmount </span>-e ip|wc -l</pre>
  </div>
</div>

3. 监控MySQL服务器，简单方法监控mysql的3306端口，或者使用zabbix提供的Mysql模板，如果有其他更佳方案可以说明；

<div>
  <div class="cnblogs_code">
    <pre>    方法1：端口（通过3306的mysql端口） net.tcp.port[,3306<span style="color: #000000;">]
    方法2：mysql远程登录
    方法3：使用zabbix agent自带的模板及key</span></pre>
  </div>
</div>

4. 监控2台web服务器，简单方法监控80端口，如果有其他更佳方案可以说明；

<div>
  <div class="cnblogs_code">
    <pre>    方法1：端口（通过80的web端口） net.tcp.port[,80<span style="color: #000000;">]
    方法2：看网页状态码、返回内容</span>==zabbix 自带WEB检测</pre>
  </div>
</div>

5. 监控URL地址来更精确的监控我们的网站运行正常；

<div>
  <div class="cnblogs_code">
    <pre>    使用zabbix自带的监控Web监测 进行监控</pre>
  </div>
</div>

6. 监控反向代理服务器，PPTP服务器等你在期中架构部署的服务。

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">nginx，pptp
ntp 端口udp </span>123</pre>
  </div>
</div>

7. 监控Nginx的7种连接状态。

<div>
  <div class="cnblogs_code">
    <pre>    自定义监控</pre>
  </div>
</div>

### <span id="533">5.3.3 监控服务通用方法</span>

　　1. 监控端口 netstat ss lsof&nbsp; ==》 wc -l

　　2. 监控进程 ps -ef|grep 进程|wc -l&nbsp; 试运行一下

　　3. 模拟客户端的使用方式监控服务端

&nbsp;&nbsp;　　 &nbsp;&nbsp; web&nbsp; ==》 curl

&nbsp;&nbsp; &nbsp;&nbsp;　　 mysql ==》 select insert

&nbsp;&nbsp; 　　&nbsp;&nbsp; memcache ==》 set再get

## <span id="54">5.4 实施全网监控</span>

_安装客户端脚本，for centos6_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008000;">#</span><span style="color: #008000;">!/bin/bash</span>

<span style="color: #008000;">#</span><span style="color: #008000;">设置解析</span><span style="color: #008000;">
#</span><span style="color: #008000;"> echo '10.0.0.1 mirrors.aliyuncs.com mirrors.aliyun.com repo.zabbix.com' >> /etc/hosts</span>

<span style="color: #008000;">#</span><span style="color: #008000;">安装zabbix源、aliyu nYUM源</span>
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6<span style="color: #000000;">.repo
curl </span>-o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-6<span style="color: #000000;">.repo
rpm </span>-ivh http://repo.zabbix.com/zabbix/3.0/rhel/6/x86_64/zabbix-release-3.0-1<span style="color: #000000;">.el6.noarch.rpm

yum clean all
yum clean all
</span><span style="color: #008000;">#</span><span style="color: #008000;">安装zabbix客户端</span>
yum install zabbix-agent -<span style="color: #000000;">y
sed </span>-i.ori <span style="color: #800000;">'</span><span style="color: #800000;">s#Server=127.0.0.1#Server=172.16.1.61#</span><span style="color: #800000;">'</span> /etc/zabbix/<span style="color: #000000;">zabbix_agentd.conf
</span>/etc/init.d/zabbix-<span style="color: #000000;">agent start

</span><span style="color: #008000;">#</span><span style="color: #008000;">写入开机自启动</span>
chmod +x /etc/rc.d/<span style="color: #000000;">rc.local
cat </span>>>/etc/rc.d/rc.local&lt;&lt;<span style="color: #000000;">EOF
</span>/etc/init.d/zabbix-<span style="color: #000000;">agent start
EOF</span></pre>
  </div>
</div>

### <span id="541">5.4.1 使用自动发现规则</span>

添加自动发现规则

<p style="text-align: center;">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170517165-1405132488.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

&nbsp;&nbsp; 创建发现动作

<p style="text-align: center;">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170548790-1913820945.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

&nbsp;&nbsp; 查看自动发现的机器。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170556196-399583243.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="542">5.4.2 监控备份服务器</span>

利用系统自带键值进行监控_<span style="text-decoration: underline;">net.tcp.listen[port]</span>_ 创建新的模板

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170605461-15380890.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

在服务端进行测试

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.41 -p 10050 -k "net.tcp.listen[873]"</span>
1

<span style="color: #008000;">#</span><span style="color: #008000;"> 1为端口在监听 0为端口未监听</span></pre>
  </div>
</div>

将模板添加到主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170623211-1934949165.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="543_NFS">5.4.3 监控NFS服务器</span>

创建nfs监控模板

使用_ <span style="text-decoration: underline;">proc.num[<name>,<user>,<state>,<cmdline>]</span>_&nbsp; 键值，检测nfs进程的数量

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170642743-1530283916.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

在服务端进行测试

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.31 -p 10050 -k "proc.num[,,,rpc]"</span>
5<span style="color: #000000;">

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.31 -p 10050 -k "proc.num[nfsd,,,]</span>
8</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

将模板绑定到主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170706790-398071765.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="544_MySQL">5.4.4 监控MySQL服务器</span>

将自带的mysqlkey值加上mysql的账户密码，否则不能获取到数据。

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170716633-1544519962.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

使用系统自带模板 &nbsp;net.tcp.port[<ip>,port] 利用自带的监控端口键值进行监控

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170725196-1197139845.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

添加新的mysql监控项端口

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170733586-1565655135.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.51 -p 10050 -k "net.tcp.port[,3306]"</span>
1

<span style="color: #008000;">#</span><span style="color: #008000;">检查是否能建立 TCP 连接到指定端口。返回 0 - 不能连接；1 - 可以连接</span></pre>
  </div>
</div>

将模板关联到主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170751618-1705878150.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="545_web">5.4.5 监控web服务器</span>

创建监控模板 监控 nginx服务与 80 端口

<div>
  <div class="cnblogs_code">
    <pre>    proc.num[&lt;name>,&lt;user>,&lt;state>,&lt;cmdline><span style="color: #000000;">]   进程数。返回整数
    net.tcp.port[</span>&lt;ip>,port] 检查是否能建立 TCP 连接到指定端口。返回 0 - 不能连接；1 - 可以连接</pre>
  </div>
</div>

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170811836-1213059726.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.8 -p 10050 -k "proc.num[,,,nginx]"</span>
2<span style="color: #000000;">

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.8 -p 10050 -k "net.tcp.port[,80]"</span>
1</pre>
  </div>
</div>

将模板关联到主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170830836-1859599772.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="546_URL">5.4.6 监控URL地址</span>

创建监测页面

<div>
  <div class="cnblogs_code">
    <pre>echo ok >> /application/nginx/html/www/check.html</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

测试监控面页

<div>
  <div class="cnblogs_code">
    <pre>[root@web03 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> for ip in 7 8 9 ;do curl 10.0.0.$ip/check.html ;done</span>
<span style="color: #000000;">ok
ok
ok</span></pre>
  </div>
</div>

创建web监测模板

&nbsp;&nbsp; _<span style="text-decoration: underline;">创建应用集</span>_

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170901946-1613642326.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; _<span style="text-decoration: underline;">创建Web</span>__<span style="text-decoration: underline;">场景</span>_

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170913008-168117527.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; _<span style="text-decoration: underline;">创建图形</span>_

<p style="text-align: center;">
  <a href="/wp-content/themes/clsn-003/inc/go.php?url=http://10.0.0.61/zabbix/chart2.php?graphid=668&period=60&stime=20191122164643&updateProfile=1&profileIdx=web.screens&profileIdx2=668&width=1052&sid=8a1fe32c9614ae35&screenid=&curtime=1511340464305" ><img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170934040-1367666440.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" /></a><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://10.0.0.61/zabbix/chart2.php?graphid=668&period=60&stime=20191122164643&updateProfile=1&profileIdx=web.screens&profileIdx2=668&width=1052&sid=8a1fe32c9614ae35&screenid=&curtime=1511340464305" >&nbsp;</a>
</p>

将模板关联到主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170948165-1061354150.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

监测结果

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123170954774-1374734119.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="547">5.4.7 监控反向代理服务器</span>

创建自定义key

<div>
  <div class="cnblogs_code">
    <pre>[root@lb01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  /etc/zabbix/zabbix_agentd.d/userparameter_nk.conf</span>
UserParameter=keep-ip,ip a |grep 10.0.0.3|wc -l</pre>
  </div>
</div>

在服务端测试

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.5  -p 10050 -k "keep-ip"</span>
1<span style="color: #000000;">

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.6  -p 10050 -k "keep-ip"</span>
0</pre>
  </div>
</div>

在web界面添加模板

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171017774-684187486.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

将模板关联到主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171025993-90494591.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="548_Nginx7">5.4.8 监控Nginx的7种连接状态</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">nginx服务器显示status
&hellip;&hellip;
    location </span>/<span style="color: #000000;">status {
           stub_status on;
           access_log off;
    }
&hellip;&hellip;</span></pre>
</div>

&nbsp;

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> for ip in 7 8 9 ;do curl 172.16.1.$ip/status ;done</span>
Active connections: 1<span style="color: #000000;">
server accepts handled requests
 </span>73 73 69<span style="color: #000000;">
Reading: 0 Writing: </span>1<span style="color: #000000;"> Waiting: 0

Active connections: </span>1<span style="color: #000000;">
server accepts handled requests
 </span>134 134 127<span style="color: #000000;">
Reading: 0 Writing: </span>1<span style="color: #000000;"> Waiting: 0

Active connections: </span>1<span style="color: #000000;">
server accepts handled requests
 </span>7 7 7<span style="color: #000000;">
Reading: 0 Writing: </span>1 Waiting: 0</pre>
  </div>
</div>

在nginx服务器上添加key

<div>
  <div class="cnblogs_code">
    <pre>cat >/etc/zabbix/zabbix_agentd.d/userparameter_nginx_status.conf &lt;&lt;<span style="color: #800000;">'</span><span style="color: #800000;">EOF</span><span style="color: #800000;">'</span><span style="color: #000000;">
UserParameter</span>=nginx_active,curl -s  127.0.0.1/status|awk <span style="color: #800000;">'</span><span style="color: #800000;">/Active/ {print $NF}</span><span style="color: #800000;">'</span><span style="color: #000000;">
UserParameter</span>=nginx_accepts,curl -s  127.0.0.1/status|awk <span style="color: #800000;">'</span><span style="color: #800000;">NR==3 {print $1}</span><span style="color: #800000;">'</span><span style="color: #000000;">
UserParameter</span>=nginx_handled,curl -s  127.0.0.1/status|awk <span style="color: #800000;">'</span><span style="color: #800000;">NR==3 {print $2}</span><span style="color: #800000;">'</span><span style="color: #000000;">
UserParameter</span>=nginx_requests,curl -s  127.0.0.1/status|awk <span style="color: #800000;">'</span><span style="color: #800000;">NR==3 {print $3}</span><span style="color: #800000;">'</span><span style="color: #000000;">
UserParameter</span>=nginx_reading,curl -s  127.0.0.1/status|awk <span style="color: #800000;">'</span><span style="color: #800000;">NR==4 {print $2}</span><span style="color: #800000;">'</span><span style="color: #000000;">
UserParameter</span>=nginx_writing,curl -s  127.0.0.1/status|awk <span style="color: #800000;">'</span><span style="color: #800000;">NR==4 {print $4}</span><span style="color: #800000;">'</span><span style="color: #000000;">
UserParameter</span>=nginx_waiting,curl -s  127.0.0.1/status|awk <span style="color: #800000;">'</span><span style="color: #800000;">NR==4 {print $6}</span><span style="color: #800000;">'</span><span style="color: #000000;">
EOF</span></pre>
  </div>
</div>

服务端测试

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.7  -p 10050 -k "nginx_waiting"</span>
<span style="color: #000000;">0

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.8  -p 10050 -k "nginx_waiting"</span>
<span style="color: #000000;">0

[root@m01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> zabbix_get -s 172.16.1.9  -p 10050 -k "nginx_waiting"</span>
0</pre>
  </div>
</div>

在zabbix-web上添加

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171149430-1273155516.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

监控项

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171156868-200802360.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

添加图形

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171204665-213131354.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

将模板关联到主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171212368-480366619.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

查看添加的图形

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171219024-1107722962.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<p style="text-align: center;">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171227727-507986617.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

# <span id="6">第6章 自动发现与自动注册</span>

## <span id="61">6.1 自动注册与自动注册</span>

### <span id="611">6.1.1 简介</span>

**_<span style="text-decoration: underline;">自动发现：</span>_**

<div>
  <div class="cnblogs_code">
    <pre>zabbix Server主动发现所有客户端，然后将客户端登记自己的小本本上，缺点zabbix server压力山大（网段大，客户端多），时间消耗多。</pre>
  </div>
</div>

**_<span style="text-decoration: underline;">自动注册：</span>_**

<div>
  <div class="cnblogs_code">
    <pre>zabbix agent主动到zabbix Server上报到，登记；缺点agent有可能找不到Server（配置出错）</pre>
  </div>
</div>

### <span id="612">6.1.2 两种模式</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">被动模式：默认  agent被server抓取数据 （都是在agent的立场上说）
主动模式：agent主动将数据发到server端 （都是在agent的立场上说）</span></pre>
  </div>
  
  <p class="a3">
    <strong>&nbsp; &nbsp; &nbsp;<span style="background-color: #00ff00;">注意：</span> </strong><strong>两种模式都是在agent</strong><strong>上进行配置</strong>
  </p>
</div>

**&nbsp; &nbsp; &nbsp;zabbix** **的使用要在hosts****文件中预先做好主机名的解析**

## <span id="62">6.2 自动发现--被动模式</span>

　第一个里程碑：完成之前的安装

<div>
  <div class="cnblogs_code">
    <pre>zabbix Server安装完毕</pre>
  </div>
</div>

&nbsp;&nbsp; 第二个里程碑：配置agent客户端

<div>
  <div class="cnblogs_code">
    <pre>zabbix agent安装完毕，注意配置Server=172.16.1.61</pre>
  </div>
</div>

&nbsp;&nbsp; 第三个里程碑：在web界面上进行配置

<div>
  <div class="cnblogs_code">
    <pre>    web界面：配置 >> 自动发现 >><span style="color: #000000;"> Local network
        使用自带的自动发现规则（进行修改）即可</span></pre>
  </div>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171340696-1752663359.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">    在ip范围内输入ip，注意格式；
    延迟在实际的生产环境中要大一些，实验环境可以小一些</span></pre>
  </div>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171400430-1257844571.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; **创建发现动作**

<div>
  <div class="cnblogs_code">
    <pre>    配置 >> 动作 >> Auto discovery. Linux servers.</pre>
  </div>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171416243-643188941.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

①&nbsp; 配置动作

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171442665-1032645950.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

②&nbsp; 在条件中添加条件，让添加更准确

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171450352-773318655.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

③&nbsp; 在操作中添加

a)&nbsp; 添加主机与启用主机

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171459915-585599756.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <p class="a3">
    &nbsp;&nbsp;　　&nbsp; 然后等待者客户端自动上门就好?
  </p>
</div>

## <span id="63">6.3 自动注册--主动模式</span>

　第一个里程碑：zabbix Server安装完毕 （完成）

<div>
  <div class="cnblogs_code">
    <pre>zabbix Server安装完毕</pre>
  </div>
</div>

&nbsp;&nbsp; 第二个里程碑：zabbix agent安装完毕，需要额外增加的配置

<div>
  <div class="cnblogs_code">
    <pre>vim /etc/zabbix/<span style="color: #000000;">zabbix_agentd.conf
ServerActive</span>=172.16.1.61
<span style="color: #008000;">#</span><span style="color: #008000;"> Hostname=Zabbix server</span>
HostnameItem=<span style="color: #000000;">system.hostname
 
systemctl restart zabbix</span>-<span style="color: #000000;">agent.service
netstat </span>-tunlp|grep zabbix</pre>
  </div>
</div>

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 源文件与修改后对比
  </p>
</div>

<p style="text-align: center;">
  <img src="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171548352-671150030.png" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" /><img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171553258-1045944692.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

　&nbsp; &nbsp;第三个里程碑：在web见面上进行配置

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">1</span> 配置 >> 动作 >> 事件源(自动注册) >> 创建动作</pre>
  </div>
</div>

<p style="text-align: center;">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171620446-2129676737.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />&nbsp;
</p>

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 创建动作，添加名称即可
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171641633-1899137338.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 条件中也无需修改
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171650227-1035954322.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 在动作中添加动作
  </p>
  
  <p class="a3">
    （添加主机、添加到主机群组、链接到模板）
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171658743-2090662666.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 添加完动作后，等待就行了
  </p>
  
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 注意：重启客户端可以加速发现。但是在生产环境中勿用。
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171705680-1480125827.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

# <span id="7_SNMP">第7章 分布式监控与SNMP监控</span>

## <span id="71">7.1 分布式监控</span>

### <span id="711">7.1.1 作用</span>

&nbsp; 　　分担压力，减轻负载

&nbsp; 　　多机房监控

　<span style="background-color: #00ff00;">　zabbix Server&nbsp; ===》&nbsp; zabbix agent （只能同一个局域网监控）</span>

**_<span style="text-decoration: underline;">分担压力，降低负载</span>_**

<div>
  <div class="cnblogs_code">
    <pre>  zabbix Server ===》  zabbix proxy  ===<span style="color: #000000;">》zabbix agent1 agent2 agent3 。。。
    </span>172.16.1.61           172.16.1.21        172.16.1.0/24
                ===》  zabbix proxy  ===》zabbix agent4 agent5 agent6 。。。</pre>
  </div>
</div>

**_<span style="text-decoration: underline;">多机房监控</span>_**

<div>
  <div class="cnblogs_code">
    <pre>    zabbix Server(北京)           ==》  zabbix proxy（每个机房搭建）  ==<span style="color: #000000;">》 zabbix agent
    </span>122.71.240.233/172.16.1.61          122.71.241.11/172.16.2.21     172.16.2.0/24</pre>
  </div>
</div>

### <span id="712">7.1.2 环境说明</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">    zabbix server m01
    zabbix proxy cache01
    zabbix agent  cache01</span></pre>
  </div>
</div>

### <span id="713_zabbix_proxy">7.1.3 配置zabbix proxy</span>

**　第一个里程碑**：配置zabbix yum源，并安装proxy

<div>
  <div class="cnblogs_code">
    <pre>rpm -ivh http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1<span style="color: #000000;">.el7.noarch.rpm
yum install zabbix</span>-proxy-mysql -y</pre>
  </div>
</div>

**&nbsp;&nbsp;** **第二个里程碑：安装数据库**

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; zabbix&nbsp; proxy也需要数据库，这个数据库不是用于存储监控数据的 只是用于存储配置信息
  </p>
</div>

&nbsp;&nbsp; #安装数据库

<div>
  <div class="cnblogs_code">
    <pre>yum -y install mariadb-<span style="color: #000000;">server
systemctl start mariadb.service</span></pre>
  </div>
</div>

&nbsp;&nbsp; #建立数据库

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">mysql
create database zabbix_proxy character set utf8 collate utf8_bin;
grant all privileges on zabbix_proxy.</span>* to zabbix@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> identified by <span style="color: #800000;">'</span><span style="color: #800000;">zabbix</span><span style="color: #800000;">'</span><span style="color: #000000;">;
exit</span></pre>
  </div>
</div>

&nbsp;&nbsp; #导入数据文件

<div>
  <div class="cnblogs_code">
    <pre>zcat /usr/share/doc/zabbix-proxy-mysql-3.0.13/schema.sql.gz |mysql -uzabbix -pzabbix zabbix_proxy</pre>
  </div>
</div>

&nbsp;&nbsp; #配置zabbix proxy 连接数据库

<div>
  <div class="cnblogs_code">
    <pre>sed -i.ori <span style="color: #800000;">'</span><span style="color: #800000;">162a DBPassword=zabbix</span><span style="color: #800000;">'</span> /etc/zabbix/<span style="color: #000000;">zabbix_proxy.conf
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s#Server=127.0.0.1#Server=172.16.1.61#</span><span style="color: #800000;">'</span> /etc/zabbix/<span style="color: #000000;">zabbix_proxy.conf
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s#Hostname=Zabbix proxy#Hostname=cache01#</span><span style="color: #800000;">'</span> /etc/zabbix/<span style="color: #000000;">zabbix_proxy.conf

</span><span style="color: #008000;">#</span><span style="color: #008000;"> Hostname 作为后面添加的代理程序名称，要保持一致</span></pre>
  </div>
</div>

&nbsp;&nbsp; #启动

<div>
  <div class="cnblogs_code">
    <pre>systemctl restart zabbix-proxy.service</pre>
  </div>
</div>

&nbsp;&nbsp; #检查端口

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> netstat -lntup |grep zabbix</span>
tcp        0      0 0.0.0.0:10050     0.0.0.0:*       LISTEN      105762/<span style="color: #000000;">zabbix_agent
tcp        0      0 </span>0.0.0.0:10051   0.0.0.0:*         LISTEN      85273/<span style="color: #000000;">zabbix_proxy 
tcp6       0      0 :::</span>10050       :::*      LISTEN      105762/<span style="color: #000000;">zabbix_agent
tcp6       0      0 :::</span>10051  :::*           LISTEN      85273/zabbix_proxy </pre>
  </div>
</div>

&nbsp;&nbsp; **第三个里程碑：**修改agent配置指向 proxy

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep ^Server /etc/zabbix/zabbix_agentd.conf</span>
Server=172.16.1.61<span style="color: #000000;">
ServerActive</span>=172.16.1.61<span style="color: #000000;">

[root@cache01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed -i 's#172.16.1.61#172.16.1.21#g' /etc/zabbix/zabbix_agentd.conf</span>
<span style="color: #000000;">
[root@cache01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep ^Server /etc/zabbix/zabbix_agentd.conf</span>
Server=172.16.1.21<span style="color: #000000;">
ServerActive</span>=172.16.1.21<span style="color: #000000;">

[root@cache01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl restart zabbix-agent.service</span></pre>
  </div>
</div>

&nbsp;&nbsp; **第四个里程碑：**web界面添加代理

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 管理 >> agent代理程序 >> 创建代理
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171925430-1950514120.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 代理程序名称要填写主机名

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171933336-673235419.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 稍等片刻就能在程序中出现代理

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171940665-2068638027.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

&nbsp;&nbsp; 在主机中能发现主机代理

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123171948649-339116378.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

## <span id="72_SNMP">7.2 SNMP监控</span>

### <span id="721">7.2.1 使用范围</span>

　　无法安装agent&nbsp; 很多前辈的监控软件都可以监控各种设备&nbsp; 都是通过snmp监控

　　snmp simple network manager protocol 简单网络管理协议

&nbsp;&nbsp;　&nbsp;简单网络管理协议（SNMP），由一组网络管理的标准组成，包含一个应用层协议（application layer protocol）、数据库模型（database schema）和一组资源对象。该协议能够支持网络管理系统，用以监测连接到网络上的设备是否有任何引起管理上关注的情况。

### <span id="722_snmp">7.2.2 安装snmp程序</span>

<div>
  <div class="cnblogs_code">
    <pre>yum -y install net-snmp net-snmp-utils</pre>
  </div>
</div>

### <span id="723_snmp">7.2.3 配置snmp程序</span>

<div>
  <div class="cnblogs_code">
    <pre>sed -i.ori <span style="color: #800000;">'</span><span style="color: #800000;">57a view systemview   included  .1</span><span style="color: #800000;">'</span> /etc/snmp/<span style="color: #000000;">snmpd.conf
systemctl start snmpd.service</span></pre>
  </div>
</div>

### <span id="724_snmp">7.2.4 测试snmp</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> snmpwalk -v 2c -c public 127.0.0.1 sysname</span>
SNMPv2-MIB::sysName.0 = STRING: m01</pre>
  </div>
</div>

<div>
  <p class="a3">
    <span style="background-color: #00ff00;"><strong>说明：</strong></span>
  </p>
  
  <p class="a3">
    &nbsp;　　&nbsp;&nbsp; # snmpwalk 类似 zabbix_get
  </p>
  
  <p class="a3">
    　　　# -v 2c&nbsp; 指定使用snmp协议的版本&nbsp; snmp分为v1 v2 v3
  </p>
  
  <p class="a3">
    　　　# -c public&nbsp; 指定暗号
  </p>
  
  <p class="a3">
    　　　# sysname&nbsp; 类似zabbix的key
  </p>
</div>

### <span id="725_web">7.2.5 在web界面进行配置</span>

<div>
  <p class="a3">
    添加新的主机，注意使用snmp接口
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123172038696-1317400757.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <p class="a3">
    选择模板，注意使用SNMP的模板
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123172052305-1411663771.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

<div>
  <p class="a3">
    &nbsp;&nbsp;&nbsp; 添加完成就能够在主机中看到snmp监控对的主机
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171123172059024-1436589674.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Zabbix 3.0 从入门到精通(zabbix使用详解)" alt="" />
</p>

### <span id="726">7.2.6 附录</span>

<div>
  <div class="cnblogs_code">
    <pre>    <span style="color: #008000;">#</span><span style="color: #008000;">#SNMP OID列表 监控需要用到的OID</span>
    http://www.ttlsa.com/monitor/snmp-oid/<span style="color: #000000;">
    cmdb 资源管理系统</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<p style="text-align: right;">
  &nbsp;<strong>此文章出自惨绿少年，转载请注明&nbsp;</strong>
</p>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1_zabbix">第1章 zabbix监控</a><ul>
        <li>
          <a href="#11">1.1 为什么要监控</a><ul>
            <li>
              <a href="#111">1.1.1 网站可用性</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#12">1.2 监控什么东西</a><ul>
            <li>
              <a href="#121">1.2.1 监控范畴</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#13">1.3 怎么来监控</a><ul>
            <li>
              <a href="#131">1.3.1 远程管理服务器</a>
            </li>
            <li>
              <a href="#132">1.3.2 监控硬件</a>
            </li>
            <li>
              <a href="#133_cpu">1.3.3 查看cpu相关</a>
            </li>
            <li>
              <a href="#134">1.3.4 内存够不够可以用</a>
            </li>
            <li>
              <a href="#135">1.3.5 磁盘剩多少写的快不快可以用</a>
            </li>
            <li>
              <a href="#136">1.3.6 监控网络</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#14">1.4 监控工具总览</a>
        </li>
        <li>
          <a href="#15_zabbix">1.5 zabbix介绍</a><ul>
            <li>
              <a href="#151_zabbix">1.5.1 zabbix的组成</a>
            </li>
            <li>
              <a href="#152_zabbix">1.5.2 zabbix监控范畴</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2_zabbix">第2章 安装zabbix</a><ul>
        <li>
          <a href="#21">2.1 环境检查</a>
        </li>
        <li>
          <a href="#22_zabbix">2.2 安装zabbix过程</a><ul>
            <li>
              <a href="#221">2.2.1 安装方式选择</a>
            </li>
            <li>
              <a href="#222">2.2.2 服务端快速安装脚本</a>
            </li>
            <li>
              <a href="#223">2.2.3 客户端快速部署脚本</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#23">2.3 检测连通性</a><ul>
            <li>
              <a href="#231_zabbix-get">2.3.1 服务端安装zabbix-get检测工具</a>
            </li>
            <li>
              <a href="#232">2.3.2 在服务端进行测试</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3_web">第3章 web界面操作</a><ul>
        <li>
          <a href="#31_zabbixweb">3.1 zabbix的web安装</a><ul>
            <li>
              <a href="#311">3.1.1 使用浏览器访问</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#32">3.2 添加监控信息</a><ul>
            <li>
              <a href="#321_zabbix_server">3.2.1 修改监控管理机zabbix server</a>
            </li>
            <li>
              <a href="#322">3.2.2 添加新的主机</a>
            </li>
            <li>
              <a href="#323">3.2.3 查看监控内容</a>
            </li>
            <li>
              <a href="#324">3.2.4 查看图像</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#4">第4章 自定义监控与监控报警</a><ul>
        <li>
          <a href="#41">4.1 自定义监控</a><ul>
            <li>
              <a href="#411">4.1.1 说明</a>
            </li>
            <li>
              <a href="#412">4.1.2 预备知识</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#42">4.2 实现自定义监控</a><ul>
            <li>
              <a href="#421">4.2.1 自定义语法</a>
            </li>
            <li>
              <a href="#422_agent">4.2.2 agent注册</a>
            </li>
            <li>
              <a href="#423_serverweb">4.2.3 在server端注册(web操作)</a>
            </li>
            <li>
              <a href="#424">4.2.4 查看监控的图形</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#43">4.3 监控报警</a><ul>
            <li>
              <a href="#431">4.3.1 第三方报警平台</a>
            </li>
            <li>
              <a href="#432_onealert">4.3.2 onealert配置</a>
            </li>
            <li>
              <a href="#433_onealert_Agent">4.3.3 安装 onealert Agent</a>
            </li>
            <li>
              <a href="#431_onealert_Agent">4.3.1 如何删除onealert Agent</a>
            </li>
            <li>
              <a href="#432">4.3.2 触发器响应，发送报警信息</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#44">4.4 监控可视化</a><ul>
            <li>
              <a href="#441">4.4.1 聚合图形</a>
            </li>
            <li>
              <a href="#442">4.4.2 幻灯片</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#45">4.5 模板的共享</a><ul>
            <li>
              <a href="#451">4.5.1 主机共享</a>
            </li>
            <li>
              <a href="#452">4.5.2 模板共享</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#5">第5章 监控全网服务器</a><ul>
        <li>
          <a href="#51">5.1 需求说明</a>
        </li>
        <li>
          <a href="#52">5.2 规划方案</a><ul>
            <li>
              <a href="#521_apicurl">5.2.1 api接口使用（curl）</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#53">5.3 具体实施规划</a><ul>
            <li>
              <a href="#531">5.3.1 硬件、系统、网络监控</a>
            </li>
            <li>
              <a href="#532">5.3.2 应用服务监控</a>
            </li>
            <li>
              <a href="#533">5.3.3 监控服务通用方法</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#54">5.4 实施全网监控</a><ul>
            <li>
              <a href="#541">5.4.1 使用自动发现规则</a>
            </li>
            <li>
              <a href="#542">5.4.2 监控备份服务器</a>
            </li>
            <li>
              <a href="#543_NFS">5.4.3 监控NFS服务器</a>
            </li>
            <li>
              <a href="#544_MySQL">5.4.4 监控MySQL服务器</a>
            </li>
            <li>
              <a href="#545_web">5.4.5 监控web服务器</a>
            </li>
            <li>
              <a href="#546_URL">5.4.6 监控URL地址</a>
            </li>
            <li>
              <a href="#547">5.4.7 监控反向代理服务器</a>
            </li>
            <li>
              <a href="#548_Nginx7">5.4.8 监控Nginx的7种连接状态</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#6">第6章 自动发现与自动注册</a><ul>
        <li>
          <a href="#61">6.1 自动注册与自动注册</a><ul>
            <li>
              <a href="#611">6.1.1 简介</a>
            </li>
            <li>
              <a href="#612">6.1.2 两种模式</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#62">6.2 自动发现--被动模式</a>
        </li>
        <li>
          <a href="#63">6.3 自动注册--主动模式</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#7_SNMP">第7章 分布式监控与SNMP监控</a><ul>
        <li>
          <a href="#71">7.1 分布式监控</a><ul>
            <li>
              <a href="#711">7.1.1 作用</a>
            </li>
            <li>
              <a href="#712">7.1.2 环境说明</a>
            </li>
            <li>
              <a href="#713_zabbix_proxy">7.1.3 配置zabbix proxy</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#72_SNMP">7.2 SNMP监控</a><ul>
            <li>
              <a href="#721">7.2.1 使用范围</a>
            </li>
            <li>
              <a href="#722_snmp">7.2.2 安装snmp程序</a>
            </li>
            <li>
              <a href="#723_snmp">7.2.3 配置snmp程序</a>
            </li>
            <li>
              <a href="#724_snmp">7.2.4 测试snmp</a>
            </li>
            <li>
              <a href="#725_web">7.2.5 在web界面进行配置</a>
            </li>
            <li>
              <a href="#726">7.2.6 附录</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</div>