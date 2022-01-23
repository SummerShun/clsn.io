---
title: 磁盘管理 之 parted命令添加swap，文件系统
author: 惨绿少年
type: post
date: 2017-10-08T18:09:00+00:00
url: /clsn/lx961.html
Baidusubmit:
  - 1
views:
  - 104
categories:
  - Linux运维
  - 存储
  - 安全
  - 运维基本功

---
# <span id="1">第1章 <span style="font-family: 新宋体;">磁盘管理</span></span>

## <span id="11">1.1 <span style="font-family: 新宋体;">必须要了解的。</span></span>

### <span id="111_ps_aux_RSS_VSZ">1.1.1 ps aux <span style="font-family: 新宋体;">命令中</span> RSS <span style="font-family: 新宋体;">与</span>VSZ<span style="font-family: 新宋体;">的含义</span></span>

<p style="text-indent: 21.0pt;">
  rss <span style="font-family: 新宋体;">进程占用的物理内存的大小</span> <span style="font-family: 新宋体;">单位：</span>kb <span style="font-family: 新宋体;">；</span>
</p>

&nbsp;&nbsp; vsz <span style="font-family: 新宋体;">进程占用的虚拟的内存大小（物理内存</span>+swap<span style="font-family: 新宋体;">）</span>

### <span id="112_top">1.1.2 top<span style="font-family: 新宋体;">命令的参数</span></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; M&nbsp;&nbsp; <span style="font-family: 新宋体;">按照内存使用率排序</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; P&nbsp;&nbsp; <span style="font-family: 新宋体;">按照</span>cpu<span style="font-family: 新宋体;">的使用率排序</span>

### <span id="113_htop">1.1.3 htop <span style="font-family: 新宋体;">命令的安装方法</span></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">要配置</span> epel<span style="font-family: 新宋体;">源</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; http://mirrors.aliyun.com

## <span id="12_parted_gpt">1.2 <span style="font-family: 新宋体;">磁盘分区之</span>parted + gpt</span>

### <span id="121_fdisk_parted">1.2.1 fdisk <span style="font-family: 新宋体;">与</span> parted <span style="font-family: 新宋体;">的区别</span></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: aqua;">fdisk&nbsp; &nbsp;&nbsp;mbr </span><span style="font-family: 新宋体;">分区表</span>&nbsp;&nbsp; <span style="font-family: 新宋体;">硬盘容量<span style="background: aqua;">小于</span></span><span style="background: aqua;">2TB</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: fuchsia;">parted &nbsp;&nbsp;gpt &nbsp;</span><span style="font-family: 新宋体;">分区表</span>&nbsp;&nbsp; <span style="font-family: 新宋体;">硬盘容量<span style="background: fuchsia;">大于</span></span><span style="background: fuchsia;">2TB</span>

### <span id="122">1.2.2 <span style="font-family: 新宋体;">查看下帮助信息</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">parted</span> <span style="background: lime;">/dev/sdc</span>
  </p>
  
  <p>
    GNU Parted 2.1
  </p>
  
  <p>
    Using /dev/sdc
  </p>
  
  <p>
    Welcome to GNU Parted! Type 'help' to view a list of commands.
  </p>
  
  <p>
    (parted)&nbsp;&nbsp;&nbsp; <span style="background: lime;">h</span>
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    &nbsp; <span style="background: lime;">mklabel,mktable LABEL-TYPE&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create a new disklabel (partition table)</span>
  </p>
  
  <p>
    &nbsp;<span style="font-family: 新宋体; background: lime;">创建分区表</span>
  </p>
  
  <p>
    &nbsp; <span style="background: yellow;">mkpart PART-TYPE [FS-TYPE] START END&nbsp;&nbsp;&nbsp;&nbsp; make a partition</span>
  </p>
  
  <p>
    &nbsp;<span style="font-family: 新宋体; background: yellow;">创建一个分区</span>
  </p>
  
  <p>
    &nbsp; <span style="background: lime;">mkpartfs PART-TYPE FS-TYPE START END&nbsp;&nbsp;&nbsp;&nbsp; make a partition with a file system</span>
  </p>
  
  <p>
    &nbsp;<span style="font-family: 新宋体; background: lime;">创建一个分区</span> <span style="font-family: 新宋体; background: lime;">分区带着文件系统</span>
  </p>
  
  <p>
    &nbsp; <span style="background: yellow;">print [devices|free|list,all|NUMBER]&nbsp;&nbsp;&nbsp;&nbsp; display the partition table, available</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; devices, free space, all found partitions, or a particular partition
  </p>
  
  <p>
    &nbsp;<span style="font-family: 新宋体; background: yellow;">显示分区信息</span>
  </p>
  
  <p>
    &nbsp; <span style="background: lime;">rm NUMBER&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; delete partition NUMBER</span>
  </p>
  
  <p>
    &nbsp;<span style="font-family: 新宋体; background: lime;">删除一个分区</span>
  </p>
