---
title: 软硬链接、文件删除原理、linux中的三种时间、chkconfig优化
author: 惨绿少年
type: post
date: 2017-09-19T22:36:00+00:00
url: /clsn/lx987.html
Baidusubmit:
  - 1
views:
  - 89
categories:
  - Linux运维
  - 玩转Linux
  - 运维基本功

---
# <span id="1">第1章 <span style="font-family: 新宋体;">软硬链接</span></span>

## <span id="11">1.1 <span style="font-family: 新宋体;">硬链接</span></span>

### <span id="111">1.1.1 <span style="font-family: 新宋体;">含义</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">多个文件拥有相同的</span>inode<span style="font-family: 新宋体;">号码</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">硬链接即文件的多个入口</span>
</p>

### <span id="112">1.1.2 <span style="font-family: 新宋体;">作用</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">防止你误删除文件</span>
</p>

### <span id="113">1.1.3 <span style="font-family: 新宋体;">如何创建硬链接</span></span>

<p style="margin-left: 21.0pt;">
  ln <span style="font-family: 新宋体;">命令，前面是源文件</span>&nbsp;<span style="font-family: 新宋体;">后面是创建的链接文件</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix clsn]# ln clsn.txt clsn.txt-hard
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">查看两文件的</span>inode<span style="font-family: 新宋体;">号相同。</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix clsn]# ls -lhi clsn.txt clsn.txt-hard
  </p>
  
  <p>
    151273 -rw-r--r-- 2 root root 607 Aug 30 09:13 clsn.txt
  </p>
  
  <p>
    151273 -rw-r--r-- 2 root root 607 Aug 30 09:13 clsn.txt-hard
  </p>
</div>

## <span id="12">1.2 <span style="font-family: 新宋体;">软连接</span></span>

### <span id="121">1.2.1 <span style="font-family: 新宋体;">含义</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">为了快捷，省事，方便使用</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">软连接中存放的是源文件的位置</span>
</p>

### <span id="122">1.2.2 <span style="font-family: 新宋体;">创建软连接</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">使用</span>ln -s <span style="font-family: 新宋体;">命令创建软连接</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix clsn]# ln -s clsn.txt clsn.txt-soft
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">查看软硬链接的</span>inode<span style="font-family: 新宋体;">号不相同</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">但是同时指向的是同一文件</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix clsn]# ll -i clsn*
  </p>
  
  <p>
    151273 -rw-r--r-- 2 root root 607 Aug 30 09:13 clsn.txt
  </p>
  
  <p>
    132910 -rw-r--r-- 1 root root 607 Aug 30 09:14 clsn.txt.bak
  </p>
  
  <p>
    151273 -rw-r--r-- 2 root root 607 Aug 30 09:13 clsn.txt-hard
  </p>
  
  <p>
    132951 lrwxrwxrwx 1 root root&nbsp; 13 Aug 30 09:22 clsn.txt-soft -> clsn.txt
  </p>
</div>

## <span id="13">1.3 <span style="font-family: 新宋体;">软连接与硬链接的区别</span></span>

<p style="text-align: center;" align="center">
  &nbsp;
</p>

### <span id="131">1.3.1 <span style="font-family: 新宋体;">含义</span></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">软链接：</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">软连接相当于快捷方式</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">里面存放的是源文件的位置</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">硬链接：</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">在同一个分区中，多个文件拥有<span style="background: lime;">相同的</span></span><span style="background: lime;">inode</span><span style="font-family: 新宋体;">号</span>

### <span id="132">1.3.2 <span style="font-family: 新宋体;">创建方式不同</span></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ln <span style="font-family: 新宋体;">创建硬链接</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ln -s <span style="font-family: 新宋体;">软连接</span>

### <span id="133">1.3.3 <span style="font-family: 新宋体;">不同的特点</span></span>

<p style="margin-left: 48.0pt;">
  1<span style="font-family: 新宋体;">）软连接可以随意创建</span>
</p>

<p style="margin-left: 48.0pt;">
  2<span style="font-family: 新宋体;">）不能对目录创建硬链接</span>
</p>

<p style="margin-left: 48.0pt;">
  3<span style="font-family: 新宋体;">）对文件创建硬链接可以防止文件被误删除</span>
