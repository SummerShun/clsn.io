---
title: Linux系统安装_Centos6.9
author: 惨绿少年
type: post
date: 2017-09-06T23:06:00+00:00
url: /clsn/lx999.html
Baidusubmit:
  - 1
views:
  - 119
categories:
  - Linux运维
  - 安全
  - 玩转Linux
  - 网络技术
  - 自动化
  - 运维基本功

---
## 第1章&nbsp;<span style="font-family: 新宋体;">虚拟机安装</span>

<div>&nbsp;1.1 <span style="font-family: 新宋体;">镜像下载</span></div>

### 1.1.1 <span style="font-family: 新宋体;">新版本下载</span>

http://mirrors.aliyun.com&nbsp; #<span style="font-family: 新宋体;">阿里云官方镜像站点</span>

### 1.1.2 <span style="font-family: 新宋体;">旧版本下载</span>

http://vault.centos.org/&nbsp;&nbsp; #vault&nbsp; <span style="font-family: 新宋体;">电子仓库</span>

<span style="font-family: 新宋体;">尽量使用种子文件下载，速度较快</span>

![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221300687-1484084097.png)

## 1.2 VMware<span style="font-family: 新宋体;">新建虚拟机</span>

### 1.2.1 <span style="font-family: 新宋体;">新建虚拟机</span>

![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221320546-768373430.png)

### 1.2.2 <span style="font-family: 新宋体;">选择自定义</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221344609-54448267.png)

### 1.2.3 <span style="font-family: 新宋体;">兼容性选择</span>

<span style="font-family: 新宋体;">兼容性选择默认即可。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221406656-1144356862.png)

&nbsp;

### 1.2.4 <span style="font-family: 新宋体;">系统选择</span>

<span style="font-family: 新宋体;">选择<span style="background: lime;">稍后安装操作系统</span>，直接使用镜像会自动安装。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221430937-918321842.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">选择</span>centos 64 <span style="font-family: 新宋体;">位系统。</span>

![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221438906-1439106463.png)

### 1.2.5 <span style="font-family: 新宋体;">虚拟机位置</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221450140-473036602.png)

### 1.2.6 cpu<span style="font-family: 新宋体;">、内存</span>

cpu<span style="font-family: 新宋体;">个数根据需要设置。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221505515-1149355158.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">内存大小，在安装时内存<span style="background: lime;">不能少于</span></span><span style="background: lime;">1G</span><span style="font-family: 新宋体;">。</span>

![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221518281-2021578890.png)&nbsp;

### 1.2.7 <span style="font-family: 新宋体;">网络</span>

<span style="font-family: 新宋体;">网络使用</span>NAT

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221530202-294093382.png)

&nbsp;

### 1.2.8 <span style="font-family: 新宋体;">磁盘</span>

<span style="font-family: 新宋体;">磁盘类型选择默认的类型。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221556702-1501112735.png)![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221604343-1792544080.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">新创建一个虚拟磁盘，磁盘大小为</span>10G<span style="font-family: 新宋体;">。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">将磁盘拆分为多个文件。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">使用默认的磁盘文件名称。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221619734-406009392.png)![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221627468-194765352.png)&nbsp;

![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221646734-511098084.png)

### 1.2.9 <span style="font-family: 新宋体;">创建成功</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221706812-612623671.png)

## 1.3 <span style="font-family: 新宋体;">系统安装</span>

### 1.3.1 <span style="font-family: 新宋体;">挂载镜像</span>

<span style="font-family: 新宋体;">在</span>VMware<span style="font-family: 新宋体;">中打开新建虚拟机，单机</span>CD/DVD ,<span style="font-family: 新宋体;">选择使用</span>ISO<span style="font-family: 新宋体;">映像文件。找到下载好的文件，确定。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221723562-865290452.png)

### 1.3.2 <span style="font-family: 新宋体;">安装</span>

<span style="font-family: 新宋体;">开机，选择第一个，安装系统。</span>

Rescue installed system &nbsp;<span style="font-family: 新宋体;">救援系统。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221738046-265680436.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">检查完整性，选择<span style="background: lime;">跳过</span>。</span>

![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221751499-250014040.png)

