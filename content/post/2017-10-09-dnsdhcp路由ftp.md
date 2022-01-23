---
title: 'DNS  DHCP   路由  FTP'
author: 惨绿少年
type: post
date: 2017-10-08T18:28:00+00:00
url: /clsn/lx946.html
Baidusubmit:
  - 1
views:
  - 120
categories:
  - Linux运维
  - 网络技术
  - 运维基本功

---
# <span id="1">第1章 <span style="font-family: '微软雅黑',sans-serif;">网络基础</span></span>

## <span id="11_IP">1.1 <span style="background: lime;">IP</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman"; background: lime;">地址分类</span></span>

    IP<span style="font-family: '微软雅黑',sans-serif;">地址的类别</span>-<span style="font-family: '微软雅黑',sans-serif;">按</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址数值范围划分</span>

    IP<span style="font-family: '微软雅黑',sans-serif;">地址的类别</span>-<span style="font-family: '微软雅黑',sans-serif;">按</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址用途分类</span>

    IP<span style="font-family: '微软雅黑',sans-serif;">地址的类别</span>-<span style="font-family: '微软雅黑',sans-serif;">按网络通信方式划分</span>

## <span id="12">1.2 <span style="font-family: '微软雅黑',sans-serif;">局域网上网原理过程</span></span>

    DHCP<span style="font-family: '微软雅黑',sans-serif;">原理过程详情：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=%20http://www.zyops.com/dhcp-working-procedure"  target="_blank"> http://www.zyops.com/dhcp-working-procedure</a>

<img data-original="/wp-content/uploads/2018/11/686769-20161019110002810-907858031-300x121.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="DNS  DHCP   路由  FTP" alt="dhcp" /> 

<p style="text-indent: 21.0pt;">
  DHCP<span style="font-family: '微软雅黑',sans-serif;">（</span>Dynamic Host Configuration Protocol<span style="font-family: '微软雅黑',sans-serif;">，动态主机配置协议）通常被应用在大型的局域网络环境中，主要作用是集中的管理、分配</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址，使网络环境中的主机动态的获得</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址、网关地址、</span>DNS<span style="font-family: '微软雅黑',sans-serif;">服务器地址等信息，并能够提升地址的使用率。</span>
</p>

## <span id="13_DHCP">1.3 DHCP<span style="font-family: '微软雅黑',sans-serif;">服务原理</span></span>

### <span id="131_DHCPIP">1.3.1 DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器</span>IP<span style="font-family: '微软雅黑',sans-serif;">分配方式</span></span>

<p style="text-align: center;">
  <img id="uploading_image_49621" data-original="https://clsn.io/wp-content/uploads/2018/03/loading.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="DNS  DHCP   路由  FTP" alt="" /> 
</p>

DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器提供三种</span>IP<span style="font-family: '微软雅黑',sans-serif;">分配方式：</span>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">l </span><strong><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: aqua;">自动分配</span></strong><strong><span style="font-family: '微软雅黑',sans-serif;">（</span>Automatic Allocation</strong><strong><span style="font-family: '微软雅黑',sans-serif;">）</span></strong>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">自动分配是当</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">客户端第一次成功地从</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器端分配到一个</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址之后，就永远使用这个地址。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">l </span><strong><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: aqua;">动态分配</span></strong><strong><span style="font-family: '微软雅黑',sans-serif;">（</span>Dynamic Allocation</strong><strong><span style="font-family: '微软雅黑',sans-serif;">）</span></strong>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">动态分配是当</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">客户端第一次从</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器分配到</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址后，并非永久地使用该地址，每次使用完后，</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">客户端就得释放这个</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址，以给其他客户端使用。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">l </span><strong><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: aqua;">手动分配</span></strong>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">手动分配是由</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器管理员专门为客户端指定</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址。</span>
</p>

### <span id="132_DHCP">1.3.2 DHCP<span style="font-family: '微软雅黑',sans-serif;">服务工作流程</span></span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  DHCP<span style="font-family: '微软雅黑',sans-serif;">客户机在<span style="background: lime;">启动时</span>，会<span style="background: lime;">搜寻网络中是否存在</span></span><span style="background: lime;">DHCP</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">服务器</span><span style="font-family: '微软雅黑',sans-serif;">。如果找到，则给</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器发送一个请求。</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器接到请求后，为</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">客户机选择</span>TCP/IP<span style="font-family: '微软雅黑',sans-serif;">配置的参数，并把这些参数发送给客户端。</span> <span style="font-family: '微软雅黑',sans-serif;">如果已配置冲突检测设置，则</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">服务器在将租约中的地址提供给客户机之前会<span style="background: lime;">使用</span></span><span style="background: lime;">Ping</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">测试作用域中每个可用地址的连通性</span><span style="font-family: '微软雅黑',sans-serif;">。这可确保提供给客户的每个</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址都没有被使用手动</span>TCP/IP<span style="font-family: '微软雅黑',sans-serif;">配置的另一台非</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">计算机使用。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
   
</p>

## <span id="14_DNS">1.4 DNS<span style="font-family: '微软雅黑',sans-serif;">服务原理</span></span>

**<span style="font-family: '微软雅黑',sans-serif;">客户端用户从</span>web****<span style="font-family: '微软雅黑',sans-serif;">浏览器里输入网站后看到网站的完整内容的完整访问过程</span>**

<p style="margin-left: 21.0pt; text-indent: 21.0pt; line-height: 150%;">
  01.<span style="font-family: '微软雅黑',sans-serif;">客户端用户在浏览器中输入</span>www.znix.top <span style="font-family: '微软雅黑',sans-serif;">网站地址后回车，系统会<span style="background: lime;">首先查找本地</span></span><span style="background: lime;">host</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">文件以及</span><span style="background: lime;">DNS</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">缓存</span><span style="font-family: '微软雅黑',sans-serif;">信息查找是否存在</span>www.znix.top <span style="font-family: '微软雅黑',sans-serif;">对应的</span>ip<span style="font-family: '微软雅黑',sans-serif;">解析记录。如果有就直接获取</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址，然后去访问这个</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址对应的域名服务器，一般第一次请求时，</span><span style="background: lime;">DNS</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">缓存</span><span style="font-family: '微软雅黑',sans-serif;">是没有解析记录的。</span>
</p>

**windows** **<span style="font-family: '微软雅黑',sans-serif;">系统中的</span>dns****<span style="font-family: '微软雅黑',sans-serif;">缓存</span>**

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           <span style="color: #548dd4;">#windows </span><span style="font-family: '微软雅黑',sans-serif; color: #548dd4;">系统查看与刷新缓存记录信息</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           ipconfig /flushdns        <span style="color: #76923c;"><-- </span><span style="font-family: '微软雅黑',sans-serif; color: #76923c;">清除缓存命令</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           ipconfig /displaydns      <span style="color: #76923c;"><-- </span><span style="font-family: '微软雅黑',sans-serif; color: #76923c;">显示缓存命令</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           <span style="color: #548dd4;">#</span><span style="font-family: '微软雅黑',sans-serif; color: #548dd4;">显示</span><span style="color: #548dd4;">hosts</span><span style="font-family: '微软雅黑',sans-serif; color: #548dd4;">文件域名与地址映射关系配置信息</span><span style="color: #548dd4;">(hosts</span><span style="font-family: '微软雅黑',sans-serif; color: #548dd4;">文件位置</span><span style="color: #548dd4;">)</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           c:\windows\system32\drivers\etc\hosts
  </p>