</div>

### <span id="123">1.2.3 <span style="font-family: 新宋体;">创建分区表</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]<span style="background: lime;"># parted /dev/sdc</span>
  </p>
  
  <p>
    GNU Parted 2.1
  </p>
  
  <p>
    Using /dev/sdc
  </p>
  
  <p>
    Welcome to GNU Parted! Type 'help' to view a list of commands.
  </p>
  
  <p>
    (parted) <span style="background: lime;">mklabel gpt&nbsp;&nbsp; #</span><span style="font-family: 新宋体; background: yellow;">创建</span><span style="background: yellow;">GPT</span><span style="font-family: 新宋体; background: yellow;">分区表</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  </p>
  
  <p>
    (parted)<span style="background: lime;"> p&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>
  </p>
  
  <p>
    Model: VMware, VMware Virtual S (scsi)
  </p>
  
  <p>
    Disk /dev/sdc: 107MB
  </p>
  
  <p>
    Sector size (logical/physical): 512B/512B
  </p>
  
  <p>
    Partition Table: gpt
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    Number&nbsp; Start&nbsp; End&nbsp; Size&nbsp; File system&nbsp; Name&nbsp; Flags
  </p>
</div>

&nbsp;

### <span id="124_mkpart_PART-TYPE">1.2.4 mkpart <span style="font-family: 新宋体;">可以使用的</span>PART-TYPE<span style="font-family: 新宋体;">类型</span></span>

<p style="margin-left: 21.0pt;">
  GPT <span style="font-family: 新宋体;">格式可以创建</span>N<span style="font-family: 新宋体;">个主分区，所以类型都选为主分区即可。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    (parted) <span style="background: lime;">help mkpart&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>
  </p>
  
  <p>
    &nbsp; mkpart PART-TYPE [FS-TYPE] START END&nbsp;&nbsp;&nbsp;&nbsp; make a partition
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; PART-TYPE is one of: <span style="background: lime;">primary</span>,<span style="background: lime;"> logical</span>, <span style="background: lime;">extended</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体; background: lime;">主分区</span> <span style="font-family: 新宋体;">，<span style="background: lime;">逻辑分区</span></span> <span style="font-family: 新宋体;">，<span style="background: lime;">扩展分区</span></span>
  </p>
</div>

### <span id="125">1.2.5 <span style="font-family: 新宋体;">对磁盘进行分区</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    (parted) <span style="background: yellow;">mkpart<span style="background: lime;"> primary</span><span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;"> 0 10</span> </span>
  </p>
  
  <p>
    <span style="color: red;">Warning: </span>The resulting partition is not properly aligned for best performance.
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">提示分区没有对齐，这个错误无视即可。</span>
  </p>
  
  <p>
    Ignore/Cancel? <span style="background: lime;">I&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family: 新宋体;">忽略</span>/<span style="font-family: 新宋体;">取消</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  </p>
  
  <p>
    (parted) <span style="background: lime;">p&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>
  </p>
  
  <p>
    Model: VMware, VMware Virtual S (scsi)
  </p>
  
  <p>
    Disk /dev/sdc: 107MB
  </p>
  
  <p>
    Sector size (logical/physical): 512B/512B
  </p>
  
  <p>
    Partition Table: gpt
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    Number&nbsp; Start&nbsp;&nbsp; End&nbsp;&nbsp;&nbsp;&nbsp; Size&nbsp;&nbsp;&nbsp; File system&nbsp; Name&nbsp;&nbsp;&nbsp;&nbsp; Flags
  </p>
  
  <p>
    &nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 17.4kB&nbsp; 10.0MB&nbsp; 9983kB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; primary
  </p>
</div>

### <span id="126">1.2.6 <span style="font-family: 新宋体;">再创建一个分区</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    (parted) <span style="background: lime;">mkpart primary 10 20</span>
  </p>
  
  <p>
    (parted) <span style="background: lime;">p&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>
  </p>
  
  <p>
    Model: VMware, VMware Virtual S (scsi)
  </p>
  
  <p>
    Disk /dev/sdc: 107MB
  </p>
  
  <p>
    Sector size (logical/physical): 512B/512B
  </p>
  
  <p>
    Partition Table: gpt
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    Number&nbsp; Start&nbsp;&nbsp; End&nbsp;&nbsp;&nbsp;&nbsp; Size&nbsp;&nbsp;&nbsp; File system&nbsp; Name&nbsp;&nbsp;&nbsp;&nbsp; Flags
  </p>
  
  <p>
    &nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 17.4kB&nbsp; 10.0MB&nbsp; 9983kB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; primary
  </p>
  
  <p>
    &nbsp;2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 10.5MB&nbsp; 19.9MB&nbsp; 9437kB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; primary
  </p>
