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
## <span id="1nbsp">第1章&nbsp;<span style="font-family: 新宋体;">虚拟机安装</span></span>

<div>
  &nbsp;1.1 <span style="font-family: 新宋体;">镜像下载</span>
</div>

### <span id="111">1.1.1 <span style="font-family: 新宋体;">新版本下载</span></span>

http://mirrors.aliyun.com&nbsp; #<span style="font-family: 新宋体;">阿里云官方镜像站点</span>

### <span id="112">1.1.2 <span style="font-family: 新宋体;">旧版本下载</span></span>

http://vault.centos.org/&nbsp;&nbsp; #vault&nbsp; <span style="font-family: 新宋体;">电子仓库</span>

<span style="font-family: 新宋体;">尽量使用种子文件下载，速度较快</span>

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221300687-1484084097.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" /> 

## <span id="12_VMware">1.2 VMware<span style="font-family: 新宋体;">新建虚拟机</span></span>

### <span id="121">1.2.1 <span style="font-family: 新宋体;">新建虚拟机</span></span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221320546-768373430.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="122">1.2.2 <span style="font-family: 新宋体;">选择自定义</span></span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221344609-54448267.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="123">1.2.3 <span style="font-family: 新宋体;">兼容性选择</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">兼容性选择默认即可。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221406656-1144356862.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;

### <span id="124">1.2.4 <span style="font-family: 新宋体;">系统选择</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">选择<span style="background: lime;">稍后安装操作系统</span>，直接使用镜像会自动安装。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221430937-918321842.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">选择</span>centos 64 <span style="font-family: 新宋体;">位系统。</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221438906-1439106463.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="125">1.2.5 <span style="font-family: 新宋体;">虚拟机位置</span></span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221450140-473036602.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="126_cpu">1.2.6 cpu<span style="font-family: 新宋体;">、内存</span></span>

<p style="margin-left: 21.0pt;">
  cpu<span style="font-family: 新宋体;">个数根据需要设置。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221505515-1149355158.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">内存大小，在安装时内存<span style="background: lime;">不能少于</span></span><span style="background: lime;">1G</span><span style="font-family: 新宋体;">。</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221518281-2021578890.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />&nbsp;
</p>

### <span id="127">1.2.7 <span style="font-family: 新宋体;">网络</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">网络使用</span>NAT
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221530202-294093382.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;

### <span id="128">1.2.8 <span style="font-family: 新宋体;">磁盘</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">磁盘类型选择默认的类型。</span>
</p>

&nbsp;![][1]<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221604343-1792544080.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">新创建一个虚拟磁盘，磁盘大小为</span>10G<span style="font-family: 新宋体;">。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">将磁盘拆分为多个文件。</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">使用默认的磁盘文件名称。</span>

&nbsp;![][2]<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221627468-194765352.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />&nbsp;

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221646734-511098084.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" /> 

### <span id="129">1.2.9 <span style="font-family: 新宋体;">创建成功</span></span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221706812-612623671.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

## <span id="13">1.3 <span style="font-family: 新宋体;">系统安装</span></span>

### <span id="131">1.3.1 <span style="font-family: 新宋体;">挂载镜像</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">在</span>VMware<span style="font-family: 新宋体;">中打开新建虚拟机，单机</span>CD/DVD ,<span style="font-family: 新宋体;">选择使用</span>ISO<span style="font-family: 新宋体;">映像文件。找到下载好的文件，确定。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221723562-865290452.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="132">1.3.2 <span style="font-family: 新宋体;">安装</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">开机，选择第一个，安装系统。</span>
</p>

<p style="text-indent: 21.0pt;">
  Rescue installed system &nbsp;<span style="font-family: 新宋体;">救援系统。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221738046-265680436.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">检查完整性，选择<span style="background: lime;">跳过</span>。</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221751499-250014040.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">在安装系统时，如果屏幕看不到</span>next <span style="font-family: 新宋体;">可以使用</span><span style="background: lime;">F12.</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221809390-360874653.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="133">1.3.3 <span style="font-family: 新宋体;">语言选择</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">语言选择</span><span style="background: lime;">English</span><span style="font-family: 新宋体;">，键盘模式默认即可。英文与中文在<span style="background: lime;">安装时会有差异</span>。</span>
</p>

&nbsp;![][3]<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221822281-1929466481.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />

### <span id="134">1.3.4 <span style="font-family: 新宋体;">储存类型</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">选择基础的储存方式。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221830968-639338265.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">使用整块硬盘，并清除硬盘上的数据。</span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221838984-47939676.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="135">1.3.5 <span style="font-family: 新宋体;">主机名称</span></span>

<span style="font-family: 新宋体;">根据实际情况，设置主机名称。注意：主机名支持的特殊字符不多。</span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221847843-24746696.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="136">1.3.6 <span style="font-family: 新宋体;">日期与时间</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">时间选择</span>shanghai<span style="font-family: 新宋体;">，</span> <span style="font-family: 新宋体;">取消勾选</span>system clock uses UTC <span style="font-family: 新宋体;">。否则会出错。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221856062-528864636.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="137">1.3.7 <span style="font-family: 新宋体;">用户密码</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">为了便于使用，密码为</span>123456<span style="font-family: 新宋体;">，该密码系统提示为弱密码，选择使用（</span>use anyway<span style="font-family: 新宋体;">）。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221903437-2076331944.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="138">1.3.8 <span style="font-family: 新宋体;">磁盘分区</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">使用最后一项，自定义分区。</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221916031-241425793.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