</div>

**Linux** **<span style="font-family: '微软雅黑',sans-serif;">系统中的</span>dns****<span style="font-family: '微软雅黑',sans-serif;">缓存</span>**

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           <span style="color: #548dd4;">#</span><span style="font-family: '微软雅黑',sans-serif; color: #548dd4;">缓存方式</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           nccd <span style="font-family: '微软雅黑',sans-serif;">或者</span> BIND <span style="font-family: '微软雅黑',sans-serif;">或者</span> dnsmasq
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           <span style="color: #548dd4;">#hosts</span><span style="font-family: '微软雅黑',sans-serif; color: #548dd4;">文件</span> <span style="font-family: '微软雅黑',sans-serif; color: #548dd4;">位置</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
           /etc/hossts
  </p>
</div>

<p style="margin-left: 21.0pt; text-indent: 21.0pt; line-height: 150%;">
  02.<span style="font-family: '微软雅黑',sans-serif;">如果客户端本地缓存或</span>hosts <span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">没有对应的</span><span style="background: lime;">www.znix.top </span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman"; background: lime;">域名的解析记录</span><span style="font-family: '微软雅黑',sans-serif;">，那么，系统会把浏览器的解析<span style="background: lime;">请求交给在客户端本地设置的</span></span><span style="background: lime;">DNS</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman"; background: lime;">服务器</span><span style="font-family: '微软雅黑',sans-serif;">地址</span>( <span style="font-family: '微软雅黑',sans-serif;">通常称此</span>DNS<span style="font-family: '微软雅黑',sans-serif;">为</span>LDNS<span style="font-family: '微软雅黑',sans-serif;">，即</span>:<span style="background: lime;">localDNS )</span><span style="font-family: '微软雅黑',sans-serif;">解析，如果</span>LDNS<span style="font-family: '微软雅黑',sans-serif;">服务器的本地缓存有对应的解析记录就会直接返回</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址，如果没有，</span>LDNS<span style="font-family: '微软雅黑',sans-serif;">会负责继续请求其它的</span>DNS<span style="font-family: '微软雅黑',sans-serif;">服务器</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt; line-height: 150%;">
  03.LDNS <span style="font-family: '微软雅黑',sans-serif;">会从</span>DNS <span style="font-family: '微软雅黑',sans-serif;">系统的（</span><span style="background: lime;">.</span><span style="font-family: '微软雅黑',sans-serif;">）根开始请求</span>www.znix.top <span style="font-family: '微软雅黑',sans-serif;">域名的解析</span>,<span style="font-family: '微软雅黑',sans-serif;">经过一系列的查找各个层级的</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器，最终会查到</span>etiantian .org <span style="font-family: '微软雅黑',sans-serif;">域名对应的授权</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器，而这个授权</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器正是企业购买域名时用于管理域名解析的服务器，这个服务器会有</span>www.znix.top<span style="font-family: '微软雅黑',sans-serif;">对应的</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址解析记录</span>( A<span style="font-family: '微软雅黑',sans-serif;">记录</span>),<span style="font-family: '微软雅黑',sans-serif;">如果此时没有，就表示企业的运维人员没有给</span>www.znix.top <span style="font-family: '微软雅黑',sans-serif;">域名做解析。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt; line-height: 150%;">
  04.www.znix.top <span style="font-family: '微软雅黑',sans-serif;">域名对应的授权</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器会把</span>www.znix.top <span style="font-family: '微软雅黑',sans-serif;">对应的最终</span>IP <span style="font-family: '微软雅黑',sans-serif;">解析记录</span>(<span style="font-family: '微软雅黑',sans-serif;">例如</span>1.1.1.1)<span style="font-family: '微软雅黑',sans-serif;">发给</span>LDNS
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt; line-height: 150%;">
  05.LDNS <span style="font-family: '微软雅黑',sans-serif;">把收到的来自授权</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器</span>www.znix.top <span style="font-family: '微软雅黑',sans-serif;">对应的</span>IP <span style="font-family: '微软雅黑',sans-serif;">解析记录发给客户端浏览器，并且在</span>LDNS<span style="font-family: '微软雅黑',sans-serif;">本地把域名和</span>IP<span style="font-family: '微软雅黑',sans-serif;">的对应解析缓存起来，以便下一次更快的返回相同解析请求的记录。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt; line-height: 150%;">
  06.<span style="font-family: '微软雅黑',sans-serif;">客户端浏览器获取到</span>www.znix.top<span style="font-family: '微软雅黑',sans-serif;">的对应的</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址</span>,<span style="font-family: '微软雅黑',sans-serif;">接下来，浏览器会请求获得的</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址对应的</span>web <span style="font-family: '微软雅黑',sans-serif;">服务器，</span>web <span style="font-family: '微软雅黑',sans-serif;">服务器收到客户的请求并响应处理，将客户请求的内容返回给客户端浏览器，至此，一次访问浏览器网页的完整过程完成了其中此步骤客户端获取到服务器</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址后，利用浏览器请求</span>web<span style="font-family: '微软雅黑',sans-serif;">服务器获取网页信息的过程称为</span>HTTP <span style="font-family: '微软雅黑',sans-serif;">原理</span>
</p>

<p style="text-align: center;" align="center">
   
</p>