</p>

### <span id="134">1.3.4 <span style="font-family: 新宋体;">如何删除</span></span>

<p style="margin-left: 36.0pt; text-indent: 12.0pt;">
  1<span style="font-family: 新宋体;">）删除文件的硬链接，文件可以继续使用</span>
</p>

<p style="margin-left: 36.0pt;">
  &nbsp; &nbsp;2<span style="font-family: 新宋体;">）只有把这个文件的所有硬链接都删除才可</span>
</p>

<p style="margin-left: 36.0pt; text-indent: 12.0pt;">
  3<span style="font-family: 新宋体;">）只删除源文件软连接无法使用</span>
</p>

<p style="margin-left: 36.0pt;">
  &nbsp; &nbsp;4<span style="font-family: 新宋体;">）只删除软连接对文件没有影响</span>
</p>

# <span id="2">第2章 <span style="font-family: 新宋体;">文件删除原理</span></span>

## <span id="21">2.1 <span style="font-family: 新宋体;">彻底删除一个文件</span></span>

<p style="text-indent: 36.0pt;">
  1.<span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">硬链接数为</span><span style="background: lime;"></span> <span style="font-family: 新宋体;">与这个文件有关的所有硬链接都被删除。</span>
</p>

<p style="margin-left: 78.0pt; text-indent: -21.0pt;">
  a)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family: 新宋体;">使用</span>rm<span style="font-family: 新宋体;">目录进行删除</span>
</p>

<p style="margin-left: 54.0pt; text-indent: -18.0pt;">
  2.<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">进程调用数为</span><span style="background: lime;"></span><span style="font-family: 新宋体;">，没有人在使用这个文件才能释放磁盘空间。</span>
</p>

<p style="margin-left: 78.0pt; text-indent: -21.0pt;">
  a)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family: 新宋体;">使用</span>lsof <span style="font-family: 新宋体;">查看谁在使用这文件</span>
</p>

<p style="margin-left: 78.0pt; text-indent: -21.0pt;">
  b)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family: 新宋体;">重启对应的软件</span>/<span style="font-family: 新宋体;">服务就能释放磁盘</span>
</p>

## <span id="22">2.2 <span style="font-family: 新宋体;">查看某个文件是否总有人在用</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 新宋体;">使用</span>lsof<span style="font-family: 新宋体;">命令可以列出所有正在使用的文件和相应的进程</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# lsof /var/log/secure
  </p>
  
  <p>
    COMMAND&nbsp;&nbsp; PID USER&nbsp;&nbsp; FD&nbsp;&nbsp; TYPE DEVICE SIZE/OFF&nbsp;&nbsp; NODE NAME
  </p>
  
  <p>
    rsyslogd 1262 root&nbsp;&nbsp;&nbsp; 4w&nbsp;&nbsp; REG&nbsp;&nbsp;&nbsp; 8,3&nbsp;&nbsp;&nbsp;&nbsp; 1927 270856 /var/log/secure
  </p>
  
  <p>
    [root@znix ~]# lsof |grep message
  </p>
  
  <p>
    rsyslogd&nbsp; 1262&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; root&nbsp;&nbsp;&nbsp; 1w&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; REG&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8,3&nbsp;&nbsp;&nbsp;&nbsp; 1044&nbsp;&nbsp;&nbsp;&nbsp; 270855 /var/log/messages
  </p>
</div>

## <span id="23">2.3 <span style="font-family: 新宋体;">重启对应的软件</span>/<span style="font-family: 新宋体;">服务</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 新宋体;">找到软件对应的管理地址，让软件重启，释放空间。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# /etc/init.d/rsyslog
  </p>
  
  <p>
    Usage: /etc/init.d/rsyslog {start|stop|restart|condrestart|try-restart|reload|force-reload|status}
  </p>
</div>

## <span id="24">2.4 <span style="font-family: 新宋体;">磁盘空间满了（<span style="background: lime;">三种情况</span>）</span></span>

<p style="margin-left: 18.0pt;">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; inode<span style="font-family: 新宋体;">满了&hellip;&hellip;查找出系统目录比较大</span>(1M)
</p>

