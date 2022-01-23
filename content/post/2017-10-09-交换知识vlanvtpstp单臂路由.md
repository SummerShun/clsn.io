---
title: '交换知识 VLAN  VTP  STP 单臂路由'
author: 惨绿少年
type: post
date: 2017-10-08T18:34:00+00:00
url: /clsn/lx944.html
Baidusubmit:
  - 1
views:
  - 128
categories:
  - Linux运维
  - 玩转Linux
  - 网络技术

---
<div id="log-box">
  <div id="catalog">
    <ul id="catalog-ul">
      <li>
        <i class="be be-arrowright"></i> <a href="#title-0" title="2.9.3.1 S-vlan-1信息">2.9.3.1 S-vlan-1信息</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-1" title="2.9.3.2 S-vlan-2信息">2.9.3.2 S-vlan-2信息</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-2" title="2.9.3.3 查看trunk信息">2.9.3.3 查看trunk信息</a>
      </li>
    </ul>
    
    <span class="log-zd"><span class="log-close"><a title="隐藏目录"><i class="be be-cross"></i><strong>目录</strong></a></span></span>
  </div>
</div>

# <span id="1"><img class="alignnone size-medium wp-image-1332" data-original="/wp-content/uploads/2017/10/u39136090673687797672fm26gp0-300x163.jpg" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="" width="300" height="163" srcset="/wp-content/uploads/2017/10/u39136090673687797672fm26gp0-300x163.jpg 300w, /wp-content/uploads/2017/10/u39136090673687797672fm26gp0.jpg 551w" sizes="(max-width: 300px) 100vw, 300px" />第1章 交换基础</span>

## <span id="11">1.1 园区网分层结构</span>

<table style="border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 226.55pt; border-top: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-right: none; background: #4f81bd; padding: 0cm 5.4pt;" width="302">
      <p style="text-align: center;" align="center">
        <strong>层次</strong>
      </p>
    </td>
    
    <td style="width: 296.25pt; border-top: 1pt solid white; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: none; background: #4f81bd; padding: 0cm 5.4pt;" width="395">
      <p style="text-align: center;" align="center">
        <strong>作用</strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 226.55pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" width="302">
      <p style="text-align: center;" align="center">
        <strong>出口层</strong>
      </p>
    </td>
    
    <td style="width: 296.25pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #B8CCE4; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="395">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        广域网接入
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        出口策略
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        带宽控制
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 226.55pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" width="302">
      <p style="text-align: center;" align="center">
        <strong>核心层</strong>
      </p>
    </td>
    
    <td style="width: 296.25pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #DBE5F1; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="395">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        高速转发
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        服务器接入
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        路由选择
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 226.55pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" width="302">
      <p style="text-align: center;" align="center">
        <strong>汇聚层</strong>
      </p>
    </td>
    
    <td style="width: 296.25pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #B8CCE4; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="395">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        流量汇聚
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        链路冗余
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        设备冗余
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        路由选择
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 226.55pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4f81bd; padding: 0cm 5.4pt;" width="302">
      <p style="text-align: center;" align="center">
        <strong>接入层</strong>
      </p>
    </td>
    
    <td style="width: 296.25pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #DBE5F1; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="395">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        用户接入
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        接入安全
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        访问控制
      </p>
    </td>
  </tr>
</table>

## <span id="12">1.2 交换机的主要功能</span>

<p style="margin-left: 7.1pt;">
  MAC地址表      address learning
</p>

<p style="margin-left: 7.1pt;">
  转发和过滤的决策forward/filter decision
</p>

<p style="margin-left: 7.1pt;">
  环路的避免       loop avoidance
</p>

## <span id="13_MAC">1.3 MAC地址</span>

<p style="margin-left: 33.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;"><img data-original="file:///C:/Users/DEFAUL~1.DES/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="*" width="17" height="17" /> </span>MAC地址有48位，通常被表示为点分十六进制
</p>

<p style="margin-left: 33.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;"><img data-original="file:///C:/Users/DEFAUL~1.DES/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="*" width="17" height="17" /> </span>MAC地址全球唯一，由IEEE对这些地址进行管理和分配。
</p>

<p style="margin-left: 33.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;"><img data-original="file:///C:/Users/DEFAUL~1.DES/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="*" width="17" height="17" /> </span>每个地址由两部分组成，分别是供应商和序列号。其中前24位二进制代表该供应商代码，剩下的24位由厂商自己分配
</p>

## <span id="14">1.4 交换寻址</span>

<p style="text-indent: 7.1pt;">
  初始状态mac地址表为空
</p>

<p style="text-indent: 7.1pt;">
  第一次会泛洪数据帧
</p>

# <span id="2_VLAN">第2章 VLAN 虚拟局域网</span>

## <span id="21_vlan">2.1 vlan概述</span>

<p style="margin-left: 7.1pt;">
   分段性、灵活性、安全性
</p>

 将交换机的端口进行vlan划分（划分不同的广播域）

通过十进制数进行标识

## <span id="22_vlan">2.2 vlan的成员模式</span>

<table style="border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 184.05pt; border-top: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-right: none; background: #4bacc6; padding: 0cm 5.4pt;" width="245">
      <p style="text-align: center;" align="center">
        <strong>分类</strong>
      </p>
    </td>
    
    <td style="width: 338.75pt; border-top: 1pt solid white; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: none; background: #4bacc6; padding: 0cm 5.4pt;" width="452">
      <p style="text-align: center;" align="center">
        <strong>方式</strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 184.05pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4bacc6; padding: 0cm 5.4pt;" width="245">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>静态<span style="color: white;">vlan</span></strong><strong>（<span style="color: white;">Static Vlan</span></strong><strong>）</strong>
      </p>
    </td>
    
    <td style="width: 338.75pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #B6DDE8; padding: 0cm 5.4pt 0cm 5.4pt;" width="452">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        交换机上的端口以手动方式分配给Vlan
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 184.05pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4bacc6; padding: 0cm 5.4pt;" width="245">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>动态<span style="color: white;">Vlan</span></strong><strong>（<span style="color: white;">Dynamic VLAN</span></strong><strong>）</strong>
      </p>
    </td>
    
    <td style="width: 338.75pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #DAEEF3; padding: 0cm 5.4pt 0cm 5.4pt;" width="452">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        使用VMPS可以根据连接到交换机端口的设备源MAC地址，动态地将端口分配给VLAN
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 184.05pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #4bacc6; padding: 0cm 5.4pt;" width="245">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>语音<span style="color: white;">Vlan</span></strong><strong>（<span style="color: white;">Voice VLAN</span></strong><strong>）</strong>
      </p>
    </td>
    
    <td style="width: 338.75pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #B6DDE8; padding: 0cm 5.4pt 0cm 5.4pt;" width="452">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        将端口配置到语音模式可以使端口支持连接到该端口的IP电话
      </p>
    </td>
  </tr>