### <span id="141_DNS">1.4.1 DNS<span style="font-family: '微软雅黑',sans-serif;">能干什么</span></span>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong>A</strong><strong><span style="font-family: '微软雅黑',sans-serif;">记录：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">将域名指向一个</span><span style="font-size: 10.5pt;">IPv4</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">地址（例如：</span><span style="font-size: 10.5pt;">10.10.10.10</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">），需要增加</span><span style="font-size: 10.5pt;">A</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong>CNAME</strong><strong><span style="font-family: '微软雅黑',sans-serif;">记录：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">如果将域名指向一个域名，实现与被指向域名相同的访问效果，需要增加</span><span style="font-size: 10.5pt;">CNAME</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录（</span><span style="font-size: 10.5pt;">CDN</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">加速）</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong>MX</strong><strong><span style="font-family: '微软雅黑',sans-serif;">记录：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">建立电子邮箱服务，将指向邮件服务器地址，需要设置</span><span style="font-size: 10.5pt;">MX</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong>NS</strong><strong><span style="font-family: '微软雅黑',sans-serif;">记录：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">域名解析服务器记录，如果要将子域名指定某个域名服务器来解析，需要设置</span><span style="font-size: 10.5pt;">NS</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong>TXT</strong><strong><span style="font-family: '微软雅黑',sans-serif;">记录：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">可任意填写（可为空），通常用做</span><span style="font-size: 10.5pt;">SPF</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录（反垃圾邮件）使用</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong>AAAA</strong><strong><span style="font-family: '微软雅黑',sans-serif;">记录：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">将主机名（或域名）指向一个</span><span style="font-size: 10.5pt;">IPv6</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">地址（例如：</span><span style="font-size: 10.5pt;">ff03:0:0:0:0:0:0:c1</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">），需要添加</span><span style="font-size: 10.5pt;">AAAA</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong>PTR</strong><strong><span style="font-family: '微软雅黑',sans-serif;">记录：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">设置</span><span style="font-size: 10.5pt;">PTR</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录，反向解析，即把</span><span style="font-size: 10.5pt;">IP</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">地址解析为对应的域名，和</span><span style="font-size: 10.5pt;">A</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">记录的解析相反（<span style="background: yellow;">邮件服务等</span>）</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong><span style="font-family: '微软雅黑',sans-serif;">显性</span>URL</strong><strong><span style="font-family: '微软雅黑',sans-serif;">：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">将域名指向一个</span><span style="font-size: 10.5pt;">http</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">（</span><span style="font-size: 10.5pt;">s)</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">协议地址，访问域名时，自动跳转至目标地址（例如：将</span><span style="font-size: 10.5pt;">www.net.cn</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">显性转发到</span><span style="font-size: 10.5pt;">www.hichina.com</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">后，访问</span><span style="font-size: 10.5pt;">www.net.cn</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">时，地址栏显示的地址为：</span><span style="font-size: 10.5pt;">www.hichina.com</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">）。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: -21.0pt;">
  <span style="font-family: Wingdings;">Ø </span><strong><span style="font-family: '微软雅黑',sans-serif;">隐性</span>URL</strong><strong><span style="font-family: '微软雅黑',sans-serif;">：</span></strong>
</p>

<p style="margin-left: 12.0pt;">
  <span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">与显性</span><span style="font-size: 10.5pt;">URL</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">类似，但隐性转发会隐藏真实的目标地址（例如：将</span><span style="font-size: 10.5pt;">www.net.cn</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">隐性转发到</span><span style="font-size: 10.5pt;">www.hichina.com</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">后，访问</span><span style="font-size: 10.5pt;">www.net.cn</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">时，地址栏显示的地址仍然为：</span><span style="font-size: 10.5pt;">www.net.cn</span><span style="font-size: 10.5pt; font-family: '微软雅黑',sans-serif;">）。</span>
</p>

<p style="text-align: center;" align="center">
   
</p>

## <span id="15">1.5 <span style="font-family: '微软雅黑',sans-serif;">递归查询和迭代查询的区别</span></span>

### <span id="151">1.5.1 <span style="font-family: '微软雅黑',sans-serif;">递归查询</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">递归查询是一种</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器的查询模式，在该模式下</span><span style="background: lime;">DNS </span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">服务器接收到客户机请求，必须使用一个准确的查询结果回复客户机</span><span style="font-family: '微软雅黑',sans-serif;">。如果</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器本地没有存储查询</span>DNS <span style="font-family: '微软雅黑',sans-serif;">信息，那么该服务器会询问其他服务器，并将返回的查询结果提交给客户机。</span>
</p>

### <span id="152">1.5.2 <span style="font-family: '微软雅黑',sans-serif;">迭代查询</span></span>

<p style="text-indent: 21.0pt;">
  DNS <span style="font-family: '微软雅黑',sans-serif;">服务器另外一种查询方式为迭代查询，</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器会向客户机提供其他能够解析查询请求的</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器地址，当客户机发送查询请求时，</span><span style="background: lime;">DNS </span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">服务器并不直接回复查询结果</span><span style="font-family: '微软雅黑',sans-serif;">，而是告诉客户机另一台</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器地址，客户机再向这台</span>DNS <span style="font-family: '微软雅黑',sans-serif;">服务器提交请求，依次循环直到返回查询的结果为止。</span>
</p>

<p style="text-align: center; text-indent: 21.0pt;" align="center">
   
</p>

## <span id="16_DNS">1.6 DNS<span style="font-family: '微软雅黑',sans-serif;">的相关命令</span></span>

### <span id="161_dig">1.6.1 dig<span style="font-family: '微软雅黑',sans-serif;">命令</span>    </span>

<div align="center">
  <table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
    <tr style="height: 3.5pt;">
      <td style="width: 130.7pt; border-width: 1pt; border-style: solid; border-color: windowtext; background: #95b3d7; padding: 0cm 5.4pt; height: 3.5pt;" width="174">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong><span style="font-family: '微软雅黑',sans-serif;">命令</span></strong>
        </p>
      </td>
      
      <td style="width: 88.8pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #95b3d7; padding: 0cm 5.4pt; height: 3.5pt;" width="118">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong><span style="font-size: 11.0pt;">LDNS</span></strong>
        </p>
      </td>
      
      <td style="width: 92.1pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #95b3d7; padding: 0cm 5.4pt; height: 3.5pt;" width="123">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong><span style="font-family: '微软雅黑',sans-serif;">记录类型</span></strong>
        </p>
      </td>
      
      <td style="width: 134.7pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #95b3d7; padding: 0cm 5.4pt; height: 3.5pt;" width="180">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong><span style="font-family: '微软雅黑',sans-serif;">网址</span></strong>
        </p>
      </td>
    </tr>
    
    <tr style="height: 28.75pt;">
      <td style="width: 130.7pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt; height: 28.75pt;" width="174">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong>dig</strong>
        </p>
      </td>
      
      <td style="width: 88.8pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 28.75pt;" width="118">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong>@8.8.8.8</strong>
        </p>
      </td>
      
      <td style="width: 92.1pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 28.75pt;" width="123">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong>A</strong>
        </p>
      </td>
      
      <td style="width: 134.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 28.75pt;" width="180">
        <p style="text-align: justify; text-justify: inter-ideograph;">
          <strong>www.baidu.com</strong>
        </p>
      </td>
    </tr>
  </table>
</div>

 

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">dig @8.8.8.8 gov.cn</span><br />  <br /> ; <<>> DiG 9.8.2rc1-RedHat-9.8.2-0.62.rc1.el6 <<>> @8.8.8.8 gov.cn<br /> ; (1 server found)<br /> ;; global options: +cmd<br /> ;; Got answer:<br /> ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51019 ;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 0   ;; QUESTION SECTION: ;gov.cn.               IN  A   ;; AUTHORITY SECTION: gov.cn.         6855    IN  SOA a.dns.cn. root.cnnic.cn. 2021852429 7200 3600 2419200 21600   ;; Query time: 11 msec ;; SERVER: 8.8.8.8#53(8.8.8.8) ;; WHEN: Thu Sep 28 18:59:49 2017 ;; MSG SIZE  rcvd: 77 #dig @8.8.8.8 MX baidu.com <span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">说明</span><span style="color: #e36c0a;">:</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">通过</span><span style="color: #e36c0a;">dig</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">命令查看解析原理，可以看到全球</span><span style="color: #e36c0a;">13</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">台根服务器</span>
</div>