</div>

<span style="color: red;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family: 'Segoe UI Emoji',sans-serif; times new roman"4times new roman"; color: red;">⚠</span><span style="font-family: 新宋体; times new roman"4times new roman";color: red;">注意：</span><span style="color: red;">parted </span><span style="font-family: 新宋体; times new roman"4times new roman";color: red;">创建分区实时生效，比较危险。</span>

## <span id="13_parted">1.3 <span style="font-family: 新宋体;">使用</span>parted<span style="font-family: 新宋体;">命令非交互式创建分区</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">parted /dev/sdc mkpart primary 50 100</span>
  </p>
  
  <p>
    Information: You may need to update /etc/fstab.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    [root@znix ~]# <span style="background: lime;">parted /dev/sdc print</span> <span style="background: lime;">#</span><span style="font-family: 新宋体; background: lime;">显示磁盘的格式</span>
  </p>
  
  <p>
    Model: VMware, VMware Virtual S (scsi)
  </p>
  
  <p>
    Disk /dev/sdc: 107MB
  </p>
  
  <p>
    Sector size (logical/physical): 512B/512B
  </p>
  
  <p>
    Partition Table: gpt
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    Number&nbsp; Start&nbsp;&nbsp; End&nbsp;&nbsp;&nbsp;&nbsp; Size&nbsp;&nbsp;&nbsp; File system&nbsp; Name&nbsp;&nbsp;&nbsp;&nbsp; Flags
  </p>
  
  <p>
    &nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 17.4kB&nbsp; 50.0MB&nbsp; 50.0MB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; primary
  </p>
  
  <p>
    &nbsp;2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 50.3MB&nbsp; 99.6MB&nbsp; 49.3MB&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; primary
  </p>
</div>

## <span id="14_swap">1.4 <span style="font-family: 新宋体;">创建</span>swap<span style="font-family: 新宋体;">分区及使用</span></span>

<span style="font-family: 新宋体;">【</span><span style="color: #1f497d;">JAVA</span><span style="font-family: 新宋体; times new roman"4times new roman"; color: #1F497D;">环境常见</span><span style="font-family: 新宋体;">】</span>linux<span style="font-family: 新宋体;">内存不够用，会使用</span>swap<span style="font-family: 新宋体;">分区。</span>

### <span id="141_swap">1.4.1 <span style="font-family: 新宋体;">手动添加</span>swap<span style="font-family: 新宋体;">空间，创建一个文件</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">使用</span>dd <span style="font-family: 新宋体;">命令创建一个块文件。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">dd if=/dev/zero of=/tmp/100M bs=1M count=100</span>
  </p>
  
  <p>
    100+0 records in
  </p>
  
  <p>
    100+0 records out
  </p>
  
  <p>
    104857600 bytes (105 MB) copied, 2.96654 s, 35.3 MB/s
  </p>
  
  <p>
    [root@znix ~]# ll -h /tmp/100M
  </p>
  
  <p>
    -rw-r--r-- 1 root root 100M Sep 18 10:01 /tmp/100M
  </p>
</div>

### <span id="142">1.4.2 <span style="font-family: 新宋体;">查看创建出来的文件的类型</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">现在的类型为</span>data <span style="font-family: 新宋体;">数据块。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">file /tmp/100M</span>
  </p>
  
  <p>
    /tmp/100M: <span style="background: lime;">data</span>
  </p>
</div>

### <span id="143_swap">1.4.3 <span style="font-family: 新宋体;">将这个文件变成</span>swap</span>

<p style="margin-left: 21.0pt;">
  mkswap<span style="font-family: 新宋体;">命令将文件类型格式化成</span>swap<span style="font-family: 新宋体;">格式</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">mkswap /tmp/100M</span>
  </p>
  
  <p>
    mkswap: /tmp/100M: warning: don't erase bootbits sectors
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; on whole disk. Use -f to force.
  </p>
  
  <p>
    Setting up swapspace version 1, size = 102396 KiB
  </p>
  
  <p>
    no label, UUID=81fa08be-a18f-4bc6-b950-fa3d90f969a1
  </p>
</div>

### <span id="144">1.4.4 <span style="font-family: 新宋体;">修改之后的文件类型：</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">file /tmp/100M</span>
  </p>
  
  <p>
    /tmp/100M: Linux/i386 <span style="background: lime;">swap</span> file (new style) 1 (4K pages) size 25599 pages
  </p>
</div>

### <span id="145_swap">1.4.5 <span style="font-family: 新宋体;">让这个文件起作用，将</span>swap<span style="font-family: 新宋体;">空间添加到系统中</span></span>