<p style="margin-left: 18.0pt;">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; block<span style="font-family: 新宋体;">满了&hellip;&hellip;使用</span><span style="background: lime;">du -sh /*</span> <span style="font-family: 新宋体;">一层一层找，把较大的文件删除</span>
</p>

<p style="margin-left: 18.0pt;">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">硬链接数为</span><span style="font-family: 新宋体;">，进程调用数不为</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">使用</span>&nbsp; lsof |grep delete <span style="font-family: 新宋体;">查看占用的文件</span>
  </p>
</div>

## <span id="25">2.5 <span style="font-family: 新宋体;">故障案例</span></span>

<p style="text-indent: 18.0pt;">
  <span style="font-family: 新宋体;">没有被彻底删除</span>-<span style="font-family: 新宋体;">硬链接数为</span>0,<span style="font-family: 新宋体;">进程调用数不为零</span>
</p>

### <span id="251">2.5.1 <span style="font-family: 新宋体;">环境</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">向</span> /var/log/message<span style="font-family: 新宋体;">中放入大量数据</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    seq 100000000 >>/var/log/messages
  </p>
</div>

### <span id="252">2.5.2 <span style="font-family: 新宋体;">查看此时此刻磁盘使用情况</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix apache]# df -h
  </p>
  
  <p>
    Filesystem&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Size&nbsp; Used Avail Use% Mounted on
  </p>
  
  <p>
    /dev/sda3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8.8G&nbsp; 3.9G&nbsp; 3.7G&nbsp; 59% /
  </p>
  
  <p>
    tmpfs&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;238M&nbsp;&nbsp;&nbsp;&nbsp; 0&nbsp; 238M&nbsp;&nbsp; 0% /dev/shm
  </p>
  
  <p>
    /dev/sda1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 190M&nbsp;&nbsp; 40M&nbsp; 141M&nbsp; 22% /boot
  </p>
</div>

### <span id="253">2.5.3 <span style="font-family: 新宋体;">删除文件</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix apache]# \rm -f /var/log/messages
  </p>
</div>

### <span id="254">2.5.4 <span style="font-family: 新宋体;">检查空间没有被释放</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix apache]# df -h
  </p>
  
  <p>
    Filesystem&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Size&nbsp; Used Avail Use% Mounted on
  </p>
  
  <p>
    /dev/sda3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8.8G&nbsp; 3.9G&nbsp; 3.7G&nbsp; 59% /
  </p>
  
  <p>
    tmpfs&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 238M&nbsp;&nbsp;&nbsp;&nbsp; 0&nbsp; 238M&nbsp;&nbsp; 0% /dev/shm
  </p>
  
  <p>
    /dev/sda1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 190M&nbsp;&nbsp; 40M&nbsp; 141M&nbsp; 22% /boot
  </p>
</div>

### <span id="255_0">2.5.5 <span style="font-family: 新宋体;">查看被删除（硬链接数为</span><span style="font-family: 新宋体;">）但是还被进程调用的文件</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix apache]# lsof |grep delete
  </p>
  
  <p>
    <span style="background: lime;">rsyslogd</span>&nbsp; 1168&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; root&nbsp;&nbsp;&nbsp; 1w&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; REG&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8,3 889335632&nbsp;&nbsp;&nbsp;&nbsp; 259962 /var/log/messages (deleted)
  </p>
</div>

### <span id="256">2.5.6 <span style="font-family: 新宋体;">重启对应的服务</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix apache]# /etc/init.d/rsyslog restart
  </p>
  
  <p>
    Shutting down system logger:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [&nbsp; OK&nbsp; ]
  </p>
  
  <p>
    Starting system logger:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [&nbsp; OK&nbsp; ]
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">查看磁盘空间</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix apache]# df -h
  </p>
  
  <p>
    Filesystem&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Size&nbsp; Used Avail Use% Mounted on
  </p>
  
  <p>
    /dev/sda3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8.8G&nbsp; 1.5G&nbsp; 6.9G&nbsp; 18% /
  </p>
  
  <p>
    tmpfs&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 238M&nbsp;&nbsp;&nbsp;&nbsp; 0&nbsp; 238M&nbsp;&nbsp; 0% /dev/shm
  </p>
  
  <p>
    /dev/sda1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 190M&nbsp;&nbsp; 40M&nbsp; 141M&nbsp; 22% /boot
  </p>
</div>

# <span id="3_nbsp">第3章 <span style="font-family: 新宋体;">找出某个文件的其他的硬链接</span>&nbsp;</span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 新宋体;">使用</span>find<span style="font-family: 新宋体;">命令</span> -inum<span style="font-family: 新宋体;">参数找</span>inode<span style="font-family: 新宋体;">号码，找到相同的</span>inode <span style="font-family: 新宋体;">互为硬链接。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# ls -lhi&nbsp; test.txt
  </p>
  
  <p>
    260141 -rw-r--r--. 2 root root 265 Aug 29 19:16 test.txt
  </p>
  
  <p>
    [root@znix ~]# find /* -type f -inum 260141
  </p>
  
  <p>
    /root/test.txt
  </p>
  
  <p>
    /root/test.txt-hard
  </p>
</div>

# <span id="4">第4章 <span style="font-family: 新宋体;">三种时间戳</span></span>

## <span id="41">4.1 <span style="font-family: 新宋体;">含义</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    Modify&nbsp;&nbsp; mtime<span style="font-family: 新宋体;">修改时间</span> <span style="font-family: 新宋体;">（最常用）</span> <span style="font-family: 新宋体;">文件的内容</span> <span style="font-family: 新宋体;">增加</span> <span style="font-family: 新宋体;">删除</span> <span style="font-family: 新宋体;">修改改变</span>
  </p>
  
  <p>
    Change&nbsp;&nbsp; ctime<span style="font-family: 新宋体;">属性变更时间</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">文件属性发生改变时更改</span>
  </p>
  
  <p>
    Access&nbsp;&nbsp; atime<span style="font-family: 新宋体;">访问时间</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">查看文件的时间</span> <span style="font-family: 新宋体;">（只有文件内容有修改时才会改变）</span>
  </p>
</div>

## <span id="42_stat">4.2 <span style="font-family: 新宋体;">使用</span>stat<span style="font-family: 新宋体;">命令查看文件的信息</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# stat clsn.txt
  </p>
  
  <p>
    &nbsp; File: `clsn.txt'
  </p>
  
  <p>
    &nbsp; Size: 237&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; Blocks: 8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; IO Block: 4096&nbsp;&nbsp; regular file
  </p>
  
  <p>
    Device: 803h/2051d&nbsp; Inode: 16976&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Links: 2
  </p>
  
  <p>
    Access: (0644/-rw-r--r--)&nbsp; Uid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)&nbsp;&nbsp; Gid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)
  </p>
  
  <p>
    Access: 2017-08-28 11:45:26.734849384 +0800
  </p>
  
  <p>
    Modify: 2017-08-28 11:45:26.734849384 +0800
  </p>
  
  <p>
    Change: 2017-08-30 11:30:27.783364422 +0800
  </p>
</div>

## <span id="43_mtimechange">4.3 <span style="font-family: 新宋体;">修改</span>mtime&change</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# echo "123">>clsn.txt
  </p>
  
  <p>
    [root@znix ~]# stat clsn.txt
  </p>
  
  <p>
    &nbsp; File: `clsn.txt'
  </p>
  
  <p>
    &nbsp; Size: 241&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; Blocks: 8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; IO Block: 4096&nbsp;&nbsp; regular file
  </p>
  
  <p>
    Device: 803h/2051d&nbsp; Inode: 16976&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Links: 2
  </p>
  
  <p>
    Access: (0644/-rw-r--r--)&nbsp; Uid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)&nbsp;&nbsp; Gid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)
  </p>
  
  <p>
    Access: 2017-08-28 11:45:26.734849384 +0800
  </p>
  
  <p>
    Modify: 2017-08-30 11:40:57.384368932 +0800
  </p>
  
  <p>
    Change: 2017-08-30 11:40:57.384368932 +0800
  </p>
</div>

## <span id="44_ctime">4.4 <span style="font-family: 新宋体;">修改</span>ctime <span style="font-family: 新宋体;">（属性变更时间）</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# ln clsn.txt&nbsp; clsn.txt-hard
  </p>
  
  <p>
    [root@znix ~]# stat clsn.txt
  </p>
  
  <p>
    &nbsp; File: `clsn.txt'
  </p>
  
  <p>
    &nbsp; Size: 241&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; Blocks: 8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; IO Block: 4096&nbsp;&nbsp; regular file
  </p>
  
  <p>
    Device: 803h/2051d&nbsp; Inode: 16976&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Links: 3
  </p>
  
  <p>
    Access: (0644/-rw-r--r--)&nbsp; Uid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)&nbsp;&nbsp; Gid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)
  </p>
  
  <p>
    Access: 2017-08-28 11:45:26.734849384 +0800
  </p>
  
  <p>
    Modify: 2017-08-30 11:40:57.384368932 +0800
  </p>
  
  <p>
    Change: 2017-08-30 11:42:32.981364780 +0800
  </p>
</div>

## <span id="45_atime">4.5 atime<span style="font-family: 新宋体;">修改</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# tail -1 clsn.txt
  </p>
  
  <p>
    123
  </p>
  
  <p>
    [root@znix ~]# stat clsn.txt
  </p>
  
  <p>
    &nbsp; File: `clsn.txt'
  </p>
  
  <p>
    &nbsp; Size: 241&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; Blocks: 8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; IO Block: 4096&nbsp;&nbsp; regular file
  </p>
  
  <p>
    Device: 803h/2051d&nbsp; Inode: 16976&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Links: 3
  </p>
  
  <p>
    Access: (0644/-rw-r--r--)&nbsp; Uid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)&nbsp;&nbsp; Gid: (&nbsp;&nbsp;&nbsp; 0/&nbsp;&nbsp;&nbsp; root)
  </p>
  
  <p>
    Access: 2017-08-30 11:43:15.288357930 +0800
  </p>
  
  <p>
    Modify: 2017-08-30 11:40:57.384368932 +0800
  </p>
  
  <p>
    Change: 2017-08-30 11:42:32.981364780 +0800
  </p>
</div>

&nbsp;

# <span id="5_chkconfig">第5章 chkconfig<span style="font-family: 新宋体;">命令相关</span></span>

<p style="margin-left: 18.0pt;">
  chkconfig<span style="font-family: 新宋体;">命令实际上控制的是</span>/etc/rc3.d/<span style="font-family: 新宋体;">（</span>3<span style="font-family: 新宋体;">运行模式下）下的软连接。通过不通的软连接，实现不通的控制。实际受</span>/etc/init.d/<span style="font-family: 新宋体;">下脚本的关系。</span>
</p>

## <span id="51_chkconfig_iptables_on_chkconfig_iptables_off">5.1 <span style="font-family: 新宋体;">执行</span>chkconfig iptables on & chkconfig iptables off<span style="font-family: 新宋体;">之后发生了什么？</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 新宋体;">执行</span>chkconfig iptables on<span style="font-family: 新宋体;">后</span>/etc/rc3.d/<span style="font-family: 新宋体;">下的</span>/etc/init.d/iptables<span style="font-family: 新宋体;">改为</span>S08iptables
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# chkconfig iptables on
  </p>
  
  <p>
    [root@znix rc3.d]# chkconfig |grep ipt
  </p>
  
  <p>
    iptables&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0:off&nbsp;&nbsp; 1:off&nbsp;&nbsp; 2:on&nbsp;&nbsp;&nbsp; 3:on&nbsp;&nbsp;&nbsp; 4:on&nbsp;&nbsp;&nbsp; 5:on&nbsp;&nbsp;&nbsp; 6:off
  </p>
  
  <p>
    [root@znix rc3.d]# ls -l /etc/rc3.d/ |grep ipt
  </p>
  
  <p>
    lrwxrwxrwx&nbsp; 1 root root 18 Aug 30 12:03 S08iptables -> ../init.d/iptables
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">执行</span>chkconfig iptables off /etc/rc3.d/<span style="font-family: 新宋体;">下的</span>/etc/init.d/iptables<span style="font-family: 新宋体;">软连接改为</span>K92iptables

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# chkconfig iptables off
  </p>
  
  <p>
    [root@znix rc3.d]# chkconfig |grep ipt
  </p>
  
  <p>
    iptables&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0:off&nbsp;&nbsp; 1:off&nbsp;&nbsp; 2:off&nbsp;&nbsp; 3:off&nbsp;&nbsp; 4:off&nbsp;&nbsp; 5:off&nbsp;&nbsp; 6:off
  </p>
  
  <p>
    [root@znix rc3.d]# ls -l /etc/rc3.d/ |grep ipt
  </p>
  
  <p>
    lrwxrwxrwx&nbsp; 1 root root 18 Aug 30 12:04 K92iptables -> ../init.d/iptables
  </p>
</div>

<p style="margin-left: 24.0pt;">
  <span style="background: lime;">iptables </span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">开机自启动</span><span style="background: lime;"> &nbsp;&nbsp;&nbsp;</span><span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">软连接</span><span style="background: lime;">&nbsp;&nbsp; S</span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">开头</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="background: lime;">iptables </span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">开机不自启动</span>&nbsp;<span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">软连接</span><span style="background: lime;">&nbsp;&nbsp; K</span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">开头</span>
</p>

## <span id="52">5.2 <span style="font-family: 新宋体;">让一个软件开机自启动</span></span>

<p style="margin-left: 42.0pt;">
  1<span style="font-family: 新宋体;">）把脚本放入</span>/etc/rc.local
</p>

<p style="margin-left: 42.0pt;">
  2<span style="font-family: 新宋体;">）通过</span>chkconfig <span style="font-family: 新宋体;">管理命令或脚本，让他开机自启动</span>
</p>

## <span id="53_chkconfig">5.3 <span style="font-family: 新宋体;">如何让一个服务或命令通过</span>chkconfig<span style="font-family: 新宋体;">管理</span></span>

### <span id="531_etcinitd">5.3.1 <span style="font-family: 新宋体;">脚本必须放在</span>/etc/init.d/<span style="font-family: 新宋体;">目录下面</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# echo "hostname" > /etc/init.d/clsnd
  </p>
</div>

### <span id="532_chkconfig">5.3.2 <span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">必须写出</span><span style="background: lime;">chkconfig</span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">格式</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">写出</span><span style="background: lime;">chkconfig </span><span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">才能被</span><span style="background: lime;">chkconfig</span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">管理</span>
</p>

hkconfig: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2345 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;99 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;99

<span style="font-family: 新宋体;">默认在哪几个运行级别启动</span>&nbsp;<span style="font-family: 新宋体;">开机顺序</span>&nbsp;<span style="font-family: 新宋体;">关机顺序</span>

<span style="font-family: 新宋体;">自己创建的开机顺序一般写</span>99 <span style="font-family: 新宋体;">，</span>99<span style="font-family: 新宋体;">为最后一个</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# cat /etc/init.d/clsnd
  </p>
  
  <p>
    #!/bin/sh
  </p>
  
  <p>
    #
  </p>
  
  <p>
    #
  </p>
  
  <p>
    # chkconfig: <span style="background: lime;">2345 99 99</span>
  </p>
  
  <p>
    #
  </p>
  
  <p>
    # description: print hostname
  </p>
  
  <p>
    hostname
  </p>
</div>

### <span id="533">5.3.3 <span style="font-family: 新宋体;">给这个脚本添加上执行的权限</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">使用</span>chmod<span style="font-family: 新宋体;">命令修改文件权限</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">需要有可执行的权限，不然无法执行</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# chmod +x /etc/init.d/clsnd
  </p>
  
  <p>
    [root@znix rc3.d]# ll /etc/init.d/clsnd
  </p>
  
  <p>
    -rwxr-xr-x 1 root root 9 Aug 30 12:22 /etc/init.d/clsnd
  </p>
</div>

### <span id="534_chkconfig">5.3.4 <span style="font-family: 新宋体;">添加脚本到</span>chkconfig<span style="font-family: 新宋体;">管理</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# chkconfig --add clsnd
  </p>
</div>

### <span id="535">5.3.5 <span style="font-family: 新宋体;">查看状态</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">改为开机自启动</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# chkconfig |grep old
  </p>
  
  <p>
    clsnd&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0:off&nbsp;&nbsp; 1:off&nbsp;&nbsp; 2:on&nbsp;&nbsp;&nbsp; 3:on&nbsp;&nbsp;&nbsp; 4:on&nbsp;&nbsp;&nbsp; 5:on&nbsp;&nbsp;&nbsp; 6:off
  </p>
</div>

### <span id="536_etcrc3d">5.3.6 <span style="font-family: 新宋体;">查看</span>/etc/rc3.d/<span style="font-family: 新宋体;">目录生成相对应的软连接</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix rc3.d]# ls -l /etc/rc3.d/ |grep old
  </p>
  
  <p>
    lrwxrwxrwx&nbsp; 1 root root 17 Aug 30 12:38 S99clsnd -> ../init.d/clsnd
  </p>
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1">第1章 软硬链接</a><ul>
        <li>
          <a href="#11">1.1 硬链接</a><ul>
            <li>
              <a href="#111">1.1.1 含义</a>
            </li>
            <li>
              <a href="#112">1.1.2 作用</a>
            </li>
            <li>
              <a href="#113">1.1.3 如何创建硬链接</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#12">1.2 软连接</a><ul>
            <li>
              <a href="#121">1.2.1 含义</a>
            </li>
            <li>
              <a href="#122">1.2.2 创建软连接</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#13">1.3 软连接与硬链接的区别</a><ul>
            <li>
              <a href="#131">1.3.1 含义</a>
            </li>
            <li>
              <a href="#132">1.3.2 创建方式不同</a>
            </li>
            <li>
              <a href="#133">1.3.3 不同的特点</a>
            </li>
            <li>
              <a href="#134">1.3.4 如何删除</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2">第2章 文件删除原理</a><ul>
        <li>
          <a href="#21">2.1 彻底删除一个文件</a>
        </li>
        <li>
          <a href="#22">2.2 查看某个文件是否总有人在用</a>
        </li>
        <li>
          <a href="#23">2.3 重启对应的软件/服务</a>
        </li>
        <li>
          <a href="#24">2.4 磁盘空间满了（三种情况）</a>
        </li>
        <li>
          <a href="#25">2.5 故障案例</a><ul>
            <li>
              <a href="#251">2.5.1 环境</a>
            </li>
            <li>
              <a href="#252">2.5.2 查看此时此刻磁盘使用情况</a>
            </li>
            <li>
              <a href="#253">2.5.3 删除文件</a>
            </li>
            <li>
              <a href="#254">2.5.4 检查空间没有被释放</a>
            </li>
            <li>
              <a href="#255_0">2.5.5 查看被删除（硬链接数为0）但是还被进程调用的文件</a>
            </li>
            <li>
              <a href="#256">2.5.6 重启对应的服务</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3_nbsp">第3章 找出某个文件的其他的硬链接&nbsp;</a>
    </li>
    <li>
      <a href="#4">第4章 三种时间戳</a><ul>
        <li>
          <a href="#41">4.1 含义</a>
        </li>
        <li>
          <a href="#42_stat">4.2 使用stat命令查看文件的信息</a>
        </li>
        <li>
          <a href="#43_mtimechange">4.3 修改mtime&change</a>
        </li>
        <li>
          <a href="#44_ctime">4.4 修改ctime （属性变更时间）</a>
        </li>
        <li>
          <a href="#45_atime">4.5 atime修改</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#5_chkconfig">第5章 chkconfig命令相关</a><ul>
        <li>
          <a href="#51_chkconfig_iptables_on_chkconfig_iptables_off">5.1 执行chkconfig iptables on & chkconfig iptables off之后发生了什么？</a>
        </li>
        <li>
          <a href="#52">5.2 让一个软件开机自启动</a>
        </li>
        <li>
          <a href="#53_chkconfig">5.3 如何让一个服务或命令通过chkconfig管理</a><ul>
            <li>
              <a href="#531_etcinitd">5.3.1 脚本必须放在/etc/init.d/目录下面</a>
            </li>
            <li>
              <a href="#532_chkconfig">5.3.2 必须写出chkconfig格式</a>
            </li>
            <li>
              <a href="#533">5.3.3 给这个脚本添加上执行的权限</a>
            </li>
            <li>
              <a href="#534_chkconfig">5.3.4 添加脚本到chkconfig管理</a>
            </li>
            <li>
              <a href="#535">5.3.5 查看状态</a>
            </li>
            <li>
              <a href="#536_etcrc3d">5.3.6 查看/etc/rc3.d/目录生成相对应的软连接</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</div>