### <span id="162_nslookup">1.6.2 nslookup</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">nslookup</span>        <span style="color: #984806;"> <- </span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">直接解析指定域名对应的地址</span><br /> > <span style="background: lime;">www.gov.cn</span><br /> Server:     223.5.5.5                      <span style="color: #984806;"><- </span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">解析域名信息的</span><span style="color: #984806;">DNS</span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">服务器信息</span><br /> Address:    223.5.5.5#53<br />  <br /> Non-authoritative answer:<br /> www.gov.cn  canonical name = www.gov.cn.qingcdn.com.<br /> www.gov.cn.qingcdn.com  canonical name = uz91.v.qingcdn.com.  <span style="color: #984806;"><-</span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">域名的</span><span style="color: #984806;">CNAME </span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">信息</span><br /> Name:   uz91.v.qingcdn.com<br /> Address: 125.39.21.26                       <span style="color: #984806;"> <- </span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">域名对应的</span><span style="color: #984806;">ip</span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">地址</span><br /> Name:   uz91.v.qingcdn.com<br /> Address: 125.39.21.6<br /> Name:   uz91.v.qingcdn.com<br /> Address: 125.39.21.4<br /> Name:   uz91.v.qingcdn.com<br /> Address: 125.39.21.5<br /> Name:   uz91.v.qingcdn.com
</div>

### <span id="163_host">1.6.3 host<span style="font-family: '微软雅黑',sans-serif;">命令</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">host www.gov.cn</span>       <span style="color: #984806;">- </span><span style="font-family: '微软雅黑',sans-serif; color: #984806;">直接解析指定域名对应的地址</span><br /> www.gov.cn is an alias for www.gov.cn.qingcdn.com.<br /> www.gov.cn.qingcdn.com is an alias for uz91.v.qingcdn.com.<br /> uz91.v.qingcdn.com has address 125.39.21.7<br /> uz91.v.qingcdn.com has address 125.39.21.26<br /> uz91.v.qingcdn.com has address 125.39.21.6
</div>

### <span id="164_ping">1.6.4 ping <span style="font-family: '微软雅黑',sans-serif;">命令</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">ping www.gov.cn -c1</span><br /> PING uz91.v.qingcdn.com (125.39.21.7) 56(84) bytes of data.<br /> 64 bytes from no-data (<span style="background: yellow;">125.39.21.7</span>): icmp_seq=1 ttl=128 time=27.6 ms<br />  <br /> --- uz91.v.qingcdn.com ping statistics ---<br /> 1 packets transmitted, 1 received, 0% packet loss, time 78ms<br /> rtt min/avg/max/mdev = 27.639/27.639/27.639/0.000 ms
</div>

ping <span style="font-family: '微软雅黑',sans-serif;">命令参数</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    -c <span style="font-family: '微软雅黑',sans-serif;">包的个数</span>
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    -i <span style="font-family: '微软雅黑',sans-serif;">设置间隔时间</span>
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    -s <span style="font-family: '微软雅黑',sans-serif;">数据包的大小</span>
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    -q <span style="font-family: '微软雅黑',sans-serif;">中间的信息不显示</span>
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    -f  <span style="font-family: '微软雅黑',sans-serif;">急速</span>ping
  </p>
</div>

## <span id="17">1.7 <span style="font-family: '微软雅黑',sans-serif;">网卡信息</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    [root@clsn ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    DEVICE=eth0               <span style="color: #31849b;">#</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">第一块网卡逻辑设备名</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    HWADDR=00:0c:29:a8:e4:14  <span style="color: #31849b;">#</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">硬件地址，</span><span style="color: #31849b;">MAC</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">地址（</span><span style="color: #31849b; background: yellow;">vm</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b; background: yellow;">克隆后删除</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">）</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    TYPE=Ethernet             <span style="color: #31849b;">#</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">上网类型</span> -<span style="font-family: '微软雅黑',sans-serif;">以太网</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    UUID=a3b2265e-9dac-4a29-aff6-d2e88eb28cfc<span style="color: #31849b;">#</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">通用唯一标识码（</span><span style="color: #31849b; background: yellow;">vm</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b; background: yellow;">克隆后删除</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">）</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    ONBOOT=yes               <span style="color: #31849b;"> #</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">开机自启动</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    NM_CONTROLLED=yes        <span style="color: #31849b;"> #</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">网络管理控制参数</span> <span style="font-family: '微软雅黑',sans-serif; color: #31849b;">设置为</span><span style="color: #31849b;">no </span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">（</span><span style="color: #31849b;">centos6</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">）</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    BOOTPROTO=none           <span style="color: #31849b;"> #ip</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">获取方式</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    IPADDR=10.0.0.201        <span style="color: #31849b;"> #ip</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">地址</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    NETMASK=255.255.255.0    <span style="color: #31849b;"> #</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">掩码</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    GATEWAY=10.0.0.2         <span style="color: #31849b;"> #</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">网关</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    USERCTL=no
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    PEERDNS=yes              <span style="color: #31849b;"> #</span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">确认网卡配置文件中的</span><span style="color: #31849b;">DNS </span><span style="font-family: '微软雅黑',sans-serif; color: #31849b;">配置优先与</span><span style="color: #31849b;">/etc/resolv.conf</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    IPV6INIT=no      
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    DNS2=223.6.6.6           <span style="color: #31849b;"> #DNS2</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    DNS1=223.5.5.5           <span style="color: #31849b;"> #DNS1</span>
  </p>
</div>

### <span id="171">1.7.1 <span style="font-family: '微软雅黑',sans-serif;">网卡详细配置文件信息</span></span>

<span style="font-family: '微软雅黑',sans-serif;">更多网卡配置相关知识</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  less <span style="background: yellow;">/usr/share/doc/initscripts-*/sysconfig.txt </span>
</div>

## <span id="18">1.8 <span style="font-family: '微软雅黑',sans-serif;">网卡生效方式</span></span>

<span style="font-family: '微软雅黑',sans-serif;">推荐的网卡重启方式</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 0cm 4.0pt; background: #F2F2F2;">
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    <span style="background: darkcyan;">ifdown eth0 && ifup wth0</span>
  </p>
</div>

       <span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman";background: lime;">配置那块网卡，重启哪块网卡</span>

<span style="font-family: '微软雅黑',sans-serif;">对所有的网卡进行重启（<span style="color: red;">工作场景中谨慎使用</span>）</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <span style="color: red;">/etc/init.d/network restart</span>
</div>

**<span style="font-size: 16.0pt; font-family: '微软雅黑',sans-serif;">注意：</span>**

<p style="margin-left: 39.0pt; text-indent: -18.0pt;">
  <span style="font-family: 宋体;">① </span><span style="font-family: 宋体;">网卡如果配置DNS，会优先于/etc/resolv.conf 的配置生效，并且重启网卡，会把/etc/resolv.conf里的覆盖</span>