<p style="margin-left: 28.0pt; text-indent: 21.0pt;">
  /boot&nbsp;&nbsp;&nbsp; 200M <span style="font-family: 新宋体;">存放系统的引导的信息</span>
</p>

<p style="margin-left: 28.0pt; text-indent: 21.0pt;">
  swap&nbsp;&nbsp;&nbsp; 768M <span style="font-family: 新宋体;">交换分区</span>&nbsp;<span style="font-family: 新宋体;">内存快用完之前使用交换分区</span>&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">临时内存</span>&nbsp;&nbsp;
</p>

<p style="margin-left: 84.0pt; text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">如果你的内存是</span>8G<span style="font-family: 新宋体;">以内的</span> <span style="font-family: 新宋体;">交换分区给内存的</span>1.5<span style="font-family: 新宋体;">倍</span>
</p>

<p style="margin-left: 84.0pt; text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">如果你的内存是</span>8G<span style="font-family: 新宋体;">以上的</span> <span style="font-family: 新宋体;">交换分区就给</span>8G
</p>

<p style="margin-left: 84.0pt; text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">以后内存给</span>512M<span style="font-family: 新宋体;">即可，</span>768M
</p>

<p style="margin-left: 28.0pt; text-indent: 28.0pt;">
  /&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">根分区</span>&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">剩下多少给多少</span>
</p>

<p style="margin-left: 28.0pt;">
  <span style="font-family: 新宋体;">先建立</span>/boot<span style="font-family: 新宋体;">分区用于引导系统，建立</span>swap <span style="font-family: 新宋体;">交互分区，将剩余空间分根目录。</span>
</p>

&nbsp;![][4]![][5]<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221934093-390402346.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">分区建立完成后，弹出对话框，是否格式化分区，单击格式化。</span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115222000734-917864134.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">点击下一步，将修改的信息写入硬盘</span> write changes to disk<span style="font-family: 新宋体;">。</span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115222012031-835735960.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">系统安装位置选择默认的磁盘即可。</span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115222019859-1480413351.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

### <span id="139">1.3.9 <span style="font-family: 新宋体;">安装版本</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">选择最小化安装</span>-minimal----<span style="font-family: 新宋体;">使用什么安装</span>
</p>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115222027484-362525020.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">在这里选择</span>customize&nbsp; now <span style="font-family: 新宋体;">（自定义），添加其他的功能。</span>

<p style="text-align: center;" align="center">
  &nbsp;<img src="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115222044515-234261997.png" alt="Linux系统安装_Centos6.9" alt="" /><img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115222051593-1277893657.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">然后就进入安装过程，稍等一段时间就安装完成，选择</span>reboot <span style="font-family: 新宋体;">重启进入系统。</span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171115222059952-977034977.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Linux系统安装_Centos6.9" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1nbsp">第1章&nbsp;虚拟机安装</a><ul>
        <li>
          <a href="#111">1.1.1 新版本下载</a>
        </li>
        <li>
          <a href="#112">1.1.2 旧版本下载</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_VMware">1.2 VMware新建虚拟机</a><ul>
        <li>
          <a href="#121">1.2.1 新建虚拟机</a>
        </li>
        <li>
          <a href="#122">1.2.2 选择自定义</a>
        </li>
        <li>
          <a href="#123">1.2.3 兼容性选择</a>
        </li>
        <li>
          <a href="#124">1.2.4 系统选择</a>
        </li>
        <li>
          <a href="#125">1.2.5 虚拟机位置</a>
        </li>
        <li>
          <a href="#126_cpu">1.2.6 cpu、内存</a>
        </li>
        <li>
          <a href="#127">1.2.7 网络</a>
        </li>
        <li>
          <a href="#128">1.2.8 磁盘</a>
        </li>
        <li>
          <a href="#129">1.2.9 创建成功</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13">1.3 系统安装</a><ul>
        <li>
          <a href="#131">1.3.1 挂载镜像</a>
        </li>
        <li>
          <a href="#132">1.3.2 安装</a>
        </li>
        <li>
          <a href="#133">1.3.3 语言选择</a>
        </li>
        <li>
          <a href="#134">1.3.4 储存类型</a>
        </li>
        <li>
          <a href="#135">1.3.5 主机名称</a>
        </li>
        <li>
          <a href="#136">1.3.6 日期与时间</a>
        </li>
        <li>
          <a href="#137">1.3.7 用户密码</a>
        </li>
        <li>
          <a href="#138">1.3.8 磁盘分区</a>
        </li>
        <li>
          <a href="#139">1.3.9 安装版本</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

 [1]: https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221556702-1501112735.png
 [2]: https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221619734-406009392.png
 [3]: https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221818577-1410667839.png
 [4]: https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221927202-84470230.png
 [5]: https://clsn.io/wp-content/uploads/2018/03/1190037-20171115221930343-1529741652.png