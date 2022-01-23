---
title: 分布式文件系统—GlusterFS
author: 惨绿少年
type: post
date: 2018-02-27T17:51:00+00:00
url: /clsn/lx28.html
Baidusubmit:
  - 1
views:
  - 360
specs_zan:
  - 1
zm_favorites:
  - 1
categories:
  - Linux运维
  - 云计算
  - 存储
  - 运维基本功

---
## <span id="11">1.1 分布式文件系统</span>

### <span id="111">1.1.1 什么是分布式文件系统</span>

　　相对于本机端的文件系统而言，分布式文件系统（英语：_Distributed file system_, _DFS_），或是网络文件系统（英语：_Network File System_），是一种允许文件通过网络在多台主机上分享的文件系统，可让多机器上的多用户分享文件和存储空间。

　　在这样的文件系统中，客户端并非直接访问底层的数据存储区块，而是通过网络，以特定的通信协议和服务器沟通。借由通信协议的设计，可以让客户端和服务器端都能根据访问控制清单或是授权，来限制对于文件系统的访问。

### <span id="112_glusterfs">1.1.2 glusterfs是什么</span>

　　Gluster是一个分布式文件系统。它是各种不同的存储服务器之上的组合，这些服务器由以太网或无限带宽技术Infiniband以及远程直接内存访问RDMA互相融汇，最终所形成的一个大的并行文件系统网络。

<p style="text-align: center;">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180207214211138-322405144.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="分布式文件系统---GlusterFS" alt="" />
</p>

　　它有包括云计算在内的多重应用，诸如：生物医药科学，文档存储。Gluster是由GNU托管的自由软件，证书是AGPL。Gluster公司是Gluster的首要商业赞助商，且提供商业产品以及基于Gluster的解决方案。

## <span id="12_GlusterFS">1.2 快速部署GlusterFS</span>

### <span id="121">1.2.1 环境说明</span>

注意：最少需要拥有两块硬盘

<p style="text-align: center;">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180207214222451-1627110552.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="分布式文件系统---GlusterFS" alt="" />
</p>

&nbsp;&nbsp; 系统环境说明

