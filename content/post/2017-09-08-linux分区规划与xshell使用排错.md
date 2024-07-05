---
title: Linux分区规划与xshell使用排错
author: 惨绿少年
type: post
date: 2017-09-07T18:14:00+00:00
url: /clsn/lx997.html
Baidusubmit:
  - 1
views:
  - 125
zm_favorites:
  - 1
categories:
  - Linux运维
  - 运维基本功

---
<div>  <table border="1" cellspacing="0" cellpadding="0" width="691" style="width: 518.6pt; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;"><tbody>  <tr style="height:132.3pt">   <td width="691" valign="top" style="width: 518.6pt; border-top: none; border-right: none; border-left: none; border-bottom: 4.5pt double #4f81bd; padding: 0cm 5.4pt; height: 132.3pt;">   

## 1.1 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">没有重要数据</span>

/boot&nbsp; &nbsp;200M&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">存放系统的引导信息</span> <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">内核</span>

swap&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">交换分区</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">防止内存用光了</span> <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">临时的一个内存</span>&nbsp;

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">如果你的内存小于</span>8G swap<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">是内存的</span>1.5<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">倍</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">如果你的内存大于</span>8G swap<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">给</span>8G 

/&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">根分区</span>&nbsp;<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">剩余多少给多少</span>

## 1.2 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">很多重要数据</span>

/boot&nbsp; &nbsp;200M&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">存放系统的引导信息</span> <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">内核</span>

swap&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">交换分区</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">防止内存用光了</span> <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">临时的一个内存</span>&nbsp;

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">如果你的内存小于</span>8G swap<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">是内存的</span>1.5<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">倍</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">如果你的内存大于</span>8G swap<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">给</span>8G 

/&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">根分区</span>&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20G-200G 

/data&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">存放重要的数据</span> <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">剩余多少给多少</span>

## 1.3 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">不知道是否重要</span>

/boot&nbsp; &nbsp;200M&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">存放系统的引导信息</span> <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">内核</span>

swap&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">交换分区</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">防止内存用光了</span> <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">临时的一个内存</span>&nbsp;

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">如果你的内存小于</span>8G swap<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">是内存的</span>1.5<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">倍</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">如果你的内存大于</span>8G swap<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">给</span>8G 

/&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">根分区</span>&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20G-200G 

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">剩余空间不分</span> <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">放着谁使用这台服务器谁来分区</span>

# 第2章 Xshell<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">优化</span>

## 2.1 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">安装</span>xshell<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">软件</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">尽量选用免费软件，不要破解软件，防止信息泄露。</span>

## 2.2 Xshell<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">优化</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">终端类型改为</span>linux<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">，缓冲区大小改为最大。</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">禁止更改终端标题。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">设置字体大小，光标闪烁。</span>

   4

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;;">保存日志文件，设置日志保存方式。</span>

## 2.3 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;;">连接虚拟机</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">点击新建会话</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">连接成功。</span>

# 第3章 <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">远程排错</span>

## 3.1 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">预备知识</span>

### 3.1.1 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">远程软件</span>

Xshell/SecureCRT/putty

### 3.1.2 IP<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">地址</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">服务器的位置</span>

### 3.1.3 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">端口号码</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">一台主机上的不同服务功能，就是通过端口区分，然后让外部人员访问。</span>

实例3-1<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">远程连接服务（</span>sshd<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">）</span> ===&gt; <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">端口号</span>22

### 3.1.4 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">协议</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">默认遵守的规则</span>

### 3.1.5 Vmware <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">网络模式</span>

VMware<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">提供了三种工作模式，分别是</span>bridged<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">（桥接模式）、</span>NAT<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">（网络地址转换模式）和</span>host-only<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">（仅主机模式）</span>

实例3-2<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>bridged<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">（桥接模式）</span>

实例3-3<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>NAT

实例3-4<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>Host-only<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">（仅主机）</span>

## 3.2 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">排错过程</span>

### 3.2.1 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">路是否通</span>

<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">本地</span>Shell<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">运行命令</span> <span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">程序</span>&nbsp; -------windows<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">运行命令或程序</span>