</table>

## <span id="23_trunk">2.3 <span style="background: yellow;">trunk</span>干道</span>

<p style="margin-left: 7.1pt;">
  当一条链路需要承载多vlan信息的时候，需要使用trunk来实现
</p>

<p style="margin-left: 7.1pt;">
  一般见于交换机之间或交换机与路由器之间
</p>

### <span id="231_ISL">2.3.1 ISL封装协议（使用较少）</span>

<p style="margin-left: 28.1pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span>通过硬件（ASLC）实现
</p>

<p style="margin-left: 28.1pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span>ISL标识不会出现在工作站，客户端并不知道ISL的封装新信息
</p>

<p style="margin-left: 28.1pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span>在交换机或路由器 与交换机之间，在交换机与具有ISL网卡的服务器之间可以实现
</p>

<p style="margin-left: 7.1pt;">
  ISL头部26bytes
</p>

### <span id="232_dot1q8011q">2.3.2 dot1q协议(801.1q)</span>

数据包格式

<p style="text-indent: 21.0pt;">
  802.1q 并非实际封入原始帧中。相反，在以太网帧格式里，在MAC地址源与以太网类型/长度的原始帧里添加一个32位的域(field)。VLAN标签领域必须遵守下列格式:
</p>

<div align="center">
  <table style="border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
    <tr style="height: 2.9pt;">
      <td style="border-top: 1pt solid #f79646; border-bottom: 1pt solid #f79646; border-left: 1pt solid #f79646; border-right: none; background: #f79646; padding: 0cm 5.4pt; height: 2.9pt;" valign="top">
        <p style="text-align: center; layout-grid-mode: both; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">16 bits</span>
        </p>
      </td>
      
      <td style="border-top: solid #F79646 1.0pt; border-left: none; border-bottom: solid #F79646 1.0pt; border-right: none; background: #F79646; padding: 0cm 5.4pt 0cm 5.4pt; height: 2.9pt;" valign="top">
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">3 bits</span>
        </p>
      </td>
      
      <td style="border-top: solid #F79646 1.0pt; border-left: none; border-bottom: solid #F79646 1.0pt; border-right: none; background: #F79646; padding: 0cm 5.4pt 0cm 5.4pt; height: 2.9pt;" valign="top">
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">1 bit</span>
        </p>
      </td>
      
      <td style="border-top: 1pt solid #f79646; border-right: 1pt solid #f79646; border-bottom: 1pt solid #f79646; border-left: none; background: #f79646; padding: 0cm 5.4pt; height: 2.9pt;" valign="top">
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">12 bits</span>
        </p>
      </td>
    </tr>
    
    <tr style="height: 22.35pt;">
      <td style="border-right: 1pt solid #fabf8f; border-bottom: 1pt solid #fabf8f; border-left: 1pt solid #fabf8f; border-top: none; background: #fde9d9; padding: 0cm 5.4pt; height: 22.35pt;" valign="top">
        <p style="text-align: center; layout-grid-mode: both; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">TPID</span>
        </p>
        
        <p style="text-align: center; layout-grid-mode: both; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt; font-family: 宋体; color: #222222;">标签协议识别符</span>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid #FABF8F 1.0pt; border-right: solid #FABF8F 1.0pt; background: #FDE9D9; padding: 0cm 5.4pt 0cm 5.4pt; height: 22.35pt;" valign="top">
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">PCP</span>
        </p>
        
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">优先权代码点</span>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid #FABF8F 1.0pt; border-right: solid #FABF8F 1.0pt; background: #FDE9D9; padding: 0cm 5.4pt 0cm 5.4pt; height: 22.35pt;" valign="top">
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">CFI</span>
        </p>
        
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">标准格式指示</span>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid #FABF8F 1.0pt; border-right: solid #FABF8F 1.0pt; background: #FDE9D9; padding: 0cm 5.4pt 0cm 5.4pt; height: 22.35pt;" valign="top">
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">VID</span>
        </p>
        
        <p style="text-align: center; margin: 12.0pt 0cm 12.0pt 0cm;" align="center">
          <span style="font-size: 11.5pt;">虚拟局域网识别符</span>
        </p>
      </td>
    </tr>
  </table>
</div>

## <span id="24_VLAN">2.4 <span style="background: yellow;">VLAN</span>的特点</span>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  1) 一个vlan中所有的设备都在同一广播域内，<span style="background: lime;">广播不能跨越</span><span style="background: lime;">VLAN</span>传播
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  2) 一个Vlan为一个逻辑子网，由被配置为此VLAN成员的设备组成，<span style="background: lime;">不同</span><span style="background: lime;">vlan</span>间需要通过路由器实现互相通讯
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  3) vlan中成员多基于switch端口号码，划分vlan就是对switch接口划分
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  4) vlan工作与OSI参考模型的第二层
</p>

## <span id="25_ciscovlan">2.5 cisco上vlan范围</span>