</p>

<p style="margin-left: 39.0pt; text-indent: -18.0pt;">
  <span style="font-family: 宋体;">② </span><span style="font-family: 宋体;">网卡如果没有配置DNS，那么在/etc/resolv.conf 里配置会生效</span>
</p>

<p style="margin-left: 39.0pt; text-indent: -18.0pt;">
  <span style="font-family: 宋体;">③ </span><span style="font-family: 宋体;">如果有多块网卡（DHCP 获取方式）时候，可能会覆盖/etc/resolv.conf 里已有的配置。</span>
</p>

## <span id="19">1.9 <span style="font-family: '微软雅黑',sans-serif;">主机名修改规范</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">第一步</span> <span style="font-family: '微软雅黑',sans-serif;">首先，利用命令临时修改主机名</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">hostname</span> clsn 
</div>

<p style="text-indent: 24.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">第二步</span> <span style="font-family: '微软雅黑',sans-serif;">其次，修改</span>network <span style="font-family: '微软雅黑',sans-serif;">配置文件，使主机名永久生效</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# cat <span style="background: lime;">/etc/sysconfig/network</span><br /> NETWORKING=yes<br /> HOSTNAME=clsn
</div>

       <span style="font-family: '微软雅黑',sans-serif;">第三步</span> <span style="font-family: '微软雅黑',sans-serif;">最后，修改</span>hosts<span style="font-family: '微软雅黑',sans-serif;">文件，建立主机名与环回</span>ip<span style="font-family: '微软雅黑',sans-serif;">的映射关系</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# cat <span style="background: lime;">/etc/hosts</span><br /> 127.0.0.1   localhost.localdomain   localhost.localdomain   localhost4    clsn<br /> ::1 localhost.localdomain   localhost.localdomain   localhost6    localhost6.localdomain6 localhost<br /> 127.0.0.1       clsn
</div>

## <span id="110_linux">1.10 <span style="background: yellow;">linux</span><span style="font-family: '微软雅黑',sans-serif; times new roman"4times new roman"; background: yellow;">下的路由配置</span></span>

### <span id="1101">1.10.1 <span style="font-family: '微软雅黑',sans-serif;">配置默认网关</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">添加：</span></strong><br /> [root@clsn ~]# <span style="background: lime;">route add default gw  10.0.0.254</span>   <br /> [root@clsn ~]# route -n<br /> Kernel IP routing table<br /> Destination     Gateway         Genmask         Flags Metric Ref    Use Iface<br /> 10.0.0.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0<br /> 169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0<br /> 0.0.0.0         10.0.0.254      0.0.0.0         UG    0      0        0 eth0<br /> 0.0.0.0         10.0.0.2        0.0.0.0         UG    0      0        0 eth0<br /> <strong><span style="font-family: '微软雅黑',sans-serif;">删除：</span></strong><br /> [root@clsn ~]# <span style="background: lime;">route del default gw  10.0.0.254</span><br /> [root@clsn ~]# route -n<br /> Kernel IP routing table<br /> Destination     Gateway         Genmask         Flags Metric Ref    Use Iface<br /> 10.0.0.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0<br /> 169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0<br /> 0.0.0.0         10.0.0.2        0.0.0.0         UG    0      0        0 eth0
</div>

### <span id="1102">1.10.2 <span style="font-family: '微软雅黑',sans-serif;">静态网段</span> <span style="font-family: '微软雅黑',sans-serif;">（临时的，重启网卡失效）</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">添加：</span></strong><br /> [root@clsn ~]# <span style="background: lime;">route add -net 192.168.0.0 netmask 255.255.0.0 gw 10.0.0.2</span><br /> [root@clsn ~]# route -n<br /> Kernel IP routing table<br /> Destination     Gateway         Genmask         Flags Metric Ref    Use Iface<br /> 10.0.0.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0<br /> 169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0<br /> 192.168.0.0     10.0.0.2        255.255.0.0     UG    0      0        0 eth0<br /> 0.0.0.0         10.0.0.2        0.0.0.0         UG    0      0        0 eth0<br /> <strong><span style="font-family: '微软雅黑',sans-serif;">删除：</span></strong><br /> [root@clsn ~]# <span style="background: lime;">route del -net 192.168.0.0 netmask 255.255.0.0 gw 10.0.0.2</span><br /> [root@clsn ~]# route -n<br /> Kernel IP routing table<br /> Destination     Gateway         Genmask         Flags Metric Ref    Use Iface<br /> 10.0.0.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0<br /> 169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0<br /> 0.0.0.0         10.0.0.2        0.0.0.0         UG    0      0        0 eth0
</div>

### <span id="1103">1.10.3 <span style="font-family: '微软雅黑',sans-serif;">永久生效的方法</span></span>

#### <span id="11031">1.10.3.1 <span style="font-family: '微软雅黑',sans-serif;">方法一</span> <span style="font-family: '微软雅黑',sans-serif;">生成配置文件，将路由信息写入。</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <span style="background: lime;">/etc/sysconfig/network-scipts/route-eth0</span><br /> 172.16.1.0/24 via 192.168.1.1
</div>

#### <span id="11032">1.10.3.2 <span style="font-family: '微软雅黑',sans-serif;">方法二</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <span style="background: lime;">/etc/sysconfig/static-routes</span><br /> any net 172.16.1.0/24  gw 192.168.1.1
</div>

#### <span id="11033">1.10.3.3 <span style="font-family: '微软雅黑',sans-serif;">方法三</span> <span style="font-family: '微软雅黑',sans-serif;">将命令写入开机自启动文件</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  /etc/rc.local
</div>

### <span id="1104">1.10.4 <span style="font-family: '微软雅黑',sans-serif;">默认静态主机路由添加</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  route add  -host  192.168.2.13 dev eth2<br /> route del  -host  192.168.2.13 dev eth2
</div>

## <span id="111_ip">1.11 <span style="font-family: '微软雅黑',sans-serif;">配置虚拟</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址方法</span></span>

### <span id="1111_ipip">1.11.1 <span style="font-family: '微软雅黑',sans-serif;">配置虚拟</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址（别名</span>ip<span style="font-family: '微软雅黑',sans-serif;">）</span></span>

<span style="font-family: '微软雅黑',sans-serif;">命令格式：</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <span style="background: lime;">ifconfig eth0:0 10.0.0.101/24 up</span>        <span style="font-family: '微软雅黑',sans-serif;">配置别名</span>ip<span style="font-family: '微软雅黑',sans-serif;">，并启用</span><br /> <span style="background: lime;">ifconfig eth0:0 10.0.0.101/24 down</span>      <span style="font-family: '微软雅黑',sans-serif;">停用制定的别名</span>IP
</div>