glusterfs01信息

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> hostname</span>
<span style="color: #000000;">glusterfs01
[root@glusterfs01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r</span>
3.10.0-693<span style="color: #000000;">.el7.x86_64
[root@glusterfs01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> sestatus </span>
<span style="color: #000000;">SELinux status:                 disabled
[root@glusterfs01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status firewalld.service </span>
● firewalld.service - firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(</span>1<span style="color: #000000;">)
[root@glusterfs01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> hostname -I</span>
10.0.0.120 172.16.1.120</pre>
  </div>
</div>

glusterfs02信息

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r</span>
3.10.0-693<span style="color: #000000;">.el7.x86_64
[root@glusterfs02 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> sestatus </span>
<span style="color: #000000;">SELinux status:                 disabled
[root@glusterfs02 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status firewalld.service </span>
● firewalld.service - firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(</span>1<span style="color: #000000;">)
[root@glusterfs02 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> hostname -I</span>
10.0.0.121 172.16.1.121</pre>
  </div>
</div>

&nbsp;&nbsp; <span style="color: #ff0000;">注意配置好hosts解析</span>

### <span id="122">1.2.2 前期准备</span>

gluster01主机挂载磁盘

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkfs.xfs /dev/sdb</span>
[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /data/brick1</span>
[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo '/dev/sdb /data/brick1 xfs defaults 0 0' >> /etc/fstab</span>
[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mount -a && mount</span></pre>
  </div>
</div>

gluster02主机挂载磁盘

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkfs.xfs /dev/sdb</span>
[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p /data/brick1</span>
[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo '/dev/sdb /data/brick1 xfs defaults 0 0' >> /etc/fstab</span>
[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mount -a && mount</span></pre>
  </div>
</div>

## <span id="13_GlusterFS">1.3 部署GlusterFS</span>

### <span id="131">1.3.1 安装软件</span>

在两个节点上操作

<div>
  <div class="cnblogs_code">
    <pre>yum install centos-release-gluster -<span style="color: #000000;">y
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 修改镜像源加速</span>
sed -i <span style="color: #800000;">'</span><span style="color: #800000;">s#http://mirror.centos.org#https://mirrors.shuosc.org#g</span><span style="color: #800000;">'</span> /etc/yum.repos.d/CentOS-Gluster-3.12<span style="color: #000000;">.repo
yum install </span>-y glusterfs glusterfs-server glusterfs-fuse glusterfs-rdma</pre>
  </div>
</div>

软件版本

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;">  rpm  -qa glusterfs</span>
glusterfs-3.12.5-2.el7.x86_64</pre>
  </div>
</div>

### <span id="132_GlusterFS">1.3.2 启动GlusterFS</span>

在两个节点上都进行操作

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl start glusterd.service </span>
[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status glusterd.service </span>
<span style="color: #00ff00;">●</span> glusterd.service - GlusterFS, a clustered file-<span style="color: #000000;">system server
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">glusterd.service; disabled; vendor preset: disabled)
   Active: <span style="color: #00ff00;">active (running)</span> since 三 </span>2018-02-07 21:02:44<span style="color: #000000;"> CST; 2s ago
  Process: </span>1923 ExecStart=/usr/sbin/glusterd -p /var/run/glusterd.pid --log-level $LOG_LEVEL $GLUSTERD_OPTIONS (code=exited, status=0/<span style="color: #000000;">SUCCESS)
 Main PID: </span>1924<span style="color: #000000;"> (glusterd)
   CGroup: </span>/system.slice/<span style="color: #000000;">glusterd.service
           └─</span>1924 /usr/sbin/glusterd -p /var/run/glusterd.pid --log-<span style="color: #000000;">level INFO

2月 </span>07 21:02:44 glusterfs01 systemd[1]: Starting GlusterFS, a clustered file-<span style="color: #000000;">system server...
2月 </span>07 21:02:44 glusterfs01 systemd[1]: Started GlusterFS, a clustered file-<span style="color: #000000;">system server.
Hint: Some lines were ellipsized, use </span>-l to show <span style="color: #0000ff;">in</span> full.</pre>
  </div>
</div>

### <span id="133">1.3.3 配置互信（可信池）</span>

在glusterfs01上操作

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> gluster peer probe glusterfs02</span>
peer probe: success.</pre>
  </div>
</div>

在glusterfs02上操作

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> gluster peer probe glusterfs01</span>
peer probe: success.</pre>
  </div>
</div>

&nbsp;&nbsp; 注意：一旦建立了这个池，只有受信任的成员可能会将新的服务器探测到池中。新服务器无法探测池，必须从池中探测。

### <span id="134">1.3.4 检查对等状态</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;">  gluster peer status </span>
Number of Peers: 1<span style="color: #000000;">

Hostname: </span>10.0.0.121<span style="color: #000000;">
Uuid: 61d043b0</span>-5582-4354-b475-<span style="color: #000000;">2626c88bc576
State: Peer </span><span style="color: #0000ff;">in</span><span style="color: #000000;"> Cluster (Connected)
Other names:
glusterfs02</span></pre>
  </div>
</div>

&nbsp;&nbsp; 注意：看到的UUID应不相同。

<div class="cnblogs_code">
  <pre>[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;">  gluster peer status </span>
Number of Peers: 1<span style="color: #000000;">

Hostname: glusterfs01
Uuid: e2a9367c</span>-fe96-446d-a631-<span style="color: #000000;">194970c18750
State: Peer </span><span style="color: #0000ff;">in</span> Cluster (Connected)</pre>
</div>

### <span id="135_GlusterFS">1.3.5 建立一个GlusterFS卷</span>

在两个节点上操作

<div>
  <div class="cnblogs_code">
    <pre>mkdir -p /data/brick1/gv0</pre>
  </div>
</div>

&nbsp;&nbsp; 在任意一个节点上执行

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> gluster volume create gv0 replica 2 glusterfs01:/data/brick1/gv0 glusterfs02:/data/brick1/gv0</span>
Replica 2 volumes are prone to split-brain. Use Arbiter <span style="color: #0000ff;">or</span> Replica 3 to avoid this. See: http://docs.gluster.org/en/latest/Administrator%20Guide/Split%20brain%20<span style="color: #0000ff;">and</span>%20ways%20to%20deal%20with%20it/<span style="color: #000000;">.
Do you still want to </span><span style="color: #0000ff;">continue</span><span style="color: #000000;">?
 (y</span>/<span style="color: #000000;">n) y
volume create: gv0: success: please start the volume to access data</span></pre>
  </div>
</div>

&nbsp;&nbsp; 启用存储卷

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> gluster volume start gv0</span>
volume start: gv0: success</pre>
  </div>
</div>

查看信息

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> gluster volume info </span>
<span style="color: #000000;"> 
Volume Name: gv0
Type: Replicate
Volume ID: 865899b9</span>-1e5a-416a-8374-<span style="color: #000000;">63f7df93e4f5
Status: Started
Snapshot Count: 0
Number of Bricks: </span>1 x 2 = 2<span style="color: #000000;">
Transport</span>-<span style="color: #000000;">type: tcp
Bricks:
Brick1: glusterfs01:</span>/data/brick1/<span style="color: #000000;">gv0
Brick2: glusterfs02:</span>/data/brick1/<span style="color: #000000;">gv0
Options Reconfigured:
transport.address</span>-<span style="color: #000000;">family: inet
nfs.disable: on
performance.client</span>-io-threads: off</pre>
  </div>
</div>

至此，服务端配置结束

## <span id="14">1.4 客户端测试</span>

### <span id="141">1.4.1 安装客户端工具</span>

挂载测试

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install centos-release-gluster -y</span>
[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install -y glusterfs glusterfs-fuse</span></pre>
</div>

&nbsp;&nbsp; 注意：要配置好hosts文件，否则连接会出错

<div>
  <div class="cnblogs_code">
    <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mount.glusterfs  glusterfs01:/gv0 /mnt </span>
[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> df -h </span>
Filesystem        Size  Used Avail Use%<span style="color: #000000;"> Mounted on
</span>/dev/sda3          19G  2.2G   16G  13% /<span style="color: #000000;">
tmpfs             238M     0  238M   0</span>% /dev/<span style="color: #000000;">shm
</span>/dev/sda1         190M   40M  141M  22% /<span style="color: #000000;">boot
glusterfs01:</span>/gv0  100G   33M  100G   1% /mnt</pre>
  </div>
</div>

### <span id="142">1.4.2 复制文件测试</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> for i in `seq -w 1 100`; do cp -rp /var/log/messages /mnt/copy-test-$i; done</span></pre>
  </div>
</div>

客户端检查文件

<div>
  <div class="cnblogs_code">
    <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;">  ls -lA /mnt/copy* | wc -l</span>
10</pre>
  </div>
</div>

服务节点检查文件

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;">   ls -lA /data/brick1/gv0/copy* |wc -l</span>
100</pre>
  </div>
</div>

服务节点检查文件

<div>
  <div class="cnblogs_code">
    <pre>[root@glusterfs02 ~]<span style="color: #008000;">#</span><span style="color: #008000;">   ls -lA /data/brick1/gv0/copy* |wc -l</span>
100</pre>
  </div>
</div>

&nbsp;&nbsp; 至此Glusterfs简单配置完成

## <span id="15">1.5 参考文献</span>

<div>
  <blockquote>
    <div>
      <p class="a1">
        [1]&nbsp;&nbsp;<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://docs.gluster.org/en/latest/Quick-Start-Guide/Quickstart/" >http://docs.gluster.org/en/latest/Quick-Start-Guide/Quickstart/</a>
      </p>
      
      <p class="a1">
        [2]&nbsp;&nbsp;<a href="https://www.cnblogs.com/jicki/p/5801712.html">https://www.cnblogs.com/jicki/p/5801712.html</a>
      </p>
      
      <p class="a1">
        [3]&nbsp;&nbsp;<a href="https://mirrors.shuosc.org/centos/7/">https://mirrors.shuosc.org/centos/7/</a>
      </p>
    </div>
  </blockquote>
</div>

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11">1.1 分布式文件系统</a><ul>
        <li>
          <a href="#111">1.1.1 什么是分布式文件系统</a>
        </li>
        <li>
          <a href="#112_glusterfs">1.1.2 glusterfs是什么</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_GlusterFS">1.2 快速部署GlusterFS</a><ul>
        <li>
          <a href="#121">1.2.1 环境说明</a>
        </li>
        <li>
          <a href="#122">1.2.2 前期准备</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_GlusterFS">1.3 部署GlusterFS</a><ul>
        <li>
          <a href="#131">1.3.1 安装软件</a>
        </li>
        <li>
          <a href="#132_GlusterFS">1.3.2 启动GlusterFS</a>
        </li>
        <li>
          <a href="#133">1.3.3 配置互信（可信池）</a>
        </li>
        <li>
          <a href="#134">1.3.4 检查对等状态</a>
        </li>
        <li>
          <a href="#135_GlusterFS">1.3.5 建立一个GlusterFS卷</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14">1.4 客户端测试</a><ul>
        <li>
          <a href="#141">1.4.1 安装客户端工具</a>
        </li>
        <li>
          <a href="#142">1.4.2 复制文件测试</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15">1.5 参考文献</a>
    </li>
  </ul>
</div>