<div align="center">
  <table style="width: 412.8pt; border-collapse: collapse; border: initial none initial;" border="1" width="550" cellspacing="0" cellpadding="0">
    <tr style="height: 21.2pt;">
      <td style="border-top: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-right: none; background: #9bbb59; padding: 0cm 5.4pt; height: 21.2pt;">
        <p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; line-height: 13.5pt; layout-grid-mode: both;" align="center">
          <strong><span style="font-size: 9.0pt;">VLAN</span></strong><strong><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">号</span></strong>
        </p>
      </td>
      
      <td style="border-top: 1pt solid white; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: none; background: #9bbb59; padding: 0cm 5.4pt; height: 21.2pt;">
        <p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; line-height: 13.5pt; layout-grid-mode: both;" align="center">
          <strong><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">用途</span></strong>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;">
        <p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; line-height: 13.5pt; layout-grid-mode: both;" align="center">
          <strong><span style="font-size: 9.0pt;"></span></strong><strong><span style="font-size: 9.0pt; font-family: 宋体; color: #984806;">、</span></strong><strong><span style="font-size: 9.0pt;">4095</span></strong>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #D6E3BC; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top">
        <p style="margin: 0cm; margin-bottom: .0001pt; line-height: 13.5pt; layout-grid-mode: both;">
           <span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">保留</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;">
        <p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; line-height: 13.5pt; layout-grid-mode: both;" align="center">
          <strong><span style="font-size: 9.0pt;">1</span></strong>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top">
        <p style="margin: 0cm; margin-bottom: .0001pt; line-height: 13.5pt; layout-grid-mode: both;">
          <span style="font-size: 9.0pt;"> cisco</span><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">交换机端口默认属于</span><span style="font-size: 9.0pt;">VLAN1 </span><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">不能删除</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;">
        <p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; line-height: 13.5pt; layout-grid-mode: both;" align="center">
          <strong><span style="font-size: 9.0pt;">2-1001</span></strong>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #D6E3BC; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top">
        <p style="margin: 0cm; margin-bottom: .0001pt; line-height: 13.5pt; layout-grid-mode: both;">
           <span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">用于以太网</span><span style="font-size: 9.0pt;">VLAN</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;">
        <p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; line-height: 13.5pt; layout-grid-mode: both;" align="center">
          <strong><span style="font-size: 9.0pt;">1002-1005</span></strong>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top">
        <p style="margin: 0cm; margin-bottom: .0001pt; line-height: 13.5pt; layout-grid-mode: both;">
           <span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">思科缺省用于</span><span style="font-size: 9.0pt;">FDDI</span><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">、</span><span style="font-size: 9.0pt;">Token Ring</span><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">网</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #9bbb59; padding: 0cm 5.4pt;">
        <p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; line-height: 13.5pt; layout-grid-mode: both;" align="center">
          <strong><span style="font-size: 9.0pt;">1006-4094</span></strong>
        </p>
      </td>
      
      <td style="border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #D6E3BC; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top">
        <p style="margin: 0cm; margin-bottom: .0001pt; line-height: 13.5pt; layout-grid-mode: both;">
           <span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">扩展</span><span style="font-size: 9.0pt;">VLAN</span><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">，用于以太网</span><span style="font-size: 9.0pt;">VLAN</span><span style="font-size: 9.0pt; font-family: 宋体; color: #464646;">，在一些很老的平台上不支持</span>
        </p>
      </td>
    </tr>
  </table>
</div>

## <span id="26_VTP_VLAN_Trunking_Protocol">2.6 <span style="background: yellow;">VTP</span> （VLAN Trunking Protocol）</span>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;"><img data-original="file:///C:/Users/DEFAUL~1.DES/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="*" width="17" height="17" /> </span>一个能够宣告VLAN配置信息的信息系统
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;"><img data-original="file:///C:/Users/DEFAUL~1.DES/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="*" width="17" height="17" /> </span>通过一个共有的管理域，维持vlan配置信息的一致性
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;"><img data-original="file:///C:/Users/DEFAUL~1.DES/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="*" width="17" height="17" /> </span>VTP只能在主干端口（Trunk）发送要宣告的信息
</p>

<p style="margin-left: 42.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;"><img data-original="file:///C:/Users/DEFAUL~1.DES/AppData/Local/Temp/msohtmlclip1/01/clip_image001.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="交换知识 VLAN  VTP  STP 单臂路由" alt="*" width="17" height="17" /> </span>支持混合的介质主干连接（快速以太网,fddi，atm）
</p>

### <span id="261_vlan">2.6.1 vlan使用过程</span>

<p style="margin-left: 21.0pt;">
  创建vlan
</p>

<p style="margin-left: 21.0pt;">
  确立vlan之间的关系
</p>

### <span id="262_vtp">2.6.2 vtp三种模式</span>

<div align="center">
  <table style="border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
    <tr>
      <td style="width: 106.1pt; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="141">
        <p style="text-align: center;" align="center">
          <strong> </strong><strong>模式</strong>
        </p>
      </td>
      
      <td style="width: 326pt; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="435">
        <p style="text-align: center;" align="center">
          <strong>功能</strong>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 106.1pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="141">
        <p style="text-align: center;" align="center">
          <strong>sever</strong>
        </p>
      </td>
      
      <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          创建VLAN、修改vlan、删除vlan
        </p>
        
        <p style="text-align: justify; text-justify: inter-ideograph;">
          发送/转发信息宣告、同步
        </p>
        
        <p style="text-align: justify; text-justify: inter-ideograph;">
          存储在NVRAM中、Catalyst交换机默认使sever模式
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 106.1pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="141">
        <p style="text-align: center;" align="center">
          <strong>client</strong>
        </p>
      </td>
      
      <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          同步
        </p>
        
        <p style="text-align: justify; text-justify: inter-ideograph;">
          发送/转发信息宣告
        </p>
        
        <p style="text-align: justify; text-justify: inter-ideograph;">
          不会存储与NVRAM
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 106.1pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="141">
        <p style="text-align: center;" align="center">
          <strong>Transparent</strong>
        </p>
      </td>
      
      <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          创建VLAN、修改vlan、删除vlan
        </p>
        
        <p style="text-align: justify; text-justify: inter-ideograph;">
          转发、信息宣告、不同步
        </p>
        
        <p style="text-align: justify; text-justify: inter-ideograph;">
          存储于NVRAM
        </p>
      </td>
    </tr>
  </table>
</div>

### <span id="263_VTP">2.6.3 VTP的运作</span>

<p style="margin-left: 45.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">l </span>VTP协议通过组播地址01-00-0C-CC-CC-CC在Trunk链路上发送VTP通告
</p>

<p style="margin-left: 45.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">l </span>VTP Sever和Clients 通过最高的修改定号来同步数据库
</p>

<p style="margin-left: 45.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">l </span>VTP协议没隔5分钟发送一次VTP通告或者有变化时发送
</p>

### <span id="264_VTP_Pruning">2.6.4 VTP的修剪<span style="color: red;"> Pruning</span></span>

#### <span id="2641vlan">2.6.4.1 允许所有<span style="background: lime;">vlan</span>通过会有什么缺点呢？</span>

<p style="text-indent: 21.0pt;">
  你的接入层交换机上明明是没有这个vlan的业务，但是由于trunk链路允许的所有vlan通过，一些其他vlan的广播流量，组播流量，未知单播泛洪流量也莫名的转发到这台交换机上，造成没有必要的带宽资源浪费，增加了广播域的影响范围。
</p>