<span style="font-family: '微软雅黑',sans-serif;">示例：</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">ifconfig eth0:0 10.0.0.101/24 up</span><br /> [root@clsn ~]# ifconfig<br /> eth0      Link encap:Ethernet  HWaddr 00:0C:29:A8:E4:14 <br />           inet addr:10.0.0.201  Bcast:10.0.0.255  Mask:255.255.255.0<br />           inet6 addr: fe80::20c:29ff:fea8:e414/64 Scope:Link<br />           UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1<br />           RX packets:49465 errors:0 dropped:0 overruns:0 frame:0<br />           TX packets:45089 errors:0 dropped:0 overruns:0 carrier:0<br />           collisions:0 txqueuelen:1000<br />           RX bytes:20224063 (19.2 MiB)  TX bytes:7714321 (7.3 MiB)<br />  <br /> eth0:0    Link encap:Ethernet  HWaddr 00:0C:29:A8:E4:14 <br />           inet addr:10.0.0.101  Bcast:10.0.0.255  Mask:255.255.255.0<br />           UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1<br />  <br /> lo        Link encap:Local Loopback <br />           inet addr:127.0.0.1  Mask:255.0.0.0<br />           inet6 addr: ::1/128 Scope:Host<br />           UP LOOPBACK RUNNING  MTU:65536  Metric:1<br />           RX packets:38 errors:0 dropped:0 overruns:0 frame:0<br />           TX packets:38 errors:0 dropped:0 overruns:0 carrier:0<br />           collisions:0 txqueuelen:0<br />           RX bytes:2598 (2.5 KiB)  TX bytes:2598 (2.5 KiB)<br /> <span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">说明：</span><span style="color: #e36c0a;">heartbeat </span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">高可用（</span><span style="color: #e36c0a;">vIP</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">）早期用的别名</span><span style="color: #e36c0a;">IP,Centos6</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">下</span><span style="color: #e36c0a;">heatbeat</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">用的是辅助</span><span style="color: #e36c0a;">ip</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">。</span>
</div>

**<span style="font-family: '微软雅黑',sans-serif;">使别名</span>ip****<span style="font-family: '微软雅黑',sans-serif;">网卡重启不失效：</span>**

       <span style="font-family: '微软雅黑',sans-serif;">写成能配置文件（</span>/etc/sysconfig/network-scripts/ifcfg-eth0:1<span style="font-family: '微软雅黑',sans-serif;">）</span>

### <span id="1112_ip">1.11.2 <span style="font-family: '微软雅黑',sans-serif;">配置辅助</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址方法：</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">表示配置多个</span>ip<span style="font-family: '微软雅黑',sans-serif;">在同一网卡上。</span>
</p>

**<span style="font-family: '微软雅黑',sans-serif;">配置命令格式：</span>**

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  ip addr add 10.0.0.13/24 dev eth0:1  添加<br /> ip addr add 10.0.0.14/24 broadcast 10.0.0.255 dev eth0:1 添加<br /> ip addr del 10.0.0.13/24 dev eth0:1   删除<br /> ip addr del 10.0.0.14/24 broadcast 10.0.0.255 dev eth0:1  删除<br /> <span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">说明：辅助</span><span style="color: #e36c0a;">ip</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">形式在</span><span style="color: #e36c0a;">keepalived</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">高可用服务中一直使用，都是用的辅助</span><span style="color: #e36c0a;">ip</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">。</span><br /> <span style="color: #e36c0a;">      </span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">辅助</span><span style="color: #e36c0a;">ip</span><span style="font-family: '微软雅黑',sans-serif; color: #e36c0a;">的方式是未来趋势。</span>
</div>

 

**<span style="font-family: '微软雅黑',sans-serif;">示例：</span>**

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">ip addr add 10.0.0.102/24 dev eth0:1</span><br /> [root@clsn ~]# ip a<br /> 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN<br />     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00<br />     inet 127.0.0.1/8 scope host lo<br />     inet6 ::1/128 scope host<br />        valid_lft forever preferred_lft forever<br /> 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000<br />     link/ether 00:0c:29:a8:e4:14 brd ff:ff:ff:ff:ff:ff<br />     inet 10.0.0.201/24 brd 10.0.0.255 scope global eth0<br />     <span style="background: lime;">inet 10.0.0.102/24 scope global secondary eth0</span><br />     inet6 fe80::20c:29ff:fea8:e414/64 scope link<br />        valid_lft forever preferred_lft forever<br /> <strong><span style="font-size: 12.0pt; font-family: '微软雅黑',sans-serif;">删除：</span></strong><br /> [root@clsn ~]# ip addr del 10.0.0.102/24 dev eth0:1
</div>

## <span id="112">1.12 <span style="font-family: '微软雅黑',sans-serif;">【<span style="background: lime;">企业面试题</span>】</span></span>

### <span id="1121_ipESTABLISHEDip">1.12.1 <span style="font-family: '微软雅黑',sans-serif;">统计访问服务器</span>ip<span style="font-family: '微软雅黑',sans-serif;">的</span>ESTABLISHED<span style="font-family: '微软雅黑',sans-serif;">连接数最多的</span>ip<span style="font-family: '微软雅黑',sans-serif;">？</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <span style="font-family: '微软雅黑',sans-serif;">方法一：</span>[root@clsn ~]# <span style="color: #e36c0a;">netstat|awk -F "[ :]+" '/EST/{print $6}' |sort |uniq -c|sort -rn -k1</span><br />       1 10.0.0.1<br /> <span style="font-family: '微软雅黑',sans-serif;">方法二：</span>[root@clsn ~]# <span style="color: #e36c0a;">netstat|awk '/^tcp/{s[$NF]++}END{for(a in s)print a,s[a]}'|sort -rn -k1</span><br /> ESTABLISHED 1
</div>

### <span id="1122_22">1.12.2 <span style="font-family: '微软雅黑',sans-serif;">已知一个端口为</span>22<span style="font-family: '微软雅黑',sans-serif;">，如何参看端口对应的服务名？</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <span style="font-family: '微软雅黑',sans-serif;">方法一：</span><br /> [root@clsn ~]# <span style="background: lime;">lsof -i :22</span><br /> COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME<br /> sshd    1102 root    3u  IPv4  10308      0t0  TCP *:ssh (LISTEN)<br /> sshd    1102 root    4u  IPv6  10310      0t0  TCP *:ssh (LISTEN)<br /> <span style="font-family: '微软雅黑',sans-serif;">方法二：</span><br /> [root@clsn ~]# <span style="background: lime;">netstat -lntup|grep 22</span><br /> tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN      1102/sshd          <br /> tcp        0      0 :::22                       :::*                        LISTEN      1102/sshd   
</div>