ping 10.0.0.200 

&nbsp;
  <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">  

###<span style="font-family:宋体">通畅</span>

[d:\~]$ ping 10.0.0.200 

&nbsp;

<span style="font-family:宋体">正在</span> Ping 10.0.0.200 <span style="font-family:宋体">具有</span> 32 <span style="font-family:宋体">字节的数据</span>:

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

&nbsp;

10.0.0.200 <span style="font-family:宋体">的</span> Ping <span style="font-family:宋体">统计信息</span>:

&nbsp;&nbsp;&nbsp; <span style="font-family:宋体">数据包</span>: <span style="font-family:宋体">已发送</span> = 4<span style="font-family:宋体">，已接收</span> = 4<span style="font-family:宋体">，丢失</span> = 0 (0% <span style="font-family:宋体">丢失</span>)<span style="font-family:宋体">，</span>

<span style="font-family:宋体">往返行程的估计时间</span>(<span style="font-family:宋体">以毫秒为单位</span>):

&nbsp;&nbsp;&nbsp; <span style="font-family:宋体">最短</span> = 0ms<span style="font-family:宋体">，最长</span> = 0ms<span style="font-family:宋体">，平均</span> = 0ms
  </div>  

&nbsp;
  <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">  

###<span style="font-family:宋体">不通</span> 

[d:\~]$ ping 10.0.0.200 

&nbsp;

<span style="font-family:宋体">正在</span> Ping 10.0.0.200 <span style="font-family:宋体">具有</span> 32 <span style="font-family:宋体">字节的数据</span>:

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

<span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span>&lt;1ms TTL=64

&nbsp;

10.0.0.200 <span style="font-family:宋体">的</span> Ping <span style="font-family:宋体">统计信息</span>:

&nbsp;&nbsp;&nbsp; <span style="font-family:宋体">数据包</span>: <span style="font-family:宋体">已发送</span> = 4<span style="font-family:宋体">，已接收</span> = 4<span style="font-family:宋体">，丢失</span> = 0 (0% <span style="font-family:宋体">丢失</span>)<span style="font-family:宋体">，</span>

<span style="font-family:宋体">往返行程的估计时间</span>(<span style="font-family:宋体">以毫秒为单位</span>):

<span style="font-family:宋体">最短</span> = 0ms<span style="font-family:宋体">，最长</span> = 0ms<span style="font-family:宋体">，平均</span> = 0ms
  </div>  

### 3.2.2 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">是否有人打劫</span>

### 3.2.3 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">是否有人买票</span>
  <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">  

telnet 10.0.0.200 22 
  </div>  

#<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">看</span>linux<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">的</span> 22<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">端口是否开启</span>-----<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">是否有人卖票</span>

&nbsp;
  <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">  

[d:\~]$ telnet 10.0.0.200 22

&nbsp;

&nbsp;

Connecting to 10.0.0.200:22...

Connection established.

To escape to local shell, press 'Ctrl+Alt+]'.

SSH-2.0-OpenSSH_5.3

&nbsp;

Protocol mismatch.

&nbsp;

Connection closed by foreign host.

&nbsp;

Disconnected from remote host(10.0.0.200:22) at 10:10:07.

&nbsp;

Type `help' to learn how to use Xshell prompt.
  </div>  

## 3.3 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">常见错误</span>

### 3.3.1 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">无法</span>ping<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">通</span>

1)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">检查服务器</span>/<span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">虚拟机是否启动，</span>IP<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">地址是否正确</span>

2)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">检查</span>services.msc<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">服务里</span> VMware<span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">开头的服务是否开启。</span>

### 3.3.2 <span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">无法登陆</span>

1)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">检查</span>sshd<span style="font-family: 新宋体;Times New Roman&quot;;Times New Roman&quot;">服务是否运行</span>

2)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: &quot;Times New Roman&quot;;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman&quot;;Times New Roman&quot;">端口号是否正确</span>

<span style="font-size: 14px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>
</td></tr></tbody></table></div>