<p style="margin-left: 0cm; text-indent: 0cm;">
  实例1-1 <span style="font-family: 新宋体;">查看</span>swap<span style="font-family: 新宋体;">的所使用情况</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">free -h</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; total&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; used&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; free&nbsp;&nbsp;&nbsp;&nbsp; shared&nbsp;&nbsp;&nbsp; buffers&nbsp;&nbsp;&nbsp;&nbsp; cached
  </p>
  
  <p>
    Mem:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 474M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 465M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8.8M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 252K&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 15M&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;357M
  </p>
  
  <p>
    -/+ buffers/cache:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 93M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 381M
  </p>
  
  <p>
    Swap:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: yellow;">767M</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0B&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: yellow;">767M</span>
  </p>
</div>

<p style="margin-left: 0cm; text-indent: 0cm;">
  实例1-2 <span style="font-family: 新宋体;">使用</span>swap<span style="font-family: 新宋体;">命令将</span>swap<span style="font-family: 新宋体;">文件，添加到系统中。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">swapon&nbsp; /tmp/100M</span>
  </p>
</div>

<p style="margin-left: 0cm; text-indent: 0cm;">
  实例1-3 <span style="font-family: 新宋体;">现在查看</span> swap<span style="font-family: 新宋体;">的使用情况</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">free -h</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; total&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; used&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; free&nbsp;&nbsp;&nbsp;&nbsp; shared&nbsp;&nbsp;&nbsp; buffers&nbsp;&nbsp;&nbsp;&nbsp; cached
  </p>
  
  <p>
    Mem:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 474M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 465M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8.7M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 252K&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 15M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 357M
  </p>
  
  <p>
    -/+ buffers/cache:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 93M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 381M
  </p>
  
  <p>
    Swap:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: yellow;">867M</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0B&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: yellow;">867M</span>
  </p>
</div>

<p style="margin-left: 0cm; text-indent: 0cm;">
  实例1-4 <span style="font-family: 新宋体;">查看</span>swap<span style="font-family: 新宋体;">的详细信息，使用</span> swap&nbsp; -s <span style="font-family: 新宋体;">。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">swapon&nbsp; -s</span>
  </p>
  
  <p>
    Filename&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Size&nbsp;&nbsp;&nbsp; Used&nbsp;&nbsp;&nbsp; Priority
  </p>
  
  <p>
    /dev/sda2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; partition&nbsp;&nbsp; 786428&nbsp; 0&nbsp;&nbsp; -1
  </p>
  
  <p>
    /tmp/100M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; file&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 102396&nbsp; 0&nbsp;&nbsp; -2
  </p>
</div>

### <span id="146_swap">1.4.6 <span style="font-family: 新宋体;">如何让添加的</span>swap<span style="font-family: 新宋体;">文件永久生效</span></span>

<p style="margin-left: 18.0pt; text-indent: -18.0pt;">
  1）<span style="font-family: 新宋体;">把命令放入</span>/etc/rc.local <span style="font-family: 新宋体;">开机自启动文件中。</span>
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  a)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: 'Times New Roman';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="background: lime;">swapon&nbsp; /tmp/100M</span>&nbsp;<span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">命令</span>
</p>

2<span style="font-family: 新宋体;">）写入</span>/etc/fstab <span style="font-family: 新宋体;">文件中</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    /tmp/100M&nbsp;&nbsp;&nbsp; swap&nbsp;&nbsp; swap&nbsp;&nbsp; defaults&nbsp;&nbsp;&nbsp; 0 0
  </p>
  
  <p style="text-indent: 199.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: 新宋体;">第</span>5<span style="font-family: 新宋体;">列</span> dump<span style="font-family: 新宋体;">备份</span>
  </p>
  
  <p style="text-indent: 199.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: 新宋体;">第</span>6<span style="font-family: 新宋体;">列</span> <span style="font-family: 新宋体;">磁盘检查</span>
  </p>
</div>

## <span id="15">1.5 <span style="font-family: 新宋体;">文件系统</span></span>

### <span id="151">1.5.1 <span style="font-family: 新宋体;">文件系统的作用：</span></span>

<p style="margin-left: 18.0pt; text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">文件系统决定文件在磁盘上是怎么存放的</span>
</p>