## <span id="113_traceroute">1.13 <span style="font-family: '微软雅黑',sans-serif;">路由追踪</span> <span style="font-family: '微软雅黑',sans-serif;">【</span>traceroute <span style="font-family: '微软雅黑',sans-serif;">命令】</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@znix ~]# traceroute 8.8.8.8<br /> traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets<br />  1  * * *<br />  2  11.220.137.101 (11.220.137.101)  30.128 ms  30.408 ms 11.220.136.37 (11.220.136.37)  31.026 ms<br />  3  11.220.137.118 (11.220.137.118)  25.325 ms 11.220.137.122 (11.220.137.122)  25.928 ms 11.220.136.122 (11.220.136.122)  25.794 ms<br />  4  116.251.105.69 (116.251.105.69)  27.118 ms 106.11.130.190 (106.11.130.190)  28.008 ms 106.11.130.182 (106.11.130.182)  27.837 ms<br />  5  116.251.112.189 (116.251.112.189)  26.409 ms 101.200.109.134 (101.200.109.134)  39.593 ms 116.251.112.157 (116.251.112.157)  37.824 ms<br />  6  106.38.196.5 (106.38.196.5)  26.568 ms * *<br />  7  * * *<br />  8  * * *<br />  9  * * *<br /> 10  202.97.53.246 (202.97.53.246)  29.232 ms 202.97.58.98 (202.97.58.98)  27.856 ms 202.97.53.246 (202.97.53.246)  29.002 ms<br /> 11  202.97.91.114 (202.97.91.114)  66.359 ms  66.241 ms  65.974 ms<br /> 12  202.97.62.214 (202.97.62.214)  105.680 ms  105.781 ms  105.321 ms<br /> 13  108.170.241.5 (108.170.241.5)  62.860 ms  62.687 ms 108.170.241.38 (108.170.241.38)  63.391 ms<br /> 14  72.14.232.155 (72.14.232.155)  63.230 ms 64.233.174.17 (64.233.174.17)  62.013 ms 66.249.95.199 (66.249.95.199)  63.684 ms<br /> 15  216.239.46.119 (216.239.46.119)  86.082 ms 209.85.247.124 (209.85.247.124)  76.583 ms 209.85.142.172 (209.85.142.172)  74.886 ms<br /> 16  216.239.57.241 (216.239.57.241)  75.346 ms 209.85.249.75 (209.85.249.75)  74.639 ms 216.239.43.103 (216.239.43.103)  76.405 ms<br /> 17  * * *<br /> 18  * * *<br /> 19  * * *<br /> 20  * * *<br /> 21  * * *<br /> 22  * * *<br /> 23  * * *<br /> 24  * * *<br /> 25  google-public-dns-a.google.com (8.8.8.8)  77.106 ms  76.575 ms  73.135 ms
</div>

## <span id="114">1.14 <span style="font-family: '微软雅黑',sans-serif;">检测端口是否开启</span></span>

### <span id="1141_telnet">1.14.1 <span style="font-family: '微软雅黑',sans-serif;">利用</span>telnet<span style="font-family: '微软雅黑',sans-serif;">命令</span></span>

<div style="border: solid windowtext 2.0pt; padding: 0cm 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    [root@clsn ~]# <span style="background: lime;">telnet 10.0.0.201 22</span>
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    Trying 10.0.0.201...
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    Connected to 10.0.0.201.
  </p>
  
  <p style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; border: none; padding: 0cm;">
    SSH-2.0-OpenSSH_5.3
  </p>
</div>

### <span id="1142_nmap">1.14.2 <span style="font-family: '微软雅黑',sans-serif;">使用</span>nmap<span style="font-family: '微软雅黑',sans-serif;">命令</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  [root@clsn ~]# <span style="background: lime;">nmap 10.0.0.201 -p 20-23</span><br />  <br /> Starting Nmap 5.51 ( http://nmap.org ) at 2017-09-28 21:05 CST<br /> Nmap scan report for 10.0.0.201<br /> Host is up (0.00012s latency).<br /> PORT   STATE  SERVICE<br /> 20/tcp closed ftp-data<br /> 21/tcp closed ftp<br /> 22/tcp open   ssh<br /> 23/tcp closed telnet<br />  <br /> Nmap done: 1 IP address (1 host up) scanned in 0.54 seconds
</div>

# <span id="2">第2章 <span style="font-family: '微软雅黑',sans-serif;">上网无法</span></span>

 

### <span id="211">2.1.1 <span style="font-family: '微软雅黑',sans-serif;">局域网用户无法上网排查</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
   
</div>

### <span id="212">2.1.2 <span style="font-family: '微软雅黑',sans-serif;">网站访问慢的排查</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
   
</div>

# <span id="3_ftp">第3章 ftp<span style="font-family: '微软雅黑',sans-serif;">协议</span></span>

## <span id="31_FTP">3.1 FTP<span style="font-family: '微软雅黑',sans-serif;">协议介绍</span></span>

<p style="text-indent: 18.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">文件传输协议，监听的端口是</span>21/tcp<span style="font-family: '微软雅黑',sans-serif;">端口，是完成各主机之间文件共享的协议，将一个主机的服务共享给其它主机的</span>FTP<span style="font-family: '微软雅黑',sans-serif;">协议是工作于应用层的协议，是基于</span>TCP<span style="font-family: '微软雅黑',sans-serif;">协议来实现，数据的传输的可靠性；并且</span>TCP<span style="font-family: '微软雅黑',sans-serif;">是采用两个连接</span>FTP<span style="font-family: '微软雅黑',sans-serif;">协议是一种文本协议，支持</span>telnet<span style="font-family: '微软雅黑',sans-serif;">连接（文本协议都支持</span>telnet<span style="font-family: '微软雅黑',sans-serif;">的连接）</span>FTP<span style="font-family: '微软雅黑',sans-serif;">采用</span>CS<span style="font-family: '微软雅黑',sans-serif;">模式进行访问，拥有专用客户端，可以向服务器端发送大量请求的</span>ftp<span style="font-family: '微软雅黑',sans-serif;">相关的命令：</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  get  mget  put  mput  cd  ls
</div>

<span style="font-family: '微软雅黑',sans-serif;">一般控制连接：是监听在</span>tcp<span style="font-family: '微软雅黑',sans-serif;">协议的</span>21<span style="font-family: '微软雅黑',sans-serif;">端口</span>

<span style="font-family: '微软雅黑',sans-serif;">一般数据连接：监听的端口需要分两种情况而定</span>

### <span id="311">3.1.1 <span style="font-family: '微软雅黑',sans-serif;">主动模式</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">服务端会利用</span>20<span style="font-family: '微软雅黑',sans-serif;">号端口，主动向客户端发出数据传输请求，并先连</span>2002<span style="font-family: '微软雅黑',sans-serif;">端口，没有连</span>2003<span style="font-family: '微软雅黑',sans-serif;">依次类推但是客户端会拥有防火墙的概念，将服务端的请求屏蔽掉，因此产生了被动请求</span>
</p>