<span style="font-family: 新宋体;">在安装系统时，如果屏幕看不到</span>next <span style="font-family: 新宋体;">可以使用</span><span style="background: lime;">F12.</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221809390-360874653.png)

### 1.3.3 <span style="font-family: 新宋体;">语言选择</span>

<span style="font-family: 新宋体;">语言选择</span><span style="background: lime;">English</span><span style="font-family: 新宋体;">，键盘模式默认即可。英文与中文在<span style="background: lime;">安装时会有差异</span>。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221818577-1410667839.png)![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221822281-1929466481.png)

### 1.3.4 <span style="font-family: 新宋体;">储存类型</span>

<span style="font-family: 新宋体;">选择基础的储存方式。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221830968-639338265.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">使用整块硬盘，并清除硬盘上的数据。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221838984-47939676.png)

### 1.3.5 <span style="font-family: 新宋体;">主机名称</span>

<span style="font-family: 新宋体;">根据实际情况，设置主机名称。注意：主机名支持的特殊字符不多。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221847843-24746696.png)

### 1.3.6 <span style="font-family: 新宋体;">日期与时间</span>

<span style="font-family: 新宋体;">时间选择</span>shanghai<span style="font-family: 新宋体;">，</span> <span style="font-family: 新宋体;">取消勾选</span>system clock uses UTC <span style="font-family: 新宋体;">。否则会出错。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221856062-528864636.png)

### 1.3.7 <span style="font-family: 新宋体;">用户密码</span>

<span style="font-family: 新宋体;">为了便于使用，密码为</span>123456<span style="font-family: 新宋体;">，该密码系统提示为弱密码，选择使用（</span>use anyway<span style="font-family: 新宋体;">）。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221903437-2076331944.png)

### 1.3.8 <span style="font-family: 新宋体;">磁盘分区</span>

<span style="font-family: 新宋体;">使用最后一项，自定义分区。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221916031-241425793.png)

/boot&nbsp;&nbsp;&nbsp; 200M <span style="font-family: 新宋体;">存放系统的引导的信息</span>

swap&nbsp;&nbsp;&nbsp; 768M <span style="font-family: 新宋体;">交换分区</span>&nbsp;<span style="font-family: 新宋体;">内存快用完之前使用交换分区</span>&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">临时内存</span>&nbsp;&nbsp;

<span style="font-family: 新宋体;">如果你的内存是</span>8G<span style="font-family: 新宋体;">以内的</span> <span style="font-family: 新宋体;">交换分区给内存的</span>1.5<span style="font-family: 新宋体;">倍</span>

<span style="font-family: 新宋体;">如果你的内存是</span>8G<span style="font-family: 新宋体;">以上的</span> <span style="font-family: 新宋体;">交换分区就给</span>8G

<span style="font-family: 新宋体;">以后内存给</span>512M<span style="font-family: 新宋体;">即可，</span>768M

/&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">根分区</span>&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">剩下多少给多少</span>

<span style="font-family: 新宋体;">先建立</span>/boot<span style="font-family: 新宋体;">分区用于引导系统，建立</span>swap <span style="font-family: 新宋体;">交互分区，将剩余空间分根目录。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221927202-84470230.png)![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221930343-1529741652.png)![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115221934093-390402346.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">分区建立完成后，弹出对话框，是否格式化分区，单击格式化。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115222000734-917864134.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">点击下一步，将修改的信息写入硬盘</span> write changes to disk<span style="font-family: 新宋体;">。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115222012031-835735960.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">系统安装位置选择默认的磁盘即可。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115222019859-1480413351.png)

### 1.3.9 <span style="font-family: 新宋体;">安装版本</span>

<span style="font-family: 新宋体;">选择最小化安装</span>-minimal----<span style="font-family: 新宋体;">使用什么安装</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115222027484-362525020.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">在这里选择</span>customize&nbsp; now <span style="font-family: 新宋体;">（自定义），添加其他的功能。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115222044515-234261997.png)![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115222051593-1277893657.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">然后就进入安装过程，稍等一段时间就安装完成，选择</span>reboot <span style="font-family: 新宋体;">重启进入系统。</span>

&nbsp;![](http://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115222059952-977034977.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

&nbsp;