### <span id="152">1.5.2 <span style="font-family: 新宋体;">文件系统的组成：</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">超级块</span> super block<span style="font-family: 新宋体;">&middot;</span> <span style="background: lime;">dumpe2fs -h /dev/sdb1</span> <span style="font-family: 新宋体;">显示超级快中的信息。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">dumpe2fs -h /dev/sdb1</span>
  </p>
  
  <p>
    dumpe2fs 1.41.12 (17-May-2010)
  </p>
  
  <p>
    Filesystem volume name:&nbsp;&nbsp; <none>
  </p>
  
  <p>
    Last mounted on:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <not available>
  </p>
  
  <p>
    Filesystem UUID:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 7101630b-b325-49d1-92b9-0a500c2a07f6
  </p>
  
  <p>
    Filesystem magic number:&nbsp; 0xEF53
  </p>
  
  <p>
    Filesystem revision #:&nbsp;&nbsp;&nbsp; 1 (dynamic)
  </p>
  
  <p>
    Filesystem features:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; has_journal ext_attr resize_inode dir_index filetype extent flex_bg sparse_super huge_file uninit_bg dir_nlink extra_isize
  </p>
  
  <p>
    Filesystem flags:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; signed_directory_hash
  </p>
  
  <p>
    Default mount options:&nbsp;&nbsp;&nbsp; (none)
  </p>
  
  <p>
    Filesystem state:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; clean
  </p>
  
  <p>
    Errors behavior:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Continue
  </p>
  
  <p>
    Filesystem OS type:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Linux
  </p>
  
  <p>
    Inode count:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 25896
  </p>
  
  <p>
    Block count:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 103424
  </p>
  
  <p>
    Reserved block count:&nbsp;&nbsp;&nbsp;&nbsp; 5171
  </p>
  
  <p>
    Free blocks:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 94502
  </p>
  
  <p>
    Free inodes:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 25885
  </p>
  
  <p>
    First block:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1
  </p>
  
  <p>
    <span style="background: lime;">Block size:</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1024 &nbsp;# block<span style="font-family: 新宋体;">的大小</span>
  </p>
  
  <p>
    Fragment size:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1024
  </p>
  
  <p>
    Reserved GDT blocks:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 256
  </p>
  
  <p>
    Blocks per group:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8192
  </p>
  
  <p>
    Fragments per group:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 8192
  </p>
  
  <p>
    Inodes per group:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1992
  </p>
  
  <p>
    Inode blocks per group:&nbsp;&nbsp; 249
  </p>
  
  <p>
    Flex block group size:&nbsp;&nbsp;&nbsp; 16
  </p>
  
  <p>
    Filesystem created:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Fri Sep 15 12:01:27 2017
  </p>
  
  <p>
    Last mount time:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;Fri Sep 15 12:02:37 2017
  </p>
  
  <p>
    Last write time:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Fri Sep 15 16:38:30 2017
  </p>
  
  <p>
    <span style="background: lime;">Mount count:</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: lime;">#</span><span style="font-family: 新宋体; background: lime;">挂载的次数</span>
  </p>
  
  <p>
    Maximum mount count:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; -1
  </p>
  
  <p>
    <span style="font-family: 新宋体;">&hellip;&hellip;</span>
  </p>
</div>

## <span id="16">1.6 <span style="font-family: 新宋体;">常用的文件系统</span></span>

<p style="margin-left: 12.0pt;">
  opensuse linux <span style="font-family: 新宋体;">默认文件系统</span>&nbsp; <span style="background: lime;">ReiserFS</span>
</p>

<p style="margin-left: 12.0pt;">
  Centos7&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">采用</span> XFS <span style="font-family: 新宋体;">文件系统</span>
</p>

<p style="margin-left: 12.0pt;">
  Centos6 &nbsp; &nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">采用</span><span style="background: lime;">ext4</span> <span style="font-family: 新宋体;">文件系统</span>
</p>

<p style="margin-left: 12.0pt;">
  Centos5 &nbsp; &nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">采用</span>ext3 <span style="font-family: 新宋体;">文件系统</span>
</p>

<p style="margin-left: 12.0pt;">
  IBM <span style="font-family: 新宋体;">的</span> AIX<span style="font-family: 新宋体;">使用</span>JFS <span style="font-family: 新宋体;">日志文件系统。</span>
</p>

### <span id="161">1.6.1 <span style="font-family: 新宋体;">查看系统中的文件系统</span></span>

<p style="margin-left: 21.0pt;">
  df -T <span style="font-family: 新宋体;">参数，显示的是分区的文件类型</span> type <span style="font-family: 新宋体;">。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">df -Th</span>
  </p>
  
  <p>
    Filesystem&nbsp;&nbsp;&nbsp;&nbsp; Type&nbsp;&nbsp; Size&nbsp; Used Avail Use% Mounted on
  </p>
  
  <p>
    /dev/sda3&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: lime;">&nbsp;ext4&nbsp;&nbsp; </span>8.8G&nbsp; 2.1G&nbsp; 6.3G&nbsp; 26% /
  </p>
  
  <p>
    tmpfs&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: lime;">&nbsp;tmpfs&nbsp; </span>238M&nbsp;&nbsp;&nbsp;&nbsp; 0&nbsp; 238M&nbsp;&nbsp; 0% /dev/shm
  </p>
  
  <p>
    /dev/sda1&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: lime;">&nbsp;ext4&nbsp;&nbsp; </span>190M&nbsp;&nbsp; 40M&nbsp; 141M&nbsp; 22% /boot
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; tmpfs <span style="font-family: 新宋体;">是临时文件系统，速度较快。</span>