## <span id="27_VLAN">2.7 VLAN基本配置</span>

### <span id="271_VLAN">2.7.1 创建VLAN信息</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config)# vlan 2</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config)# name vlan2</span>
  </p>
</div>

### <span id="272_vlan">2.7.2 将端口划入特定的vlan</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config)# interface fa 0/1</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config-if)#switchport mode access </span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config-if)#switchport access vlan [vlan vlan# | dynamic]</span>
  </p>
</div>

### <span id="273_Trunk">2.7.3 配置Trunk封装方式</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config)# interface fa 0/15</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config)# switchport trunk encapsulation {ISL|dot1q|negotiate}</span>
  </p>
</div>

### <span id="274">2.7.4 开启端口</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: #ffffff;">S-vlan-1(config)# switchport mode{dynamic {auto|desirable}|</span><span style="color: #ffffff;">trunk</span><span style="color: #ffffff;">}</span>
  </p>
</div>

## <span id="28_VTP">2.8 VTP的基本配置</span>

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1#configure terminal
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#vtp mode ?
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
     <span style="color: red;">client</span>       Set the device to client mode.
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
     <span style="color: red;"> server </span>      Set the device to server mode.
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
     <span style="color: red;">transparent </span> Set the device to transparent mode.
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#vtp domain ?
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
     <span style="color: red;">WORD</span>  The ascii name for the VTP administrative domain.
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#vtp password ?
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="color: red;">  WORD </span> The ascii password for the VTP administrative domain.
  </p>
</div>

### <span id="281">2.8.1 注意：</span>

<p style="margin-left: 63.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">ü </span>默认情况：VTP模式为sever
</p>

<p style="margin-left: 63.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">ü </span>VTP域名有大小写敏感
</p>

<p style="margin-left: 63.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">ü </span>VTP密码大小写敏感
</p>

<p style="margin-left: 63.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">ü </span>VTP修剪默认关闭
</p>

<p style="margin-left: 63.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">ü </span>查看vtp 状态 ：<strong>show vtp status</strong>
</p>

## <span id="29_vlan">2.9 【<span style="background: yellow;">实验</span>】vlan</span>

### <span id="291">2.9.1 拓扑图</span>

### <span id="292">2.9.2 交换机配置</span>

S-vlan-1上vlan配置

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#<span style="color: #e36c0a;">vlan 10</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-vlan)#name VLAN10
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#interface f 0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-if)#switchport mode access
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-if)#switchport access vlan 10
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#<span style="color: #e36c0a;">vlan 20</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-vlan)#name VLAN20
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#interface fastEthernet 0/2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-if)#switchport mode access
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-if)#switchport access vlan 20
  </p>
</div>

S-vlan-2上VLAN配置

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config)#<span style="color: #e36c0a;">vlan 10 </span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-vlan)#name VLAN10
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config)#interface fastEthernet 0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-if)#switchport mode access
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-if)#switchport access vlan 10
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config)#<span style="color: #e36c0a;">vlan 20</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-vlan)#name VLAN20
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config)#interface fastEthernet 0/2
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-if)#switchport mode access
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-if)#switchport access vlan 20
  </p>
</div>

trunk链路配置

**S-vlan-1****：**

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config)#<strong>interface gigabitEthernet 0/1</strong>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-if)<strong>#switchport mode trunk </strong>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1(config-if)#<strong>switchport trunk allowed vlan all</strong>
  </p>
</div>

**S-vlan-1****：**

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config)#interface gigabitEthernet 0/1
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-if)#switchport mode trunk
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-2(config-if)#switchport trunk allowed vlan all
  </p>
</div>

### <span id="293_vlan">2.9.3 查看vlan信息</span>

<span class="directory"></span>

#### <span id="2931S-vlan-1">2.9.3.1 S-vlan-1信息</span> {#title-0}