### <span id="312">3.1.2 <span style="font-family: '微软雅黑',sans-serif;">被动模式</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">服务器端会通知一个报文，报文中可以中可以有的信息是</span>151:20<span style="font-family: '微软雅黑',sans-serif;">，因此通过计算可以得知服务器端开放的随机端口是</span>151*256+20<span style="font-family: '微软雅黑',sans-serif;">端口，客户端会通过服务端告知的随机端口进行数据的传输但是由于服务器端是端口随机的，因此服务器端也可能会有防火墙的概念，为解决此问题，会对防火墙的一个重要功能进行了解</span>"<span style="font-family: '微软雅黑',sans-serif;">连接追踪功能</span>"---<span style="font-family: '微软雅黑',sans-serif;">就是防火墙进程会判断连接与连接之间的关系，防火墙内部请求出去响应回防火墙的就会允许，防火墙外部请求进来的连接就会阻止，根据追踪连接请求的特征，</span>tcp<span style="font-family: '微软雅黑',sans-serif;">一段发起的建立连接请求的状态可以成为</span>new<span style="font-family: '微软雅黑',sans-serif;">状态，响应请求后完成建立的状态成为</span>ESTB<span style="font-family: '微软雅黑',sans-serif;">状态。</span>
</p>

### <span id="313">3.1.3 <span style="font-family: '微软雅黑',sans-serif;">需要注意</span></span>

<span style="font-family: '微软雅黑',sans-serif;">所有数据传输的建立，都必须先建立控制连接，也就是输入相应的</span>FTP<span style="font-family: '微软雅黑',sans-serif;">命令，然后进行数据传输因此就产生了一个概念，</span>related<span style="font-family: '微软雅黑',sans-serif;">相关联的连接，防火墙会追踪连接彼此之间是否相关联，</span> <span style="font-family: '微软雅黑',sans-serif;">只要是相关联的连接，不管是什么端口，都进行放行</span>

### <span id="314">3.1.4 <span style="font-family: '微软雅黑',sans-serif;">补充说明</span></span>

<span style="font-family: '微软雅黑',sans-serif;">对用模式的主动和被动，都是根据服务端来进行说明的</span>tcp <span style="font-family: '微软雅黑',sans-serif;">端口号是</span>0-65535 udp <span style="font-family: '微软雅黑',sans-serif;">端口号是</span>0-65535 <span style="font-family: '微软雅黑',sans-serif;">所总计应该是</span>12W<span style="font-family: '微软雅黑',sans-serif;">多个端口防火墙是对端口进行控制，对请求的流量做限制，但是响应和流出的流量是不能进行过滤的数据传输模式：同时支持两种数据传输模式</span> <span style="font-family: '微软雅黑',sans-serif;">文本模式和二进制模式，并且数据传输遵循文件本身的格式，否则强行将文本文件按二进制传输会出现乱码的情况，因此建议服务端与客户端自行协商传输模式</span>.

### <span id="315">3.1.5 <span style="font-family: '微软雅黑',sans-serif;">说明</span></span>

<span style="font-family: '微软雅黑',sans-serif;">一般数据的存储形式分为</span> <span style="font-family: '微软雅黑',sans-serif;">文本和二进制文件的格式</span>

<span style="font-family: '微软雅黑',sans-serif;">网上传输的数据一般分为三种（结构化数据</span> <span style="font-family: '微软雅黑',sans-serif;">半结构化数据</span> <span style="font-family: '微软雅黑',sans-serif;">非结构化数据）</span>

 

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1">第1章 网络基础</a><ul>
        <li>
          <a href="#11_IP">1.1 IP地址分类</a>
        </li>
        <li>
          <a href="#12">1.2 局域网上网原理过程</a>
        </li>
        <li>
          <a href="#13_DHCP">1.3 DHCP服务原理</a><ul>
            <li>
              <a href="#131_DHCPIP">1.3.1 DHCP服务器IP分配方式</a>
            </li>
            <li>
              <a href="#132_DHCP">1.3.2 DHCP服务工作流程</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#14_DNS">1.4 DNS服务原理</a><ul>
            <li>
              <a href="#141_DNS">1.4.1 DNS能干什么</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#15">1.5 递归查询和迭代查询的区别</a><ul>
            <li>
              <a href="#151">1.5.1 递归查询</a>
            </li>
            <li>
              <a href="#152">1.5.2 迭代查询</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#16_DNS">1.6 DNS的相关命令</a><ul>
            <li>
              <a href="#161_dig">1.6.1 dig命令    </a>
            </li>
            <li>
              <a href="#162_nslookup">1.6.2 nslookup</a>
            </li>
            <li>
              <a href="#163_host">1.6.3 host命令</a>
            </li>
            <li>
              <a href="#164_ping">1.6.4 ping 命令</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#17">1.7 网卡信息</a><ul>
            <li>
              <a href="#171">1.7.1 网卡详细配置文件信息</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#18">1.8 网卡生效方式</a>
        </li>
        <li>
          <a href="#19">1.9 主机名修改规范</a>
        </li>
        <li>
          <a href="#110_linux">1.10 linux下的路由配置</a><ul>
            <li>
              <a href="#1101">1.10.1 配置默认网关</a>
            </li>
            <li>
              <a href="#1102">1.10.2 静态网段 （临时的，重启网卡失效）</a>
            </li>
            <li>
              <a href="#1103">1.10.3 永久生效的方法</a><ul>
                <li>
                  <a href="#11031">1.10.3.1 方法一 生成配置文件，将路由信息写入。</a>
                </li>
                <li>
                  <a href="#11032">1.10.3.2 方法二</a>
                </li>
                <li>
                  <a href="#11033">1.10.3.3 方法三 将命令写入开机自启动文件</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#1104">1.10.4 默认静态主机路由添加</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#111_ip">1.11 配置虚拟ip地址方法</a><ul>
            <li>
              <a href="#1111_ipip">1.11.1 配置虚拟ip地址（别名ip）</a>
            </li>
            <li>
              <a href="#1112_ip">1.11.2 配置辅助ip地址方法：</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#112">1.12 【企业面试题】</a><ul>
            <li>
              <a href="#1121_ipESTABLISHEDip">1.12.1 统计访问服务器ip的ESTABLISHED连接数最多的ip？</a>
            </li>
            <li>
              <a href="#1122_22">1.12.2 已知一个端口为22，如何参看端口对应的服务名？</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#113_traceroute">1.13 路由追踪 【traceroute 命令】</a>
        </li>
        <li>
          <a href="#114">1.14 检测端口是否开启</a><ul>
            <li>
              <a href="#1141_telnet">1.14.1 利用telnet命令</a>
            </li>
            <li>
              <a href="#1142_nmap">1.14.2 使用nmap命令</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2">第2章 上网无法</a><ul>
        <li>
          <ul>
            <li>
              <a href="#211">2.1.1 局域网用户无法上网排查</a>
            </li>
            <li>
              <a href="#212">2.1.2 网站访问慢的排查</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3_ftp">第3章 ftp协议</a><ul>
        <li>
          <a href="#31_FTP">3.1 FTP协议介绍</a><ul>
            <li>
              <a href="#311">3.1.1 主动模式</a>
            </li>
            <li>
              <a href="#312">3.1.2 被动模式</a>
            </li>
            <li>
              <a href="#313">3.1.3 需要注意</a>
            </li>
            <li>
              <a href="#314">3.1.4 补充说明</a>
            </li>
            <li>
              <a href="#315">3.1.5 说明</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</div>