### <span id="162">1.6.2 <span style="font-family: 新宋体;">文件系统使用范围</span></span>

ReiserFS&nbsp; &nbsp;<span style="font-family: 新宋体;">适用于大量小文件的</span>

xfs&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">适合数据库</span>

ext4 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">使用较广，适用于大多数的用途。</span>

ext2 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">没有日志的功能</span> <span style="font-family: 新宋体;">（速度较快）</span>

## <span id="17">1.7 <span style="font-family: 新宋体;">测试磁盘的读写速度</span></span>

### <span id="171_dd">1.7.1 <span style="font-family: 新宋体;">测试写入速度</span> dd <span style="font-family: 新宋体;">命令</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">dd if=/dev/zero of=/tmp/100M bs=1M count=100</span>
  </p>
  
  <p>
    100+0 records in
  </p>
  
  <p>
    100+0 records out
  </p>
  
  <p>
    104857600 bytes (105 MB) copied, 2.96654 s, 35.3 MB/s
  </p>
  
  <p>
    [root@znix ~]# ll -h /tmp/100M
  </p>
  
  <p>
    -rw-r--r-- 1 root root 100M Sep 18 10:01 /tmp/100M
  </p>
</div>

### <span id="172_hdparm">1.7.2 <span style="font-family: 新宋体;">测试读取速度</span> hdparm</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">hdparm -t /dev/sdb</span>
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    /dev/sdb:
  </p>
  
  <p>
    &nbsp;Timing buffered disk reads: 102 MB in&nbsp; 0.81 seconds = 125.23 MB/sec
  </p>
</div>

# <span id="2_sed">第2章 sed<span style="font-family: 新宋体;">命令详解</span></span>

## <span id="21_sed">2.1 sed <span style="font-family: 新宋体;">命令的作用</span></span>

sed <span style="font-family: 新宋体;">取某一行</span> <span style="font-family: 新宋体;">查找替换。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">增加</span> <span style="font-family: 新宋体;">删除</span> <span style="font-family: 新宋体;">修改</span> <span style="font-family: 新宋体;">查询</span>

<p style="text-indent: 21.0pt;">
  sed == stream editor <span style="font-family: 新宋体;">字符流编辑器</span>
</p>

sed<span style="font-family: 新宋体;">命令的格式：</span>

<p style="text-indent: 21.0pt;">
  <span style="background: lime;">sed '</span><span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">找谁干啥</span><span style="background: lime;">' files</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="text-indent: 31.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    pattern space <span style="font-family: 新宋体;">模式空间</span>
  </p>
  
  <p style="text-indent: 31.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    hold space&nbsp;&nbsp; <span style="font-family: 新宋体;">保留空间</span>
  </p>
</div>

## <span id="22_sed">2.2 sed<span style="font-family: 新宋体;">常用命令的功能</span></span>

### <span id="221">2.2.1 <span style="font-family: 新宋体;">环境准备</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">cat person.txt</span>
  </p>
  
  <p>
    101,clsn,CEO
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

## <span id="23">2.3 <span style="font-family: 新宋体;">查询过程</span></span>

### <span id="231">2.3.1 <span style="font-family: 新宋体;">指定行号</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed -n '3p' person.txt</span>
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
</div>

### <span id="232_p">2.3.2 <span style="font-family: 新宋体;">指定内容，</span>p<span style="font-family: 新宋体;">显示</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed -n '/yy/p'</span> person.txt
  </p>
  
  <p>
    104,yy,CFO
  </p>
</div>

### <span id="233">2.3.3 <span style="font-family: 新宋体;">查找连续的行（指定行号）</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed -n '1,5p'</span> person.txt
  </p>
  
  <p>
    101,clsn,CEO
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

### <span id="234_101103">2.3.4 <span style="font-family: 新宋体;">从包含</span>101<span style="font-family: 新宋体;">的行，到包含</span>103<span style="font-family: 新宋体;">的行</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed -n '/101/,/103/p'</span> person.txt
  </p>
  
  <p>
    101,clsn,CEO
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
</div>

### <span id="235">2.3.5 <span style="font-family: 新宋体;">从某一行到最后一行</span></span>

<p style="margin-left: 21.0pt;">
  $<span style="font-family: 新宋体;">在</span>sed<span style="font-family: 新宋体;">中表示最后一行。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed -n '$p</span>' person.txt
  </p>
  
  <p>
    105,hehe,CIO
  </p>
  
  <p>
    [root@znix ~]# <span style="background: lime;">sed -n '2,$p</span>' person.txt
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