<div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    S-vlan-1#<span style="color: #e36c0a;">show vlan brief</span>
  </p>
  
  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      VLAN Name                             Status    Ports
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      ---- -------------------------------- --------- -------------------------------
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      1    default                          active    Fa0/3, Fa0/4, Fa0/5, Fa0/6
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                      Fa0/7, Fa0/8, Fa0/9, Fa0/10
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                      Fa0/11, Fa0/12, Fa0/13, Fa0/14
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                      Fa0/15, Fa0/16, Fa0/17, Fa0/18
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                      Fa0/19, Fa0/20, Fa0/21, Fa0/22
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                      Fa0/23, Fa0/24, Gig0/2
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span style="color: #e36c0a;">10   VLAN10                           active    Fa0/1</span>
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span style="color: #e36c0a;">20   VLAN20                           active    Fa0/2</span>
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      1002 fddi-default                     active
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      1003 token-ring-default               active
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      1004 fddinet-default                  active
    </p>
    
    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      1005 trnet-default                    active
    </p></div> 
    
    <span class="directory"></span>
    
    <h4 id="title-1">
      <span id="2932S-vlan-2">2.9.3.2 S-vlan-2信息</span>
    </h4>
    
    <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
        S-vlan-2#<span style="color: #e36c0a;">show vlan brief </span>
      </p>
      
      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          VLAN Name                             Status    Ports
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          ---- -------------------------------- --------- -------------------------------
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          1    default                          active    Fa0/3, Fa0/4, Fa0/5, Fa0/6
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                          Fa0/7, Fa0/8, Fa0/9, Fa0/10
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                          Fa0/11, Fa0/12, Fa0/13, Fa0/14
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                          Fa0/15, Fa0/16, Fa0/17, Fa0/18
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                          Fa0/19, Fa0/20, Fa0/21, Fa0/22
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                          Fa0/23, Fa0/24, Gig0/2
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          <span style="color: #e36c0a;">10   VLAN10                           active    Fa0/1</span>
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          <span style="color: #e36c0a;">20   VLAN20                           active    Fa0/2</span>
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          1002 fddi-default                     active
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          1003 token-ring-default               active
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          1004 fddinet-default                  active
        </p>
        
        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
          1005 trnet-default                    active
        </p></div> 
        
        <span class="directory"></span>
        
        <h4 id="title-2">
          <span id="2933trunk">2.9.3.3 查看trunk信息</span>
        </h4>
        
        <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
            S-vlan-1#show interfaces trunk
          </p>
          
          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
            Port        Mode         Encapsulation  Status        Native vlan
          </p>
          
          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
            Gig0/1      on           802.1q         trunking      1
          </p>
          
          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
            <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
              Port        Vlans allowed on trunk
            </p>
            
            <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
              Gig0/1      1-1005
            </p>
            
            <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                Port        Vlans allowed and active in management domain
              </p>
              
              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                Gig0/1      1,10,20
              </p>
              
              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                  Port        Vlans in spanning tree forwarding state and not pruned
                </p>
                
                <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                  Gig0/1      1,10,20
                </p></div> 
                
                <h2>
                  <span id="210">2.10 单臂路由</span>
                </h2>
                
                <p style="margin-left: 21.0pt; text-indent: 21.0pt;">
                  单臂路由（路由器上一棒）指的英文在路由器的一个接口上通过配置子接口（或“逻辑接口”，并不存在真正物理接口）的方式，实现原来相互隔离的不同VLAN（虚拟局域网）之间的互联互通。
                </p>
                
                <p style="margin-left: 42.0pt;">
                  在路由器中创建子接口
                </p>
                
                <h3>
                  <span id="2101">2.10.1 【<span style="background: yellow;">实验</span>】单臂路由配置</span>
                </h3>
                
                <h4>
                  <span id="21011">2.10.1.1<span style="font-variant-numeric: normal; font-weight: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">     </span>拓扑图</span>
                </h4>
                
                <p>
                  &nbsp;
                </p>
                
                <h4>
                  <span id="21012_trunk">2.10.1.2<span style="font-variant-numeric: normal; font-weight: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">     </span>交换机trunk链路配置</span>
                </h4>
                
                <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    S-vlan-2(config)#interface gigabitEthernet 0/2
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    S-vlan-2(config-if)#switchport mode trunk
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    S-vlan-2(config-if)#switchport trunk allowed vlan all
                  </p>
                </div>
                
                <h4>
                  <span id="21013">2.10.1.3<span style="font-variant-numeric: normal; font-weight: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">     </span>路由器子接口配置</span>
                </h4>
                
                <p>
                  <strong>fastEthernet 0/1.10</strong><strong>子接口配置</strong>
                </p>
                
                <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config)#interface fastEthernet 0/1.10
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config-subif)#encapsulation dot1Q 10
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config-subif)#ip address 192.168.10.254 255.255.255.0
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config-subif)#no shutdown
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config-subif)#exit
                  </p>
                </div>
                
                <p>
                  <strong>fastEthernet 0/1.20</strong><strong>子接口配置</strong>
                </p>
                
                <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config)#interface fastEthernet 0/1.20
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config-subif)#encapsulation dot1Q 20
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config-subif)#ip address 192.168.20.1 255.255.255.0
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    Router0(config-subif)#no shutdown
                  </p>
                  
                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                    </div> 
                    
                    <h4>
                      <span id="21014">2.10.1.4<span style="font-variant-numeric: normal; font-weight: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">     </span>检查配置结果</span>
                    </h4>
                    
                    <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                        C:\>ping 192.168.20.1
                      </p>
                      
                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          Pinging 192.168.20.1 with 32 bytes of data:
                        </p>
                        
                        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                            Reply from 192.168.20.1: bytes=32 time<1ms TTL=127
                          </p>
                          
                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                            Reply from 192.168.20.1: bytes=32 time=1ms TTL=127
                          </p>
                          
                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                            Reply from 192.168.20.1: bytes=32 time<1ms TTL=127
                          </p>
                          
                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                            Reply from 192.168.20.1: bytes=32 time<1ms TTL=127
                          </p>
                          
                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                            <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                              Ping statistics for 192.168.20.1:
                            </p>
                            
                            <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                  Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
                            </p>
                            
                            <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                              Approximate round trip times in milli-seconds:
                            </p>
                            
                            <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                  Minimum = 0ms, Maximum = 1ms, Average = 0ms
                            </p></div> 
                            
                            <p>
                              查看路由表
                            </p>
                            
                            <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                Router0#show ip route
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                       * - candidate default, U - per-user static route, o - ODR
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                       P - periodic downloaded static route
                              </p>
                              
                              <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                  Gateway of last resort is not set
                                </p>
                                
                                <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                    <span style="color: #e36c0a;">C    192.168.10.0/24 is directly connected, FastEthernet0/1.10</span>
                                  </p>
                                  
                                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                    <span style="color: #e36c0a;">C    192.168.20.0/24 is directly connected, FastEthernet0/1.20</span>
                                  </p>
                                  
                                  <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                    <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                      Router0#
                                    </p></div> 
                                    
                                    <h1>
                                      <span id="3_STP">第3章 STP生成树</span>
                                    </h1>
                                    
                                    <p style="margin-left: 7.1pt; text-indent: 21.0pt;">
                                      生成树协议（英语：Spanning Tree Protocol，STP），又称扩展树协议，是一基于OSI网络模型的数据链路层（第二层）通信协议，用作确保一个无环路的局域网环境。
                                    </p>
                                    
                                    <h2>
                                      <span id="31">3.1 生成树的概念</span>
                                    </h2>
                                    
                                    <h3>
                                      <span id="311">3.1.1 冗余拓扑</span>
                                    </h3>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>冗余拓扑能够解决单点故障问题
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>冗余拓扑造成广播风暴，多帧复用，Mac地址不稳定的问题
                                    </p>
                                    
                                    <h3>
                                      <span id="312">3.1.2 广播风暴</span>
                                    </h3>
                                    
                                    <p style="margin-left: 45.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>HOST x发送广播帧
                                    </p>
                                    
                                    <p style="margin-left: 45.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>交换机继续没完没了的更新广播流量
                                    </p>
                                    
                                    <h3>
                                      <span id="313">3.1.3 多帧复制</span>
                                    </h3>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>Host X 发送一个单播数据帧给 ROUTE Y
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>ROUTE Y 的MAC地址还没有被每个交换机学习到
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>ROUTE Y 接收2份相同的数据帧拷贝
                                    </p>
                                    
                                    <h3>
                                      <span id="314_MAC">3.1.4 <span style="background: yellow;">MAC</span>表紊乱</span>
                                    </h3>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>Host X 发送一个单播数据帧给 ROUTE Y
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>ROUTE Y 的MAC地址还没有被每个交换机学习到
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>switch A 和 B 在port1上学习到Host X 的MAC地址
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>到达Route Y 的数据帧被泛洪
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>Switch A和B 不正确的在Port2上学习到Host X的MAC 地址。
                                    </p>
                                    
                                    <h3>
                                      <span id="315">3.1.5 复杂的多环路网络</span>
                                    </h3>
                                    
                                    <p>
                                      ……
                                    </p>
                                    
                                    <h2>
                                      <span id="32_STP">3.2 使用STP，可以达到四个效果</span>
                                    </h2>
                                    
                                    <p style="margin-left: 24.0pt;">
                                      1、防止环路；
                                    </p>
                                    
                                    <p style="margin-left: 24.0pt;">
                                      2、防止MAC地址震荡；
                                    </p>
                                    
                                    <p style="margin-left: 24.0pt;">
                                      3、防止重复帧的出现；
                                    </p>
                                    
                                    <p style="margin-left: 24.0pt;">
                                      4、防止广播风暴的出现。
                                    </p>
                                    
                                    <h3>
                                      <span id="321_STP">3.2.1 采用生成树STP解决环路</span>
                                    </h3>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>通过将特定的端口选为Blocking state，实现无环路拓扑
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>IEEE 802.1D 规定了这一行为
                                    </p>
                                    
                                    <p style="margin-left: 42.0pt; text-indent: -21.0pt;">
                                      <span style="font-family: Wingdings;">l </span>Cisco 采用 IEEE 802.1D 的增强的私有协议生成树PVST+
                                    </p>
                                    
                                    <h2>
                                      <span id="33_STP">3.3 STP的操作</span>
                                    </h2>
                                    
                                    <p style="margin-left: 57.0pt; text-indent: -36.0pt;">
                                      1.<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">      </span>每个广播域选择一个<span style="background: lime;">根桥</span>root
                                    </p>
                                    
                                    <p style="margin-left: 57.0pt; text-indent: -36.0pt;">
                                      2.<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">      </span>每个非根桥上选择一个<span style="background: lime;">根端口</span>RP
                                    </p>
                                    
                                    <p style="margin-left: 57.0pt; text-indent: -36.0pt;">
                                      3.<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">      </span>每个段选择一个<span style="background: lime;">指定端口</span>
                                    </p>
                                    
                                    <p style="margin-left: 57.0pt; text-indent: -36.0pt;">
                                      4.<span style="font-variant-numeric: normal; font-stretch: normal; font-size: 7pt; line-height: normal;">      </span>选择一个非指定端口
                                    </p>
                                    
                                    <h3>
                                      <span id="331_Bridge_ID">3.3.1 每个广播域选择一个<span style="background: lime;">根桥</span> （<span style="color: #c00000; background: lime;">Bridge ID</span>）</span>
                                    </h3>
                                    
                                    <p style="margin-left: 21.0pt;">
                                      <span style="background: lime;">BPDU</span>数据包--泛洪--选举
                                    </p>
                                    
                                    <p>
                                            产生一个或多个非指定端口进行阻塞
                                    </p>
                                    
                                    <p>
                                      <span style="color: #c00000;">   <span style="background: lime;">Bridge ID</span> </span>（两部分组成）: 其依据是网桥优先级（bridge priority）和MAC地址组合生成的<span style="color: red;">桥</span><span style="color: red;">ID</span>
                                    </p>
                                    
                                    <p>
                                      &nbsp;
                                    </p>
                                    
                                    <h3>
                                      <span id="332">3.3.2 每个非根桥上选择一个<span style="background: lime;">根端口</span></span>
                                    </h3>
                                    
                                    <p style="margin-left: 21.0pt;">
                                      根端口：具有最低的根路径的接口
                                    </p>
                                    
                                    <p style="margin-left: 21.0pt;">
                                      要考虑的因素：
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      1. 最低的根桥ID
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      2. 最低的根路径代价
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      3. 最低的发送者根桥ID
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      4. 最低端口ID
                                    </p>
                                    
                                    <h3>
                                      <span id="333_stpcost">3.3.3 stp路径开销<span style="color: red;">cost</span></span>
                                    </h3>
                                    
                                    <div align="center">
                                      <table style="border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
                                        <tr style="height: 15.6pt;">
                                          <td style="width: 134.45pt; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt; height: 15.6pt;" width="179">
                                            <p style="text-align: center;" align="center">
                                              <strong><span style="color: white;">Link Speed</span></strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 184.25pt; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt; height: 15.6pt;" width="246">
                                            <p style="text-align: center;" align="center">
                                              <strong><span style="color: white;">Cost</span></strong>
                                            </p>
                                            
                                            <p style="text-align: center;" align="center">
                                              <strong><span style="color: white;">(New IEEE Specification)</span></strong>
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr style="height: 15.6pt;">
                                          <td style="width: 134.45pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt; height: 15.6pt;" width="179">
                                            <p style="text-align: center;" align="center">
                                              <strong>10Gb/s</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 184.25pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt; height: 15.6pt;" width="246">
                                            <p style="text-align: center;" align="center">
                                              2
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr style="height: 15.6pt;">
                                          <td style="width: 134.45pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt; height: 15.6pt;" width="179">
                                            <p style="text-align: center;" align="center">
                                              <strong>1Gb/s</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 184.25pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 15.6pt;" width="246">
                                            <p style="text-align: center;" align="center">
                                              4
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr style="height: 15.6pt;">
                                          <td style="width: 134.45pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt; height: 15.6pt;" width="179">
                                            <p style="text-align: center;" align="center">
                                              <strong>100Mb/s</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 184.25pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt; height: 15.6pt;" width="246">
                                            <p style="text-align: center;" align="center">
                                              19
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr style="height: 15.6pt;">
                                          <td style="width: 134.45pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt; height: 15.6pt;" width="179">
                                            <p style="text-align: center;" align="center">
                                              <strong>10Mb/s</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 184.25pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 15.6pt;" width="246">
                                            <p style="text-align: center;" align="center">
                                              100
                                            </p>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                    
                                    <p style="text-align: center; text-indent: 21.0pt;" align="center">
                                      最短路径时cost累加，而cost时基于链路的速率的
                                    </p>
                                    
                                    <h3>
                                      <span id="334">3.3.4 每个段选择一个指定端口</span>
                                    </h3>
                                    
                                    <p style="margin-left: 21.0pt;">
                                      指定端口：具有最低根路径的接口
                                    </p>
                                    
                                    <p style="margin-left: 21.0pt;">
                                      要考虑的因素：
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      1. 最低的根桥ID
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      2. 最低的根路径代价
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      3. 最低的发送者根桥ID
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      4. 最低端口ID
                                    </p>
                                    
                                    <h2>
                                      <span id="34_root_rp_dp">3.4 【案例】root 、rp 、dp</span>
                                    </h2>
                                    
                                    <p>
                                      比较过程：
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      1. 最低的根桥ID
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      2. 最低的根路径代价
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      3. 最低的发送者根桥ID
                                    </p>
                                    
                                    <p style="margin-left: 81.0pt; text-indent: -18.0pt;">
                                      4. 最低端口ID
                                    </p>
                                    
                                    <h2>
                                      <span id="35_STP">3.5 STP端口状态</span>
                                    </h2>
                                    
                                    <div align="center">
                                      <table style="border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
                                        <tr>
                                          <td style="width: 162.8pt; border-top: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-right: none; background: #c0504d; padding: 0cm 5.4pt;" valign="top" width="217">
                                            <p style="text-align: center;" align="center">
                                              <strong>状态</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 326pt; border-top: 1pt solid white; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: none; background: #c0504d; padding: 0cm 5.4pt;" valign="top" width="435">
                                            <p style="text-align: center;" align="center">
                                              <strong>功能</strong>
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr>
                                          <td style="width: 162.8pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #c0504d; padding: 0cm 5.4pt;" width="217">
                                            <p style="text-align: center;" align="center">
                                              <strong>阻塞（<span style="color: white;">Blocking</span></strong><strong>）</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #E5B8B7; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
                                            <p style="text-align: justify; text-justify: inter-ideograph;">
                                              这种二层LAN端口不能参与帧转发，侦听BPDU包
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr>
                                          <td style="width: 162.8pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #c0504d; padding: 0cm 5.4pt;" width="217">
                                            <p style="text-align: center;" align="center">
                                              <strong>侦听（<span style="color: white;">Listening</span></strong><strong>）</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #F2DBDB; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
                                            <p style="text-align: justify; text-justify: inter-ideograph;">
                                              这是端口自阻塞状态后的第一个过渡状态。STP认为这种状态的二层LAN端口应当参与帧转发（不转发，不学习） 15s
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr>
                                          <td style="width: 162.8pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #c0504d; padding: 0cm 5.4pt;" width="217">
                                            <p style="text-align: center;" align="center">
                                              <strong>学习（<span style="color: white;">Learning</span></strong><strong>）</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #E5B8B7; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
                                            <p style="text-align: justify; text-justify: inter-ideograph;">
                                              这种状态的二层LAN端口处于准备参与帧转发状态，学习MAC地址
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr>
                                          <td style="width: 162.8pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #c0504d; padding: 0cm 5.4pt;" width="217">
                                            <p style="text-align: center;" align="center">
                                              <strong>转发（<span style="color: white;">Forwarding</span></strong><strong>）</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #F2DBDB; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
                                            <p style="text-align: justify; text-justify: inter-ideograph;">
                                              这种状态的二层LAN端口就可以正式转发帧了
                                            </p>
                                          </td>
                                        </tr>
                                        
                                        <tr>
                                          <td style="width: 162.8pt; border-right: 1pt solid white; border-bottom: 1pt solid white; border-left: 1pt solid white; border-top: none; background: #c0504d; padding: 0cm 5.4pt;" width="217">
                                            <p style="text-align: center;" align="center">
                                              <strong>禁止（<span style="color: white;">Disabled</span></strong><strong>）</strong>
                                            </p>
                                          </td>
                                          
                                          <td style="width: 326.0pt; border-top: none; border-left: none; border-bottom: solid white 1.0pt; border-right: solid white 1.0pt; background: #E5B8B7; padding: 0cm 5.4pt 0cm 5.4pt;" width="435">
                                            <p style="text-align: justify; text-justify: inter-ideograph;">
                                              这种状态的二层LAN端口不参与STP，不转发帧
                                            </p>
                                          </td>
                                        </tr>
                                      </table>
                                    </div>
                                    
                                    <h2>
                                      <span id="36_stp">3.6 【实验】stp</span>
                                    </h2>
                                    
                                    <h3>
                                      <span id="361">3.6.1 拓扑图</span>
                                    </h3>
                                    
                                    <h3>
                                      <span id="362">3.6.2 配置命令</span>
                                    </h3>
                                    
                                    <p style="margin-left: 7.1pt;">
                                      修改优先级
                                    </p>
                                    
                                    <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        S-STP-1 (config-if)spanning-tree vlan vlan-id port-priority
                                      </p>
                                    </div>
                                    
                                    <h3>
                                      <span id="363_STP">3.6.3 检查STP情况的命令</span>
                                    </h3>
                                    
                                    <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        S-STP-1#show spanning-tree
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        S-STP-1#show spanning-tree active
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        S-STP-1#show spanning-tree detail
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        S-STP-1#show spanning-tree interface interface-id
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        S-STP-1#show spanning-tree vlan vlanid
                                      </p>
                                    </div>
                                    
                                    <p>
                                      &nbsp;
                                    </p>
                                    
                                    <div style="border: solid #404040 4.5pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #404040;">
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        S-STP-1(config)#do sh spann
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        VLAN0001
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                          Spanning tree enabled protocol ieee
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                          Root ID    Priority    32769
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                     Address     0001.C944.E360
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                    <span style="color: #e36c0a;"> This bridge is the root</span>
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                     Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                                      </p>
                                      
                                      <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                            Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                                        </p>
                                        
                                        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                       Address     0001.C944.E360
                                        </p>
                                        
                                        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                       Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                                        </p>
                                        
                                        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                                       Aging Time  20
                                        </p>
                                        
                                        <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                            Interface        Role Sts Cost      Prio.Nbr Type
                                          </p>
                                          
                                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                            ---------------- ---- --- --------- -------- --------------------------------
                                          </p>
                                          
                                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                            Fa0/1            Desg FWD 19        128.1    P2p
                                          </p>
                                          
                                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                            Gi0/1            Desg FWD 4         128.25   P2p
                                          </p>
                                          
                                          <p style="text-indent: 15.75pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
                                            Gi0/2            Desg FWD 4         128.26   P2p
                                          </p></div> 
                                          
                                          <div id="toc_container" class="toc_white have_bullets">
                                            <ul class="toc_list">
                                              <li>
                                                <a href="#1">第1章 交换基础</a><ul>
                                                  <li>
                                                    <a href="#11">1.1 园区网分层结构</a>
                                                  </li>
                                                  <li>
                                                    <a href="#12">1.2 交换机的主要功能</a>
                                                  </li>
                                                  <li>
                                                    <a href="#13_MAC">1.3 MAC地址</a>
                                                  </li>
                                                  <li>
                                                    <a href="#14">1.4 交换寻址</a>
                                                  </li>
                                                </ul>
                                              </li>
                                              
                                              <li>
                                                <a href="#2_VLAN">第2章 VLAN 虚拟局域网</a><ul>
                                                  <li>
                                                    <a href="#21_vlan">2.1 vlan概述</a>
                                                  </li>
                                                  <li>
                                                    <a href="#22_vlan">2.2 vlan的成员模式</a>
                                                  </li>
                                                  <li>
                                                    <a href="#23_trunk">2.3 trunk干道</a><ul>
                                                      <li>
                                                        <a href="#231_ISL">2.3.1 ISL封装协议（使用较少）</a>
                                                      </li>
                                                      <li>
                                                        <a href="#232_dot1q8011q">2.3.2 dot1q协议(801.1q)</a>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#24_VLAN">2.4 VLAN的特点</a>
                                                  </li>
                                                  <li>
                                                    <a href="#25_ciscovlan">2.5 cisco上vlan范围</a>
                                                  </li>
                                                  <li>
                                                    <a href="#26_VTP_VLAN_Trunking_Protocol">2.6 VTP （VLAN Trunking Protocol）</a><ul>
                                                      <li>
                                                        <a href="#261_vlan">2.6.1 vlan使用过程</a>
                                                      </li>
                                                      <li>
                                                        <a href="#262_vtp">2.6.2 vtp三种模式</a>
                                                      </li>
                                                      <li>
                                                        <a href="#263_VTP">2.6.3 VTP的运作</a>
                                                      </li>
                                                      <li>
                                                        <a href="#264_VTP_Pruning">2.6.4 VTP的修剪 Pruning</a><ul>
                                                          <li>
                                                            <a href="#2641vlan">2.6.4.1 允许所有vlan通过会有什么缺点呢？</a>
                                                          </li>
                                                        </ul>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#27_VLAN">2.7 VLAN基本配置</a><ul>
                                                      <li>
                                                        <a href="#271_VLAN">2.7.1 创建VLAN信息</a>
                                                      </li>
                                                      <li>
                                                        <a href="#272_vlan">2.7.2 将端口划入特定的vlan</a>
                                                      </li>
                                                      <li>
                                                        <a href="#273_Trunk">2.7.3 配置Trunk封装方式</a>
                                                      </li>
                                                      <li>
                                                        <a href="#274">2.7.4 开启端口</a>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#28_VTP">2.8 VTP的基本配置</a><ul>
                                                      <li>
                                                        <a href="#281">2.8.1 注意：</a>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#29_vlan">2.9 【实验】vlan</a><ul>
                                                      <li>
                                                        <a href="#291">2.9.1 拓扑图</a>
                                                      </li>
                                                      <li>
                                                        <a href="#292">2.9.2 交换机配置</a>
                                                      </li>
                                                      <li>
                                                        <a href="#293_vlan">2.9.3 查看vlan信息</a><ul>
                                                          <li>
                                                            <a href="#2931S-vlan-1">2.9.3.1 S-vlan-1信息</a>
                                                          </li>
                                                          <li>
                                                            <a href="#2932S-vlan-2">2.9.3.2 S-vlan-2信息</a>
                                                          </li>
                                                          <li>
                                                            <a href="#2933trunk">2.9.3.3 查看trunk信息</a>
                                                          </li>
                                                        </ul>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#210">2.10 单臂路由</a><ul>
                                                      <li>
                                                        <a href="#2101">2.10.1 【实验】单臂路由配置</a><ul>
                                                          <li>
                                                            <a href="#21011">2.10.1.1     拓扑图</a>
                                                          </li>
                                                          <li>
                                                            <a href="#21012_trunk">2.10.1.2     交换机trunk链路配置</a>
                                                          </li>
                                                          <li>
                                                            <a href="#21013">2.10.1.3     路由器子接口配置</a>
                                                          </li>
                                                          <li>
                                                            <a href="#21014">2.10.1.4     检查配置结果</a>
                                                          </li>
                                                        </ul>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                </ul>
                                              </li>
                                              
                                              <li>
                                                <a href="#3_STP">第3章 STP生成树</a><ul>
                                                  <li>
                                                    <a href="#31">3.1 生成树的概念</a><ul>
                                                      <li>
                                                        <a href="#311">3.1.1 冗余拓扑</a>
                                                      </li>
                                                      <li>
                                                        <a href="#312">3.1.2 广播风暴</a>
                                                      </li>
                                                      <li>
                                                        <a href="#313">3.1.3 多帧复制</a>
                                                      </li>
                                                      <li>
                                                        <a href="#314_MAC">3.1.4 MAC表紊乱</a>
                                                      </li>
                                                      <li>
                                                        <a href="#315">3.1.5 复杂的多环路网络</a>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#32_STP">3.2 使用STP，可以达到四个效果</a><ul>
                                                      <li>
                                                        <a href="#321_STP">3.2.1 采用生成树STP解决环路</a>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#33_STP">3.3 STP的操作</a><ul>
                                                      <li>
                                                        <a href="#331_Bridge_ID">3.3.1 每个广播域选择一个根桥 （Bridge ID）</a>
                                                      </li>
                                                      <li>
                                                        <a href="#332">3.3.2 每个非根桥上选择一个根端口</a>
                                                      </li>
                                                      <li>
                                                        <a href="#333_stpcost">3.3.3 stp路径开销cost</a>
                                                      </li>
                                                      <li>
                                                        <a href="#334">3.3.4 每个段选择一个指定端口</a>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                  
                                                  <li>
                                                    <a href="#34_root_rp_dp">3.4 【案例】root 、rp 、dp</a>
                                                  </li>
                                                  <li>
                                                    <a href="#35_STP">3.5 STP端口状态</a>
                                                  </li>
                                                  <li>
                                                    <a href="#36_stp">3.6 【实验】stp</a><ul>
                                                      <li>
                                                        <a href="#361">3.6.1 拓扑图</a>
                                                      </li>
                                                      <li>
                                                        <a href="#362">3.6.2 配置命令</a>
                                                      </li>
                                                      <li>
                                                        <a href="#363_STP">3.6.3 检查STP情况的命令</a>
                                                      </li>
                                                    </ul>
                                                  </li>
                                                </ul>
                                              </li>
                                            </ul>
                                          </div>