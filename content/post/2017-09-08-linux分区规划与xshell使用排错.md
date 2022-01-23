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
<div>
  <table border="1" cellspacing="0" cellpadding="0" width="691" style="width: 518.6pt; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;">
    <tr style="height:132.3pt">
      <td width="691" valign="top" style="width: 518.6pt; border-top: none; border-right: none; border-left: none; border-bottom: 4.5pt double #4f81bd; padding: 0cm 5.4pt; height: 132.3pt;">
        <p align="center" style="text-align:center">
        </p>
        
        <h2>
          <span id="11">1.1 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">没有重要数据</span></span>
        </h2>
        
        <p>
          /boot&nbsp; &nbsp;200M&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">存放系统的引导信息</span> <span style="font-family:新宋体;Times New Roman";Times New Roman"">内核</span>
        </p>
        
        <p>
          swap&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">交换分区</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">防止内存用光了</span> <span style="font-family:新宋体;Times New Roman";Times New Roman"">临时的一个内存</span>&nbsp;
        </p>
        
        <p style="text-indent:21.0pt">
          <span style="font-family:新宋体;Times New Roman";Times New Roman"">如果你的内存小于</span>8G swap<span style="font-family:新宋体;Times New Roman";Times New Roman"">是内存的</span>1.5<span style="font-family:新宋体;Times New Roman";Times New Roman"">倍</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">如果你的内存大于</span>8G swap<span style="font-family:新宋体;Times New Roman";Times New Roman"">给</span>8G
        </p>
        
        <p>
          /&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">根分区</span>&nbsp;<span style="font-family:新宋体;Times New Roman";Times New Roman"">剩余多少给多少</span>
        </p>
        
        <h2>
          <span id="12">1.2 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">很多重要数据</span></span>
        </h2>
        
        <p>
          /boot&nbsp; &nbsp;200M&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">存放系统的引导信息</span> <span style="font-family:新宋体;Times New Roman";Times New Roman"">内核</span>
        </p>
        
        <p>
          swap&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">交换分区</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">防止内存用光了</span> <span style="font-family:新宋体;Times New Roman";Times New Roman"">临时的一个内存</span>&nbsp;
        </p>
        
        <p style="text-indent:21.0pt">
          <span style="font-family:新宋体;Times New Roman";Times New Roman"">如果你的内存小于</span>8G swap<span style="font-family:新宋体;Times New Roman";Times New Roman"">是内存的</span>1.5<span style="font-family:新宋体;Times New Roman";Times New Roman"">倍</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">如果你的内存大于</span>8G swap<span style="font-family:新宋体;Times New Roman";Times New Roman"">给</span>8G
        </p>
        
        <p>
          /&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;<span style="font-family:新宋体;Times New Roman";Times New Roman"">根分区</span>&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20G-200G
        </p>
        
        <p>
          /data&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">存放重要的数据</span> <span style="font-family: 新宋体;Times New Roman";Times New Roman"">剩余多少给多少</span>
        </p>
        
        <h2>
          <span id="13">1.3 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">不知道是否重要</span></span>
        </h2>
        
        <p>
          /boot&nbsp; &nbsp;200M&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">存放系统的引导信息</span> <span style="font-family:新宋体;Times New Roman";Times New Roman"">内核</span>
        </p>
        
        <p>
          swap&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">交换分区</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">防止内存用光了</span> <span style="font-family:新宋体;Times New Roman";Times New Roman"">临时的一个内存</span>&nbsp;
        </p>
        
        <p style="text-indent:21.0pt">
          <span style="font-family:新宋体;Times New Roman";Times New Roman"">如果你的内存小于</span>8G swap<span style="font-family:新宋体;Times New Roman";Times New Roman"">是内存的</span>1.5<span style="font-family:新宋体;Times New Roman";Times New Roman"">倍</span>&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">如果你的内存大于</span>8G swap<span style="font-family:新宋体;Times New Roman";Times New Roman"">给</span>8G
        </p>
        
        <p>
          /&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;<span style="font-family:新宋体;Times New Roman";Times New Roman"">根分区</span>&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20G-200G
        </p>
        
        <p>
          <span style="font-family:新宋体;Times New Roman";Times New Roman"">剩余空间不分</span> <span style="font-family: 新宋体;Times New Roman";Times New Roman"">放着谁使用这台服务器谁来分区</span>
        </p>
        
        <h1>
          <span id="2_Xshell">第2章 Xshell<span style="font-family:新宋体;Times New Roman";Times New Roman"">优化</span></span>
        </h1>
        
        <h2>
          <span id="21_xshell">2.1 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">安装</span>xshell<span style="font-family:新宋体;Times New Roman";Times New Roman"">软件</span></span>
        </h2>
        
        <p style="margin-left:21.0pt">
          <span style="font-family:新宋体;Times New Roman";Times New Roman"">尽量选用免费软件，不要破解软件，防止信息泄露。</span>
        </p>
        
        <h2>
          <span id="22_Xshell">2.2 Xshell<span style="font-family:新宋体;Times New Roman";Times New Roman"">优化</span></span>
        </h2>
        
        <p style="margin-left:21.0pt">
          <span style="font-family:新宋体;Times New Roman";Times New Roman"">终端类型改为</span>linux<span style="font-family:新宋体;Times New Roman";Times New Roman"">，缓冲区大小改为最大。</span>
        </p>
        
        <p align="center" style="margin-left:21.0pt;text-align:center">
          <p style="margin-left:21.0pt;text-indent:21.0pt">
            <span style="font-family:新宋体;Times New Roman";Times New Roman"">禁止更改终端标题。</span>
          </p>
          
          <p align="center" style="text-align:center">
            <p>
              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">设置字体大小，光标闪烁。</span>
            </p>
            
            <p align="center" style="text-align:center">
              4
            </p>
            
            <p style="text-indent:21.0pt">
              <span style="font-family:新宋体;Times New Roman";Times New Roman";">保存日志文件，设置日志保存方式。</span>
            </p>
            
            <p align="center" style="text-align:center">
              <h2>
                <span id="23">2.3 <span style="font-family: 新宋体;Times New Roman";Times New Roman";">连接虚拟机</span></span>
              </h2>
              
              <p style="margin-left:21.0pt">
                <span style="font-family:新宋体;Times New Roman";Times New Roman"">点击新建会话</span>
              </p>
              
              <p align="center" style="margin-left:21.0pt;text-align:center">
                <p style="margin-left:21.0pt">
                  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family:新宋体;Times New Roman";Times New Roman"">连接成功。</span>
                </p>
                
                <h1>
                  <span id="3">第3章 <span style="font-family:新宋体;Times New Roman";Times New Roman"">远程排错</span></span>
                </h1>
                
                <h2>
                  <span id="31">3.1 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">预备知识</span></span>
                </h2>
                
                <h3>
                  <span id="311">3.1.1 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">远程软件</span></span>
                </h3>
                
                <p style="text-indent:18.0pt">
                  Xshell/SecureCRT/putty
                </p>
                
                <h3>
                  <span id="312_IP">3.1.2 IP<span style="font-family:新宋体;Times New Roman";Times New Roman"">地址</span></span>
                </h3>
                
                <p>
                  <span style="font-family:新宋体;Times New Roman";Times New Roman"">服务器的位置</span>
                </p>
                
                <h3>
                  <span id="313">3.1.3 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">端口号码</span></span>
                </h3>
                
                <p>
                  <span style="font-family:新宋体;Times New Roman";Times New Roman"">一台主机上的不同服务功能，就是通过端口区分，然后让外部人员访问。</span>
                </p>
                
                <p style="margin-left:0cm;text-indent:0cm;">
                  实例3-1<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;Times New Roman";Times New Roman"">远程连接服务（</span>sshd<span style="font-family:新宋体;Times New Roman";Times New Roman"">）</span> ===> <span style="font-family:新宋体;Times New Roman";Times New Roman"">端口号</span>22
                </p>
                
                <h3>
                  <span id="314">3.1.4 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">协议</span></span>
                </h3>
                
                <p style="text-indent:21.0pt">
                  <span style="font-family:新宋体;Times New Roman";Times New Roman"">默认遵守的规则</span>
                </p>
                
                <h3>
                  <span id="315_Vmware">3.1.5 Vmware <span style="font-family:新宋体;Times New Roman";Times New Roman"">网络模式</span></span>
                </h3>
                
                <p style="margin-left:21.0pt;text-indent:.3pt">
                  VMware<span style="font-family:新宋体;Times New Roman";Times New Roman"">提供了三种工作模式，分别是</span>bridged<span style="font-family:新宋体;Times New Roman";Times New Roman"">（桥接模式）、</span>NAT<span style="font-family:新宋体;Times New Roman";Times New Roman"">（网络地址转换模式）和</span>host-only<span style="font-family:新宋体;Times New Roman";Times New Roman"">（仅主机模式）</span>
                </p>
                
                <p style="margin-left:0cm;text-indent:0cm;">
                  实例3-2<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>bridged<span style="font-family:新宋体;Times New Roman";Times New Roman"">（桥接模式）</span>
                </p>
                
                <p align="center" style="text-align:center">
                  <p style="margin-left:0cm;text-indent:0cm;">
                    实例3-3<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>NAT
                  </p>
                  
                  <p align="center" style="margin-left:18.0pt;text-align:center">
                    <p style="margin-left:0cm;text-indent:0cm;">
                      实例3-4<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>Host-only<span style="font-family:新宋体;Times New Roman";Times New Roman"">（仅主机）</span>
                    </p>
                    
                    <p align="center" style="text-align:center">
                      <h2>
                        <span id="32">3.2 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">排错过程</span></span>
                      </h2>
                      
                      <h3>
                        <span id="321">3.2.1 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">路是否通</span></span>
                      </h3>
                      
                      <p>
                        <span style="font-family:新宋体;Times New Roman";Times New Roman"">本地</span>Shell<span style="font-family:新宋体;Times New Roman";Times New Roman"">运行命令</span> <span style="font-family:新宋体;Times New Roman";Times New Roman"">程序</span>&nbsp; -------windows<span style="font-family:新宋体;Times New Roman";Times New Roman"">运行命令或程序</span>
                      </p>
                      
                      <p>
                        ping 10.0.0.200
                      </p>
                      
                      <p>
                        &nbsp;
                      </p>
                      
                      <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          ###<span style="font-family:宋体">通畅</span>
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          [d:\~]$ ping 10.0.0.200
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">正在</span> Ping 10.0.0.200 <span style="font-family:宋体">具有</span> 32 <span style="font-family:宋体">字节的数据</span>:
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          10.0.0.200 <span style="font-family:宋体">的</span> Ping <span style="font-family:宋体">统计信息</span>:
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;&nbsp;&nbsp; <span style="font-family:宋体">数据包</span>: <span style="font-family:宋体">已发送</span> = 4<span style="font-family:宋体">，已接收</span> = 4<span style="font-family:宋体">，丢失</span> = 0 (0% <span style="font-family:宋体">丢失</span>)<span style="font-family:宋体">，</span>
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">往返行程的估计时间</span>(<span style="font-family:宋体">以毫秒为单位</span>):
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;&nbsp;&nbsp; <span style="font-family:宋体">最短</span> = 0ms<span style="font-family:宋体">，最长</span> = 0ms<span style="font-family:宋体">，平均</span> = 0ms
                        </p></p>
                      </div>
                      
                      <p>
                        &nbsp;
                      </p>
                      
                      <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          ###<span style="font-family:宋体">不通</span>
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          [d:\~]$ ping 10.0.0.200
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">正在</span> Ping 10.0.0.200 <span style="font-family:宋体">具有</span> 32 <span style="font-family:宋体">字节的数据</span>:
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">来自</span> 10.0.0.200 <span style="font-family:宋体">的回复</span>: <span style="font-family:宋体">字节</span>=32 <span style="font-family:宋体">时间</span><1ms TTL=64
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          10.0.0.200 <span style="font-family:宋体">的</span> Ping <span style="font-family:宋体">统计信息</span>:
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;&nbsp;&nbsp; <span style="font-family:宋体">数据包</span>: <span style="font-family:宋体">已发送</span> = 4<span style="font-family:宋体">，已接收</span> = 4<span style="font-family:宋体">，丢失</span> = 0 (0% <span style="font-family:宋体">丢失</span>)<span style="font-family:宋体">，</span>
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">往返行程的估计时间</span>(<span style="font-family:宋体">以毫秒为单位</span>):
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <span style="font-family:宋体">最短</span> = 0ms<span style="font-family:宋体">，最长</span> = 0ms<span style="font-family:宋体">，平均</span> = 0ms
                        </p></p>
                      </div>
                      
                      <h3>
                        <span id="322">3.2.2 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">是否有人打劫</span></span>
                      </h3>
                      
                      <h3>
                        <span id="323">3.2.3 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">是否有人买票</span></span>
                      </h3>
                      
                      <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          telnet 10.0.0.200 22
                        </p></p>
                      </div>
                      
                      <p>
                        #<span style="font-family:新宋体;Times New Roman";Times New Roman"">看</span>linux<span style="font-family:新宋体;Times New Roman";Times New Roman"">的</span> 22<span style="font-family:新宋体;Times New Roman";Times New Roman"">端口是否开启</span>-----<span style="font-family:新宋体;Times New Roman";Times New Roman"">是否有人卖票</span>
                      </p>
                      
                      <p>
                        &nbsp;
                      </p>
                      
                      <div style="border:solid windowtext 1.0pt;padding:1.0pt 4.0pt 1.0pt 4.0pt; background:#F2F2F2;">
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          [d:\~]$ telnet 10.0.0.200 22
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          Connecting to 10.0.0.200:22...
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          Connection established.
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          To escape to local shell, press 'Ctrl+Alt+]'.
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          SSH-2.0-OpenSSH_5.3
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          Protocol mismatch.
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          Connection closed by foreign host.
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          Disconnected from remote host(10.0.0.200:22) at 10:10:07.
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          &nbsp;
                        </p>
                        
                        <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          Type `help' to learn how to use Xshell prompt.
                        </p></p>
                      </div>
                      
                      <h2>
                        <span id="33">3.3 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">常见错误</span></span>
                      </h2>
                      
                      <h3>
                        <span id="331_ping">3.3.1 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">无法</span>ping<span style="font-family:新宋体;Times New Roman";Times New Roman"">通</span></span>
                      </h3>
                      
                      <p style="margin-left:21.0pt;text-indent:-21.0pt;">
                        1)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman";Times New Roman"">检查服务器</span>/<span style="font-family: 新宋体;Times New Roman";Times New Roman"">虚拟机是否启动，</span>IP<span style="font-family:新宋体;Times New Roman";Times New Roman"">地址是否正确</span>
                      </p>
                      
                      <p style="margin-left:21.0pt;text-indent:-21.0pt;">
                        2)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman";Times New Roman"">检查</span>services.msc<span style="font-family:新宋体;Times New Roman";Times New Roman"">服务里</span> VMware<span style="font-family:新宋体;Times New Roman";Times New Roman"">开头的服务是否开启。</span>
                      </p>
                      
                      <h3>
                        <span id="332">3.3.2 <span style="font-family: 新宋体;Times New Roman";Times New Roman"">无法登陆</span></span>
                      </h3>
                      
                      <p style="margin-left:21.0pt;text-indent:-21.0pt;">
                        1)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman";Times New Roman"">检查</span>sshd<span style="font-family: 新宋体;Times New Roman";Times New Roman"">服务是否运行</span>
                      </p>
                      
                      <p style="margin-left:21.0pt;text-indent:-21.0pt;">
                        2)<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal; font-family: "Times New Roman";">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family:新宋体;Times New Roman";Times New Roman"">端口号是否正确</span>
                      </p>
                      
                      <p>
                        <span style="font-size: 14px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>
                      </p></td> </tr> </tbody> </table> </div> 
                      
                      <div id="toc_container" class="toc_white have_bullets">
                        <ul class="toc_list">
                          <ul>
                            <li>
                              <a href="#11">1.1 没有重要数据</a>
                            </li>
                            <li>
                              <a href="#12">1.2 很多重要数据</a>
                            </li>
                            <li>
                              <a href="#13">1.3 不知道是否重要</a>
                            </li>
                          </ul></li>
                          
                          <li>
                            <a href="#2_Xshell">第2章 Xshell优化</a><ul>
                              <li>
                                <a href="#21_xshell">2.1 安装xshell软件</a>
                              </li>
                              <li>
                                <a href="#22_Xshell">2.2 Xshell优化</a>
                              </li>
                              <li>
                                <a href="#23">2.3 连接虚拟机</a>
                              </li>
                            </ul>
                          </li>
                          
                          <li>
                            <a href="#3">第3章 远程排错</a><ul>
                              <li>
                                <a href="#31">3.1 预备知识</a><ul>
                                  <li>
                                    <a href="#311">3.1.1 远程软件</a>
                                  </li>
                                  <li>
                                    <a href="#312_IP">3.1.2 IP地址</a>
                                  </li>
                                  <li>
                                    <a href="#313">3.1.3 端口号码</a>
                                  </li>
                                  <li>
                                    <a href="#314">3.1.4 协议</a>
                                  </li>
                                  <li>
                                    <a href="#315_Vmware">3.1.5 Vmware 网络模式</a>
                                  </li>
                                </ul>
                              </li>
                              
                              <li>
                                <a href="#32">3.2 排错过程</a><ul>
                                  <li>
                                    <a href="#321">3.2.1 路是否通</a>
                                  </li>
                                  <li>
                                    <a href="#322">3.2.2 是否有人打劫</a>
                                  </li>
                                  <li>
                                    <a href="#323">3.2.3 是否有人买票</a>
                                  </li>
                                </ul>
                              </li>
                              
                              <li>
                                <a href="#33">3.3 常见错误</a><ul>
                                  <li>
                                    <a href="#331_ping">3.3.1 无法ping通</a>
                                  </li>
                                  <li>
                                    <a href="#332">3.3.2 无法登陆</a>
                                  </li>
                                </ul>
                              </li>
                            </ul>
                          </li>
                        </ul>
                      </div>