### <span id="236_145">2.3.6 <span style="font-family: 新宋体;">找第</span>1<span style="font-family: 新宋体;">，</span>4<span style="font-family: 新宋体;">，</span>5<span style="font-family: 新宋体;">行</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">一行中有多个命令用</span>;<span style="font-family: 新宋体;">分隔。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed '1p;4p;5p' -n</span> person.txt
  </p>
  
  <p>
    101,clsn,CEO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

## <span id="24_sed">2.4 sed<span style="font-family: 新宋体;">的删除测试</span></span>

<p style="margin-left: 18.0pt;">
  d <span style="font-family: 新宋体;">删除</span>
</p>

### <span id="241">2.4.1 <span style="font-family: 新宋体;">删除第一行</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">加上</span>-i <span style="font-family: 新宋体;">参数，删除文件的内容</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed '1d'</span> person.txt
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

### <span id="242_clsn">2.4.2 <span style="font-family: 新宋体;">显示不包含</span>clsn<span style="font-family: 新宋体;">的行</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体; times new roman"4times new roman"; color: red;">！</span><span style="font-family: 新宋体;">表示取反</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed '/clsn/d'</span> person.txt
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
  
  <p>
    [root@znix ~]# <span style="background: lime;">sed -n&nbsp; '/clsn/!p'</span> person.txt
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

## <span id="25">2.5 <span style="font-family: 新宋体;">插入</span></span>

### <span id="251_i_insert">2.5.1 i <span style="font-family: 新宋体;">插入到文件的行的上一行</span> insert</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed '3</span><span style="background: yellow;">i</span><span style="background: lime;"> 100,znix,OOO'</span> person.txt &nbsp;#i<span style="font-family: 新宋体;">之后的空格就可以不些</span>
  </p>
  
  <p>
    101,clsn,CEO
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    100,znix,OOO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

### <span id="252_a_append">2.5.2 a <span style="font-family: 新宋体;">追加到文件的行的下一行</span> append</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sed '3</span><span style="background: yellow;">a</span><span style="background: lime;"> 100,znix,OOO'</span> person.txt #a<span style="font-family: 新宋体;">之后的空格就可以不些</span>
  </p>
  
  <p>
    101,clsn,CEO
  </p>
  
  <p>
    102,znix,CTO
  </p>
  
  <p>
    103,Nmtui,COO
  </p>
  
  <p>
    100,znix,OOO
  </p>
  
  <p>
    104,yy,CFO
  </p>
  
  <p>
    105,hehe,CIO
  </p>
</div>

# <span id="3_linuxwindows">第3章 linux<span style="font-family: 新宋体;">里面与</span>windows<span style="font-family: 新宋体;">互相传文件</span></span>

## <span id="31_lrzsz_yum">3.1 <span style="font-family: 新宋体;">使用</span> lrzsz <span style="font-family: 新宋体;">，需要</span>yum <span style="font-family: 新宋体;">安装</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# yum install lrzsz
  </p>
</div>

<p style="text-indent: 21.0pt;">
  <span style="background: lime;">rz&nbsp; </span><span style="font-family: 新宋体;">把文件上传到</span>linux <span style="font-family: 新宋体;">（直接把</span>windows<span style="font-family: 新宋体;">文件拖到</span><span style="color: red;">xshell</span><span style="font-family: 新宋体; times new roman"4times new roman";color: red;">窗口</span><span style="font-family: 新宋体;">即可）</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="background: lime;">sz&nbsp; </span><span style="font-family: 新宋体;">把</span>linux<span style="font-family: 新宋体;">的文件下载到</span>windows<span style="font-family: 新宋体;">中</span>.
</p>

## <span id="32">3.2 <span style="font-family: 新宋体;">把文件打包，压缩。</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 新宋体;">打包格式要在</span>linux<span style="font-family: 新宋体;">和</span>windows <span style="font-family: 新宋体;">中都可以使用，可以选择</span>zip<span style="font-family: 新宋体;">格式。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: yellow;">zip -r /tmp/etc_$(date +%F).zip /etc/</span>
  </p>
  
  <p>
    &nbsp; adding: etc/ (stored 0%)
  </p>
  
  <p>
    &nbsp; adding: etc/passwd (deflated 61%)
  </p>
  
  <p>
    &nbsp; adding: etc/ltrace.conf (deflated 73%)
  </p>
  
  <p>
    &nbsp; adding: etc/filesystems (deflated 16%)
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">&hellip;&hellip;</span>
  </p>
</div>

## <span id="33">3.3 <span style="font-family: 新宋体;">下载文件</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: yellow;">sz</span> /tmp/ser_2017-09-08_16.tar.gz&nbsp;
  </p>
</div>

## <span id="34">3.4 <span style="font-family: 新宋体;">长传文件</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">rz</span>&nbsp; &nbsp;&nbsp;
  </p>
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1">第1章 磁盘管理</a><ul>
        <li>
          <a href="#11">1.1 必须要了解的。</a><ul>
            <li>
              <a href="#111_ps_aux_RSS_VSZ">1.1.1 ps aux 命令中 RSS 与VSZ的含义</a>
            </li>
            <li>
              <a href="#112_top">1.1.2 top命令的参数</a>
            </li>
            <li>
              <a href="#113_htop">1.1.3 htop 命令的安装方法</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#12_parted_gpt">1.2 磁盘分区之parted + gpt</a><ul>
            <li>
              <a href="#121_fdisk_parted">1.2.1 fdisk 与 parted 的区别</a>
            </li>
            <li>
              <a href="#122">1.2.2 查看下帮助信息</a>
            </li>
            <li>
              <a href="#123">1.2.3 创建分区表</a>
            </li>
            <li>
              <a href="#124_mkpart_PART-TYPE">1.2.4 mkpart 可以使用的PART-TYPE类型</a>
            </li>
            <li>
              <a href="#125">1.2.5 对磁盘进行分区</a>
            </li>
            <li>
              <a href="#126">1.2.6 再创建一个分区</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#13_parted">1.3 使用parted命令非交互式创建分区</a>
        </li>
        <li>
          <a href="#14_swap">1.4 创建swap分区及使用</a><ul>
            <li>
              <a href="#141_swap">1.4.1 手动添加swap空间，创建一个文件</a>
            </li>
            <li>
              <a href="#142">1.4.2 查看创建出来的文件的类型</a>
            </li>
            <li>
              <a href="#143_swap">1.4.3 将这个文件变成swap</a>
            </li>
            <li>
              <a href="#144">1.4.4 修改之后的文件类型：</a>
            </li>
            <li>
              <a href="#145_swap">1.4.5 让这个文件起作用，将swap空间添加到系统中</a>
            </li>
            <li>
              <a href="#146_swap">1.4.6 如何让添加的swap文件永久生效</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#15">1.5 文件系统</a><ul>
            <li>
              <a href="#151">1.5.1 文件系统的作用：</a>
            </li>
            <li>
              <a href="#152">1.5.2 文件系统的组成：</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#16">1.6 常用的文件系统</a><ul>
            <li>
              <a href="#161">1.6.1 查看系统中的文件系统</a>
            </li>
            <li>
              <a href="#162">1.6.2 文件系统使用范围</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#17">1.7 测试磁盘的读写速度</a><ul>
            <li>
              <a href="#171_dd">1.7.1 测试写入速度 dd 命令</a>
            </li>
            <li>
              <a href="#172_hdparm">1.7.2 测试读取速度 hdparm</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2_sed">第2章 sed命令详解</a><ul>
        <li>
          <a href="#21_sed">2.1 sed 命令的作用</a>
        </li>
        <li>
          <a href="#22_sed">2.2 sed常用命令的功能</a><ul>
            <li>
              <a href="#221">2.2.1 环境准备</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#23">2.3 查询过程</a><ul>
            <li>
              <a href="#231">2.3.1 指定行号</a>
            </li>
            <li>
              <a href="#232_p">2.3.2 指定内容，p显示</a>
            </li>
            <li>
              <a href="#233">2.3.3 查找连续的行（指定行号）</a>
            </li>
            <li>
              <a href="#234_101103">2.3.4 从包含101的行，到包含103的行</a>
            </li>
            <li>
              <a href="#235">2.3.5 从某一行到最后一行</a>
            </li>
            <li>
              <a href="#236_145">2.3.6 找第1，4，5行</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#24_sed">2.4 sed的删除测试</a><ul>
            <li>
              <a href="#241">2.4.1 删除第一行</a>
            </li>
            <li>
              <a href="#242_clsn">2.4.2 显示不包含clsn的行</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#25">2.5 插入</a><ul>
            <li>
              <a href="#251_i_insert">2.5.1 i 插入到文件的行的上一行 insert</a>
            </li>
            <li>
              <a href="#252_a_append">2.5.2 a 追加到文件的行的下一行 append</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3_linuxwindows">第3章 linux里面与windows互相传文件</a><ul>
        <li>
          <a href="#31_lrzsz_yum">3.1 使用 lrzsz ，需要yum 安装</a>
        </li>
        <li>
          <a href="#32">3.2 把文件打包，压缩。</a>
        </li>
        <li>
          <a href="#33">3.3 下载文件</a>
        </li>
        <li>
          <a href="#34">3.4 长传文件</a>
        </li>
      </ul>
    </li>
  </ul>
</div>