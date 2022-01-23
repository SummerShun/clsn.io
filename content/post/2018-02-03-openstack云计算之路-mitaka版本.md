---
title: OpenStack云计算之路-Mitaka 版本
author: 惨绿少年
type: post
date: 2018-02-02T20:49:00+00:00
url: /clsn/lx132.html
Baidusubmit:
  - 1
views:
  - 397
specs_zan:
  - 1
zm_favorites:
  - -1
categories:
  - Linux运维
  - OpenStack
  - 云计算

---
## <span id="11">1.1 <span style="font-family: '微软雅黑',sans-serif;">云计算简介</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">云计算（英语：</span><span style="text-decoration: underline;">cloud computing</span> <span style="font-family: '微软雅黑',sans-serif;">），是一种基于互联网的计算方式，通过这种方式，共享的软硬件资源和信息可以按需求提供给计算机各种终端和其他设备。</span>
</p>

<p style="text-align: center; text-indent: 21.0pt;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127174019272-614459598.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">云计算是继</span>1980<span style="font-family: '微软雅黑',sans-serif;">年代大型计算机到客户端</span>-<span style="font-family: '微软雅黑',sans-serif;">服务器的大转变之后的又一种巨变。用户不再需要了解“云”中基础设施的细节，不必具有相应的专业知识，也无需直接进行控制。云计算描述了一种基于互联网的新的</span>IT<span style="font-family: '微软雅黑',sans-serif;">服务增加、使用和交付模式，通常涉及通过互联网来提供动态易扩展而且经常是虚拟化的资源。</span>
</p>

### <span id="111">1.1.1 <span style="font-family: '微软雅黑',sans-serif;">云计算的特点</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">互联网上的云计算服务特征和自然界的<span style="text-decoration: underline;">云、水循环</span>具有一定的相似性，因此，云是一个相当贴切的比喻。根据技术研究院的定义如下。</span>
</p>

<p style="text-indent: 21.0pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">云计算服务应该具备以下几条特征：</span></strong>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">随需应变自助服务。</span>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">随时随地用任何网络设备访问。</span>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">多人共享资源池。</span>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">快速重新部署灵活度。</span>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">可被监控与量测的服务。</span>
</p>

<p style="text-indent: 21.0pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">一般认为还有如下特征：</span></strong>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">基于虚拟化技术快速部署资源或获得服务。</span>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">减少用户终端的处理负担。</span>
</p>

<p style="margin-left: 33.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">降低了用户对于</span>IT<span style="font-family: '微软雅黑',sans-serif;">专业知识的依赖。</span>
</p>

### <span id="112">1.1.2 <span style="font-family: '微软雅黑',sans-serif;">云计算服务模式</span></span>

<span style="font-family: '微软雅黑',sans-serif;">云计算定义中明确了三种服务模式：</span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127174029490-1843732919.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-align: center;" align="center">
  <span style="font-family: '微软雅黑',sans-serif;">图</span> - <span style="font-family: '微软雅黑',sans-serif;">服务模式详情</span>
</p>

**<span style="font-family: '微软雅黑',sans-serif;">软件即服务（</span><span style="background: yellow;">SaaS</span>****<span style="font-family: '微软雅黑',sans-serif;">）：</span>**

**     ** <span style="font-family: '微软雅黑',sans-serif;">即</span>Software-as-a-service<span style="font-family: '微软雅黑',sans-serif;">；</span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">消费者使用应用程序，但并不掌控操作系统、硬件或运作的网络基础架构。是一种服务观念的基础，软件服务供应商，以租赁的概念提供客户服务，而非购买，比较常见的模式是提供一组账号密码。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">例如：</span>Microsoft CRM<span style="font-family: '微软雅黑',sans-serif;">与</span>Salesforce.com<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

**<span style="font-family: '微软雅黑',sans-serif;">平台即服务（</span><span style="background: yellow;">PaaS</span>****<span style="font-family: '微软雅黑',sans-serif;">）：</span>**

**     ** <span style="font-family: '微软雅黑',sans-serif;">即</span>Platform-as-a-service<span style="font-family: '微软雅黑',sans-serif;">；</span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">消费者使用主机操作应用程序。消费者掌控运作应用程序的环境（也拥有主机部分掌控权），但并不掌控操作系统、硬件或运作的网络基础架构。平台通常是应用程序基础架构。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">例如：</span>Google App Engine<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

**<span style="font-family: '微软雅黑',sans-serif;">基础设施即服务（</span><span style="background: yellow;">IaaS</span>****<span style="font-family: '微软雅黑',sans-serif;">）：</span>**

**     ** <span style="font-family: '微软雅黑',sans-serif;">即</span>Infrastructure-as-a-service<span style="font-family: '微软雅黑',sans-serif;">；</span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">消费者使用“基础计算资源”，如处理能力、存储空间、网络组件或中间件。消费者能掌控操作系统、存储空间、已部署的应用程序及网络组件（如防火墙、负载平衡器等），但并不掌控云基础架构。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">例如：</span>Amazon AWS<span style="font-family: '微软雅黑',sans-serif;">、</span>Rackspace<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<p style="text-align: right;" align="right">
  <span style="font-family: '微软雅黑',sans-serif;">关于这三种服务模式更多详情可以参考：</span><a href="https://www.zhihu.com/question/21641778">https://www.zhihu.com/question/21641778</a>
</p>

### <span id="113">1.1.3 <span style="font-family: '微软雅黑',sans-serif;">云计算的类型</span></span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127174039553-242648718.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-align: center;" align="center">
  <span style="font-family: '微软雅黑',sans-serif;">图</span> - <span style="font-family: '微软雅黑',sans-serif;">云类型示例</span>
</p>

**<span style="font-family: 'Segoe UI Emoji',sans-serif;">? </span>****<span style="font-family: '微软雅黑',sans-serif;">公有云（</span>Public Cloud****<span style="font-family: '微软雅黑',sans-serif;">）</span>**

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">简而言之，公用云服务可通过网络及第三方服务供应者，开放给客户使用，“<span style="text-decoration: underline;">公有”一词并不一定代表“免费”</span>，但也可能代表免费或相当廉价，公用云并不表示用户数据可供任何人查看，公用云供应者通常会对用户实施使用访问控制机制，公用云作为解决方案，既有弹性，又具备成本效益。</span>
</p>

**<span style="font-family: 'Segoe UI Emoji',sans-serif;">? </span>****<span style="font-family: '微软雅黑',sans-serif;">私有云（</span>Private Cloud****<span style="font-family: '微软雅黑',sans-serif;">）</span>**

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">私有云具备许多公用云环境的优点，例如弹性、适合提供服务，两者差别在于私有云服务中，数据与程序皆在组织内管理，且与公用云服务不同，不会受到网络带宽、安全疑虑、法规限制影响；此外，私有云服务让供应者及用户更能掌控云基础架构、改善安全与弹性，因为用户与网络都受到特殊限制。</span>
</p>

**<span style="font-family: 'Segoe UI Emoji',sans-serif;">? </span>****<span style="font-family: '微软雅黑',sans-serif;">混合云（</span>Hybrid Cloud****<span style="font-family: '微软雅黑',sans-serif;">）</span>**

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">混合云结合公用云及私有云，这个模式中，用户通常将非企业关键信息外包，并在公用云上处理，但同时掌控企业关键服务及数据。</span>
</p>

### <span id="114">1.1.4 <span style="font-family: '微软雅黑',sans-serif;">为什么要选择云计算</span></span>

<p style="margin-left: 12.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">、有效解决硬件单点故障问题</span>
</p>

<p style="margin-left: 12.0pt;">
  2<span style="font-family: '微软雅黑',sans-serif;">、按需增</span>/<span style="font-family: '微软雅黑',sans-serif;">减硬件资源</span>
</p>

<p style="margin-left: 12.0pt;">
  3<span style="font-family: '微软雅黑',sans-serif;">、</span>BGP<span style="font-family: '微软雅黑',sans-serif;">线路解决南北互通问题</span>
</p>

<p style="margin-left: 12.0pt;">
  4<span style="font-family: '微软雅黑',sans-serif;">、按需增</span>/<span style="font-family: '微软雅黑',sans-serif;">减带宽</span>
</p>

<p style="margin-left: 12.0pt;">
  5<span style="font-family: '微软雅黑',sans-serif;">、更有吸引力的费用支付方式</span>
</p>

<p style="text-align: right; text-indent: 7.1pt;" align="right">
  <span style="font-family: '微软雅黑',sans-serif;">详情查看《云计算之路：为什么要选择云计算》</span>
</p>

<p style="text-align: right; text-indent: 7.1pt;" align="right">
  <a href="https://www.cnblogs.com/cmt/archive/2013/02/27/why-into-cloud.html">https://www.cnblogs.com/cmt/archive/2013/02/27/why-into-cloud.html</a>
</p>

## <span id="12_OpenStack">1.2 OpenStack<span style="font-family: '微软雅黑',sans-serif;">简介</span></span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127174053522-857175765.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  OpenStack<span style="font-family: '微软雅黑',sans-serif;">是一个美国宇航局和</span>Rackspace<span style="font-family: '微软雅黑',sans-serif;">合作研发的云计算软件，以</span><span style="text-decoration: underline;">Apache</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">授权条款</span>2.0</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">授权</span></span><span style="font-family: '微软雅黑',sans-serif;">，并且是一个自由软件和开放源代码项目。</span>
</p>

<p style="text-indent: 21.0pt;">
  OpenStack<span style="font-family: '微软雅黑',sans-serif;">是基础设施即服务（</span>IaaS<span style="font-family: '微软雅黑',sans-serif;">）软件，让任何人都可以自行创建和提供云计算服务。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">此外，</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">也用作创建防火墙内的“<span style="text-decoration: underline;">私有云</span>”（</span>Private Cloud<span style="font-family: '微软雅黑',sans-serif;">），提供机构或企业内各部门共享资源。</span>
</p>

### <span id="121">1.2.1 <span style="font-family: '微软雅黑',sans-serif;">市场趋向</span></span>

<p style="text-indent: 21.0pt;">
  Rackspace<span style="font-family: '微软雅黑',sans-serif;">以</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">为基础的私有云业务每年营收</span>7<span style="font-family: '微软雅黑',sans-serif;">亿美元，增长率超过了</span>20%<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<p style="text-indent: 21.0pt;">
  OpenStack<span style="font-family: '微软雅黑',sans-serif;">虽然有些方面还不太成熟，然而它有全球大量的组织支持，大量的开发人员参与，发展迅速。国际上已经有很多使用</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">搭建的公有云、私有云、混合云，例如：</span>RackspaceCloud<span style="font-family: '微软雅黑',sans-serif;">、惠普云、</span>MercadoLibre<span style="font-family: '微软雅黑',sans-serif;">的</span>IT<span style="font-family: '微软雅黑',sans-serif;">基础设施云、</span>AT&T<span style="font-family: '微软雅黑',sans-serif;">的</span>CloudArchitec<span style="font-family: '微软雅黑',sans-serif;">、戴尔的</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">解决方案等等。而在国内</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">的热度也在逐渐升温，<span style="text-decoration: underline;">华胜天成、高德地图、京东、阿里巴巴、百度、中兴、华为</span>等都对</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">产生了浓厚的兴趣并参与其中。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">自</span>2010<span style="font-family: '微软雅黑',sans-serif;">年创立以来，已发布</span>10<span style="font-family: '微软雅黑',sans-serif;">个版本。其中</span>Icehouse<span style="font-family: '微软雅黑',sans-serif;">版本有</span>120<span style="font-family: '微软雅黑',sans-serif;">个组织、</span>1202<span style="font-family: '微软雅黑',sans-serif;">名代码贡献者参与，而最新的是</span>Juno<span style="font-family: '微软雅黑',sans-serif;">版本。</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">很可能在未来的基础设施即服务（</span>IaaS<span style="font-family: '微软雅黑',sans-serif;">）资源管理方面占据领导位置，成为公有云、私有云及混合云管理的“云操作系统”标准</span>
</p>

### <span id="122">1.2.2 <span style="font-family: '微软雅黑',sans-serif;">大型用户</span></span>

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">美国国家航空航天局</span></span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">加拿大半官方机构</span><span style="text-decoration: underline;">CANARIE</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">网络</span></span><span style="font-family: '微软雅黑',sans-serif;">的</span>DAIR<span style="font-family: '微软雅黑',sans-serif;">（</span>Digital Accelerator for Innovation and Research<span style="font-family: '微软雅黑',sans-serif;">）项目，向大学与中小型企业提供研究和开发云端运算环境。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">惠普云</span></span><span style="font-family: '微软雅黑',sans-serif;">（使用</span>Ubuntu Linux<span style="font-family: '微软雅黑',sans-serif;">）</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;">MercadoLibre</span><span style="font-family: '微软雅黑',sans-serif;">的</span>IT<span style="font-family: '微软雅黑',sans-serif;">基础设施云，现时以</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">管理超过</span>6000 <span style="font-family: '微软雅黑',sans-serif;">台虚拟机器。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;">AT&T</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">的“</span>Cloud Architect</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">”</span></span><span style="font-family: '微软雅黑',sans-serif;">，将在美国的达拉斯、圣地亚哥和新泽西州提供对外云端服务。</span>
</p>

### <span id="123_OpenStack">1.2.3 OpenStack<span style="font-family: '微软雅黑',sans-serif;">项目介绍</span></span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127174109444-562856729.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-align: center;" align="center">
  <span style="font-family: '微软雅黑',sans-serif;">图</span> - <span style="font-family: '微软雅黑',sans-serif;">各项目关系图</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">各组件的详细说明：</span>
</p>

<table style="width: 100%; border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 23%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">服务类型</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: solid #9BBB59 1.0pt; border-left: none; border-bottom: solid #9BBB59 1.0pt; border-right: none; background: #9BBB59; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">项目名称</span></strong>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="59%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">描述</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Dashboard</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Horizon
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">提供</span>web<span style="font-family: '微软雅黑',sans-serif;">界面</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">提供了一个基于</span>web<span style="font-family: '微软雅黑',sans-serif;">的自服务门户，与</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">底层服务交互，诸如启动一个实例，分配</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址以及配置访问控制。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Compute</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Nova
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">计算节点</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">在</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">环境中计算实例的生命周期管理。按需响应包括生成、调度、回收虚拟机等操作。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Networking</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Neutron
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">网络服务</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">确保为其它</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">服务提供网络连接即服务，比如</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">计算。为用户提供</span>API<span style="font-family: '微软雅黑',sans-serif;">定义网络和使用。基于插件的架构其支持众多的网络提供商和技术。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 100%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #548dd4; padding: 0cm 5.4pt;" colspan="3" width="100%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">存储</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Object Storage</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Swift
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">对象存储</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">通过一个</span> RESTful,<span style="font-family: '微软雅黑',sans-serif;">基于</span>HTTP<span style="font-family: '微软雅黑',sans-serif;">的应用程序接口存储和任意检索的非结构化数据对象。它拥有高容错机制，基于数据复制和可扩展架构。它的实现并像是一个文件服务器需要挂载目录。在此种方式下，它写入对象和文件到多个硬盘中，以确保数据是在集群内跨服务器的多份复制。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Block Storage</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Cinder
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">块存储</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">为运行实例而提供的持久性块存储。它的可插拔驱动架构的功能有助于创建和管理块存储设备。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 100%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #d99594; padding: 0cm 5.4pt;" colspan="3" width="100%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">共享服务</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Identity service</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Keystone
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">认证节点</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">为其他</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">服务提供认证和授权服务，为所有的</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">服务提供一个端点目录。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Image service</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Glance
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">镜像服务</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">存储和检索虚拟机磁盘镜像，</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">计算会在实例部署时使用此服务。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Telemetry</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Ceilometer
      </p>
      
      <p style="text-align: center;" align="center">
        <span style="font-family: '微软雅黑',sans-serif;">计费</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">为</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">云的计费、基准、扩展性以及统计等目的提供监测和计量。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 100%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="3" width="100%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">高层次服务</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>Orchestration</strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="17%">
      <p style="text-align: center;" align="center">
        Heat
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Orchestration<span style="font-family: '微软雅黑',sans-serif;">服务支持多样化的综合的云应用，通过调用</span>OpenStack-native REST API<span style="font-family: '微软雅黑',sans-serif;">和</span>CloudFormation-compatible Query API<span style="font-family: '微软雅黑',sans-serif;">，支持</span>:term:`HOT `<span style="font-family: '微软雅黑',sans-serif;">格式模板或者</span>AWS CloudFormation<span style="font-family: '微软雅黑',sans-serif;">格式模板</span>
      </p>
    </td>
  </tr>
</table>

### <span id="124">1.2.4 <span style="font-family: '微软雅黑',sans-serif;">系统环境说明</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">本文档使用主机环境均安装官方推荐进行设置：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment.html</a>
</p>

**controller****<span style="font-family: '微软雅黑',sans-serif;">节点说明</span>**

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/redhat-release </span>
CentOS Linux release 7.2.1511<span style="color: #000000;"> (Core) 
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r</span>
3.10.0-327<span style="color: #000000;">.el7.x86_64
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> sestatus </span>
<span style="color: #000000;">SELinux status:                 disabled
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status firewalld.service </span>
● firewalld.service - firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> hostname -I</span>
10.0.0.11 172.16.1.11<span style="color: #000000;"> 
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> tail -3  /etc/hosts</span>
10.0.0.11<span style="color: #000000;">   controller
</span>10.0.0.31<span style="color: #000000;">   compute1
</span>10.0.0.32   compute2</pre>
</div>

compute1<span style="font-family: '微软雅黑',sans-serif;">与</span>compute2<span style="font-family: '微软雅黑',sans-serif;">节点的配置与</span>controller<span style="font-family: '微软雅黑',sans-serif;">相同。</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">系统安装参考文档：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/8338099.html#_label1" >http://www.cnblogs.com/clsn/p/8338099.html#_label1</a>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">系统优化说明：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/8338099.html#_label4" >http://www.cnblogs.com/clsn/p/8338099.html#_label4</a>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意点：网卡的名称修改</span>
</p>

## <span id="13_OpenStack">1.3 OpenStack<span style="font-family: '微软雅黑',sans-serif;">基础配置服务</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注：本文中所使用的用户及密码都参考该文档，并且高度一致。</span>
</p>

<p style="margin-left: 21.0pt;">
  <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-security.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-security.html</a>
</p>

<p style="margin-left: 21.0pt;">
  OpenStack <span style="font-family: '微软雅黑',sans-serif;">相关服务安装流程（</span>keystone<span style="font-family: '微软雅黑',sans-serif;">服务除外）：</span>
</p>

<p style="margin-left: 45.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）在数据库中，创库，授权；</span>
</p>

<p style="margin-left: 45.0pt;">
  2<span style="font-family: '微软雅黑',sans-serif;">）在</span>keystone<span style="font-family: '微软雅黑',sans-serif;">中创建用户并授权；</span>
</p>

<p style="margin-left: 45.0pt;">
  3<span style="font-family: '微软雅黑',sans-serif;">）在</span>keystone<span style="font-family: '微软雅黑',sans-serif;">中创建服务实体，和注册</span>API<span style="font-family: '微软雅黑',sans-serif;">接口；</span>
</p>

<p style="margin-left: 45.0pt;">
  4<span style="font-family: '微软雅黑',sans-serif;">）安装软件包；</span>
</p>

<p style="margin-left: 45.0pt;">
  5<span style="font-family: '微软雅黑',sans-serif;">）修改配置文件（数据库信息）；</span>
</p>

<p style="margin-left: 45.0pt;">
  6<span style="font-family: '微软雅黑',sans-serif;">）同步数据库；</span>
</p>

<p style="margin-left: 45.0pt;">
  7<span style="font-family: '微软雅黑',sans-serif;">）启动服务。</span>
</p>

### <span id="131_OpenStack">1.3.1 OpenStack<span style="font-family: '微软雅黑',sans-serif;">服务部署顺序</span></span>

<div>
  <blockquote>
    <div>
      <p class="a">
        [1]  基础环境准备 <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment.html</a>
      </p>
      
      <p class="a">
        [2]  部署 Keystorne 认证服务,token <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone.html</a>
      </p>
      
      <p class="a">
        [3]  部署 Glance 镜像服务 <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance.html</a>
      </p>
      
      <p class="a">
        [4]  部署 Nova 计算服务(kvm) <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova.html</a>
      </p>
      
      <p class="a">
        [5]  部署 Neutron 网络服务 <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron.html</a>
      </p>
      
      <p class="a">
        [6]  部署 Horizon 提供web界面 <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/horizon.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/horizon.html</a>
      </p>
      
      <p class="a">
        [7]  部署 Cinder 块存储(硬盘) <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/horizon.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/horizon.html</a>
      </p>
    </div>
  </blockquote>
</div>

### <span id="132_yum">1.3.2 <span style="font-family: '微软雅黑',sans-serif;">配置本地</span>yum<span style="font-family: '微软雅黑',sans-serif;">源</span></span>

<span style="font-family: '微软雅黑',sans-serif;">首先将镜像挂载到</span> /mnt

<div class="cnblogs_code">
  <pre>mount /dev/cdrom /<span style="color: #000000;">mnt
echo </span><span style="color: #800000;">'</span><span style="color: #800000;">mount /dev/cdrom /mnt</span><span style="color: #800000;">'</span> &gt; /etc/rc.d/<span style="color: #000000;">rc.local
chmod </span>+x  /etc/rc.d/rc.local</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">创建</span>repo<span style="font-family: '微软雅黑',sans-serif;">文件</span>

<div class="cnblogs_code">
  <pre>cat &gt;/etc/yum.repos.d/local.repo&lt;&lt;-<span style="color: #800000;">'</span><span style="color: #800000;">EOF</span><span style="color: #800000;">'</span><span style="color: #000000;">
[local]
name</span>=<span style="color: #000000;">local
baseurl</span>=file:///<span style="color: #000000;">mnt
gpgcheck</span>=<span style="color: #000000;">0

[openstack]
name</span>=openstack-<span style="color: #000000;">mitaka
baseurl</span>=file:///opt/<span style="color: #000000;">repo
gpgcheck</span>=<span style="color: #000000;">0
EOF</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">生成</span>yum<span style="font-family: '微软雅黑',sans-serif;">缓存</span>

<div class="cnblogs_code">
  <pre>[root@controller repo]<span style="color: #008000;">#</span><span style="color: #008000;"> yum makecache</span></pre>
</div>

### <span id="133_NTP">1.3.3 <span style="font-family: '微软雅黑',sans-serif;">安装</span>NTP<span style="font-family: '微软雅黑',sans-serif;">时间服务</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-ntp.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-ntp.html</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">控制节点（提供时间服务，供其他机器同步）</span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">安装软件</span>
</p>

<div class="cnblogs_code">
  <pre>yum install chrony -y</pre>
</div>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置控制节点，修改第</span>22<span style="font-family: '微软雅黑',sans-serif;">行</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/chrony.conf </span>
<span style="color: #000000;">···
</span><span style="color: #008000;">#</span><span style="color: #008000;"> Allow NTP client access from local network.</span>
allow 10/8</pre>
</div>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">启动，设置自启动</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl enable chronyd.service
systemctl start chronyd.service</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">计算节点（配置</span><span style="background: lime;">chrony</span><span style="font-family: '微软雅黑',sans-serif;">客户端）</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">安装软件</span>
</p>

<div class="cnblogs_code">
  <pre>yum install chrony -y</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置文件第三行，删除无用的上游服务器。</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">使用</span>sed<span style="font-family: '微软雅黑',sans-serif;">命令修改</span>
</p>

<div class="cnblogs_code">
  <pre>sed -ri.bak <span style="color: #800000;">'</span><span style="color: #800000;">/server/s/^/#/g;2a server 10.0.0.11 iburst</span><span style="color: #800000;">'</span> /etc/chrony.conf</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置文件说明：</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/chrony.conf </span><span style="color: #008000;">
#</span><span style="color: #008000;"> Use public servers from the pool.ntp.org project.</span><span style="color: #008000;">
#</span><span style="color: #008000;"> Please consider joining the pool (http://www.pool.ntp.org/join.html).</span>
server 10.0.0.11 iburst</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">启动，设置自启动</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl enable chronyd.service
systemctl start chronyd.service</span></pre>
</div>

### <span id="134_OpenStack">1.3.4 OpenStack<span style="font-family: '微软雅黑',sans-serif;">的包操作（添加新的计算节点时需要安装）</span></span>

<span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-packages.html>

<span style="font-family: '微软雅黑',sans-serif;">安装</span> OpenStack <span style="font-family: '微软雅黑',sans-serif;">客户端：</span>

<div class="cnblogs_code">
  <pre>yum -y install python-openstackclient</pre>
</div>

RHEL <span style="font-family: '微软雅黑',sans-serif;">和</span> CentOS <span style="font-family: '微软雅黑',sans-serif;">默认启用了</span> SELinux

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 安装 openstack-selinux 软件包以便自动管理 OpenStack 服务的安全策略</span>
    yum -y install openstack-selinux</pre>
</div>

### <span id="135_SQL">1.3.5 SQL<span style="font-family: '微软雅黑',sans-serif;">数据库安装（在控制节点操作）</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-sql-database.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-sql-database.html</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">安装</span>mariadb<span style="font-family: '微软雅黑',sans-serif;">软件包：</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum -y install mariadb mariadb-server python2-PyMySQL</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">创建配置文件</span>

<div class="cnblogs_code">
  <pre>cat &gt; /etc/my.cnf.d/openstack.cnf &lt;&lt;-<span style="color: #800000;">'</span><span style="color: #800000;">EOF</span><span style="color: #800000;">'</span><span style="color: #000000;">
[mysqld]
bind</span>-address = 10.0.0.11<span style="color: #000000;">
default</span>-storage-engine =<span style="color: #000000;"> innodb
innodb_file_per_table
max_connections </span>= 4096<span style="color: #000000;">
collation</span>-server =<span style="color: #000000;"> utf8_general_ci
character</span>-set-server =<span style="color: #000000;"> utf8
EOF</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动</span>mariadb

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl enable mariadb.service
systemctl start mariadb.service</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">执行</span>mariadb<span style="font-family: '微软雅黑',sans-serif;">安全初始化</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">为了保证数据库服务的安全性，运行</span>``mysql_secure_installation``<span style="font-family: '微软雅黑',sans-serif;">脚本。特别需要说明的是，为数据库的</span>root<span style="font-family: '微软雅黑',sans-serif;">用户设置一个适当的密码。</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql_secure_installation</span>
<span style="color: #000000;">···
Enter current password </span><span style="color: #0000ff;">for</span> root (enter <span style="color: #0000ff;">for</span><span style="color: #000000;"> none): 
OK, successfully used password, moving on...
Set root password? [Y</span>/<span style="color: #000000;">n]<span style="color: #ff9900;"> n</span>
 ... skipping.
Remove anonymous users? [Y</span>/<span style="color: #000000;">n]<span style="color: #ff9900;"> Y</span>
 ... Success!
Disallow root login remotely? [Y</span>/<span style="color: #000000;">n] <span style="color: #ff9900;">Y</span>
 ... Success!
Remove test database </span><span style="color: #0000ff;">and</span> access to it? [Y/<span style="color: #000000;">n]<span style="color: #ff9900;"> Y
 </span></span>-<span style="color: #000000;"> Dropping test database...
 ... Success!
 </span>-<span style="color: #000000;"> Removing privileges on test database...
 ... Success!
Reload privilege tables now? [Y</span>/<span style="color: #000000;">n]<span style="color: #ff9900;"> Y</span>
 ... Success!

Thanks </span><span style="color: #0000ff;">for</span> using MariaDB!<span style="text-indent: 21pt;"> </span></pre>
</div>

### <span id="136_NoSQL">1.3.6 NoSQL <span style="font-family: '微软雅黑',sans-serif;">数据库</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-nosql-database.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-nosql-database.html</a>
</p>

<p style="text-indent: 21.0pt;">
  Telemetry <span style="font-family: '微软雅黑',sans-serif;">服务使用</span> NoSQL <span style="font-family: '微软雅黑',sans-serif;">数据库来存储信息，典型地，这个数据库运行在控制节点上。</span>
</p>

<span style="font-family: '微软雅黑',sans-serif;">向导中使用</span>MongoDB<span style="font-family: '微软雅黑',sans-serif;">。</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>ceilometer<span style="font-family: '微软雅黑',sans-serif;">中计费使用。由于本次搭建的为私有云平台，私有云不需要计费服务，这里就不进行安装了。</span>
</p>

### <span id="137">1.3.7 <span style="font-family: '微软雅黑',sans-serif;">消息队列部署</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-messaging.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-messaging.html</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">安装消息队列软件</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum -y install rabbitmq-server</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动消息队列服务并将其配置为随系统启动：</span>

<div class="cnblogs_code">
  <pre>systemctl enable rabbitmq-<span style="color: #000000;">server.service
systemctl start rabbitmq</span>-server.service</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">添加</span> openstack <span style="font-family: '微软雅黑',sans-serif;">用户：</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rabbitmqctl add_user openstack RABBIT_PASS</span>
Creating user <span style="color: #800000;">"</span><span style="color: #800000;">openstack</span><span style="color: #800000;">"</span><span style="color: #000000;"> ...
用合适的密码替换 RABBIT_DBPASS。</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">给</span>\`\`openstack\`\`<span style="font-family: '微软雅黑',sans-serif;">用户配置写和读权限：</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rabbitmqctl set_permissions openstack ".*" ".*" ".*"</span>
Setting permissions <span style="color: #0000ff;">for</span> user <span style="color: #800000;">"</span><span style="color: #800000;">openstack</span><span style="color: #800000;">"</span> <span style="color: #0000ff;">in</span> vhost <span style="color: #800000;">"</span><span style="color: #800000;">/</span><span style="color: #800000;">"</span> ...</pre>
</div>

### <span id="138_Memcached">1.3.8 Memcached<span style="font-family: '微软雅黑',sans-serif;">服务部署</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-memcached.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/environment-memcached.html</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">安装</span>memcached<span style="font-family: '微软雅黑',sans-serif;">软件包</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum -y install memcached python-memcached</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置</span>memcached<span style="font-family: '微软雅黑',sans-serif;">配置文件</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  /etc/sysconfig/memcached</span>
PORT=<span style="color: #800000;">"</span><span style="color: #800000;">11211</span><span style="color: #800000;">"</span><span style="color: #000000;">
USER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">memcached</span><span style="color: #800000;">"</span><span style="color: #000000;">
MAXCONN</span>=<span style="color: #800000;">"</span><span style="color: #800000;">1024</span><span style="color: #800000;">"</span><span style="color: #000000;">
CACHESIZE</span>=<span style="color: #800000;">"</span><span style="color: #800000;">64</span><span style="color: #800000;">"</span><span style="color: #000000;">
OPTIONS</span>=<span style="color: #800000;">"</span><span style="color: #800000;">-l 10.0.0.11</span><span style="color: #800000;">"</span>  <span style="color: #ff9900;">&lt;--修改位置，配置为memcached主机地址或网段信息</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动</span>Memcached<span style="font-family: '微软雅黑',sans-serif;">服务，并且配置它随机启动。</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl enable memcached.service
systemctl start memcached.service</span></pre>
</div>

### <span id="139">1.3.9 <span style="font-family: '微软雅黑',sans-serif;">验证以上部署的服务是否正常</span></span>

<span style="font-family: '微软雅黑',sans-serif;">查看端口信息</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> netstat -lntup</span>
<span style="color: #000000;">Active Internet connections (only servers)
Proto Recv</span>-Q Send-Q Local Address           Foreign Address         State       PID/<span style="color: #000000;">Program name    
tcp        0      0 </span>0.0.0.0:25672           0.0.0.0:*               LISTEN      17164/<span style="color: #000000;">beam          
tcp        0      0 </span>10.0.0.11:3306          0.0.0.0:*               LISTEN      16985/<span style="color: #000000;">mysqld        
tcp        0      0 </span>10.0.0.11:11211         0.0.0.0:*               LISTEN      17962/<span style="color: #000000;">memcached     
tcp        0      0 </span>0.0.0.0:4369            0.0.0.0:*               LISTEN      1/<span style="color: #000000;">systemd           
tcp        0      0 </span>0.0.0.0:22              0.0.0.0:*               LISTEN      1402/<span style="color: #000000;">sshd           
tcp6       0      0 :::</span>5672                 :::*                    LISTEN      17164/<span style="color: #000000;">beam          
tcp6       0      0 :::</span>22                   :::*                    LISTEN      1402/<span style="color: #000000;">sshd           
udp        0      0 </span>0.0.0.0:123             0.0.0.0:*                           1681/<span style="color: #000000;">chronyd        
udp        0      0 </span>127.0.0.1:323           0.0.0.0:*                           1681/<span style="color: #000000;">chronyd        
udp        0      0 </span>10.0.0.11:11211         0.0.0.0:*                           17962/<span style="color: #000000;">memcached     
udp6       0      0 ::</span>1:323                 :::*                                1681/chronyd</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">端口信息说明</span>

> <span style="font-family: 微软雅黑, sans-serif;">chronyd服务          123（提供给其他机器）、323（与上游同步端口）</span>
> 
> <span style="font-family: 微软雅黑, sans-serif;">Mariadb 数据库        3306数据接口</span>
> 
> <span style="font-family: 微软雅黑, sans-serif;">rabbitmq  消息队列    4369、25672（高可用架构使用）、5672（程序写端口）</span>
> 
> <span style="font-family: 微软雅黑, sans-serif;">memcached token保存  11211</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">　　至此</span>OpenStack <span style="font-family: '微软雅黑',sans-serif;">基础配置完成。</span>
</p>

## <span id="14_Keystone">1.4 Keystone<span style="font-family: '微软雅黑',sans-serif;">认证服务配置</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-install.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-install.html</a>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">认证管理：授权管理和服务目录服务管理提供单点整合。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">目录服务：相当于呼叫中心（前台）</span>
</p>

   <span style="font-family: '微软雅黑',sans-serif;">在控制节点上安装和配置</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">身份认证服务，代码名称</span>keystone<span style="font-family: '微软雅黑',sans-serif;">。出现性能原因，这个配置部署</span>Fernet<span style="font-family: '微软雅黑',sans-serif;">令牌和</span>Apache HTTP<span style="font-family: '微软雅黑',sans-serif;">服务处理请求。</span>

### <span id="141">1.4.1 <span style="font-family: '微软雅黑',sans-serif;">创建数据库</span></span>

<span style="font-family: '微软雅黑',sans-serif;">用数据库连接客户端以</span> root <span style="font-family: '微软雅黑',sans-serif;">用户连接到数据库服务器：</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql -u root -p</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">创建</span> keystone <span style="font-family: '微软雅黑',sans-serif;">数据库：</span>

<div class="cnblogs_code">
  <pre>MariaDB [(none)]&gt; CREATE DATABASE keystone;</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">对</span>\`\`keystone\`\`<span style="font-family: '微软雅黑',sans-serif;">数据库授予恰当的权限：</span>

<div class="cnblogs_code">
  <pre>GRANT ALL PRIVILEGES ON keystone.* TO <span style="color: #800000;">'</span><span style="color: #800000;">keystone</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">KEYSTONE_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON keystone.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">keystone</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">KEYSTONE_DBPASS</span><span style="color: #800000;">'</span>;</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">添加完成后退出数据库客户端。</span>

<div class="cnblogs_code">
  <pre>MariaDB [(none)]&gt; exit</pre>
</div>

### <span id="142_keystone">1.4.2 <span style="font-family: '微软雅黑',sans-serif;">安装</span>keystone</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-keystone httpd mod_wsgi</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">安装的软件包为</span> keystone<span style="font-family: '微软雅黑',sans-serif;">服务包，</span>http<span style="font-family: '微软雅黑',sans-serif;">服务，用于连接</span>python<span style="font-family: '微软雅黑',sans-serif;">程序与</span>web<span style="font-family: '微软雅黑',sans-serif;">服务的中间件</span>

<p style="text-align: right;" align="right">
     <span style="font-family: '微软雅黑',sans-serif;">如何理解</span> CGI, WSGI<span style="font-family: '微软雅黑',sans-serif;">？</span> <a href="https://www.zhihu.com/question/19998865">https://www.zhihu.com/question/19998865</a>
</p>

### <span id="143">1.4.3 <span style="font-family: '微软雅黑',sans-serif;">修改配置文件</span></span>

<span style="font-family: '微软雅黑',sans-serif;">备份配置文件</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cp /etc/keystone/keystone.conf{,.bak}</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">精简化配置文件</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> egrep -v '^#|^$' /etc/keystone/keystone.conf.bak  &gt;/etc/keystone/keystone.conf</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">手动修改配置文件</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]``<span style="font-family: '微软雅黑',sans-serif;">部分，定义初始管理令牌的值</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
admin_token </span>= ADMIN_TOKEN</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [database] <span style="font-family: '微软雅黑',sans-serif;">部分，配置数据库访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[database]
connection </span>= mysql+pymysql://keystone:KEYSTONE_DBPASS@controller/keystone</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[token]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>Fernet UUID<span style="font-family: '微软雅黑',sans-serif;">令牌的提供者</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[token]
provider </span>= fernet <span style="font-family: '微软雅黑',sans-serif;">关于令牌类型的说明：</span><a href="https://www.abcdocker.com/abcdocker/1797">https://www.abcdocker.com/abcdocker/1797</a></pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif;">【自动化】</span>****<span style="font-family: '微软雅黑',sans-serif;">自动化配置</span><span style="background: yellow;">-</span>****<span style="font-family: '微软雅黑',sans-serif;">配置文件</span>**<span style="font-family: '微软雅黑',sans-serif;">（本文大量使用）</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">安装自动配置软件</span>openstack-utils
</p>

<div class="cnblogs_code">
  <pre>yum install openstack-utils.noarch -<span style="color: #000000;">y
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -ql openstack-utils</span>
/usr/bin/openstack-config</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">自动化配置命令</span>

<div class="cnblogs_code">
  <pre>cp /etc/keystone/<span style="color: #000000;">keystone.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/keystone/keystone.conf.bak &gt; /etc/keystone/<span style="color: #000000;">keystone.conf
openstack</span>-config --set /etc/keystone/<span style="color: #000000;">keystone.conf DEFAULT admin_token  ADMIN_TOKEN
openstack</span>-config --set /etc/keystone/keystone.conf database connection  mysql+pymysql://keystone:KEYSTONE_DBPASS@controller/<span style="color: #000000;">keystone
openstack</span>-config --set /etc/keystone/keystone.conf token provider  fernet</pre>
</div>

### <span id="144">1.4.4 <span style="font-family: '微软雅黑',sans-serif;">初始化身份认证服务的数据库</span>(<span style="font-family: '微软雅黑',sans-serif;">同步数据库</span>)</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> su -s /bin/sh -c "keystone-manage db_sync" keystone</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">验证数据库是否同步成功</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql keystone -e 'show tables'</span></pre>
</div>

### <span id="145_Fernet_keys">1.4.5 <span style="font-family: '微软雅黑',sans-serif;">初始化</span>Fernet keys</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令执行后会在</span>/etc/keystone/<span style="font-family: '微软雅黑',sans-serif;">目录下生成</span>fernet-keys  <span style="font-family: '微软雅黑',sans-serif;">文件：</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ls /etc/keystone/</span>
<span style="color: #000000;">default_catalog.templates  keystone.conf.bak   policy.json
fernet</span>-keys                keystone-<span style="color: #000000;">paste.ini  sso_callback_template.html
keystone.conf              logging.conf</span></pre>
</div>

### <span id="146_Apache_HTTP">1.4.6 <span style="font-family: '微软雅黑',sans-serif;">配置</span> Apache HTTP <span style="font-family: '微软雅黑',sans-serif;">服务器</span></span>

<span style="font-family: '微软雅黑',sans-serif;">编辑</span>\`\`/etc/httpd/conf/httpd.conf\`\` <span style="font-family: '微软雅黑',sans-serif;">文件，配置</span>\`\`ServerName\`\`<span style="font-family: '微软雅黑',sans-serif;">。</span>

<div class="cnblogs_code">
  <pre>echo <span style="color: #800000;">'</span><span style="color: #800000;">ServerName controller</span><span style="color: #800000;">'</span> &gt;&gt;/etc/httpd/conf/httpd.conf</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">创建配置文件</span> /etc/httpd/conf.d/wsgi-keystone.conf

<span style="font-family: '微软雅黑',sans-serif;">注：</span>keystone<span style="font-family: '微软雅黑',sans-serif;">服务较为特殊，其他的服务可自行创建配置文件。</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[root@controller ~]# cat /etc/httpd/conf.d/wsgi-keystone.conf
Listen 5000
Listen 35357

</span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">VirtualHost </span><span style="color: #ff0000;">*:5000</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
    WSGIDaemonProcess keystone-public processes=5 threads=1 user=keystone group=keystone display-name=%{GROUP}
    WSGIProcessGroup keystone-public
    WSGIScriptAlias / /usr/bin/keystone-wsgi-public
    WSGIApplicationGroup %{GLOBAL}
    WSGIPassAuthorization On
    ErrorLogFormat "%{cu}t %M"
    ErrorLog /var/log/httpd/keystone-error.log
    CustomLog /var/log/httpd/keystone-access.log combined

    </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">Directory </span><span style="color: #ff0000;">/usr/bin</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        Require all granted
    </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">Directory</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">VirtualHost</span><span style="color: #0000ff;">&gt;</span>

<span style="color: #0000ff;">&lt;</span><span style="color: #800000;">VirtualHost </span><span style="color: #ff0000;">*:35357</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
    WSGIDaemonProcess keystone-admin processes=5 threads=1 user=keystone group=keystone display-name=%{GROUP}
    WSGIProcessGroup keystone-admin
    WSGIScriptAlias / /usr/bin/keystone-wsgi-admin
    WSGIApplicationGroup %{GLOBAL}
    WSGIPassAuthorization On
    ErrorLogFormat "%{cu}t %M"
    ErrorLog /var/log/httpd/keystone-error.log
    CustomLog /var/log/httpd/keystone-access.log combined

    </span><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">Directory </span><span style="color: #ff0000;">/usr/bin</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
        Require all granted
    </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">Directory</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">VirtualHost</span><span style="color: #0000ff;">&gt;</span></pre>
</div>

### <span id="147_Apache_HTTP">1.4.7 <span style="font-family: '微软雅黑',sans-serif;">启动</span> Apache HTTP <span style="font-family: '微软雅黑',sans-serif;">服务并配置其随系统启动</span></span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl enable httpd.service
systemctl start httpd.service</span></pre>
</div>

### <span id="148_API">1.4.8 <span style="font-family: '微软雅黑',sans-serif;">创建服务实体和</span>API<span style="font-family: '微软雅黑',sans-serif;">端点</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-services.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-services.html</a>
</p>

<span style="background: lime;">a.</span><span style="font-family: '微软雅黑',sans-serif;">配置环境变量</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置认证令牌</span>
</p>

<div class="cnblogs_code">
  <pre>export OS_TOKEN=ADMIN_TOKEN</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置端点</span>URL
</p>

<div class="cnblogs_code">
  <pre>export OS_URL=http://controller:35357/v3</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置认证</span> API <span style="font-family: '微软雅黑',sans-serif;">版本</span>
</p>

<div class="cnblogs_code">
  <pre>export OS_IDENTITY_API_VERSION=3</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">查看环境变量</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> env |grep OS</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集：</span>

<div class="cnblogs_code">
  <pre>export OS_TOKEN=<span style="color: #000000;">ADMIN_TOKEN
export OS_URL</span>=http://controller:35357/<span style="color: #000000;">v3
export OS_IDENTITY_API_VERSION</span>=3<span style="color: #000000;">
env </span>|grep OS</pre>
</div>

<span style="background: lime;">b.</span><span style="font-family: '微软雅黑',sans-serif;">创建服务实体和</span><span style="background: lime;">API</span><span style="font-family: '微软雅黑',sans-serif;">端点</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建命令</span>
</p>

<div class="cnblogs_code">
  <pre>openstack service create --name keystone --description <span style="color: #800000;">"</span><span style="color: #800000;">OpenStack Identity</span><span style="color: #800000;">"</span> identity</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">执行过程</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;">  openstack service create \</span>
&gt;   --name keystone --description <span style="color: #800000;">"</span><span style="color: #800000;">OpenStack Identity</span><span style="color: #800000;">"</span><span style="color: #000000;"> identity
</span>+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | OpenStack Identity               |
| enabled     | True                             |
| id          | f08ec36b2b7340d6976fcb2bbd24e83b |
| name        | keystone                         |
| type        | identity                         |
+-------------+----------------------------------+</pre>
</div>

<span style="background: lime;">c.</span><span style="font-family: '微软雅黑',sans-serif;">创建认证服务的</span> <span style="background: lime;">API </span><span style="font-family: '微软雅黑',sans-serif;">端点</span>

<span style="font-family: '微软雅黑',sans-serif;">命令集</span>

<div class="cnblogs_code">
  <pre>openstack endpoint create --region RegionOne identity public http://controller:5000/<span style="color: #000000;">v3
openstack endpoint create </span>--region RegionOne identity internal http://controller:5000/<span style="color: #000000;">v3
openstack endpoint create </span>--region RegionOne identity admin http://controller:35357/v3</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">执行过程</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack endpoint create --region RegionOne \</span>
&gt;   identity public http://controller:5000/<span style="color: #000000;">v3
</span>+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | e27dd713753f47b8a1062ac50ca33845 |
| interface    | public                           |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | f08ec36b2b7340d6976fcb2bbd24e83b |
| service_name | keystone                         |
| service_type | identity                         |
| url          | http://controller:5000/v3        |
+--------------+----------------------------------+<span style="color: #000000;">

[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack endpoint create --region RegionOne \</span>
&gt;   identity internal http://controller:5000/<span style="color: #000000;">v3
</span>+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 71b7435fa2df4c58bb6ca5cc38a434a7 |
| interface    | internal                         |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | f08ec36b2b7340d6976fcb2bbd24e83b |
| service_name | keystone                         |
| service_type | identity                         |
| url          | http://controller:5000/v3        |
+--------------+----------------------------------+<span style="color: #000000;">

[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack endpoint create --region RegionOne \</span>
&gt;   identity admin http://controller:35357/<span style="color: #000000;">v3
</span>+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | cf58eee084c04777a520d487adc1a88f |
| interface    | admin                            |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | f08ec36b2b7340d6976fcb2bbd24e83b |
| service_name | keystone                         |
| service_type | identity                         |
| url          | http://controller:35357/v3       |
+--------------+----------------------------------+</pre>
</div>

### <span id="149">1.4.9 <span style="font-family: '微软雅黑',sans-serif;">创建域、项目、用户和角色</span></span>

<p style="margin-left: 42.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-users.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-users.html</a>
</p>

<span style="background: lime;">a.</span><span style="font-family: '微软雅黑',sans-serif;">创建域</span><span style="background: lime;">``default``</span>

<div class="cnblogs_code">
  <pre>openstack domain create --description <span style="color: #800000;">"</span><span style="color: #800000;">Default Domain</span><span style="color: #800000;">"</span> default</pre>
</div>

<span style="background: lime;">b.</span><span style="font-family: '微软雅黑',sans-serif;">在你的环境中，为进行管理操作，创建管理的项目、用户和角色</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> admin <span style="font-family: '微软雅黑',sans-serif;">项目</span>
</p>

<div class="cnblogs_code">
  <pre>openstack project create --domain default --description <span style="color: #800000;">"</span><span style="color: #800000;">Admin Project</span><span style="color: #800000;">"</span> admin</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> admin <span style="font-family: '微软雅黑',sans-serif;">用户</span>
</p>

<div class="cnblogs_code">
  <pre>openstack user create --domain default --password-prompt admin</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> admin <span style="font-family: '微软雅黑',sans-serif;">角色</span>
</p>

<div class="cnblogs_code">
  <pre>openstack role create admin</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">添加</span>``admin`` <span style="font-family: '微软雅黑',sans-serif;">角色到</span> admin <span style="font-family: '微软雅黑',sans-serif;">项目和用户上</span>
</p>

<div class="cnblogs_code">
  <pre>openstack role add --project admin --user admin admin</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集：</span>

<div class="cnblogs_code">
  <pre>openstack domain create --description <span style="color: #800000;">"</span><span style="color: #800000;">Default Domain</span><span style="color: #800000;">"</span><span style="color: #000000;"> default
openstack project create </span>--domain default --description <span style="color: #800000;">"</span><span style="color: #800000;">Admin Project</span><span style="color: #800000;">"</span><span style="color: #000000;"> admin
openstack user create </span>--domain default --<span style="color: #000000;">password ADMIN_PASS admin
openstack role create admin
openstack role add </span>--project admin --user admin admin</pre>
</div>

<span style="background: lime;">c.</span><span style="font-family: '微软雅黑',sans-serif;">创建</span><span style="background: lime;">servers</span><span style="font-family: '微软雅黑',sans-serif;">项目</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;">  openstack project create --domain default --description "Service Project" service</span>
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | Service Project                  |
| domain_id   | df6407ae93bb407d876f2ee4787ede79 |
| enabled     | True                             |
| id          | cd2107aa3a8f4066a871ca029641cfd7 |
| is_domain   | False                            |
| name        | service                          |
| parent_id   | df6407ae93bb407d876f2ee4787ede79 |
+-------------+----------------------------------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">验证之前的所有操作</span>

<span style="font-family: '微软雅黑',sans-serif;">命令集：</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">openstack service list 
openstack endpoint list </span>| grep keystone |wc -<span style="color: #000000;">l 
openstack domain list 
openstack project list 
openstack user list 
openstack role list </span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看服务列表</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack service list </span>
+----------------------------------+----------+----------+
| ID                               | Name     | Type     |
+----------------------------------+----------+----------+
| f08ec36b2b7340d6976fcb2bbd24e83b | keystone | identity |
+----------------------------------+----------+----------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看当前的域</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack domain list </span>
+----------------------------------+---------+---------+----------------+
| ID                               | Name    | Enabled | Description    |
+----------------------------------+---------+---------+----------------+
| df6407ae93bb407d876f2ee4787ede79 | default | True    | Default Domain |
+----------------------------------+---------+---------+----------------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看集合</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack project list </span>
+----------------------------------+---------+
| ID                               | Name    |
+----------------------------------+---------+
| cd2107aa3a8f4066a871ca029641cfd7 | service |
| d0dfbdbc115b4a728c24d28bc1ce1e62 | admin   |
+----------------------------------+---------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看当前的用户列表</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack user list </span>
+----------------------------------+-------+
| ID                               | Name  |
+----------------------------------+-------+
| d8f4a1d74f52482d8ebe2184692d2c1c | admin |
+----------------------------------+-------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看当前的角色</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack role list </span>
+----------------------------------+-------+
| ID                               | Name  |
+----------------------------------+-------+
| 4de514c418ee480d898773e4f543b79d | admin |
+----------------------------------+-------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">关于域、项目、用户和角色的说明：</span>

<table style="width: 100%; border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 16.22%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="16%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">类型</span></strong>
      </p>
    </td>
    
    <td style="width: 83.78%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="83%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 16.22%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="16%">
      <p style="text-align: center;" align="center">
        <strong>Domain</strong>
      </p>
    </td>
    
    <td style="width: 83.78%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="83%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">表示</span> project <span style="font-family: '微软雅黑',sans-serif;">和</span> user <span style="font-family: '微软雅黑',sans-serif;">的集合，在公有云或者私有云中常常表示一个客户</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 16.22%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="16%">
      <p style="text-align: center;" align="center">
        <strong>Group</strong>
      </p>
    </td>
    
    <td style="width: 83.78%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="83%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">一个</span>domain <span style="font-family: '微软雅黑',sans-serif;">中的部分用户的集合</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 16.22%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="16%">
      <p style="text-align: center;" align="center">
        <strong>Project</strong>
      </p>
    </td>
    
    <td style="width: 83.78%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="83%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">项目、</span>IT<span style="font-family: '微软雅黑',sans-serif;">基础设施资源的集合，比如虚机，卷，镜像等</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 16.22%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="16%">
      <p style="text-align: center;" align="center">
        <strong>Role</strong>
      </p>
    </td>
    
    <td style="width: 83.78%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="83%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">授权，角色，表示一个</span> user <span style="font-family: '微软雅黑',sans-serif;">对一个</span> project resource <span style="font-family: '微软雅黑',sans-serif;">的权限</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 16.22%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="16%">
      <p style="text-align: center;" align="center">
        <strong>Token</strong>
      </p>
    </td>
    
    <td style="width: 83.78%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="83%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">一个</span> user <span style="font-family: '微软雅黑',sans-serif;">对于某个目标（</span>project <span style="font-family: '微软雅黑',sans-serif;">或者</span> domain<span style="font-family: '微软雅黑',sans-serif;">）的一个有限时间段内的身份令牌</span>
      </p>
    </td>
  </tr>
</table>

### <span id="1410_OpenStack">1.4.10 <span style="font-family: '微软雅黑',sans-serif;">创建</span> OpenStack <span style="font-family: '微软雅黑',sans-serif;">客户端环境脚本</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-openrc.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/keystone-openrc.html</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">编辑文件</span> admin-openrc <span style="font-family: '微软雅黑',sans-serif;">并添加如下内容</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vi admin-openrc</span>
export OS_PROJECT_DOMAIN_NAME=<span style="color: #000000;">default
export OS_USER_DOMAIN_NAME</span>=<span style="color: #000000;">default
export OS_PROJECT_NAME</span>=<span style="color: #000000;">admin
export OS_USERNAME</span>=<span style="color: #000000;">admin
export OS_PASSWORD</span>=<span style="color: #000000;">ADMIN_PASS
export OS_AUTH_URL</span>=http://controller:35357/<span style="color: #000000;">v3
export OS_IDENTITY_API_VERSION</span>=3<span style="color: #000000;">
export OS_IMAGE_API_VERSION</span>=2</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">【重要】务必使用环境变量脚本</span>

<span style="font-family: '微软雅黑',sans-serif;">使用脚本创建环境变量</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> source admin-openrc </span>
[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> env|grep OS</span>
HOSTNAME=<span style="color: #000000;">controller
OS_USER_DOMAIN_NAME</span>=<span style="color: #000000;">default
OS_IMAGE_API_VERSION</span>=2<span style="color: #000000;">
OS_PROJECT_NAME</span>=<span style="color: #000000;">admin
OS_IDENTITY_API_VERSION</span>=3<span style="color: #000000;">
OS_PASSWORD</span>=<span style="color: #000000;">ADMIN_PASS
OS_AUTH_URL</span>=http://controller:35357/<span style="color: #000000;">v3
OS_USERNAME</span>=<span style="color: #000000;">admin
OS_PROJECT_DOMAIN_NAME</span>=default</pre>
</div>

## <span id="15_glance">1.5 <span style="font-family: '微软雅黑',sans-serif;">镜像服务</span>glance<span style="font-family: '微软雅黑',sans-serif;">部署</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance.html</a>
</p>

### <span id="151">1.5.1 <span style="font-family: '微软雅黑',sans-serif;">创库授权</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance-install.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance-install.html</a>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 登陆mysql数据库</span>
[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> glance <span style="font-family: '微软雅黑',sans-serif;">数据库：</span>
</p>

<div class="cnblogs_code">
  <pre>CREATE DATABASE glance;</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">对</span>``glance``<span style="font-family: '微软雅黑',sans-serif;">数据库授予恰当的权限：</span>
</p>

<div class="cnblogs_code">
  <pre>GRANT ALL PRIVILEGES ON glance.* TO <span style="color: #800000;">'</span><span style="color: #800000;">glance</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">GLANCE_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON glance.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">glance</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">GLANCE_DBPASS</span><span style="color: #800000;">'</span>;</pre>
</div>

### <span id="152_glance">1.5.2 <span style="font-family: '微软雅黑',sans-serif;">创建</span>glance<span style="font-family: '微软雅黑',sans-serif;">用户和授权</span></span>

<p style="text-indent: 15.75pt;">
  <span style="color: red; background: yellow;">[</span><strong><span style="font-family: '微软雅黑',sans-serif;">重要</span></strong><span style="color: red; background: yellow;">]</span><span style="font-family: '微软雅黑',sans-serif;">加载环境变量</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注：每次使用</span>openstack<span style="font-family: '微软雅黑',sans-serif;">管理命令时都依赖与环境变量</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> . admin-openrc</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> glance <span style="font-family: '微软雅黑',sans-serif;">用户</span>
</p>

<div class="cnblogs_code">
  <pre>openstack user create --domain default --password GLANCE_PASS glance</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">添加</span> admin <span style="font-family: '微软雅黑',sans-serif;">角色到</span> glance <span style="font-family: '微软雅黑',sans-serif;">用户和</span> service <span style="font-family: '微软雅黑',sans-serif;">项目上</span>
</p>

<div class="cnblogs_code">
  <pre>openstack role add --project service --user glance admin</pre>
</div>

### <span id="153_API">1.5.3 <span style="font-family: '微软雅黑',sans-serif;">创建镜像服务的</span> API <span style="font-family: '微软雅黑',sans-serif;">端点，并注册</span></span>

<span style="font-family: '微软雅黑',sans-serif;">创建</span>\`\`glance\`\`<span style="font-family: '微软雅黑',sans-serif;">服务实体</span>

<div class="cnblogs_code">
  <pre>openstack service create --name glance --description <span style="color: #800000;">"</span><span style="color: #800000;">OpenStack Image</span><span style="color: #800000;">"</span> image</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">执行过程</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack service create --name glance --description "OpenStack Image" image</span>
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description | OpenStack Image                  |
| enabled     | True                             |
| id          | 30357ca18e5046b98dbc0dd4f1e7d69c |
| name        | glance                           |
| type        | image                            |
+-------------+----------------------------------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">创建镜像服务的</span> API <span style="font-family: '微软雅黑',sans-serif;">端点</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre>openstack endpoint create --region RegionOne image public http://controller:9292<span style="color: #000000;">
openstack endpoint create </span>--region RegionOne image internal http://controller:9292<span style="color: #000000;">
openstack endpoint create </span>--region RegionOne image admin http://controller:9292</pre>
</div>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">执行过程</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack endpoint create --region RegionOne image public http://controller:9292</span>
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 671486d2528448e9a4067ab04a15e015 |
| interface    | public                           |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 30357ca18e5046b98dbc0dd4f1e7d69c |
| service_name | glance                           |
| service_type | image                            |
| url          | http://controller:9292           |
+--------------+----------------------------------+<span style="color: #000000;">
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack endpoint create --region RegionOne image internal http://controller:9292</span>
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 8ff6131b7e1b4234bb4f34daecbbd615 |
| interface    | internal                         |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 30357ca18e5046b98dbc0dd4f1e7d69c |
| service_name | glance                           |
| service_type | image                            |
| url          | http://controller:9292           |
+--------------+----------------------------------+<span style="color: #000000;">
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack endpoint create --region RegionOne image admin http://controller:9292</span>
+--------------+----------------------------------+
| Field        | Value                            |
+--------------+----------------------------------+
| enabled      | True                             |
| id           | 4a1b3341a0604dbfb710eaa63aab626a |
| interface    | admin                            |
| region       | RegionOne                        |
| region_id    | RegionOne                        |
| service_id   | 30357ca18e5046b98dbc0dd4f1e7d69c |
| service_name | glance                           |
| service_type | image                            |
| url          | http://controller:9292           |
+--------------+----------------------------------+</pre>
</div>

### <span id="154_glance">1.5.4 <span style="font-family: '微软雅黑',sans-serif;">安装</span>glance<span style="font-family: '微软雅黑',sans-serif;">软件包</span></span>

<div class="cnblogs_code">
  <pre>yum install openstack-glance -y</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">服务说明：</span>

> <p style="color: #000000;">
>   glance-api <span style="font-family: '微软雅黑',sans-serif;">负责镜像的上传、下载、查看、删除</span>
> </p>
> 
> <p style="color: #000000;">
>   glance-registry <span style="font-family: '微软雅黑',sans-serif;">修改镜像的源数据：镜像所需的配置</span>
> </p>

### <span id="155_glance">1.5.5 <span style="font-family: '微软雅黑',sans-serif;">修改</span>glance<span style="font-family: '微软雅黑',sans-serif;">相关配置文件</span></span>

> <p style="color: #000000;">
>   /etc/glance/glance-api.conf      # <span style="font-family: '微软雅黑',sans-serif;">接收镜像</span>API<span style="font-family: '微软雅黑',sans-serif;">的调用，诸如镜像发现、恢复、存储。</span>
> </p>
> 
> <p style="color: #000000;">
>   /etc/glance/glance-registry.conf #<span style="font-family: '微软雅黑',sans-serif;">存储、处理和恢复镜像的元数据，元数据包括项诸如大小和类型。</span>
> </p>

<span style="background: lime;">1</span><span style="font-family: '微软雅黑',sans-serif;">、编辑文件</span> <span style="background: lime;">/etc/glance/glance-registry.conf</span>

[database] <span style="font-family: '微软雅黑',sans-serif;">部分，配置数据库访问</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [database]
    ...
    connection </span>= mysql+pymysql://glance:GLANCE_DBPASS@controller/glance</pre>
</div>

[keystone_authtoken] <span style="font-family: '微软雅黑',sans-serif;">和</span> [paste_deploy] <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [keystone_authtoken]
    ...
    auth_uri </span>= http://controller:5000<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    memcached_servers </span>= controller:11211<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> glance
    password </span>=<span style="color: #000000;"> GLANCE_PASS

    [paste_deploy]
    ...
    flavor </span>= keystone</pre>
</div>

[glance_store] <span style="font-family: '微软雅黑',sans-serif;">部分，配置本地文件系统存储和镜像文件位置</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [glance_store]
    ...
    stores </span>=<span style="color: #000000;"> file,http
    default_store </span>=<span style="color: #000000;"> file
    filesystem_store_datadir </span>= /var/lib/glance/images/</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集</span>

<div class="cnblogs_code">
  <pre>cp /etc/glance/glance-<span style="color: #000000;">api.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/glance/glance-api.conf.bak &gt;/etc/glance/glance-<span style="color: #000000;">api.conf
openstack</span>-config --set /etc/glance/glance-api.conf  database  connection  mysql+pymysql://glance:GLANCE_DBPASS@controller/<span style="color: #000000;">glance
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  glance_store stores  file,http
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  glance_store default_store  file
openstack</span>-config --set /etc/glance/glance-api.conf  glance_store filesystem_store_datadir  /var/lib/glance/images/<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-api.conf  keystone_authtoken auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-api.conf  keystone_authtoken auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-api.conf  keystone_authtoken memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken auth_type  password
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken project_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken user_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken project_name  service
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken username  glance
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken password  GLANCE_PASS
openstack</span>-config --set /etc/glance/glance-api.conf  paste_deploy flavor  keystone</pre>
</div>

<span style="background: lime;">2</span><span style="font-family: '微软雅黑',sans-serif;">、编辑文件</span> <span style="background: lime;">``/etc/glance/glance-registry.conf``</span>

[database] <span style="font-family: '微软雅黑',sans-serif;">部分，配置数据库访问</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [database]
    ...
    connection </span>= mysql+pymysql://glance:GLANCE_DBPASS@controller/glance</pre>
</div>

[keystone_authtoken] <span style="font-family: '微软雅黑',sans-serif;">和</span> [paste_deploy] <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [keystone_authtoken]
    ...
    auth_uri </span>= http://controller:5000<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    memcached_servers </span>= controller:11211<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> glance
    password </span>=<span style="color: #000000;"> GLANCE_PASS
 
    [paste_deploy]
    ...
    flavor </span>= keystone</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集</span>

<div class="cnblogs_code">
  <pre>cp /etc/glance/glance-<span style="color: #000000;">registry.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/glance/glance-registry.conf.bak &gt; /etc/glance/glance-<span style="color: #000000;">registry.conf
openstack</span>-config --set /etc/glance/glance-registry.conf  database  connection  mysql+pymysql://glance:GLANCE_DBPASS@controller/<span style="color: #000000;">glance
openstack</span>-config --set /etc/glance/glance-registry.conf  keystone_authtoken auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-registry.conf  keystone_authtoken auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-registry.conf  keystone_authtoken memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken auth_type  password
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken project_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken user_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken project_name  service
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken username  glance
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken password  GLANCE_PASS
openstack</span>-config --set /etc/glance/glance-registry.conf  paste_deploy flavor  keystone</pre>
</div>

### <span id="156">1.5.6 <span style="font-family: '微软雅黑',sans-serif;">同步数据库</span></span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;">  su -s /bin/sh -c "glance-manage db_sync" glance</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注：忽略输出中任何不推荐使用的信息。</span>
</p>

<span style="font-family: '微软雅黑',sans-serif;">检查数据库是否同步成功</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql glance -e "show tables" |wc -l </span>
21</pre>
</div>

### <span id="157_glance">1.5.7 <span style="font-family: '微软雅黑',sans-serif;">启动</span>glance<span style="font-family: '微软雅黑',sans-serif;">服务</span></span>

<span style="font-family: '微软雅黑',sans-serif;">启动镜像服务、配置他们随机启动</span>

<div class="cnblogs_code">
  <pre>systemctl enable openstack-glance-api.service openstack-glance-<span style="color: #000000;">registry.service
systemctl start  openstack</span>-glance-api.service openstack-glance-registry.service</pre>
</div>

### <span id="158_glance">1.5.8 <span style="font-family: '微软雅黑',sans-serif;">验证</span>glance<span style="font-family: '微软雅黑',sans-serif;">服务操作</span></span>

a.<span style="font-family: '微软雅黑',sans-serif;">设置环境变量</span>

<div class="cnblogs_code">
  <pre>. admin-openrc</pre>
</div>

b.<span style="font-family: '微软雅黑',sans-serif;">下载源镜像</span>

<div class="cnblogs_code">
  <pre>wget http://download.cirros-cloud.net/0.3.4/cirros-0.3.4-x86_64-disk.img</pre>
</div>

c.<span style="font-family: '微软雅黑',sans-serif;">使用</span> QCOW2 <span style="font-family: '微软雅黑',sans-serif;">磁盘格式，</span> bare <span style="font-family: '微软雅黑',sans-serif;">容器格式上传镜像到镜像服务并设置公共可见，这样所有的项目都可以访问它</span>

<div class="cnblogs_code">
  <pre>openstack image create <span style="color: #800000;">"</span><span style="color: #800000;">cirros</span><span style="color: #800000;">"</span> --file cirros-0.3.4-x86_64-disk.img --disk-format qcow2 --container-format bare --public</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">执行过程如下</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack image create "cirros" --file cirros-0.3.4-x86_64-disk.img --disk-format qcow2 --container-format bare --public</span>
+------------------+------------------------------------------------------+
| Field            | Value                                                |
+------------------+------------------------------------------------------+
| checksum         | ee1eca47dc88f4879d8a229cc70a07c6                     |
| container_format | bare                                                 |
| created_at       | 2018-01-23T10:20:19Z                                 |
| disk_format      | qcow2                                                |
| file             | /v2/images/9d92c601-0824-493a-bc6e-cecb10e9a4c6/file |
| id               | 9d92c601-0824-493a-bc6e-cecb10e9a4c6                 |
| min_disk         | 0                                                    |
| min_ram          | 0                                                    |
| name             | cirros                                               |
| owner            | d0dfbdbc115b4a728c24d28bc1ce1e62                     |
| protected        | False                                                |
| schema           | /v2/schemas/image                                    |
| size             | 13287936                                             |
| status           | active                                               |
| tags             |                                                      |
| updated_at       | 2018-01-23T10:20:20Z                                 |
| virtual_size     | None                                                 |
| visibility       | public                                               |
+------------------+------------------------------------------------------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看镜像列表</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack image list </span>
+--------------------------------------+--------+--------+
| ID                                   | Name   | Status |
+--------------------------------------+--------+--------+
| 9d92c601-0824-493a-bc6e-cecb10e9a4c6 | cirros | active |
+--------------------------------------+--------+--------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">镜像位置，镜像上传后以</span>id<span style="font-family: '微软雅黑',sans-serif;">命名。</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll -h  /var/lib/glance/images/ </span>
<span style="color: #000000;">total 13M
</span>-rw-r----- 1 glance glance 13M Jan 23 18:20 9d92c601-0824-493a-bc6e-cecb10e9a4c6</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">至此</span><span style="color: red; background: yellow;">glance</span><span style="font-family: '微软雅黑',sans-serif;">服务配置完成</span>

## <span id="16_nova">1.6 <span style="font-family: '微软雅黑',sans-serif;">计算服务</span>(nova)<span style="font-family: '微软雅黑',sans-serif;">部署</span></span>

<p style="margin-left: 25.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova.html</a>
</p>

### <span id="161">1.6.1 <span style="font-family: '微软雅黑',sans-serif;">在控制节点安装并配置</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考文献：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova-controller-install.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova-controller-install.html</a>
</p>

<span style="background: yellow;">1</span><span style="font-family: '微软雅黑',sans-serif;">）在数据库中，创库，授权</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">数据库连接客户端以</span> root <span style="font-family: '微软雅黑',sans-serif;">用户连接到数据库服务器</span>
</p>

<div class="cnblogs_code">
  <pre>mysql -u root -p</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> nova_api <span style="font-family: '微软雅黑',sans-serif;">和</span> nova <span style="font-family: '微软雅黑',sans-serif;">数据库：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">CREATE DATABASE nova_api;
CREATE DATABASE nova;</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">对数据库进行正确的授权</span>
</p>

<div class="cnblogs_code">
  <pre>GRANT ALL PRIVILEGES ON nova_api.* TO <span style="color: #800000;">'</span><span style="color: #800000;">nova</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">NOVA_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON nova_api.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">nova</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">NOVA_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON nova.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">nova</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">NOVA_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON nova.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">nova</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">NOVA_DBPASS</span><span style="color: #800000;">'</span>;</pre>
</div>

<span style="background: yellow;">2</span><span style="font-family: '微软雅黑',sans-serif;">）在</span><span style="background: yellow;">keystone</span><span style="font-family: '微软雅黑',sans-serif;">中创建用户并授权</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">加载环境变量</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;">  . admin-openrc</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建用户</span>
</p>

<div class="cnblogs_code">
  <pre>openstack user create --domain default --password NOVA_PASS nova</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">关联角色</span>
</p>

<div class="cnblogs_code">
  <pre>openstack role add --project service --user nova admin</pre>
</div>

<span style="background: yellow;">3</span><span style="font-family: '微软雅黑',sans-serif;">）在</span><span style="background: yellow;">keystone</span><span style="font-family: '微软雅黑',sans-serif;">中创建服务实体，和注册</span><span style="background: yellow;">API</span><span style="font-family: '微软雅黑',sans-serif;">接口</span>

<span style="font-family: '微软雅黑',sans-serif;">创建服务实体</span>

<div class="cnblogs_code">
  <pre>openstack service create --name nova --description <span style="color: #800000;">"</span><span style="color: #800000;">OpenStack Compute</span><span style="color: #800000;">"</span> compute</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">注册</span>API<span style="font-family: '微软雅黑',sans-serif;">接口</span>

<div class="cnblogs_code">
  <pre>openstack endpoint create --region RegionOne compute public http://controller:8774/v2.1/%<span style="color: #000000;">\(tenant_id\)s
openstack endpoint create </span>--region RegionOne compute internal http://controller:8774/v2.1/%<span style="color: #000000;">\(tenant_id\)s
openstack endpoint create </span>--region RegionOne compute admin http://controller:8774/v2.1/%\(tenant_id\)s</pre>
</div>

4<span style="font-family: '微软雅黑',sans-serif;">）安装软件包</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-nova-api openstack-nova-conductor openstack-nova-console openstack-nova-novncproxy openstack-nova-scheduler</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">软件包说明</span>

> nova-api             # 提供api接口
> 
> nova-scheduler  # 调度
> 
> nova-conductor  # 替代计算节点进入数据库操作
> 
> nova-consoleauth   # 提供web界面版的vnc管理
> 
> nova-novncproxy  # 提供web界面版的vnc管理
> 
> nova-compute   # 调度libvirtd进行虚拟机生命周期的管理

5<span style="font-family: '微软雅黑',sans-serif;">）修改配置文件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/nova/nova.conf``<span style="font-family: '微软雅黑',sans-serif;">文件并完成下面的操作</span>:
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]``<span style="font-family: '微软雅黑',sans-serif;">部分，只启用计算和元数据</span>API<span style="font-family: '微软雅黑',sans-serif;">：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    enabled_apis </span>= osapi_compute,metadata</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[api_database]``<span style="font-family: '微软雅黑',sans-serif;">和</span>``[database]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置数据库的连接：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [api_database]
    ...
    connection </span>= mysql+pymysql://nova:NOVA_DBPASS@controller/<span style="color: #000000;">nova_api

    [database]
    ...
    connection </span>= mysql+pymysql://nova:NOVA_DBPASS@controller/nova</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[oslo_messaging_rabbit]<span style="font-family: '微软雅黑',sans-serif;">”部分，配置</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>RabbitMQ<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">消息队列访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    rpc_backend </span>=<span style="color: #000000;"> rabbit

    [oslo_messaging_rabbit]
    ...
    rabbit_host </span>=<span style="color: #000000;"> controller
    rabbit_userid </span>=<span style="color: #000000;"> openstack
    rabbit_password </span>= RABBIT_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[keystone_authtoken]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    auth_strategy </span>=<span style="color: #000000;"> keystone

    [keystone_authtoken]
    ...
    auth_uri </span>= http://controller:5000<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    memcached_servers </span>= controller:11211<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> nova
    password </span>= NOVA_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>``my_ip`` <span style="font-family: '微软雅黑',sans-serif;">来使用控制节点的管理接口的</span>IP <span style="font-family: '微软雅黑',sans-serif;">地址。</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    my_ip </span>= 10.0.0.11</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT] <span style="font-family: '微软雅黑',sans-serif;">部分，使能</span> Networking <span style="font-family: '微软雅黑',sans-serif;">服务：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    use_neutron </span>=<span style="color: #000000;"> True
    firewall_driver </span>= nova.virt.firewall.NoopFirewallDriver</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[vnc]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>VNC<span style="font-family: '微软雅黑',sans-serif;">代理使用控制节点的管理接口</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址</span> <span style="font-family: '微软雅黑',sans-serif;">：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [vnc]
    ...
    vncserver_listen </span>=<span style="color: #000000;"> $my_ip
    vncserver_proxyclient_address </span>= $my_ip</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [glance] <span style="font-family: '微软雅黑',sans-serif;">区域，配置镜像服务</span> API <span style="font-family: '微软雅黑',sans-serif;">的位置：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [glance]
    ...
    api_servers </span>= http://controller:9292</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [oslo_concurrency] <span style="font-family: '微软雅黑',sans-serif;">部分，配置锁路径：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [oslo_concurrency]
    ...
    lock_path </span>= /var/lib/nova/tmp</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre>cp /etc/nova/<span style="color: #000000;">nova.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/nova/nova.conf.bak &gt;/etc/nova/<span style="color: #000000;">nova.conf
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT enabled_apis  osapi_compute,metadata
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT rpc_backend  rabbit
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT auth_strategy  keystone
openstack</span>-config --set /etc/nova/nova.conf  DEFAULT my_ip  10.0.0.11<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT use_neutron  True
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT firewall_driver  nova.virt.firewall.NoopFirewallDriver
openstack</span>-config --set /etc/nova/nova.conf  api_database connection  mysql+pymysql://nova:NOVA_DBPASS@controller/<span style="color: #000000;">nova_api
openstack</span>-config --set /etc/nova/nova.conf  database  connection  mysql+pymysql://nova:NOVA_DBPASS@controller/<span style="color: #000000;">nova
openstack</span>-config --set /etc/nova/nova.conf  glance api_servers  http://controller:9292<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  auth_type  password
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  project_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  user_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  project_name  service
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  username  nova
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  password  NOVA_PASS
openstack</span>-config --set /etc/nova/nova.conf  oslo_concurrency lock_path  /var/lib/nova/<span style="color: #000000;">tmp
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_host  controller
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_userid  openstack
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_password  RABBIT_PASS
openstack</span>-config --set /etc/nova/nova.conf  vnc vncserver_listen  <span style="color: #800000;">'</span><span style="color: #800000;">$my_ip</span><span style="color: #800000;">'</span><span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  vnc vncserver_proxyclient_address  <span style="color: #800000;">'</span><span style="color: #800000;">$my_ip</span><span style="color: #800000;">'</span></pre>
</div>

6<span style="font-family: '微软雅黑',sans-serif;">）同步数据库</span>

<div class="cnblogs_code">
  <pre>su -s /bin/sh -c <span style="color: #800000;">"</span><span style="color: #800000;">nova-manage api_db sync</span><span style="color: #800000;">"</span><span style="color: #000000;"> nova
su </span>-s /bin/sh -c <span style="color: #800000;">"</span><span style="color: #800000;">nova-manage db sync</span><span style="color: #800000;">"</span> nova</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意：忽略执行过程中输出中任何不推荐使用的信息</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql nova_api -e 'show tables' |wc -l </span>
10<span style="color: #000000;">
[root@controller </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql nova -e 'show tables' |wc -l </span>
110</pre>
</div>

7<span style="font-family: '微软雅黑',sans-serif;">）启动服务</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">设置开启自启动</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl enable openstack-nova-api.service openstack-nova-consoleauth.service openstack-nova-scheduler.service openstack-nova-conductor.service openstack-nova-novncproxy.service</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">启动服务</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl start openstack-nova-api.service openstack-nova-consoleauth.service openstack-nova-scheduler.service openstack-nova-conductor.service openstack-nova-novncproxy.service</pre>
</div>

<p style="text-indent: 15.75pt;">
  # <span style="font-family: '微软雅黑',sans-serif;">查看服务状态</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status openstack-nova-api.service openstack-nova-consoleauth.service openstack-nova-scheduler.service openstack-nova-conductor.service openstack-nova-novncproxy.service |grep 'active (running)' |wc -l</span>

5</pre>
</div>

### <span id="162">1.6.2 <span style="font-family: '微软雅黑',sans-serif;">在计算节点安装和配置</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">查考文献：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova-compute-install.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/nova-compute-install.html</a>
</p>

1<span style="font-family: '微软雅黑',sans-serif;">）安装软件包</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-nova-compute</pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">）修改配置文件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/nova/nova.conf``<span style="font-family: '微软雅黑',sans-serif;">文件并完成下面的操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]`` <span style="font-family: '微软雅黑',sans-serif;">和</span> [oslo_messaging_rabbit]<span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>``RabbitMQ``<span style="font-family: '微软雅黑',sans-serif;">消息队列的连接：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    rpc_backend </span>=<span style="color: #000000;"> rabbit

    [oslo_messaging_rabbit]
    ...
    rabbit_host </span>=<span style="color: #000000;"> controller
    rabbit_userid </span>=<span style="color: #000000;"> openstack
    rabbit_password </span>= RABBIT_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[keystone_authtoken]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    auth_strategy </span>=<span style="color: #000000;"> keystone

    [keystone_authtoken]
    ...
    auth_uri </span>= http://controller:5000<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    memcached_servers </span>= controller:11211<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> nova
    password </span>= NOVA_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT] <span style="font-family: '微软雅黑',sans-serif;">部分，配置</span> my_ip <span style="font-family: '微软雅黑',sans-serif;">选项：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    my_ip </span>= MANAGEMENT_INTERFACE_IP_ADDRESS</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">注意：</span> <span style="font-family: '微软雅黑',sans-serif;">将其中的</span> <span style="color: red;">MANAGEMENT_INTERFACE_IP_ADDRESS </span><span style="font-family: '微软雅黑',sans-serif;">替换为计算节点上的管理网络接口的</span><span style="color: red;">IP </span><span style="font-family: '微软雅黑',sans-serif;">地址，例如</span> <span style="color: red;">:ref:`example architecture `</span><span style="font-family: '微软雅黑',sans-serif;">中所示的第一个节点</span> <span style="color: red;">10.0.0.31 </span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT] <span style="font-family: '微软雅黑',sans-serif;">部分，使能</span> Networking <span style="font-family: '微软雅黑',sans-serif;">服务：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    use_neutron </span>=<span style="color: #000000;"> True
    firewall_driver </span>= nova.virt.firewall.NoopFirewallDriver</pre>
</div>

<p style="text-indent: 21.0pt;">
   <span style="font-family: '微软雅黑',sans-serif;">在</span>``[vnc]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用并配置远程控制台访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [vnc]
    ...
    enabled </span>=<span style="color: #000000;"> True
    vncserver_listen </span>= 0.0.0.0<span style="color: #000000;">
    vncserver_proxyclient_address </span>=<span style="color: #000000;"> $my_ip
    novncproxy_base_url </span>= http://controller:6080/vnc_auto.html</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [glance] <span style="font-family: '微软雅黑',sans-serif;">区域，配置镜像服务</span> API <span style="font-family: '微软雅黑',sans-serif;">的位置：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [glance]
    ...
    api_servers </span>= http://controller:9292</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [oslo_concurrency] <span style="font-family: '微软雅黑',sans-serif;">部分，配置锁路径：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [oslo_concurrency]
    ...
    lock_path </span>= /var/lib/nova/tmp</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre>cp /etc/nova/<span style="color: #000000;">nova.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/nova/nova.conf.bak &gt;/etc/nova/<span style="color: #000000;">nova.conf
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT enabled_apis  osapi_compute,metadata
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT rpc_backend  rabbit
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT auth_strategy  keystone
openstack</span>-config --set /etc/nova/nova.conf  DEFAULT my_ip  10.0.0.31<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT use_neutron  True
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT firewall_driver  nova.virt.firewall.NoopFirewallDriver
openstack</span>-config --set /etc/nova/nova.conf  glance api_servers  http://controller:9292<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  auth_type  password
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  project_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  user_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  project_name  service
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  username  nova
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  password  NOVA_PASS
openstack</span>-config --set /etc/nova/nova.conf  oslo_concurrency lock_path  /var/lib/nova/<span style="color: #000000;">tmp
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_host  controller
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_userid  openstack
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_password  RABBIT_PASS
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  vnc enabled  True
openstack</span>-config --set /etc/nova/nova.conf  vnc vncserver_listen  0.0.0.0<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  vnc vncserver_proxyclient_address  <span style="color: #800000;">'</span><span style="color: #800000;">$my_ip</span><span style="color: #800000;">'</span><span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  vnc novncproxy_base_url  http://controller:6080/vnc_auto.html</pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">）启动服务</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">确定您的计算节点是否支持虚拟机的硬件加速</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;">  egrep -c '(vmx|svm)' /proc/cpuinfo</span>
1</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">说明：如果这个命令返回了</span> 1 <span style="font-family: '微软雅黑',sans-serif;">或更大的值，那么你的计算节点支持硬件加速且不需要额外的配置。</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">启动，开机自启动</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl enable libvirtd.service openstack-nova-<span style="color: #000000;">compute.service
systemctl start libvirtd.service openstack</span>-nova-<span style="color: #000000;">compute.service
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 查看状态</span>
systemctl status libvirtd.service openstack-nova-compute.service</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在控制节点查看计算节点状态</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> source admin-openrc </span>
[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack compute service list </span>
+----+------------------+------------+----------+---------+-------+----------------------------+
| Id | Binary           | Host       | Zone     | Status  | State | Updated At                 |
+----+------------------+------------+----------+---------+-------+----------------------------+
|  1 | nova-scheduler   | controller | internal | enabled | up    | 2018-01-23T12:02:04.000000 |
|  2 | nova-conductor   | controller | internal | enabled | up    | 2018-01-23T12:02:03.000000 |
|  3 | nova-consoleauth | controller | internal | enabled | up    | 2018-01-23T12:02:05.000000 |
|  6 | nova-compute     | compute1   | nova     | enabled | up    | 2018-01-23T12:02:05.000000 |
+----+------------------+------------+----------+---------+-------+----------------------------+</pre>
</div>

### <span id="163">1.6.3 <span style="font-family: '微软雅黑',sans-serif;">验证服务</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在进行下一步操作之前，先验证之前部署的服务是否正常。</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意：</span> <span style="font-family: '微软雅黑',sans-serif;">执行命令前需先加载环境变量脚本</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 检查认证服务</span>
<span style="color: #000000;">openstack user list 
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 检查镜像服务</span>
<span style="color: #000000;">openstack image list 
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 检查计算服务</span>
openstack compute service list</pre>
</div>

## <span id="17_Networkingneutron">1.7 Networking(neutron)<span style="font-family: '微软雅黑',sans-serif;">服务</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron.html</a>
</p>

### <span id="171">1.7.1 <span style="font-family: '微软雅黑',sans-serif;">安装并配置控制节点</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">以下全命令全在</span> controller <span style="font-family: '微软雅黑',sans-serif;">主机中执行</span>
</p>

   <span style="font-family: '微软雅黑',sans-serif;">参考文献：</span><https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-controller-install.html>

1<span style="font-family: '微软雅黑',sans-serif;">）在数据库中，创库，授权</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">连接到数据库服务器</span>
</p>

<div class="cnblogs_code">
  <pre>mysql</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span>``neutron`` <span style="font-family: '微软雅黑',sans-serif;">数据库</span>
</p>

<div class="cnblogs_code">
  <pre>CREATE DATABASE neutron;</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">对</span>``neutron`` <span style="font-family: '微软雅黑',sans-serif;">数据库授予合适的访问权限</span>
</p>

<div class="cnblogs_code">
  <pre>GRANT ALL PRIVILEGES ON neutron.* TO <span style="color: #800000;">'</span><span style="color: #800000;">neutron</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">NEUTRON_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON neutron.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">neutron</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">NEUTRON_DBPASS</span><span style="color: #800000;">'</span>;</pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">）在</span>keystone<span style="font-family: '微软雅黑',sans-serif;">中创建用户并授权</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span>``neutron``<span style="font-family: '微软雅黑',sans-serif;">用户</span>
</p>

<div class="cnblogs_code">
  <pre>openstack user create --domain default --password NEUTRON_PASS neutron</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">添加</span>``admin`` <span style="font-family: '微软雅黑',sans-serif;">角色到</span>``neutron`` <span style="font-family: '微软雅黑',sans-serif;">用户</span>
</p>

<div class="cnblogs_code">
  <pre>openstack role add --project service --user neutron admin</pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">）在</span>keystone<span style="font-family: '微软雅黑',sans-serif;">中创建服务实体，和注册</span>API<span style="font-family: '微软雅黑',sans-serif;">接口</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span>``neutron``<span style="font-family: '微软雅黑',sans-serif;">服务实体</span>
</p>

<div class="cnblogs_code">
  <pre>openstack service create --name neutron --description <span style="color: #800000;">"</span><span style="color: #800000;">OpenStack Networking</span><span style="color: #800000;">"</span> network</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建网络服务</span>API<span style="font-family: '微软雅黑',sans-serif;">端点</span>
</p>

<div class="cnblogs_code">
  <pre>openstack endpoint create --region RegionOne network public http://controller:9696<span style="color: #000000;">
openstack endpoint create </span>--region RegionOne network internal http://controller:9696<span style="color: #000000;">
openstack endpoint create </span>--region RegionOne network admin http://controller:9696</pre>
</div>

4<span style="font-family: '微软雅黑',sans-serif;">）安装软件包</span>

<span style="font-family: '微软雅黑',sans-serif;">这这里我选用的时<em>’网络选项</em></span>_1__<span style="font-family: '微软雅黑',sans-serif;">：公共网络‘</span>_ <span style="font-family: '微软雅黑',sans-serif;">该网络模式较为简单。</span>

<span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-controller-install-option1.html>

<span style="font-family: '微软雅黑',sans-serif;">安装软件包</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-neutron openstack-neutron-ml2 openstack-neutron-linuxbridge ebtables</pre>
</div>

5<span style="font-family: '微软雅黑',sans-serif;">）修改配置文件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">①</span> <span style="font-family: '微软雅黑',sans-serif;">编辑</span><span style="background: aqua;">``/etc/neutron/neutron.conf`` </span><span style="font-family: '微软雅黑',sans-serif;">文件并完成如下操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [database] <span style="font-family: '微软雅黑',sans-serif;">部分，配置数据库访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [database]
    ...
    connection </span>= mysql+pymysql://neutron:NEUTRON_DBPASS@controller/neutron</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用</span>ML2<span style="font-family: '微软雅黑',sans-serif;">插件并禁用其他插件</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    core_plugin </span>=<span style="color: #000000;"> ml2
    service_plugins </span>=</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[oslo_messaging_rabbit]<span style="font-family: '微软雅黑',sans-serif;">”部分，配置</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>RabbitMQ<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">消息队列的连接</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    rpc_backend </span>=<span style="color: #000000;"> rabbit

    [oslo_messaging_rabbit]
    ...
    rabbit_host </span>=<span style="color: #000000;"> controller
    rabbit_userid </span>=<span style="color: #000000;"> openstack
    rabbit_password </span>= RABBIT_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[keystone_authtoken]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    auth_strategy </span>=<span style="color: #000000;"> keystone

    [keystone_authtoken]
    ...
    auth_uri </span>= http://controller:5000<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    memcached_servers </span>= controller:11211<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> neutron
    password </span>= NEUTRON_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]``<span style="font-family: '微软雅黑',sans-serif;">和</span>``[nova]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置网络服务来通知计算节点的网络拓扑变化</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    notify_nova_on_port_status_changes </span>=<span style="color: #000000;"> True
    notify_nova_on_port_data_changes </span>=<span style="color: #000000;"> True

    [nova]
    ...
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    region_name </span>=<span style="color: #000000;"> RegionOne
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> nova
    password </span>= NOVA_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [oslo_concurrency] <span style="font-family: '微软雅黑',sans-serif;">部分，配置锁路径</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [oslo_concurrency]
    ...
    lock_path </span>= /var/lib/neutron/tmp</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre>cp /etc/neutron/<span style="color: #000000;">neutron.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/neutron/neutron.conf.bak &gt;/etc/neutron/<span style="color: #000000;">neutron.conf
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT core_plugin  ml2
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT service_plugins
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT rpc_backend  rabbit
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT auth_strategy  keystone
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT notify_nova_on_port_status_changes  True
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT notify_nova_on_port_data_changes  True
openstack</span>-config --set /etc/neutron/neutron.conf  database connection  mysql+pymysql://neutron:NEUTRON_DBPASS@controller/<span style="color: #000000;">neutron
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken auth_type  password
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken project_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken user_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken project_name  service
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken username  neutron
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken password  NEUTRON_PASS
openstack</span>-config --set /etc/neutron/neutron.conf  nova auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  nova auth_type  password 
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  nova project_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  nova user_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  nova region_name  RegionOne
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  nova project_name  service
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  nova username  nova
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  nova password  NOVA_PASS
openstack</span>-config --set /etc/neutron/neutron.conf  oslo_concurrency lock_path  /var/lib/neutron/<span style="color: #000000;">tmp
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  oslo_messaging_rabbit rabbit_host  controller
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  oslo_messaging_rabbit rabbit_userid  openstack
openstack</span>-config --set /etc/neutron/neutron.conf  oslo_messaging_rabbit rabbit_password  RABBIT_PASS</pre>
</div>

<span style="font-family: 宋体; background: aqua;">② 配置 Modular Layer 2 (ML2) 插件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/neutron/plugins/ml2/ml2_conf.ini``<span style="font-family: '微软雅黑',sans-serif;">文件并完成以下操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用</span>flat<span style="font-family: '微软雅黑',sans-serif;">和</span>VLAN<span style="font-family: '微软雅黑',sans-serif;">网络</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [ml2]
    ...
    type_drivers </span>= flat,vlan</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2]``<span style="font-family: '微软雅黑',sans-serif;">部分，禁用私有网络</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [ml2]
    ...
    tenant_network_types </span>=</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">机制</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [ml2]
    ...
    mechanism_drivers </span>= linuxbridge</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2]`` <span style="font-family: '微软雅黑',sans-serif;">部分，启用端口安全扩展驱动</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [ml2]
    ...
    extension_drivers </span>= port_security</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2_type_flat]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置公共虚拟网络为</span>flat<span style="font-family: '微软雅黑',sans-serif;">网络</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [ml2_type_flat]
    ...
    flat_networks </span>= provider</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> ``[securitygroup]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用</span> ipset <span style="font-family: '微软雅黑',sans-serif;">增加安全组规则的高效性</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [securitygroup]
    ...
    enable_ipset </span>= True</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集</span>

<div class="cnblogs_code">
  <pre>cp /etc/neutron/plugins/ml2/<span style="color: #000000;">ml2_conf.ini{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/neutron/plugins/ml2/ml2_conf.ini.bak &gt;/etc/neutron/plugins/ml2/<span style="color: #000000;">ml2_conf.ini
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">ml2_conf.ini  ml2 type_drivers  flat,vlan
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">ml2_conf.ini  ml2 tenant_network_types 
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">ml2_conf.ini  ml2 mechanism_drivers  linuxbridge
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">ml2_conf.ini  ml2 extension_drivers  port_security
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">ml2_conf.ini  ml2_type_flat flat_networks  provider
openstack</span>-config --set /etc/neutron/plugins/ml2/ml2_conf.ini  securitygroup enable_ipset  True</pre>
</div>

<span style="font-family: 宋体; background: aqua;">③ 配置Linuxbridge代理</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/neutron/plugins/ml2/linuxbridge_agent.ini``<span style="font-family: '微软雅黑',sans-serif;">文件并且完成以下操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[linux_bridge]``<span style="font-family: '微软雅黑',sans-serif;">部分，将公共虚拟网络和公共物理网络接口对应起来</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [linux_bridge]
    physical_interface_mappings </span>= provider:PROVIDER_INTERFACE_NAME</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">注意：将</span><span style="color: red;">``PUBLIC_INTERFACE_NAME`` </span><span style="font-family: '微软雅黑',sans-serif;">替换为底层的物理公共网络接口，例如</span><span style="color: red;">eth0</span><span style="font-family: '微软雅黑',sans-serif;">。</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[vxlan]``<span style="font-family: '微软雅黑',sans-serif;">部分，禁止</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">覆盖网络</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [vxlan]
    enable_vxlan </span>= False</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">在</span> \`\`[securitygroup]\`\`<span style="font-family: '微软雅黑',sans-serif;">部分，启用安全组并配置</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [securitygroup]
    ...
    enable_security_group </span>=<span style="color: #000000;"> True
    firewall_driver </span>= neutron.agent.linux.iptables_firewall.IptablesFirewallDriver</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集</span>

<div class="cnblogs_code">
  <pre>cp /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/neutron/plugins/ml2/linuxbridge_agent.ini.bak &gt;/etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  linux_bridge physical_interface_mappings  provider:eth0
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  securitygroup enable_security_group  True
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  securitygroup firewall_driver  neutron.agent.linux.iptables_firewall.IptablesFirewallDriver
openstack</span>-config --set /etc/neutron/plugins/ml2/linuxbridge_agent.ini  vxlan enable_vxlan  False</pre>
</div>

<span style="font-family: 宋体; background: aqua;">④ 配置DHCP代理</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/neutron/dhcp_agent.ini``<span style="font-family: '微软雅黑',sans-serif;">文件并完成下面的操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">驱动接口，</span>DHCP<span style="font-family: '微软雅黑',sans-serif;">驱动并启用隔离元数据，这样在公共网络上的实例就可以通过网络来访问元数据</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    interface_driver </span>=<span style="color: #000000;"> neutron.agent.linux.interface.BridgeInterfaceDriver
    dhcp_driver </span>=<span style="color: #000000;"> neutron.agent.linux.dhcp.Dnsmasq
    enable_isolated_metadata </span>= True</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">neutron.agent.linux.interface.BridgeInterfaceDriver
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">dhcp_agent.ini  DEFAULT dhcp_driver neutron.agent.linux.dhcp.Dnsmasq
openstack</span>-config --set /etc/neutron/dhcp_agent.ini  DEFAULT enable_isolated_metadata true</pre>
</div>

<p style="text-indent: 24.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">⑤</span> <span style="font-family: '微软雅黑',sans-serif;">配置元数据代理</span>
</p>

<span style="font-family: '微软雅黑',sans-serif;">编辑</span>\`\`/etc/neutron/metadata_agent.ini\`\`<span style="font-family: '微软雅黑',sans-serif;">文件并完成以下操作</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]`` <span style="font-family: '微软雅黑',sans-serif;">部分，配置元数据主机以及共享密码</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    nova_metadata_ip </span>=<span style="color: #000000;"> controller
    metadata_proxy_shared_secret </span>= METADATA_SECRET</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre>openstack-config --set /etc/neutron/<span style="color: #000000;">metadata_agent.ini DEFAULT nova_metadata_ip  controller
openstack</span>-config --set /etc/neutron/metadata_agent.ini DEFAULT metadata_proxy_shared_secret  METADATA_SECRET</pre>
</div>

<span style="font-family: 宋体; background: aqua;">⑥ 为nove配置网络服务</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">再次编辑</span>``/etc/nova/nova.conf``<span style="font-family: '微软雅黑',sans-serif;">文件并完成以下操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[neutron]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置访问参数，启用元数据代理并设置密码</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [neutron]
    ...
    url </span>= http://controller:9696<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    region_name </span>=<span style="color: #000000;"> RegionOne
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> neutron
    password </span>=<span style="color: #000000;"> NEUTRON_PASS

    service_metadata_proxy </span>=<span style="color: #000000;"> True
    metadata_proxy_shared_secret </span>= METADATA_SECRET</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集</span>

<div class="cnblogs_code">
  <pre>openstack-config --set /etc/nova/nova.conf  neutron url  http://controller:9696<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  neutron auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron auth_type  password
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron project_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron user_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron region_name  RegionOne
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron project_name  service
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron username  neutron
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron password  NEUTRON_PASS
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron service_metadata_proxy  True
openstack</span>-config --set /etc/nova/nova.conf  neutron metadata_proxy_shared_secret  METADATA_SECRET</pre>
</div>

6<span style="font-family: '微软雅黑',sans-serif;">）同步数据库</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">网络服务初始化脚本需要一个超链接</span> /etc/neutron/plugin.ini``<span style="font-family: '微软雅黑',sans-serif;">指向</span>ML2<span style="font-family: '微软雅黑',sans-serif;">插件配置文件</span>/etc/neutron/plugins/ml2/ml2_conf.ini``<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">如果超链接不存在，使用下面的命令创建它</span>
</p>

<div class="cnblogs_code">
  <pre>ln -s /etc/neutron/plugins/ml2/ml2_conf.ini /etc/neutron/plugin.ini</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">同步数据库</span>
</p>

<div class="cnblogs_code">
  <pre>su -s /bin/sh -c <span style="color: #800000;">"</span><span style="color: #800000;">neutron-db-manage --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugins/ml2/ml2_conf.ini upgrade head</span><span style="color: #800000;">"</span> neutron</pre>
</div>

7<span style="font-family: '微软雅黑',sans-serif;">）启动服务</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">重启计算</span>API <span style="font-family: '微软雅黑',sans-serif;">服务</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl restart openstack-nova-api.service</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">当系统启动时，启动</span> Networking <span style="font-family: '微软雅黑',sans-serif;">服务并配置它启动。</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl enable neutron-server.service neutron-linuxbridge-agent.service neutron-dhcp-agent.service neutron-metadata-<span style="color: #000000;">agent.service
systemctl start neutron</span>-server.service neutron-linuxbridge-agent.service neutron-dhcp-agent.service neutron-metadata-<span style="color: #000000;">agent.service
systemctl status neutron</span>-server.service neutron-linuxbridge-agent.service neutron-dhcp-agent.service neutron-metadata-agent.service</pre>
</div>

### <span id="172">1.7.2 <span style="font-family: '微软雅黑',sans-serif;">安装和配置计算节点</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-compute-install.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-compute-install.html</a>
</p>

1<span style="font-family: '微软雅黑',sans-serif;">）安装组件</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-neutron-linuxbridge ebtables ipset</pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">）修改配置文件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在计算节点配置选择</span> <span style="font-family: '微软雅黑',sans-serif;">网络选项</span><span style="color: #ffc000; background: darkcyan;">1</span><span style="font-family: '微软雅黑',sans-serif;">：公共网络，与控制节点相同</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">①</span> <span style="font-family: '微软雅黑',sans-serif;">编辑</span><span style="background: aqua;">``/etc/neutron/neutron.conf`` </span><span style="font-family: '微软雅黑',sans-serif;">文件并完成如下操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[oslo_messaging_rabbit]<span style="font-family: '微软雅黑',sans-serif;">”部分，配置</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>RabbitMQ<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">消息队列的连接</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    rpc_backend </span>=<span style="color: #000000;"> rabbit

    [oslo_messaging_rabbit]
    ...
    rabbit_host </span>=<span style="color: #000000;"> controller
    rabbit_userid </span>=<span style="color: #000000;"> openstack
    rabbit_password </span>= RABBIT_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[keystone_authtoken]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [DEFAULT]
    ...
    auth_strategy </span>=<span style="color: #000000;"> keystone

    [keystone_authtoken]
    ...
    auth_uri </span>= http://controller:5000<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    memcached_servers </span>= controller:11211<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> neutron
    password </span>= NEUTRON_PASS</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">在</span> [oslo_concurrency] <span style="font-family: '微软雅黑',sans-serif;">部分，配置锁路径</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [oslo_concurrency]
    ...
    lock_path </span>= /var/lib/neutron/tmp</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集</span>

<div class="cnblogs_code">
  <pre>cp /etc/neutron/<span style="color: #000000;">neutron.conf{,.bak}
grep </span>-Ev <span style="color: #800000;">'</span><span style="color: #800000;">^$|#</span><span style="color: #800000;">'</span> /etc/neutron/neutron.conf.bak &gt;/etc/neutron/<span style="color: #000000;">neutron.conf
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT rpc_backend  rabbit
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT auth_strategy  keystone
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken auth_type  password
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken project_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken user_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken project_name  service
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken username  neutron
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken password  NEUTRON_PASS
openstack</span>-config --set /etc/neutron/neutron.conf  oslo_concurrency lock_path  /var/lib/neutron/<span style="color: #000000;">tmp
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  oslo_messaging_rabbit rabbit_host  controller
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  oslo_messaging_rabbit rabbit_userid  openstack
openstack</span>-config --set /etc/neutron/neutron.conf  oslo_messaging_rabbit rabbit_password  RABBIT_PASS</pre>
</div>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 宋体; background: aqua;">② </span><span style="font-family: '微软雅黑',sans-serif;">配置</span><span style="background: aqua;">Linuxbridge</span><span style="font-family: '微软雅黑',sans-serif;">代理</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/neutron/plugins/ml2/linuxbridge_agent.ini``<span style="font-family: '微软雅黑',sans-serif;">文件并且完成以下操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[linux_bridge]``<span style="font-family: '微软雅黑',sans-serif;">部分，将公共虚拟网络和公共物理网络接口对应起来</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [linux_bridge]
    physical_interface_mappings </span>= provider:PROVIDER_INTERFACE_NAME</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">注意：将</span><span style="color: red;">``PUBLIC_INTERFACE_NAME`` </span><span style="font-family: '微软雅黑',sans-serif;">替换为底层的物理公共网络接口，例如</span><span style="color: red;">eth0</span><span style="font-family: '微软雅黑',sans-serif;">。</span>     <span style="font-family: '微软雅黑',sans-serif;">在</span>\`\`[vxlan]\`\`<span style="font-family: '微软雅黑',sans-serif;">部分，禁止</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">覆盖网络</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [vxlan]        
    enable_vxlan </span>= False</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">在</span> \`\`[securitygroup]\`\`<span style="font-family: '微软雅黑',sans-serif;">部分，启用安全组并配置</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [securitygroup]        
    ...        
    enable_security_group </span>=<span style="color: #000000;"> True        
    firewall_driver </span>= neutron.agent.linux.iptables_firewall.IptablesFirewallDriver</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre>cp /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/neutron/plugins/ml2/linuxbridge_agent.ini.bak &gt;/etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  linux_bridge physical_interface_mappings  provider:eth0
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  securitygroup enable_security_group  True
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  securitygroup firewall_driver  neutron.agent.linux.iptables_firewall.IptablesFirewallDriver
openstack</span>-config --set /etc/neutron/plugins/ml2/linuxbridge_agent.ini  vxlan enable_vxlan  False</pre>
</div>

<span style="font-family: 宋体; background: aqua;">③ 为计算节点配置网络服务</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/nova/nova.conf``<span style="font-family: '微软雅黑',sans-serif;">文件并完成下面的操作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[neutron]`` <span style="font-family: '微软雅黑',sans-serif;">部分，配置访问参数</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    [neutron]
    ...
    url </span>= http://controller:9696<span style="color: #000000;">
    auth_url </span>= http://controller:35357<span style="color: #000000;">
    auth_type </span>=<span style="color: #000000;"> password
    project_domain_name </span>=<span style="color: #000000;"> default
    user_domain_name </span>=<span style="color: #000000;"> default
    region_name </span>=<span style="color: #000000;"> RegionOne
    project_name </span>=<span style="color: #000000;"> service
    username </span>=<span style="color: #000000;"> neutron
    password </span>= NEUTRON_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">命令集</span>
</p>

<div class="cnblogs_code">
  <pre>openstack-config --set /etc/nova/nova.conf  neutron url  http://controller:9696<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  neutron auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron auth_type  password
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron project_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron user_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron region_name  RegionOne
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron project_name  service
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron username  neutron
openstack</span>-config --set /etc/nova/nova.conf  neutron password  NEUTRON_PASS</pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">）启动服务</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">重启计算服务</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl restart openstack-nova-compute.service</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">启动</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">代理并配置它开机自启动</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl enable neutron-linuxbridge-<span style="color: #000000;">agent.service
systemctl start neutron</span>-linuxbridge-agent.service</pre>
</div>

### <span id="173">1.7.3 <span style="font-family: '微软雅黑',sans-serif;">验证操作</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方验证方法</span>
</p>

> <p style="margin-left: 21.0pt;">
>    https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-verify.html
> </p>
> 
> <p style="margin-left: 21.0pt;">
>    https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-verify-option1.html
> </p>
> 
> <p style="margin-left: 21.0pt;">
>    # <span style="font-family: 微软雅黑, sans-serif; text-indent: 15.75pt; color: #000000;">在这里，我只进行验证网络，网络正常说明服务正常</span>
> </p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> neutron agent-list </span>
+--------------------------------------+--------------------+------------+-------------------+-------+----------------+---------------------------+
| id                                   | agent_type         | host       | availability_zone | alive | admin_state_up | binary                    |
+--------------------------------------+--------------------+------------+-------------------+-------+----------------+---------------------------+
| 3ab2f17f-737e-4c3f-86f0-2289c56a541b | DHCP agent         | controller | nova              | :-)   | True           | neutron-dhcp-agent        |
| 4f64caf6-a9b0-4742-b0d1-0d961063200a | Linux bridge agent | controller |                   | :-)   | True           | neutron-linuxbridge-agent |
| 630540de-d0a0-473b-96b5-757afc1057de | Linux bridge agent | compute1   |                   | :-)   | True           | neutron-linuxbridge-agent |
| 9989ddcb-6aba-4b7f-9bd7-7d61f774f2bb | Metadata agent     | controller |                   | :-)   | True           | neutron-metadata-agent    |
+--------------------------------------+--------------------+------------+-------------------+-------+----------------+---------------------------+</pre>
</div>

## <span id="18_Dashboardhorizon-web">1.8 Dashboard<span style="font-family: '微软雅黑',sans-serif;">（</span>horizon-web<span style="font-family: '微软雅黑',sans-serif;">界面）安装</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/horizon.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/horizon.html</a>
</p>

### <span id="181">1.8.1 <span style="font-family: '微软雅黑',sans-serif;">安全并配置组件</span><span style="font-family: '微软雅黑',sans-serif;">（单独主机安装）</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">查考文献：</span>https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/horizon-install.html#install-and-configure-components
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">安装软件包</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum -y install openstack-dashboard</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">由于</span>Dashboard<span style="font-family: '微软雅黑',sans-serif;">服务需要使用到</span>httpd<span style="font-family: '微软雅黑',sans-serif;">服务，安装在控制节点，可能回影响到</span>Keystone<span style="font-family: '微软雅黑',sans-serif;">服务的正常运行，所以选择单独安装，与官方文档略有不同。</span>

### <span id="182">1.8.2 <span style="font-family: '微软雅黑',sans-serif;">修改配置文件</span></span>

<span style="font-family: '微软雅黑',sans-serif;">编辑文件</span> /etc/openstack-dashboard/local_settings <span style="font-family: '微软雅黑',sans-serif;">并完成如下动作</span>

<span style="font-family: '微软雅黑',sans-serif;">在</span> controller <span style="font-family: '微软雅黑',sans-serif;">节点上配置仪表盘以使用</span> OpenStack <span style="font-family: '微软雅黑',sans-serif;">服务</span>

<div class="cnblogs_code">
  <pre>    OPENSTACK_HOST = <span style="color: #800000;">"</span><span style="color: #800000;">controller</span><span style="color: #800000;">"</span>
    <span style="color: #008000;">#</span><span style="color: #008000;"> 指向认证服务的地址=keystone</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">允许所有主机访问仪表板</span>

<div class="cnblogs_code">
  <pre>ALLOWED_HOSTS = [<span style="color: #800000;">'</span><span style="color: #800000;">*</span><span style="color: #800000;">'</span>, ]</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置</span> memcached <span style="font-family: '微软雅黑',sans-serif;">会话存储服务</span>

<div class="cnblogs_code">
  <pre>    SESSION_ENGINE = <span style="color: #800000;">'</span><span style="color: #800000;">django.contrib.sessions.backends.cache</span><span style="color: #800000;">'</span><span style="color: #000000;">

    CACHES </span>=<span style="color: #000000;"> {
        </span><span style="color: #800000;">'</span><span style="color: #800000;">default</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
             </span><span style="color: #800000;">'</span><span style="color: #800000;">BACKEND</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">django.core.cache.backends.memcached.MemcachedCache</span><span style="color: #800000;">'</span><span style="color: #000000;">,
             </span><span style="color: #800000;">'</span><span style="color: #800000;">LOCATION</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">controller:11211</span><span style="color: #800000;">'</span><span style="color: #000000;">,
        }
    }</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启用第</span>3<span style="font-family: '微软雅黑',sans-serif;">版认证</span>API:

<div class="cnblogs_code">
  <pre>OPENSTACK_KEYSTONE_URL = <span style="color: #800000;">"</span><span style="color: #800000;">http://%s:5000/v3</span><span style="color: #800000;">"</span> % OPENSTACK_HOST</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启用对域的支持</span>

<div class="cnblogs_code">
  <pre>OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT = True</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置</span>API<span style="font-family: '微软雅黑',sans-serif;">版本</span>

<div class="cnblogs_code">
  <pre>    OPENSTACK_API_VERSIONS =<span style="color: #000000;"> {
        </span><span style="color: #800000;">"</span><span style="color: #800000;">identity</span><span style="color: #800000;">"</span>: 3<span style="color: #000000;">,
        </span><span style="color: #800000;">"</span><span style="color: #800000;">image</span><span style="color: #800000;">"</span>: 2<span style="color: #000000;">,
        </span><span style="color: #800000;">"</span><span style="color: #800000;">volume</span><span style="color: #800000;">"</span>: 2<span style="color: #000000;">,
    }</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">通过仪表盘创建用户时的默认域配置为</span> default :

<div class="cnblogs_code">
  <pre>OPENSTACK_KEYSTONE_DEFAULT_DOMAIN = <span style="color: #800000;">"</span><span style="color: #800000;">default</span><span style="color: #800000;">"</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">通过仪表盘创建的用户默认角色配置为</span> user

<div class="cnblogs_code">
  <pre>OPENSTACK_KEYSTONE_DEFAULT_ROLE = <span style="color: #800000;">"</span><span style="color: #800000;">user</span><span style="color: #800000;">"</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">如果您选择网络选项</span>1<span style="font-family: '微软雅黑',sans-serif;">，需要禁用支持</span>3<span style="font-family: '微软雅黑',sans-serif;">层网络服务</span>

<div class="cnblogs_code">
  <pre>    OPENSTACK_NEUTRON_NETWORK =<span style="color: #000000;"> {
        ...
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_router</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_quotas</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_distributed_router</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_ha_router</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_lb</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_firewall</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_vpn</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
        </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_fip_topology_check</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
    }</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">可以选择性地配置时区</span>

<div class="cnblogs_code">
  <pre>TIME_ZONE = <span style="color: #800000;">"</span><span style="color: #800000;">Asia/Shanghai</span><span style="color: #800000;">"</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">最终配置文件</span>

<div class="cnblogs_code">
  <pre>wget https://files.cnblogs.com/files/clsn/local_settings.zip</pre>
</div>

文件详情：

<div class="cnblogs_code">
  <p>
    <img id="code_img_closed_5f4a0c8d-9018-44f3-8d12-a671a525b950" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-2.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" /><img id="code_img_opened_5f4a0c8d-9018-44f3-8d12-a671a525b950" class="code_img_opened" style="display: none;" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-2.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
  </p>
  
  <div id="cnblogs_code_open_5f4a0c8d-9018-44f3-8d12-a671a525b950" class="cnblogs_code_hide">
    <pre><span style="color: #008080;">  1</span> <span style="color: #008000;">#</span><span style="color: #008000;"> -*- coding: utf-8 -*-</span>
<span style="color: #008080;">  2</span> 
<span style="color: #008080;">  3</span> <span style="color: #0000ff;">import</span><span style="color: #000000;"> os
</span><span style="color: #008080;">  4</span> 
<span style="color: #008080;">  5</span> <span style="color: #0000ff;">from</span> django.utils.translation <span style="color: #0000ff;">import</span><span style="color: #000000;"> ugettext_lazy as _
</span><span style="color: #008080;">  6</span> 
<span style="color: #008080;">  7</span> 
<span style="color: #008080;">  8</span> <span style="color: #0000ff;">from</span> openstack_dashboard <span style="color: #0000ff;">import</span><span style="color: #000000;"> exceptions
</span><span style="color: #008080;">  9</span> <span style="color: #0000ff;">from</span> openstack_dashboard.settings <span style="color: #0000ff;">import</span><span style="color: #000000;"> HORIZON_CONFIG
</span><span style="color: #008080;"> 10</span> 
<span style="color: #008080;"> 11</span> DEBUG =<span style="color: #000000;"> False
</span><span style="color: #008080;"> 12</span> TEMPLATE_DEBUG =<span style="color: #000000;"> DEBUG
</span><span style="color: #008080;"> 13</span> 
<span style="color: #008080;"> 14</span> 
<span style="color: #008080;"> 15</span> <span style="color: #008000;">#</span><span style="color: #008000;"> WEBROOT is the location relative to Webserver root</span>
<span style="color: #008080;"> 16</span> <span style="color: #008000;">#</span><span style="color: #008000;"> should end with a slash.</span>
<span style="color: #008080;"> 17</span> WEBROOT = <span style="color: #800000;">'</span><span style="color: #800000;">/dashboard/</span><span style="color: #800000;">'</span>
<span style="color: #008080;"> 18</span> <span style="color: #008000;">#</span><span style="color: #008000;">LOGIN_URL = WEBROOT + 'auth/login/'</span>
<span style="color: #008080;"> 19</span> <span style="color: #008000;">#</span><span style="color: #008000;">LOGOUT_URL = WEBROOT + 'auth/logout/'</span>
<span style="color: #008080;"> 20</span> <span style="color: #008000;">#
</span><span style="color: #008080;"> 21</span> <span style="color: #008000;">#</span><span style="color: #008000;"> LOGIN_REDIRECT_URL can be used as an alternative for</span>
<span style="color: #008080;"> 22</span> <span style="color: #008000;">#</span><span style="color: #008000;"> HORIZON_CONFIG.user_home, if user_home is not set.</span>
<span style="color: #008080;"> 23</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Do not set it to '/home/', as this will cause circular redirect loop</span>
<span style="color: #008080;"> 24</span> <span style="color: #008000;">#</span><span style="color: #008000;">LOGIN_REDIRECT_URL = WEBROOT</span>
<span style="color: #008080;"> 25</span> 
<span style="color: #008080;"> 26</span> <span style="color: #008000;">#</span><span style="color: #008000;"> If horizon is running in production (DEBUG is False), set this</span>
<span style="color: #008080;"> 27</span> <span style="color: #008000;">#</span><span style="color: #008000;"> with the list of host/domain names that the application can serve.</span>
<span style="color: #008080;"> 28</span> <span style="color: #008000;">#</span><span style="color: #008000;"> For more information see:</span>
<span style="color: #008080;"> 29</span> <span style="color: #008000;">#</span><span style="color: #008000;"> https://docs.djangoproject.com/en/dev/ref/settings/#allowed-hosts</span>
<span style="color: #008080;"> 30</span> ALLOWED_HOSTS = [<span style="color: #800000;">'</span><span style="color: #800000;">*</span><span style="color: #800000;">'</span><span style="color: #000000;">, ]
</span><span style="color: #008080;"> 31</span> 
<span style="color: #008080;"> 32</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Set SSL proxy settings:</span>
<span style="color: #008080;"> 33</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Pass this header from the proxy after terminating the SSL,</span>
<span style="color: #008080;"> 34</span> <span style="color: #008000;">#</span><span style="color: #008000;"> and don't forget to strip it from the client's request.</span>
<span style="color: #008080;"> 35</span> <span style="color: #008000;">#</span><span style="color: #008000;"> For more information see:</span>
<span style="color: #008080;"> 36</span> <span style="color: #008000;">#</span><span style="color: #008000;"> https://docs.djangoproject.com/en/1.8/ref/settings/#secure-proxy-ssl-header</span>
<span style="color: #008080;"> 37</span> <span style="color: #008000;">#</span><span style="color: #008000;">SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')</span>
<span style="color: #008080;"> 38</span> 
<span style="color: #008080;"> 39</span> <span style="color: #008000;">#</span><span style="color: #008000;"> If Horizon is being served through SSL, then uncomment the following two</span>
<span style="color: #008080;"> 40</span> <span style="color: #008000;">#</span><span style="color: #008000;"> settings to better secure the cookies from security exploits</span>
<span style="color: #008080;"> 41</span> <span style="color: #008000;">#</span><span style="color: #008000;">CSRF_COOKIE_SECURE = True</span>
<span style="color: #008080;"> 42</span> <span style="color: #008000;">#</span><span style="color: #008000;">SESSION_COOKIE_SECURE = True</span>
<span style="color: #008080;"> 43</span> 
<span style="color: #008080;"> 44</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The absolute path to the directory where message files are collected.</span>
<span style="color: #008080;"> 45</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The message file must have a .json file extension. When the user logins to</span>
<span style="color: #008080;"> 46</span> <span style="color: #008000;">#</span><span style="color: #008000;"> horizon, the message files collected are processed and displayed to the user.</span>
<span style="color: #008080;"> 47</span> <span style="color: #008000;">#</span><span style="color: #008000;">MESSAGES_PATH=None</span>
<span style="color: #008080;"> 48</span> 
<span style="color: #008080;"> 49</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Overrides for OpenStack API versions. Use this setting to force the</span>
<span style="color: #008080;"> 50</span> <span style="color: #008000;">#</span><span style="color: #008000;"> OpenStack dashboard to use a specific API version for a given service API.</span>
<span style="color: #008080;"> 51</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Versions specified here should be integers or floats, not strings.</span>
<span style="color: #008080;"> 52</span> <span style="color: #008000;">#</span><span style="color: #008000;"> NOTE: The version should be formatted as it appears in the URL for the</span>
<span style="color: #008080;"> 53</span> <span style="color: #008000;">#</span><span style="color: #008000;"> service API. For example, The identity service APIs have inconsistent</span>
<span style="color: #008080;"> 54</span> <span style="color: #008000;">#</span><span style="color: #008000;"> use of the decimal point, so valid options would be 2.0 or 3.</span>
<span style="color: #008080;"> 55</span> OPENSTACK_API_VERSIONS =<span style="color: #000000;"> {
</span><span style="color: #008080;"> 56</span> <span style="color: #008000;">#</span><span style="color: #008000;">    "data-processing": 1.1,</span>
<span style="color: #008080;"> 57</span>     <span style="color: #800000;">"</span><span style="color: #800000;">identity</span><span style="color: #800000;">"</span>: 3<span style="color: #000000;">,
</span><span style="color: #008080;"> 58</span>     <span style="color: #800000;">"</span><span style="color: #800000;">image</span><span style="color: #800000;">"</span>: 2<span style="color: #000000;">,
</span><span style="color: #008080;"> 59</span>     <span style="color: #800000;">"</span><span style="color: #800000;">volume</span><span style="color: #800000;">"</span>: 2<span style="color: #000000;">,
</span><span style="color: #008080;"> 60</span>     <span style="color: #800000;">"</span><span style="color: #800000;">compute</span><span style="color: #800000;">"</span>: 2<span style="color: #000000;">,
</span><span style="color: #008080;"> 61</span> <span style="color: #000000;">}
</span><span style="color: #008080;"> 62</span> 
<span style="color: #008080;"> 63</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Set this to True if running on multi-domain model. When this is enabled, it</span>
<span style="color: #008080;"> 64</span> <span style="color: #008000;">#</span><span style="color: #008000;"> will require user to enter the Domain name in addition to username for login.</span>
<span style="color: #008080;"> 65</span> OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT =<span style="color: #000000;"> True
</span><span style="color: #008080;"> 66</span> 
<span style="color: #008080;"> 67</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Overrides the default domain used when running on single-domain model</span>
<span style="color: #008080;"> 68</span> <span style="color: #008000;">#</span><span style="color: #008000;"> with Keystone V3. All entities will be created in the default domain.</span>
<span style="color: #008080;"> 69</span> <span style="color: #008000;">#</span><span style="color: #008000;"> NOTE: This value must be the ID of the default domain, NOT the name.</span>
<span style="color: #008080;"> 70</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Also, you will most likely have a value in the keystone policy file like this</span>
<span style="color: #008080;"> 71</span> <span style="color: #008000;">#</span><span style="color: #008000;">    "cloud_admin": "rule:admin_required and domain_id:"</span>
<span style="color: #008080;"> 72</span> <span style="color: #008000;">#</span><span style="color: #008000;"> This value must match the domain id specified there.</span>
<span style="color: #008080;"> 73</span> OPENSTACK_KEYSTONE_DEFAULT_DOMAIN = <span style="color: #800000;">'</span><span style="color: #800000;">default</span><span style="color: #800000;">'</span>
<span style="color: #008080;"> 74</span> 
<span style="color: #008080;"> 75</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Set this to True to enable panels that provide the ability for users to</span>
<span style="color: #008080;"> 76</span> <span style="color: #008000;">#</span><span style="color: #008000;"> manage Identity Providers (IdPs) and establish a set of rules to map</span>
<span style="color: #008080;"> 77</span> <span style="color: #008000;">#</span><span style="color: #008000;"> federation protocol attributes to Identity API attributes.</span>
<span style="color: #008080;"> 78</span> <span style="color: #008000;">#</span><span style="color: #008000;"> This extension requires v3.0+ of the Identity API.</span>
<span style="color: #008080;"> 79</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_KEYSTONE_FEDERATION_MANAGEMENT = False</span>
<span style="color: #008080;"> 80</span> 
<span style="color: #008080;"> 81</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Set Console type:</span>
<span style="color: #008080;"> 82</span> <span style="color: #008000;">#</span><span style="color: #008000;"> valid options are "AUTO"(default), "VNC", "SPICE", "RDP", "SERIAL" or None</span>
<span style="color: #008080;"> 83</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Set to None explicitly if you want to deactivate the console.</span>
<span style="color: #008080;"> 84</span> <span style="color: #008000;">#</span><span style="color: #008000;">CONSOLE_TYPE = "AUTO"</span>
<span style="color: #008080;"> 85</span> 
<span style="color: #008080;"> 86</span> <span style="color: #008000;">#</span><span style="color: #008000;"> If provided, a "Report Bug" link will be displayed in the site header</span>
<span style="color: #008080;"> 87</span> <span style="color: #008000;">#</span><span style="color: #008000;"> which links to the value of this setting (ideally a URL containing</span>
<span style="color: #008080;"> 88</span> <span style="color: #008000;">#</span><span style="color: #008000;"> information on how to report issues).</span>
<span style="color: #008080;"> 89</span> <span style="color: #008000;">#</span><span style="color: #008000;">HORIZON_CONFIG["bug_url"] = "http://bug-report.example.com"</span>
<span style="color: #008080;"> 90</span> 
<span style="color: #008080;"> 91</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Show backdrop element outside the modal, do not close the modal</span>
<span style="color: #008080;"> 92</span> <span style="color: #008000;">#</span><span style="color: #008000;"> after clicking on backdrop.</span>
<span style="color: #008080;"> 93</span> <span style="color: #008000;">#</span><span style="color: #008000;">HORIZON_CONFIG["modal_backdrop"] = "static"</span>
<span style="color: #008080;"> 94</span> 
<span style="color: #008080;"> 95</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Specify a regular expression to validate user passwords.</span>
<span style="color: #008080;"> 96</span> <span style="color: #008000;">#</span><span style="color: #008000;">HORIZON_CONFIG["password_validator"] = {</span>
<span style="color: #008080;"> 97</span> <span style="color: #008000;">#</span><span style="color: #008000;">    "regex": '.*',</span>
<span style="color: #008080;"> 98</span> <span style="color: #008000;">#</span><span style="color: #008000;">    "help_text": _("Your password does not meet the requirements."),</span>
<span style="color: #008080;"> 99</span> <span style="color: #008000;">#</span><span style="color: #008000;">}</span>
<span style="color: #008080;">100</span> 
<span style="color: #008080;">101</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Disable simplified floating IP address management for deployments with</span>
<span style="color: #008080;">102</span> <span style="color: #008000;">#</span><span style="color: #008000;"> multiple floating IP pools or complex network requirements.</span>
<span style="color: #008080;">103</span> <span style="color: #008000;">#</span><span style="color: #008000;">HORIZON_CONFIG["simple_ip_management"] = False</span>
<span style="color: #008080;">104</span> 
<span style="color: #008080;">105</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Turn off browser autocompletion for forms including the login form and</span>
<span style="color: #008080;">106</span> <span style="color: #008000;">#</span><span style="color: #008000;"> the database creation workflow if so desired.</span>
<span style="color: #008080;">107</span> <span style="color: #008000;">#</span><span style="color: #008000;">HORIZON_CONFIG["password_autocomplete"] = "off"</span>
<span style="color: #008080;">108</span> 
<span style="color: #008080;">109</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Setting this to True will disable the reveal button for password fields,</span>
<span style="color: #008080;">110</span> <span style="color: #008000;">#</span><span style="color: #008000;"> including on the login form.</span>
<span style="color: #008080;">111</span> <span style="color: #008000;">#</span><span style="color: #008000;">HORIZON_CONFIG["disable_password_reveal"] = False</span>
<span style="color: #008080;">112</span> 
<span style="color: #008080;">113</span> LOCAL_PATH = <span style="color: #800000;">'</span><span style="color: #800000;">/tmp</span><span style="color: #800000;">'</span>
<span style="color: #008080;">114</span> 
<span style="color: #008080;">115</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Set custom secret key:</span>
<span style="color: #008080;">116</span> <span style="color: #008000;">#</span><span style="color: #008000;"> You can either set it to a specific value or you can let horizon generate a</span>
<span style="color: #008080;">117</span> <span style="color: #008000;">#</span><span style="color: #008000;"> default secret key that is unique on this machine, e.i. regardless of the</span>
<span style="color: #008080;">118</span> <span style="color: #008000;">#</span><span style="color: #008000;"> amount of Python WSGI workers (if used behind Apache+mod_wsgi): However,</span>
<span style="color: #008080;">119</span> <span style="color: #008000;">#</span><span style="color: #008000;"> there may be situations where you would want to set this explicitly, e.g.</span>
<span style="color: #008080;">120</span> <span style="color: #008000;">#</span><span style="color: #008000;"> when multiple dashboard instances are distributed on different machines</span>
<span style="color: #008080;">121</span> <span style="color: #008000;">#</span><span style="color: #008000;"> (usually behind a load-balancer). Either you have to make sure that a session</span>
<span style="color: #008080;">122</span> <span style="color: #008000;">#</span><span style="color: #008000;"> gets all requests routed to the same dashboard instance or you set the same</span>
<span style="color: #008080;">123</span> <span style="color: #008000;">#</span><span style="color: #008000;"> SECRET_KEY for all of them.</span>
<span style="color: #008080;">124</span> SECRET_KEY=<span style="color: #800000;">'</span><span style="color: #800000;">65941f1393ea1c265ad7</span><span style="color: #800000;">'</span>
<span style="color: #008080;">125</span> 
<span style="color: #008080;">126</span> <span style="color: #008000;">#</span><span style="color: #008000;"> We recommend you use memcached for development; otherwise after every reload</span>
<span style="color: #008080;">127</span> <span style="color: #008000;">#</span><span style="color: #008000;"> of the django development server, you will have to login again. To use</span>
<span style="color: #008080;">128</span> <span style="color: #008000;">#</span><span style="color: #008000;"> memcached set CACHES to something like</span>
<span style="color: #008080;">129</span> SESSION_ENGINE = <span style="color: #800000;">'</span><span style="color: #800000;">django.contrib.sessions.backends.cache</span><span style="color: #800000;">'</span>
<span style="color: #008080;">130</span> CACHES =<span style="color: #000000;"> {
</span><span style="color: #008080;">131</span>     <span style="color: #800000;">'</span><span style="color: #800000;">default</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">132</span>         <span style="color: #800000;">'</span><span style="color: #800000;">BACKEND</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">django.core.cache.backends.memcached.MemcachedCache</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">133</span>         <span style="color: #800000;">'</span><span style="color: #800000;">LOCATION</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">controller:11211</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">134</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">135</span> <span style="color: #000000;">}
</span><span style="color: #008080;">136</span> 
<span style="color: #008080;">137</span> <span style="color: #008000;">#</span><span style="color: #008000;">CACHES = {</span>
<span style="color: #008080;">138</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'default': {</span>
<span style="color: #008080;">139</span> <span style="color: #008000;">#</span><span style="color: #008000;">        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',</span>
<span style="color: #008080;">140</span> <span style="color: #008000;">#</span><span style="color: #008000;">    },</span>
<span style="color: #008080;">141</span> <span style="color: #008000;">#</span><span style="color: #008000;">}</span>
<span style="color: #008080;">142</span> 
<span style="color: #008080;">143</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Send email to the console by default</span>
<span style="color: #008080;">144</span> EMAIL_BACKEND = <span style="color: #800000;">'</span><span style="color: #800000;">django.core.mail.backends.console.EmailBackend</span><span style="color: #800000;">'</span>
<span style="color: #008080;">145</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Or send them to /dev/null</span>
<span style="color: #008080;">146</span> <span style="color: #008000;">#</span><span style="color: #008000;">EMAIL_BACKEND = 'django.core.mail.backends.dummy.EmailBackend'</span>
<span style="color: #008080;">147</span> 
<span style="color: #008080;">148</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Configure these for your outgoing email host</span>
<span style="color: #008080;">149</span> <span style="color: #008000;">#</span><span style="color: #008000;">EMAIL_HOST = 'smtp.my-company.com'</span>
<span style="color: #008080;">150</span> <span style="color: #008000;">#</span><span style="color: #008000;">EMAIL_PORT = 25</span>
<span style="color: #008080;">151</span> <span style="color: #008000;">#</span><span style="color: #008000;">EMAIL_HOST_USER = 'djangomail'</span>
<span style="color: #008080;">152</span> <span style="color: #008000;">#</span><span style="color: #008000;">EMAIL_HOST_PASSWORD = 'top-secret!'</span>
<span style="color: #008080;">153</span> 
<span style="color: #008080;">154</span> <span style="color: #008000;">#</span><span style="color: #008000;"> For multiple regions uncomment this configuration, and add (endpoint, title).</span>
<span style="color: #008080;">155</span> <span style="color: #008000;">#</span><span style="color: #008000;">AVAILABLE_REGIONS = [</span>
<span style="color: #008080;">156</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ('http://cluster1.example.com:5000/v2.0', 'cluster1'),</span>
<span style="color: #008080;">157</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ('http://cluster2.example.com:5000/v2.0', 'cluster2'),</span>
<span style="color: #008080;">158</span> <span style="color: #008000;">#</span><span style="color: #008000;">]</span>
<span style="color: #008080;">159</span> 
<span style="color: #008080;">160</span> OPENSTACK_HOST = <span style="color: #800000;">"</span><span style="color: #800000;">controller</span><span style="color: #800000;">"</span>
<span style="color: #008080;">161</span> OPENSTACK_KEYSTONE_URL = <span style="color: #800000;">"</span><span style="color: #800000;">http://%s:5000/v3</span><span style="color: #800000;">"</span> %<span style="color: #000000;"> OPENSTACK_HOST
</span><span style="color: #008080;">162</span> OPENSTACK_KEYSTONE_DEFAULT_ROLE = <span style="color: #800000;">"</span><span style="color: #800000;">user</span><span style="color: #800000;">"</span>
<span style="color: #008080;">163</span> 
<span style="color: #008080;">164</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Enables keystone web single-sign-on if set to True.</span>
<span style="color: #008080;">165</span> <span style="color: #008000;">#</span><span style="color: #008000;">WEBSSO_ENABLED = False</span>
<span style="color: #008080;">166</span> 
<span style="color: #008080;">167</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Determines which authentication choice to show as default.</span>
<span style="color: #008080;">168</span> <span style="color: #008000;">#</span><span style="color: #008000;">WEBSSO_INITIAL_CHOICE = "credentials"</span>
<span style="color: #008080;">169</span> 
<span style="color: #008080;">170</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The list of authentication mechanisms which include keystone</span>
<span style="color: #008080;">171</span> <span style="color: #008000;">#</span><span style="color: #008000;"> federation protocols and identity provider/federation protocol</span>
<span style="color: #008080;">172</span> <span style="color: #008000;">#</span><span style="color: #008000;"> mapping keys (WEBSSO_IDP_MAPPING). Current supported protocol</span>
<span style="color: #008080;">173</span> <span style="color: #008000;">#</span><span style="color: #008000;"> IDs are 'saml2' and 'oidc'  which represent SAML 2.0, OpenID</span>
<span style="color: #008080;">174</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Connect respectively.</span>
<span style="color: #008080;">175</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Do not remove the mandatory credentials mechanism.</span>
<span style="color: #008080;">176</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Note: The last two tuples are sample mapping keys to a identity provider</span>
<span style="color: #008080;">177</span> <span style="color: #008000;">#</span><span style="color: #008000;"> and federation protocol combination (WEBSSO_IDP_MAPPING).</span>
<span style="color: #008080;">178</span> <span style="color: #008000;">#</span><span style="color: #008000;">WEBSSO_CHOICES = (</span>
<span style="color: #008080;">179</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ("credentials", _("Keystone Credentials")),</span>
<span style="color: #008080;">180</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ("oidc", _("OpenID Connect")),</span>
<span style="color: #008080;">181</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ("saml2", _("Security Assertion Markup Language")),</span>
<span style="color: #008080;">182</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ("acme_oidc", "ACME - OpenID Connect"),</span>
<span style="color: #008080;">183</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ("acme_saml2", "ACME - SAML2"),</span>
<span style="color: #008080;">184</span> <span style="color: #008000;">#</span><span style="color: #008000;">)</span>
<span style="color: #008080;">185</span> 
<span style="color: #008080;">186</span> <span style="color: #008000;">#</span><span style="color: #008000;"> A dictionary of specific identity provider and federation protocol</span>
<span style="color: #008080;">187</span> <span style="color: #008000;">#</span><span style="color: #008000;"> combinations. From the selected authentication mechanism, the value</span>
<span style="color: #008080;">188</span> <span style="color: #008000;">#</span><span style="color: #008000;"> will be looked up as keys in the dictionary. If a match is found,</span>
<span style="color: #008080;">189</span> <span style="color: #008000;">#</span><span style="color: #008000;"> it will redirect the user to a identity provider and federation protocol</span>
<span style="color: #008080;">190</span> <span style="color: #008000;">#</span><span style="color: #008000;"> specific WebSSO endpoint in keystone, otherwise it will use the value</span>
<span style="color: #008080;">191</span> <span style="color: #008000;">#</span><span style="color: #008000;"> as the protocol_id when redirecting to the WebSSO by protocol endpoint.</span>
<span style="color: #008080;">192</span> <span style="color: #008000;">#</span><span style="color: #008000;"> NOTE: The value is expected to be a tuple formatted as: (, ).</span>
<span style="color: #008080;">193</span> <span style="color: #008000;">#</span><span style="color: #008000;">WEBSSO_IDP_MAPPING = {</span>
<span style="color: #008080;">194</span> <span style="color: #008000;">#</span><span style="color: #008000;">    "acme_oidc": ("acme", "oidc"),</span>
<span style="color: #008080;">195</span> <span style="color: #008000;">#</span><span style="color: #008000;">    "acme_saml2": ("acme", "saml2"),</span>
<span style="color: #008080;">196</span> <span style="color: #008000;">#</span><span style="color: #008000;">}</span>
<span style="color: #008080;">197</span> 
<span style="color: #008080;">198</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Disable SSL certificate checks (useful for self-signed certificates):</span>
<span style="color: #008080;">199</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_SSL_NO_VERIFY = True</span>
<span style="color: #008080;">200</span> 
<span style="color: #008080;">201</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The CA certificate to use to verify SSL connections</span>
<span style="color: #008080;">202</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_SSL_CACERT = '/path/to/cacert.pem'</span>
<span style="color: #008080;">203</span> 
<span style="color: #008080;">204</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The OPENSTACK_KEYSTONE_BACKEND settings can be used to identify the</span>
<span style="color: #008080;">205</span> <span style="color: #008000;">#</span><span style="color: #008000;"> capabilities of the auth backend for Keystone.</span>
<span style="color: #008080;">206</span> <span style="color: #008000;">#</span><span style="color: #008000;"> If Keystone has been configured to use LDAP as the auth backend then set</span>
<span style="color: #008080;">207</span> <span style="color: #008000;">#</span><span style="color: #008000;"> can_edit_user to False and name to 'ldap'.</span>
<span style="color: #008080;">208</span> <span style="color: #008000;">#
</span><span style="color: #008080;">209</span> <span style="color: #008000;">#</span><span style="color: #008000;"> TODO(tres): Remove these once Keystone has an API to identify auth backend.</span>
<span style="color: #008080;">210</span> OPENSTACK_KEYSTONE_BACKEND =<span style="color: #000000;"> {
</span><span style="color: #008080;">211</span>     <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">native</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">212</span>     <span style="color: #800000;">'</span><span style="color: #800000;">can_edit_user</span><span style="color: #800000;">'</span><span style="color: #000000;">: True,
</span><span style="color: #008080;">213</span>     <span style="color: #800000;">'</span><span style="color: #800000;">can_edit_group</span><span style="color: #800000;">'</span><span style="color: #000000;">: True,
</span><span style="color: #008080;">214</span>     <span style="color: #800000;">'</span><span style="color: #800000;">can_edit_project</span><span style="color: #800000;">'</span><span style="color: #000000;">: True,
</span><span style="color: #008080;">215</span>     <span style="color: #800000;">'</span><span style="color: #800000;">can_edit_domain</span><span style="color: #800000;">'</span><span style="color: #000000;">: True,
</span><span style="color: #008080;">216</span>     <span style="color: #800000;">'</span><span style="color: #800000;">can_edit_role</span><span style="color: #800000;">'</span><span style="color: #000000;">: True,
</span><span style="color: #008080;">217</span> <span style="color: #000000;">}
</span><span style="color: #008080;">218</span> 
<span style="color: #008080;">219</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Setting this to True, will add a new "Retrieve Password" action on instance,</span>
<span style="color: #008080;">220</span> <span style="color: #008000;">#</span><span style="color: #008000;"> allowing Admin session password retrieval/decryption.</span>
<span style="color: #008080;">221</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_ENABLE_PASSWORD_RETRIEVE = False</span>
<span style="color: #008080;">222</span> 
<span style="color: #008080;">223</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The Launch Instance user experience has been significantly enhanced.</span>
<span style="color: #008080;">224</span> <span style="color: #008000;">#</span><span style="color: #008000;"> You can choose whether to enable the new launch instance experience,</span>
<span style="color: #008080;">225</span> <span style="color: #008000;">#</span><span style="color: #008000;"> the legacy experience, or both. The legacy experience will be removed</span>
<span style="color: #008080;">226</span> <span style="color: #008000;">#</span><span style="color: #008000;"> in a future release, but is available as a temporary backup setting to ensure</span>
<span style="color: #008080;">227</span> <span style="color: #008000;">#</span><span style="color: #008000;"> compatibility with existing deployments. Further development will not be</span>
<span style="color: #008080;">228</span> <span style="color: #008000;">#</span><span style="color: #008000;"> done on the legacy experience. Please report any problems with the new</span>
<span style="color: #008080;">229</span> <span style="color: #008000;">#</span><span style="color: #008000;"> experience via the Launchpad tracking system.</span>
<span style="color: #008080;">230</span> <span style="color: #008000;">#
</span><span style="color: #008080;">231</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Toggle LAUNCH_INSTANCE_LEGACY_ENABLED and LAUNCH_INSTANCE_NG_ENABLED to</span>
<span style="color: #008080;">232</span> <span style="color: #008000;">#</span><span style="color: #008000;"> determine the experience to enable.  Set them both to true to enable</span>
<span style="color: #008080;">233</span> <span style="color: #008000;">#</span><span style="color: #008000;"> both.</span>
<span style="color: #008080;">234</span> <span style="color: #008000;">#</span><span style="color: #008000;">LAUNCH_INSTANCE_LEGACY_ENABLED = True</span>
<span style="color: #008080;">235</span> <span style="color: #008000;">#</span><span style="color: #008000;">LAUNCH_INSTANCE_NG_ENABLED = False</span>
<span style="color: #008080;">236</span> 
<span style="color: #008080;">237</span> <span style="color: #008000;">#</span><span style="color: #008000;"> A dictionary of settings which can be used to provide the default values for</span>
<span style="color: #008080;">238</span> <span style="color: #008000;">#</span><span style="color: #008000;"> properties found in the Launch Instance modal.</span>
<span style="color: #008080;">239</span> <span style="color: #008000;">#</span><span style="color: #008000;">LAUNCH_INSTANCE_DEFAULTS = {</span>
<span style="color: #008080;">240</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'config_drive': False,</span>
<span style="color: #008080;">241</span> <span style="color: #008000;">#</span><span style="color: #008000;">}</span>
<span style="color: #008080;">242</span> 
<span style="color: #008080;">243</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The Xen Hypervisor has the ability to set the mount point for volumes</span>
<span style="color: #008080;">244</span> <span style="color: #008000;">#</span><span style="color: #008000;"> attached to instances (other Hypervisors currently do not). Setting</span>
<span style="color: #008080;">245</span> <span style="color: #008000;">#</span><span style="color: #008000;"> can_set_mount_point to True will add the option to set the mount point</span>
<span style="color: #008080;">246</span> <span style="color: #008000;">#</span><span style="color: #008000;"> from the UI.</span>
<span style="color: #008080;">247</span> OPENSTACK_HYPERVISOR_FEATURES =<span style="color: #000000;"> {
</span><span style="color: #008080;">248</span>     <span style="color: #800000;">'</span><span style="color: #800000;">can_set_mount_point</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">249</span>     <span style="color: #800000;">'</span><span style="color: #800000;">can_set_password</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">250</span>     <span style="color: #800000;">'</span><span style="color: #800000;">requires_keypair</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">251</span> <span style="color: #000000;">}
</span><span style="color: #008080;">252</span> 
<span style="color: #008080;">253</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The OPENSTACK_CINDER_FEATURES settings can be used to enable optional</span>
<span style="color: #008080;">254</span> <span style="color: #008000;">#</span><span style="color: #008000;"> services provided by cinder that is not exposed by its extension API.</span>
<span style="color: #008080;">255</span> OPENSTACK_CINDER_FEATURES =<span style="color: #000000;"> {
</span><span style="color: #008080;">256</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_backup</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">257</span> <span style="color: #000000;">}
</span><span style="color: #008080;">258</span> 
<span style="color: #008080;">259</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The OPENSTACK_NEUTRON_NETWORK settings can be used to enable optional</span>
<span style="color: #008080;">260</span> <span style="color: #008000;">#</span><span style="color: #008000;"> services provided by neutron. Options currently available are load</span>
<span style="color: #008080;">261</span> <span style="color: #008000;">#</span><span style="color: #008000;"> balancer service, security groups, quotas, VPN service.</span>
<span style="color: #008080;">262</span> OPENSTACK_NEUTRON_NETWORK =<span style="color: #000000;"> {
</span><span style="color: #008080;">263</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_router</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">264</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_quotas</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">265</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_ipv6</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">266</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_distributed_router</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">267</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_ha_router</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">268</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_lb</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">269</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_firewall</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">270</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_vpn</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">271</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_fip_topology_check</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">272</span> 
<span style="color: #008080;">273</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> Neutron can be configured with a default Subnet Pool to be used for IPv4</span>
<span style="color: #008080;">274</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> subnet-allocation. Specify the label you wish to display in the Address</span>
<span style="color: #008080;">275</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> pool selector on the create subnet step if you want to use this feature.</span>
<span style="color: #008080;">276</span>     <span style="color: #800000;">'</span><span style="color: #800000;">default_ipv4_subnet_pool_label</span><span style="color: #800000;">'</span><span style="color: #000000;">: None,
</span><span style="color: #008080;">277</span> 
<span style="color: #008080;">278</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> Neutron can be configured with a default Subnet Pool to be used for IPv6</span>
<span style="color: #008080;">279</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> subnet-allocation. Specify the label you wish to display in the Address</span>
<span style="color: #008080;">280</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> pool selector on the create subnet step if you want to use this feature.</span>
<span style="color: #008080;">281</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> You must set this to enable IPv6 Prefix Delegation in a PD-capable</span>
<span style="color: #008080;">282</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> environment.</span>
<span style="color: #008080;">283</span>     <span style="color: #800000;">'</span><span style="color: #800000;">default_ipv6_subnet_pool_label</span><span style="color: #800000;">'</span><span style="color: #000000;">: None,
</span><span style="color: #008080;">284</span> 
<span style="color: #008080;">285</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> The profile_support option is used to detect if an external router can be</span>
<span style="color: #008080;">286</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> configured via the dashboard. When using specific plugins the</span>
<span style="color: #008080;">287</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> profile_support can be turned on if needed.</span>
<span style="color: #008080;">288</span>     <span style="color: #800000;">'</span><span style="color: #800000;">profile_support</span><span style="color: #800000;">'</span><span style="color: #000000;">: None,
</span><span style="color: #008080;">289</span>     <span style="color: #008000;">#</span><span style="color: #008000;">'profile_support': 'cisco',</span>
<span style="color: #008080;">290</span> 
<span style="color: #008080;">291</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> Set which provider network types are supported. Only the network types</span>
<span style="color: #008080;">292</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> in this list will be available to choose from when creating a network.</span>
<span style="color: #008080;">293</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> Network types include local, flat, vlan, gre, and vxlan.</span>
<span style="color: #008080;">294</span>     <span style="color: #800000;">'</span><span style="color: #800000;">supported_provider_types</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">*</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">295</span> 
<span style="color: #008080;">296</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> Set which VNIC types are supported for port binding. Only the VNIC</span>
<span style="color: #008080;">297</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> types in this list will be available to choose from when creating a</span>
<span style="color: #008080;">298</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> port.</span>
<span style="color: #008080;">299</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> VNIC types include 'normal', 'macvtap' and 'direct'.</span>
<span style="color: #008080;">300</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> Set to empty list or None to disable VNIC type selection.</span>
<span style="color: #008080;">301</span>     <span style="color: #800000;">'</span><span style="color: #800000;">supported_vnic_types</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">*</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">302</span> <span style="color: #000000;">}
</span><span style="color: #008080;">303</span> 
<span style="color: #008080;">304</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The OPENSTACK_HEAT_STACK settings can be used to disable password</span>
<span style="color: #008080;">305</span> <span style="color: #008000;">#</span><span style="color: #008000;"> field required while launching the stack.</span>
<span style="color: #008080;">306</span> OPENSTACK_HEAT_STACK =<span style="color: #000000;"> {
</span><span style="color: #008080;">307</span>     <span style="color: #800000;">'</span><span style="color: #800000;">enable_user_pass</span><span style="color: #800000;">'</span><span style="color: #000000;">: True,
</span><span style="color: #008080;">308</span> <span style="color: #000000;">}
</span><span style="color: #008080;">309</span> 
<span style="color: #008080;">310</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The OPENSTACK_IMAGE_BACKEND settings can be used to customize features</span>
<span style="color: #008080;">311</span> <span style="color: #008000;">#</span><span style="color: #008000;"> in the OpenStack Dashboard related to the Image service, such as the list</span>
<span style="color: #008080;">312</span> <span style="color: #008000;">#</span><span style="color: #008000;"> of supported image formats.</span>
<span style="color: #008080;">313</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_IMAGE_BACKEND = {</span>
<span style="color: #008080;">314</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'image_formats': [</span>
<span style="color: #008080;">315</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('', _('Select format')),</span>
<span style="color: #008080;">316</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('aki', _('AKI - Amazon Kernel Image')),</span>
<span style="color: #008080;">317</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('ami', _('AMI - Amazon Machine Image')),</span>
<span style="color: #008080;">318</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('ari', _('ARI - Amazon Ramdisk Image')),</span>
<span style="color: #008080;">319</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('docker', _('Docker')),</span>
<span style="color: #008080;">320</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('iso', _('ISO - Optical Disk Image')),</span>
<span style="color: #008080;">321</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('ova', _('OVA - Open Virtual Appliance')),</span>
<span style="color: #008080;">322</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('qcow2', _('QCOW2 - QEMU Emulator')),</span>
<span style="color: #008080;">323</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('raw', _('Raw')),</span>
<span style="color: #008080;">324</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('vdi', _('VDI - Virtual Disk Image')),</span>
<span style="color: #008080;">325</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('vhd', _('VHD - Virtual Hard Disk')),</span>
<span style="color: #008080;">326</span> <span style="color: #008000;">#</span><span style="color: #008000;">        ('vmdk', _('VMDK - Virtual Machine Disk')),</span>
<span style="color: #008080;">327</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ],</span>
<span style="color: #008080;">328</span> <span style="color: #008000;">#</span><span style="color: #008000;">}</span>
<span style="color: #008080;">329</span> 
<span style="color: #008080;">330</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The IMAGE_CUSTOM_PROPERTY_TITLES settings is used to customize the titles for</span>
<span style="color: #008080;">331</span> <span style="color: #008000;">#</span><span style="color: #008000;"> image custom property attributes that appear on image detail pages.</span>
<span style="color: #008080;">332</span> IMAGE_CUSTOM_PROPERTY_TITLES =<span style="color: #000000;"> {
</span><span style="color: #008080;">333</span>     <span style="color: #800000;">"</span><span style="color: #800000;">architecture</span><span style="color: #800000;">"</span>: _(<span style="color: #800000;">"</span><span style="color: #800000;">Architecture</span><span style="color: #800000;">"</span><span style="color: #000000;">),
</span><span style="color: #008080;">334</span>     <span style="color: #800000;">"</span><span style="color: #800000;">kernel_id</span><span style="color: #800000;">"</span>: _(<span style="color: #800000;">"</span><span style="color: #800000;">Kernel ID</span><span style="color: #800000;">"</span><span style="color: #000000;">),
</span><span style="color: #008080;">335</span>     <span style="color: #800000;">"</span><span style="color: #800000;">ramdisk_id</span><span style="color: #800000;">"</span>: _(<span style="color: #800000;">"</span><span style="color: #800000;">Ramdisk ID</span><span style="color: #800000;">"</span><span style="color: #000000;">),
</span><span style="color: #008080;">336</span>     <span style="color: #800000;">"</span><span style="color: #800000;">image_state</span><span style="color: #800000;">"</span>: _(<span style="color: #800000;">"</span><span style="color: #800000;">Euca2ools state</span><span style="color: #800000;">"</span><span style="color: #000000;">),
</span><span style="color: #008080;">337</span>     <span style="color: #800000;">"</span><span style="color: #800000;">project_id</span><span style="color: #800000;">"</span>: _(<span style="color: #800000;">"</span><span style="color: #800000;">Project ID</span><span style="color: #800000;">"</span><span style="color: #000000;">),
</span><span style="color: #008080;">338</span>     <span style="color: #800000;">"</span><span style="color: #800000;">image_type</span><span style="color: #800000;">"</span>: _(<span style="color: #800000;">"</span><span style="color: #800000;">Image Type</span><span style="color: #800000;">"</span><span style="color: #000000;">),
</span><span style="color: #008080;">339</span> <span style="color: #000000;">}
</span><span style="color: #008080;">340</span> 
<span style="color: #008080;">341</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The IMAGE_RESERVED_CUSTOM_PROPERTIES setting is used to specify which image</span>
<span style="color: #008080;">342</span> <span style="color: #008000;">#</span><span style="color: #008000;"> custom properties should not be displayed in the Image Custom Properties</span>
<span style="color: #008080;">343</span> <span style="color: #008000;">#</span><span style="color: #008000;"> table.</span>
<span style="color: #008080;">344</span> IMAGE_RESERVED_CUSTOM_PROPERTIES =<span style="color: #000000;"> []
</span><span style="color: #008080;">345</span> 
<span style="color: #008080;">346</span> <span style="color: #008000;">#</span><span style="color: #008000;"> OPENSTACK_ENDPOINT_TYPE specifies the endpoint type to use for the endpoints</span>
<span style="color: #008080;">347</span> <span style="color: #008000;">#</span><span style="color: #008000;"> in the Keystone service catalog. Use this setting when Horizon is running</span>
<span style="color: #008080;">348</span> <span style="color: #008000;">#</span><span style="color: #008000;"> external to the OpenStack environment. The default is 'publicURL'.</span>
<span style="color: #008080;">349</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_ENDPOINT_TYPE = "publicURL"</span>
<span style="color: #008080;">350</span> 
<span style="color: #008080;">351</span> <span style="color: #008000;">#</span><span style="color: #008000;"> SECONDARY_ENDPOINT_TYPE specifies the fallback endpoint type to use in the</span>
<span style="color: #008080;">352</span> <span style="color: #008000;">#</span><span style="color: #008000;"> case that OPENSTACK_ENDPOINT_TYPE is not present in the endpoints</span>
<span style="color: #008080;">353</span> <span style="color: #008000;">#</span><span style="color: #008000;"> in the Keystone service catalog. Use this setting when Horizon is running</span>
<span style="color: #008080;">354</span> <span style="color: #008000;">#</span><span style="color: #008000;"> external to the OpenStack environment. The default is None.  This</span>
<span style="color: #008080;">355</span> <span style="color: #008000;">#</span><span style="color: #008000;"> value should differ from OPENSTACK_ENDPOINT_TYPE if used.</span>
<span style="color: #008080;">356</span> <span style="color: #008000;">#</span><span style="color: #008000;">SECONDARY_ENDPOINT_TYPE = "publicURL"</span>
<span style="color: #008080;">357</span> 
<span style="color: #008080;">358</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The number of objects (Swift containers/objects or images) to display</span>
<span style="color: #008080;">359</span> <span style="color: #008000;">#</span><span style="color: #008000;"> on a single page before providing a paging element (a "more" link)</span>
<span style="color: #008080;">360</span> <span style="color: #008000;">#</span><span style="color: #008000;"> to paginate results.</span>
<span style="color: #008080;">361</span> API_RESULT_LIMIT = 1000
<span style="color: #008080;">362</span> API_RESULT_PAGE_SIZE = 20
<span style="color: #008080;">363</span> 
<span style="color: #008080;">364</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The size of chunk in bytes for downloading objects from Swift</span>
<span style="color: #008080;">365</span> SWIFT_FILE_TRANSFER_CHUNK_SIZE = 512 * 1024
<span style="color: #008080;">366</span> 
<span style="color: #008080;">367</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Specify a maximum number of items to display in a dropdown.</span>
<span style="color: #008080;">368</span> DROPDOWN_MAX_ITEMS = 30
<span style="color: #008080;">369</span> 
<span style="color: #008080;">370</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The timezone of the server. This should correspond with the timezone</span>
<span style="color: #008080;">371</span> <span style="color: #008000;">#</span><span style="color: #008000;"> of your entire OpenStack installation, and hopefully be in UTC.</span>
<span style="color: #008080;">372</span> TIME_ZONE = <span style="color: #800000;">"</span><span style="color: #800000;">Asia/Shanghai</span><span style="color: #800000;">"</span>
<span style="color: #008080;">373</span> 
<span style="color: #008080;">374</span> <span style="color: #008000;">#</span><span style="color: #008000;"> When launching an instance, the menu of available flavors is</span>
<span style="color: #008080;">375</span> <span style="color: #008000;">#</span><span style="color: #008000;"> sorted by RAM usage, ascending. If you would like a different sort order,</span>
<span style="color: #008080;">376</span> <span style="color: #008000;">#</span><span style="color: #008000;"> you can provide another flavor attribute as sorting key. Alternatively, you</span>
<span style="color: #008080;">377</span> <span style="color: #008000;">#</span><span style="color: #008000;"> can provide a custom callback method to use for sorting. You can also provide</span>
<span style="color: #008080;">378</span> <span style="color: #008000;">#</span><span style="color: #008000;"> a flag for reverse sort. For more info, see</span>
<span style="color: #008080;">379</span> <span style="color: #008000;">#</span><span style="color: #008000;"> http://docs.python.org/2/library/functions.html#sorted</span>
<span style="color: #008080;">380</span> <span style="color: #008000;">#</span><span style="color: #008000;">CREATE_INSTANCE_FLAVOR_SORT = {</span>
<span style="color: #008080;">381</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'key': 'name',</span>
<span style="color: #008080;">382</span> <span style="color: #008000;">#</span><span style="color: #008000;">     # or</span>
<span style="color: #008080;">383</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'key': my_awesome_callback_method,</span>
<span style="color: #008080;">384</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'reverse': False,</span>
<span style="color: #008080;">385</span> <span style="color: #008000;">#</span><span style="color: #008000;">}</span>
<span style="color: #008080;">386</span> 
<span style="color: #008080;">387</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Set this to True to display an 'Admin Password' field on the Change Password</span>
<span style="color: #008080;">388</span> <span style="color: #008000;">#</span><span style="color: #008000;"> form to verify that it is indeed the admin logged-in who wants to change</span>
<span style="color: #008080;">389</span> <span style="color: #008000;">#</span><span style="color: #008000;"> the password.</span>
<span style="color: #008080;">390</span> <span style="color: #008000;">#</span><span style="color: #008000;">ENFORCE_PASSWORD_CHECK = False</span>
<span style="color: #008080;">391</span> 
<span style="color: #008080;">392</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Modules that provide /auth routes that can be used to handle different types</span>
<span style="color: #008080;">393</span> <span style="color: #008000;">#</span><span style="color: #008000;"> of user authentication. Add auth plugins that require extra route handling to</span>
<span style="color: #008080;">394</span> <span style="color: #008000;">#</span><span style="color: #008000;"> this list.</span>
<span style="color: #008080;">395</span> <span style="color: #008000;">#</span><span style="color: #008000;">AUTHENTICATION_URLS = [</span>
<span style="color: #008080;">396</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'openstack_auth.urls',</span>
<span style="color: #008080;">397</span> <span style="color: #008000;">#</span><span style="color: #008000;">]</span>
<span style="color: #008080;">398</span> 
<span style="color: #008080;">399</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The Horizon Policy Enforcement engine uses these values to load per service</span>
<span style="color: #008080;">400</span> <span style="color: #008000;">#</span><span style="color: #008000;"> policy rule files. The content of these files should match the files the</span>
<span style="color: #008080;">401</span> <span style="color: #008000;">#</span><span style="color: #008000;"> OpenStack services are using to determine role based access control in the</span>
<span style="color: #008080;">402</span> <span style="color: #008000;">#</span><span style="color: #008000;"> target installation.</span>
<span style="color: #008080;">403</span> 
<span style="color: #008080;">404</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Path to directory containing policy.json files</span>
<span style="color: #008080;">405</span> POLICY_FILES_PATH = <span style="color: #800000;">'</span><span style="color: #800000;">/etc/openstack-dashboard</span><span style="color: #800000;">'</span>
<span style="color: #008080;">406</span> 
<span style="color: #008080;">407</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Map of local copy of service policy files.</span>
<span style="color: #008080;">408</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Please insure that your identity policy file matches the one being used on</span>
<span style="color: #008080;">409</span> <span style="color: #008000;">#</span><span style="color: #008000;"> your keystone servers. There is an alternate policy file that may be used</span>
<span style="color: #008080;">410</span> <span style="color: #008000;">#</span><span style="color: #008000;"> in the Keystone v3 multi-domain case, policy.v3cloudsample.json.</span>
<span style="color: #008080;">411</span> <span style="color: #008000;">#</span><span style="color: #008000;"> This file is not included in the Horizon repository by default but can be</span>
<span style="color: #008080;">412</span> <span style="color: #008000;">#</span><span style="color: #008000;"> found at</span>
<span style="color: #008080;">413</span> <span style="color: #008000;">#</span><span style="color: #008000;"> http://git.openstack.org/cgit/openstack/keystone/tree/etc/ \</span>
<span style="color: #008080;">414</span> <span style="color: #008000;">#</span><span style="color: #008000;"> policy.v3cloudsample.json</span>
<span style="color: #008080;">415</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Having matching policy files on the Horizon and Keystone servers is essential</span>
<span style="color: #008080;">416</span> <span style="color: #008000;">#</span><span style="color: #008000;"> for normal operation. This holds true for all services and their policy files.</span>
<span style="color: #008080;">417</span> <span style="color: #008000;">#</span><span style="color: #008000;">POLICY_FILES = {</span>
<span style="color: #008080;">418</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'identity': 'keystone_policy.json',</span>
<span style="color: #008080;">419</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'compute': 'nova_policy.json',</span>
<span style="color: #008080;">420</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'volume': 'cinder_policy.json',</span>
<span style="color: #008080;">421</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'image': 'glance_policy.json',</span>
<span style="color: #008080;">422</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'orchestration': 'heat_policy.json',</span>
<span style="color: #008080;">423</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'network': 'neutron_policy.json',</span>
<span style="color: #008080;">424</span> <span style="color: #008000;">#</span><span style="color: #008000;">    'telemetry': 'ceilometer_policy.json',</span>
<span style="color: #008080;">425</span> <span style="color: #008000;">#</span><span style="color: #008000;">}</span>
<span style="color: #008080;">426</span> 
<span style="color: #008080;">427</span> <span style="color: #008000;">#</span><span style="color: #008000;"> TODO: (david-lyle) remove when plugins support adding settings.</span>
<span style="color: #008080;">428</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Note: Only used when trove-dashboard plugin is configured to be used by</span>
<span style="color: #008080;">429</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Horizon.</span>
<span style="color: #008080;">430</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Trove user and database extension support. By default support for</span>
<span style="color: #008080;">431</span> <span style="color: #008000;">#</span><span style="color: #008000;"> creating users and databases on database instances is turned on.</span>
<span style="color: #008080;">432</span> <span style="color: #008000;">#</span><span style="color: #008000;"> To disable these extensions set the permission here to something</span>
<span style="color: #008080;">433</span> <span style="color: #008000;">#</span><span style="color: #008000;"> unusable such as ["!"].</span>
<span style="color: #008080;">434</span> <span style="color: #008000;">#</span><span style="color: #008000;">TROVE_ADD_USER_PERMS = []</span>
<span style="color: #008080;">435</span> <span style="color: #008000;">#</span><span style="color: #008000;">TROVE_ADD_DATABASE_PERMS = []</span>
<span style="color: #008080;">436</span> 
<span style="color: #008080;">437</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Change this patch to the appropriate list of tuples containing</span>
<span style="color: #008080;">438</span> <span style="color: #008000;">#</span><span style="color: #008000;"> a key, label and static directory containing two files:</span>
<span style="color: #008080;">439</span> <span style="color: #008000;">#</span><span style="color: #008000;"> _variables.scss and _styles.scss</span>
<span style="color: #008080;">440</span> <span style="color: #008000;">#</span><span style="color: #008000;">AVAILABLE_THEMES = [</span>
<span style="color: #008080;">441</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ('default', 'Default', 'themes/default'),</span>
<span style="color: #008080;">442</span> <span style="color: #008000;">#</span><span style="color: #008000;">    ('material', 'Material', 'themes/material'),</span>
<span style="color: #008080;">443</span> <span style="color: #008000;">#</span><span style="color: #008000;">]</span>
<span style="color: #008080;">444</span> 
<span style="color: #008080;">445</span> LOGGING =<span style="color: #000000;"> {
</span><span style="color: #008080;">446</span>     <span style="color: #800000;">'</span><span style="color: #800000;">version</span><span style="color: #800000;">'</span>: 1<span style="color: #000000;">,
</span><span style="color: #008080;">447</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> When set to True this will disable all logging except</span>
<span style="color: #008080;">448</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> for loggers specified in this configuration dictionary. Note that</span>
<span style="color: #008080;">449</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> if nothing is specified here and disable_existing_loggers is True,</span>
<span style="color: #008080;">450</span>     <span style="color: #008000;">#</span><span style="color: #008000;"> django.db.backends will still log unless it is disabled explicitly.</span>
<span style="color: #008080;">451</span>     <span style="color: #800000;">'</span><span style="color: #800000;">disable_existing_loggers</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">452</span>     <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">453</span>         <span style="color: #800000;">'</span><span style="color: #800000;">null</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">454</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">455</span>             <span style="color: #800000;">'</span><span style="color: #800000;">class</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">logging.NullHandler</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">456</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">457</span>         <span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">458</span>             <span style="color: #008000;">#</span><span style="color: #008000;"> Set the level to "DEBUG" for verbose output logging.</span>
<span style="color: #008080;">459</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">INFO</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">460</span>             <span style="color: #800000;">'</span><span style="color: #800000;">class</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">logging.StreamHandler</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">461</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">462</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">463</span>     <span style="color: #800000;">'</span><span style="color: #800000;">loggers</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">464</span>         <span style="color: #008000;">#</span><span style="color: #008000;"> Logging from django.db.backends is VERY verbose, send to null</span>
<span style="color: #008080;">465</span>         <span style="color: #008000;">#</span><span style="color: #008000;"> by default.</span>
<span style="color: #008080;">466</span>         <span style="color: #800000;">'</span><span style="color: #800000;">django.db.backends</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">467</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">null</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">468</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">469</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">470</span>         <span style="color: #800000;">'</span><span style="color: #800000;">requests</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">471</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">null</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">472</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">473</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">474</span>         <span style="color: #800000;">'</span><span style="color: #800000;">horizon</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">475</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">476</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">477</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">478</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">479</span>         <span style="color: #800000;">'</span><span style="color: #800000;">openstack_dashboard</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">480</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">481</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">482</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">483</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">484</span>         <span style="color: #800000;">'</span><span style="color: #800000;">novaclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">485</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">486</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">487</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">488</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">489</span>         <span style="color: #800000;">'</span><span style="color: #800000;">cinderclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">490</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">491</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">492</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">493</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">494</span>         <span style="color: #800000;">'</span><span style="color: #800000;">keystoneclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">495</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">496</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">497</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">498</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">499</span>         <span style="color: #800000;">'</span><span style="color: #800000;">glanceclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">500</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">501</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">502</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">503</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">504</span>         <span style="color: #800000;">'</span><span style="color: #800000;">neutronclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">505</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">506</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">507</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">508</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">509</span>         <span style="color: #800000;">'</span><span style="color: #800000;">heatclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">510</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">511</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">512</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">513</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">514</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ceilometerclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">515</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">516</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">517</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">518</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">519</span>         <span style="color: #800000;">'</span><span style="color: #800000;">swiftclient</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">520</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">521</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">522</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">523</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">524</span>         <span style="color: #800000;">'</span><span style="color: #800000;">openstack_auth</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">525</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">526</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">527</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">528</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">529</span>         <span style="color: #800000;">'</span><span style="color: #800000;">nose.plugins.manager</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">530</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">531</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">532</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">533</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">534</span>         <span style="color: #800000;">'</span><span style="color: #800000;">django</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">535</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">console</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">536</span>             <span style="color: #800000;">'</span><span style="color: #800000;">level</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DEBUG</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">537</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">538</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">539</span>         <span style="color: #800000;">'</span><span style="color: #800000;">iso8601</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">540</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">null</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">541</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">542</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">543</span>         <span style="color: #800000;">'</span><span style="color: #800000;">scss</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">544</span>             <span style="color: #800000;">'</span><span style="color: #800000;">handlers</span><span style="color: #800000;">'</span>: [<span style="color: #800000;">'</span><span style="color: #800000;">null</span><span style="color: #800000;">'</span><span style="color: #000000;">],
</span><span style="color: #008080;">545</span>             <span style="color: #800000;">'</span><span style="color: #800000;">propagate</span><span style="color: #800000;">'</span><span style="color: #000000;">: False,
</span><span style="color: #008080;">546</span> <span style="color: #000000;">        },
</span><span style="color: #008080;">547</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">548</span> <span style="color: #000000;">}
</span><span style="color: #008080;">549</span> 
<span style="color: #008080;">550</span> <span style="color: #008000;">#</span><span style="color: #008000;"> 'direction' should not be specified for all_tcp/udp/icmp.</span>
<span style="color: #008080;">551</span> <span style="color: #008000;">#</span><span style="color: #008000;"> It is specified in the form.</span>
<span style="color: #008080;">552</span> SECURITY_GROUP_RULES =<span style="color: #000000;"> {
</span><span style="color: #008080;">553</span>     <span style="color: #800000;">'</span><span style="color: #800000;">all_tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">554</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: _(<span style="color: #800000;">'</span><span style="color: #800000;">All TCP</span><span style="color: #800000;">'</span><span style="color: #000000;">),
</span><span style="color: #008080;">555</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">556</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">1</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">557</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">65535</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">558</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">559</span>     <span style="color: #800000;">'</span><span style="color: #800000;">all_udp</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">560</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: _(<span style="color: #800000;">'</span><span style="color: #800000;">All UDP</span><span style="color: #800000;">'</span><span style="color: #000000;">),
</span><span style="color: #008080;">561</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">udp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">562</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">1</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">563</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">65535</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">564</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">565</span>     <span style="color: #800000;">'</span><span style="color: #800000;">all_icmp</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">566</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: _(<span style="color: #800000;">'</span><span style="color: #800000;">All ICMP</span><span style="color: #800000;">'</span><span style="color: #000000;">),
</span><span style="color: #008080;">567</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">icmp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">568</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">-1</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">569</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">-1</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">570</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">571</span>     <span style="color: #800000;">'</span><span style="color: #800000;">ssh</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">572</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">SSH</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">573</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">574</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">22</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">575</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">22</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">576</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">577</span>     <span style="color: #800000;">'</span><span style="color: #800000;">smtp</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">578</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">SMTP</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">579</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">580</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">25</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">581</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">25</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">582</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">583</span>     <span style="color: #800000;">'</span><span style="color: #800000;">dns</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">584</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">DNS</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">585</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">586</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">53</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">587</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">53</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">588</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">589</span>     <span style="color: #800000;">'</span><span style="color: #800000;">http</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">590</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">HTTP</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">591</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">592</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">80</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">593</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">80</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">594</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">595</span>     <span style="color: #800000;">'</span><span style="color: #800000;">pop3</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">596</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">POP3</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">597</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">598</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">110</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">599</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">110</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">600</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">601</span>     <span style="color: #800000;">'</span><span style="color: #800000;">imap</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">602</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">IMAP</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">603</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">604</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">143</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">605</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">143</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">606</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">607</span>     <span style="color: #800000;">'</span><span style="color: #800000;">ldap</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">608</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">LDAP</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">609</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">610</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">389</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">611</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">389</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">612</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">613</span>     <span style="color: #800000;">'</span><span style="color: #800000;">https</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">614</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">HTTPS</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">615</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">616</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">443</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">617</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">443</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">618</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">619</span>     <span style="color: #800000;">'</span><span style="color: #800000;">smtps</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">620</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">SMTPS</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">621</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">622</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">465</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">623</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">465</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">624</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">625</span>     <span style="color: #800000;">'</span><span style="color: #800000;">imaps</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">626</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">IMAPS</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">627</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">628</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">993</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">629</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">993</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">630</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">631</span>     <span style="color: #800000;">'</span><span style="color: #800000;">pop3s</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">632</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">POP3S</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">633</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">634</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">995</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">635</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">995</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">636</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">637</span>     <span style="color: #800000;">'</span><span style="color: #800000;">ms_sql</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">638</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">MS SQL</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">639</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">640</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">1433</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">641</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">1433</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">642</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">643</span>     <span style="color: #800000;">'</span><span style="color: #800000;">mysql</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">644</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">MYSQL</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">645</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">646</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">3306</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">647</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">3306</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">648</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">649</span>     <span style="color: #800000;">'</span><span style="color: #800000;">rdp</span><span style="color: #800000;">'</span><span style="color: #000000;">: {
</span><span style="color: #008080;">650</span>         <span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">RDP</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">651</span>         <span style="color: #800000;">'</span><span style="color: #800000;">ip_protocol</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">tcp</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">652</span>         <span style="color: #800000;">'</span><span style="color: #800000;">from_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">3389</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">653</span>         <span style="color: #800000;">'</span><span style="color: #800000;">to_port</span><span style="color: #800000;">'</span>: <span style="color: #800000;">'</span><span style="color: #800000;">3389</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">654</span> <span style="color: #000000;">    },
</span><span style="color: #008080;">655</span> <span style="color: #000000;">}
</span><span style="color: #008080;">656</span> 
<span style="color: #008080;">657</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Deprecation Notice:</span>
<span style="color: #008080;">658</span> <span style="color: #008000;">#
</span><span style="color: #008080;">659</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The setting FLAVOR_EXTRA_KEYS has been deprecated.</span>
<span style="color: #008080;">660</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Please load extra spec metadata into the Glance Metadata Definition Catalog.</span>
<span style="color: #008080;">661</span> <span style="color: #008000;">#
</span><span style="color: #008080;">662</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The sample quota definitions can be found in:</span>
<span style="color: #008080;">663</span> <span style="color: #008000;">#</span><span style="color: #008000;"> /etc/metadefs/compute-quota.json</span>
<span style="color: #008080;">664</span> <span style="color: #008000;">#
</span><span style="color: #008080;">665</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The metadata definition catalog supports CLI and API:</span>
<span style="color: #008080;">666</span> <span style="color: #008000;">#</span><span style="color: #008000;">  $glance --os-image-api-version 2 help md-namespace-import</span>
<span style="color: #008080;">667</span> <span style="color: #008000;">#</span><span style="color: #008000;">  $glance-manage db_load_metadefs </span>
<span style="color: #008080;">668</span> <span style="color: #008000;">#
</span><span style="color: #008080;">669</span> <span style="color: #008000;">#</span><span style="color: #008000;"> See Metadata Definitions on: http://docs.openstack.org/developer/glance/</span>
<span style="color: #008080;">670</span> 
<span style="color: #008080;">671</span> <span style="color: #008000;">#</span><span style="color: #008000;"> TODO: (david-lyle) remove when plugins support settings natively</span>
<span style="color: #008080;">672</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Note: This is only used when the Sahara plugin is configured and enabled</span>
<span style="color: #008080;">673</span> <span style="color: #008000;">#</span><span style="color: #008000;"> for use in Horizon.</span>
<span style="color: #008080;">674</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Indicate to the Sahara data processing service whether or not</span>
<span style="color: #008080;">675</span> <span style="color: #008000;">#</span><span style="color: #008000;"> automatic floating IP allocation is in effect.  If it is not</span>
<span style="color: #008080;">676</span> <span style="color: #008000;">#</span><span style="color: #008000;"> in effect, the user will be prompted to choose a floating IP</span>
<span style="color: #008080;">677</span> <span style="color: #008000;">#</span><span style="color: #008000;"> pool for use in their cluster.  False by default.  You would want</span>
<span style="color: #008080;">678</span> <span style="color: #008000;">#</span><span style="color: #008000;"> to set this to True if you were running Nova Networking with</span>
<span style="color: #008080;">679</span> <span style="color: #008000;">#</span><span style="color: #008000;"> auto_assign_floating_ip = True.</span>
<span style="color: #008080;">680</span> <span style="color: #008000;">#</span><span style="color: #008000;">SAHARA_AUTO_IP_ALLOCATION_ENABLED = False</span>
<span style="color: #008080;">681</span> 
<span style="color: #008080;">682</span> <span style="color: #008000;">#</span><span style="color: #008000;"> The hash algorithm to use for authentication tokens. This must</span>
<span style="color: #008080;">683</span> <span style="color: #008000;">#</span><span style="color: #008000;"> match the hash algorithm that the identity server and the</span>
<span style="color: #008080;">684</span> <span style="color: #008000;">#</span><span style="color: #008000;"> auth_token middleware are using. Allowed values are the</span>
<span style="color: #008080;">685</span> <span style="color: #008000;">#</span><span style="color: #008000;"> algorithms supported by Python's hashlib library.</span>
<span style="color: #008080;">686</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_TOKEN_HASH_ALGORITHM = 'md5'</span>
<span style="color: #008080;">687</span> 
<span style="color: #008080;">688</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Hashing tokens from Keystone keeps the Horizon session data smaller, but it</span>
<span style="color: #008080;">689</span> <span style="color: #008000;">#</span><span style="color: #008000;"> doesn't work in some cases when using PKI tokens.  Uncomment this value and</span>
<span style="color: #008080;">690</span> <span style="color: #008000;">#</span><span style="color: #008000;"> set it to False if using PKI tokens and there are 401 errors due to token</span>
<span style="color: #008080;">691</span> <span style="color: #008000;">#</span><span style="color: #008000;"> hashing.</span>
<span style="color: #008080;">692</span> <span style="color: #008000;">#</span><span style="color: #008000;">OPENSTACK_TOKEN_HASH_ENABLED = True</span>
<span style="color: #008080;">693</span> 
<span style="color: #008080;">694</span> <span style="color: #008000;">#</span><span style="color: #008000;"> AngularJS requires some settings to be made available to</span>
<span style="color: #008080;">695</span> <span style="color: #008000;">#</span><span style="color: #008000;"> the client side. Some settings are required by in-tree / built-in horizon</span>
<span style="color: #008080;">696</span> <span style="color: #008000;">#</span><span style="color: #008000;"> features. These settings must be added to REST_API_REQUIRED_SETTINGS in the</span>
<span style="color: #008080;">697</span> <span style="color: #008000;">#</span><span style="color: #008000;"> form of ['SETTING_1','SETTING_2'], etc.</span>
<span style="color: #008080;">698</span> <span style="color: #008000;">#
</span><span style="color: #008080;">699</span> <span style="color: #008000;">#</span><span style="color: #008000;"> You may remove settings from this list for security purposes, but do so at</span>
<span style="color: #008080;">700</span> <span style="color: #008000;">#</span><span style="color: #008000;"> the risk of breaking a built-in horizon feature. These settings are required</span>
<span style="color: #008080;">701</span> <span style="color: #008000;">#</span><span style="color: #008000;"> for horizon to function properly. Only remove them if you know what you</span>
<span style="color: #008080;">702</span> <span style="color: #008000;">#</span><span style="color: #008000;"> are doing. These settings may in the future be moved to be defined within</span>
<span style="color: #008080;">703</span> <span style="color: #008000;">#</span><span style="color: #008000;"> the enabled panel configuration.</span>
<span style="color: #008080;">704</span> <span style="color: #008000;">#</span><span style="color: #008000;"> You should not add settings to this list for out of tree extensions.</span>
<span style="color: #008080;">705</span> <span style="color: #008000;">#</span><span style="color: #008000;"> See: https://wiki.openstack.org/wiki/Horizon/RESTAPI</span>
<span style="color: #008080;">706</span> REST_API_REQUIRED_SETTINGS = [<span style="color: #800000;">'</span><span style="color: #800000;">OPENSTACK_HYPERVISOR_FEATURES</span><span style="color: #800000;">'</span><span style="color: #000000;">,
</span><span style="color: #008080;">707</span>                               <span style="color: #800000;">'</span><span style="color: #800000;">LAUNCH_INSTANCE_DEFAULTS</span><span style="color: #800000;">'</span><span style="color: #000000;">]
</span><span style="color: #008080;">708</span> 
<span style="color: #008080;">709</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Additional settings can be made available to the client side for</span>
<span style="color: #008080;">710</span> <span style="color: #008000;">#</span><span style="color: #008000;"> extensibility by specifying them in REST_API_ADDITIONAL_SETTINGS</span>
<span style="color: #008080;">711</span> <span style="color: #008000;">#</span><span style="color: #008000;"> !! Please use extreme caution as the settings are transferred via HTTP/S</span>
<span style="color: #008080;">712</span> <span style="color: #008000;">#</span><span style="color: #008000;"> and are not encrypted on the browser. This is an experimental API and</span>
<span style="color: #008080;">713</span> <span style="color: #008000;">#</span><span style="color: #008000;"> may be deprecated in the future without notice.</span>
<span style="color: #008080;">714</span> <span style="color: #008000;">#</span><span style="color: #008000;">REST_API_ADDITIONAL_SETTINGS = []</span>
<span style="color: #008080;">715</span> 
<span style="color: #008080;">716</span> <span style="color: #008000;">#</span><span style="color: #008000;"> DISALLOW_IFRAME_EMBED can be used to prevent Horizon from being embedded</span>
<span style="color: #008080;">717</span> <span style="color: #008000;">#</span><span style="color: #008000;"> within an iframe. Legacy browsers are still vulnerable to a Cross-Frame</span>
<span style="color: #008080;">718</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Scripting (XFS) vulnerability, so this option allows extra security hardening</span>
<span style="color: #008080;">719</span> <span style="color: #008000;">#</span><span style="color: #008000;"> where iframes are not used in deployment. Default setting is True.</span>
<span style="color: #008080;">720</span> <span style="color: #008000;">#</span><span style="color: #008000;"> For more information see:</span>
<span style="color: #008080;">721</span> <span style="color: #008000;">#</span><span style="color: #008000;"> http://tinyurl.com/anticlickjack</span>
<span style="color: #008080;">722</span> <span style="color: #008000;">#</span><span style="color: #008000;">DISALLOW_IFRAME_EMBED = True</span></pre>
  </div>
  
  <p>
    <span class="cnblogs_code_collapse">View Code 修改文件全文</span><span style="background-color: #ffffff; font-family: 'PingFang SC', 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px;">  </span>
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">注</span>:<span style="font-family: '微软雅黑',sans-serif;">上传配置文件时需要注意配置文件权限问题</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /etc/openstack-dashboard/local_settings </span>
-rw-r----- 1 root apache 26505 Jan 24 11:10 /etc/openstack-dashboard/local_settings</pre>
</div>

### <span id="183">1.8.3 <span style="font-family: '微软雅黑',sans-serif;">启动服务</span></span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl restart httpd.service
systemctl enable  httpd.service</span></pre>
</div>

### <span id="184">1.8.4 <span style="font-family: '微软雅黑',sans-serif;">验证操作</span></span>

<span style="font-family: '微软雅黑',sans-serif;">使用浏览器访问</span> [http://10.0.0.31/dashboard][1] <span style="font-family: '微软雅黑',sans-serif;">，推荐使用火狐浏览器。</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127190305053-754129241.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<span style="font-family: '微软雅黑',sans-serif;">信息说明：第一次连接时速度较慢，耐心等待。</span>

> <span style="background-color: initial;">域：default</span>
> 
> 用户名:admin
> 
> 密码:ADMIN_PASS

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">至此</span><span style="color: red; background: yellow;"> horizon </span><span style="font-family: '微软雅黑',sans-serif;">安装完成</span>
</p>

## <span id="19">1.9 <span style="font-family: '微软雅黑',sans-serif;">启动第一台实例</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/launch-instance.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/launch-instance.html</a>
</p>

### <span id="191">1.9.1 <span style="font-family: '微软雅黑',sans-serif;">创建虚拟网络</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">公有网络参考</span>:https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/launch-instance-networks-provider.html
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127190334350-236795628.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-align: center;" align="center">
  <span style="font-family: '微软雅黑',sans-serif;">图</span> - <span style="font-family: '微软雅黑',sans-serif;">公共网络拓扑图</span>-<span style="font-family: '微软雅黑',sans-serif;">概述</span>
</p>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127190529865-1235963103.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-align: center;" align="center">
  <span style="font-family: '微软雅黑',sans-serif;">图</span> - <span style="font-family: '微软雅黑',sans-serif;">连接性</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">加载环境变量</span>
</p>

<div class="cnblogs_code">
  <pre>. admin-openrc</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建网络</span>
</p>

<div class="cnblogs_code">
  <pre>neutron net-create --shared --provider:physical_network provider --provider:network_type flat provider</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在网络上创建出一个子网</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">语法说明：</span>
</p>

<div class="cnblogs_code">
  <pre>neutron subnet-create --name provider --allocation-pool start=START_IP_ADDRESS,end=END_IP_ADDRESS --dns-nameserver DNS_RESOLVER --gateway PROVIDER_NETWORK_GATEWAY provider PROVIDER_NETWORK_CIDR</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">参数说明</span>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 使用提供者物理网络的子网CIDR标记替换``PROVIDER_NETWORK_CIDR``。</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 将``START_IP_ADDRESS``和``END_IP_ADDRESS``使用你想分配给实例的子网网段的第一个和最后一个IP地址。这个范围不能包括任何已经使用的IP地址。</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 将 DNS_RESOLVER 替换为DNS解析服务的IP地址。在大多数情况下，你可以从主机``/etc/resolv.conf`` 文件选择一个使用。</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 将``PUBLIC_NETWORK_GATEWAY`` 替换为公共网络的网关，一般的网关IP地址以 ”.1” 结尾。</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">配置示例：</span></strong>
</p>

<div class="cnblogs_code">
  <pre>neutron subnet-create --name provider --allocation-pool start=10.0.0.101,end=10.0.0.250 --dns-nameserver 223.5.5.5 --gateway 10.0.0.254 provider 10.0.0.0/24</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置过程</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> neutron subnet-create --name provider \</span>
&gt;   --allocation-pool start=10.0.0.101,end=10.0.0.250<span style="color: #000000;"> \
</span>&gt;   --dns-nameserver 223.5.5.5 --gateway 10.0.0.254<span style="color: #000000;"> \
</span>&gt;   provider 10.0.0.0/24<span style="color: #000000;">
Created a new subnet:
</span>+-------------------+----------------------------------------------+
| Field             | Value                                        |
+-------------------+----------------------------------------------+
| allocation_pools  | {<span style="color: #800000;">"</span><span style="color: #800000;">start</span><span style="color: #800000;">"</span>: <span style="color: #800000;">"</span><span style="color: #800000;">10.0.0.101</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">end</span><span style="color: #800000;">"</span>: <span style="color: #800000;">"</span><span style="color: #800000;">10.0.0.250</span><span style="color: #800000;">"</span>} |
| cidr              | 10.0.0.0/24                                  |
| created_at        | 2018-01-24T03:41:27                          |
| description       |                                              |
| dns_nameservers   | 223.5.5.5                                    |
| enable_dhcp       | True                                         |
| gateway_ip        | 10.0.0.254                                   |
| host_routes       |                                              |
| id                | d507bf57-28e6-4af5-b54b-d969e76f4fd6         |
| ip_version        | 4                                            |
| ipv6_address_mode |                                              |
| ipv6_ra_mode      |                                              |
| name              | provider                                     |
| network_id        | 54f942f7-cc28-4292-a4d6-e37b8833e35f         |
| subnetpool_id     |                                              |
| tenant_id         | d0dfbdbc115b4a728c24d28bc1ce1e62             |
| updated_at        | 2018-01-24T03:41:27                          |
+-------------------+----------------------------------------------+</pre>
</div>

### <span id="192_m1nano">1.9.2 <span style="font-family: '微软雅黑',sans-serif;">创建</span>m1.nano<span style="font-family: '微软雅黑',sans-serif;">规格的主机</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span>   <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/launch-instance.html#create-m1-nano-flavor">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/launch-instance.html#create-m1-nano-flavor</a>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">默认的最小规格的主机需要</span>512 MB<span style="font-family: '微软雅黑',sans-serif;">内存。对于环境中计算节点内存不足</span>4 GB<span style="font-family: '微软雅黑',sans-serif;">的，我们推荐创建只需要</span>64 MB<span style="font-family: '微软雅黑',sans-serif;">的</span>``m1.nano``<span style="font-family: '微软雅黑',sans-serif;">规格的主机。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">若单纯为了测试的目的，请使用</span>``m1.nano``<span style="font-family: '微软雅黑',sans-serif;">规格的主机来加载</span>CirrOS<span style="font-family: '微软雅黑',sans-serif;">镜像</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置命令</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack flavor create --id 0 --vcpus 1 --ram 64 --disk 1 m1.nano</span>
+----------------------------+---------+
| Field                      | Value   |
+----------------------------+---------+
| OS-FLV-DISABLED:disabled   | False   |
| OS-FLV-EXT-DATA:ephemeral  | 0       |
| disk                       | 1       |
| id                         | 0       |
| name                       | m1.nano |
| os-flavor-access:is_public | True    |
| ram                        | 64      |
| rxtx_factor                | 1.0     |
| swap                       |         |
| vcpus                      | 1       |
+----------------------------+---------+</pre>
</div>

### <span id="193">1.9.3 <span style="font-family: '微软雅黑',sans-serif;">生成一个键值对，创建密钥对</span></span>

<span style="font-family: '微软雅黑',sans-serif;">生成密钥，并使用</span>

<div class="cnblogs_code">
  <pre>ssh-keygen -q -N <span style="color: #800000;">""</span> -f ~/.ssh/<span style="color: #000000;">id_rsa
openstack keypair create </span>--public-key ~/.ssh/id_rsa.pub mykey</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">分配密钥</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack keypair create --public-key ~/.ssh/id_rsa.pub mykey</span>
+-------------+-------------------------------------------------+
| Field       | Value                                           |
+-------------+-------------------------------------------------+
| fingerprint | 4f:77:29:9d:4c:96:5c:45:e3:7c:5d:fa:0f:b0:bc:59 |
| name        | mykey                                           |
| user_id     | d8f4a1d74f52482d8ebe2184692d2c1c                |
+-------------+-------------------------------------------------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">检查密钥对</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack keypair list </span>
+-------+-------------------------------------------------+
| Name  | Fingerprint                                     |
+-------+-------------------------------------------------+
| mykey | 4f:77:29:9d:4c:96:5c:45:e3:7c:5d:fa:0f:b0:bc:59 |
+-------+-------------------------------------------------+</pre>
</div>

### <span id="194">1.9.4 <span style="font-family: '微软雅黑',sans-serif;">增加安全组规则</span></span>

<span style="font-family: '微软雅黑',sans-serif;">允许</span> ICMP (ping)

<div class="cnblogs_code">
  <pre>openstack security group rule create --proto icmp default</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">允许安全</span> shell (SSH) <span style="font-family: '微软雅黑',sans-serif;">的访问</span>

<div class="cnblogs_code">
  <pre>openstack security group rule create --proto tcp --dst-port 22 default</pre>
</div>

### <span id="195">1.9.5 <span style="font-family: '微软雅黑',sans-serif;">启动第一台云主机</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/launch-instance-provider.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/launch-instance-provider.html</a>
</p>

**<span style="font-family: '微软雅黑',sans-serif;">启动之前先进行基础环境的检查</span>**

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">一个实例指定了虚拟机资源的大致分配，包括处理器、内存和存储。</span>
</p>

<div class="cnblogs_code">
  <pre>openstack flavor list</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">列出可用镜像</span>
</p>

<div class="cnblogs_code">
  <pre>openstack image list</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">列出可用网络</span>
</p>

<div class="cnblogs_code">
  <pre>openstack network list</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">列出可用的安全组</span>
</p>

<div class="cnblogs_code">
  <pre>openstack security group list</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">获取网络</span>id

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack network list </span>
+--------------------------------------+----------+--------------------------------------+
| ID                                   | Name     | Subnets                              |
+--------------------------------------+----------+--------------------------------------+
| 54f942f7-cc28-4292-a4d6-e37b8833e35f | provider | d507bf57-28e6-4af5-b54b-d969e76f4fd6 |
+--------------------------------------+----------+--------------------------------------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动云主机，注意</span>net-id<span style="font-family: '微软雅黑',sans-serif;">为创建的</span>network ID

<div class="cnblogs_code">
  <pre>openstack server create --flavor m1.nano  --<span style="color: #000000;">image cirros \
  </span>--nic net-id=54f942f7-cc28-4292-a4d6-e37b8833e35f  --security-<span style="color: #000000;">group default \
  </span>--key-name mykey clsn</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">检查云主机的状况</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> nova list </span>
+--------------------------------------+---------------+--------+------------+-------------+---------------------+
| ID                                   | Name          | Status | Task State | Power State | Networks            |
+--------------------------------------+---------------+--------+------------+-------------+---------------------+
| aa5bcbb8-64a7-44c8-b302-6e1ccd1af6ef | www.nmtui.com | ACTIVE | -          | Running     | provider=10.0.0.102 |
+--------------------------------------+---------------+--------+------------+-------------+---------------------+</pre>
</div>

### <span id="196_WEB">1.9.6 <span style="font-family: '微软雅黑',sans-serif;">在</span>WEB<span style="font-family: '微软雅黑',sans-serif;">端进行查看</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">浏览器访问：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://10.0.0.31/dashboard/" >http://10.0.0.31/dashboard/</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">查看云主机状态</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191337256-1557463903.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<span style="font-family: '微软雅黑',sans-serif;">使用控制台登陆</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191348084-1324124193.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<span style="font-family: '微软雅黑',sans-serif;">使用控制台登陆</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191356303-339045606.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-align: center;" align="center">
   <span class="cnblogs_code"> 用户名为：cirros，密码为：cubswin:)</span>
</p>

### <span id="197_web">1.9.7 <span style="font-family: '微软雅黑',sans-serif;">使用</span>web<span style="font-family: '微软雅黑',sans-serif;">界面创建一个实例</span></span>

<p style="margin-left: 7.1pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">、选择启动实例</span>
</p>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191417475-2084115194.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

2<span style="font-family: '微软雅黑',sans-serif;">、设置主机名称，点下一项</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191423865-173896962.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

3<span style="font-family: '微软雅黑',sans-serif;">、选择一个镜像</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191432100-836865877.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

4<span style="font-family: '微软雅黑',sans-serif;">、选择一个配置</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191439428-509951799.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

5<span style="font-family: '微软雅黑',sans-serif;">、网络</span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191447615-1293695864.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

6<span style="font-family: '微软雅黑',sans-serif;">、安全组</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191454428-1387530772.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

7<span style="font-family: '微软雅黑',sans-serif;">、密钥对</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191502162-659650532.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

8<span style="font-family: '微软雅黑',sans-serif;">、启动实例</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191509100-736324206.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

9<span style="font-family: '微软雅黑',sans-serif;">、创建完成</span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127191516272-2034031397.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

10<span style="font-family: '微软雅黑',sans-serif;">、查看主机列表</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> nova list </span>
+--------------------------------------+----------------+--------+------------+-------------+---------------------+
| ID                                   | Name           | Status | Task State | Power State | Networks            |
+--------------------------------------+----------------+--------+------------+-------------+---------------------+
| ff46e8a7-9085-4afb-b7b7-193f37efb86d | clsn           | ACTIVE | -          | Running     | provider=10.0.0.103 |
| d275ceac-535a-4c05-92ab-3040ed9fb9d8 | clsn-openstack | ACTIVE | -          | Running     | provider=10.0.0.104 |
| aa5bcbb8-64a7-44c8-b302-6e1ccd1af6ef | www.nmtui.com  | ACTIVE | -          | Running     | provider=10.0.0.102 |
+--------------------------------------+----------------+--------+------------+-------------+---------------------+</pre>
</div>

11<span style="font-family: '微软雅黑',sans-serif;">、密钥连接测试</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ssh cirros@10.0.0.104</span>
The authenticity of host <span style="color: #800000;">'</span><span style="color: #800000;">10.0.0.104 (10.0.0.104)</span><span style="color: #800000;">'</span> can<span style="color: #800000;">'</span><span style="color: #800000;">t be established.</span>
RSA key fingerprint <span style="color: #0000ff;">is</span> 9d:ca:25:cd:23:c9:f8:73:c6:26:84:53:46:56:67:63<span style="color: #000000;">.
Are you sure you want to </span><span style="color: #0000ff;">continue</span> connecting (yes/<span style="color: #000000;">no)? yes
Warning: Permanently added </span><span style="color: #800000;">'</span><span style="color: #800000;">10.0.0.104</span><span style="color: #800000;">'</span><span style="color: #000000;"> (RSA) to the list of known hosts.
$ hostname
clsn</span>-openstack</pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif;">至此云主机创建完成。</span>**

## <span id="110_cinder">1.10 cinder<span style="font-family: '微软雅黑',sans-serif;">块存储服务</span></span>

<p style="margin-left: 25.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">官方文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/cinder.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/cinder.html</a>
</p>

### <span id="1101">1.10.1 <span style="font-family: '微软雅黑',sans-serif;">环境准备</span></span>

<span style="font-family: '微软雅黑',sans-serif;">为</span>compute1<span style="font-family: '微软雅黑',sans-serif;">计算节点添加两块硬盘，分别为：</span>

<div class="cnblogs_code">
  <pre>    sdb      8:16<span style="color: #000000;">   0  30G  0 disk 
    sdc      </span>8:32   0  20G  0 disk</pre>
</div>

### <span id="1102">1.10.2 <span style="font-family: '微软雅黑',sans-serif;">安装并配置控制节点</span></span>

<span style="background: yellow;">1</span><span style="font-family: '微软雅黑',sans-serif;">）在数据库中，创库，授权</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> cinder <span style="font-family: '微软雅黑',sans-serif;">数据库</span>
</p>

<div class="cnblogs_code">
  <pre>CREATE DATABASE cinder;</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">允许</span> cinder <span style="font-family: '微软雅黑',sans-serif;">数据库合适的访问权限</span>
</p>

<div class="cnblogs_code">
  <pre>GRANT ALL PRIVILEGES ON cinder.* TO <span style="color: #800000;">'</span><span style="color: #800000;">cinder</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">CINDER_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON cinder.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">cinder</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">CINDER_DBPASS</span><span style="color: #800000;">'</span>;</pre>
</div>

<span style="background: yellow;">2</span><span style="font-family: '微软雅黑',sans-serif;">）在</span><span style="background: yellow;">keystone</span><span style="font-family: '微软雅黑',sans-serif;">中创建用户并授权</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建一个</span> cinder <span style="font-family: '微软雅黑',sans-serif;">用</span>
</p>

<div class="cnblogs_code">
  <pre>openstack user create --domain default --password  CINDER_PASS cinder</pre>
</div>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">添加</span> admin <span style="font-family: '微软雅黑',sans-serif;">角色到</span> cinder <span style="font-family: '微软雅黑',sans-serif;">用户上。</span>
</p>

<div class="cnblogs_code">
  <pre>openstack role add --project service --user cinder admin</pre>
</div>

<span style="background: yellow;">3</span><span style="font-family: '微软雅黑',sans-serif;">）在</span><span style="background: yellow;">keystone</span><span style="font-family: '微软雅黑',sans-serif;">中创建服务实体，和注册</span><span style="background: yellow;">API</span><span style="font-family: '微软雅黑',sans-serif;">接口</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建</span> cinder <span style="font-family: '微软雅黑',sans-serif;">和</span> cinderv2 <span style="font-family: '微软雅黑',sans-serif;">服务实体</span>
</p>

<div class="cnblogs_code">
  <pre>openstack service create --name cinder --description <span style="color: #800000;">"</span><span style="color: #800000;">OpenStack Block Storage</span><span style="color: #800000;">"</span><span style="color: #000000;"> volume
openstack service create </span>--name cinderv2 --description <span style="color: #800000;">"</span><span style="color: #800000;">OpenStack Block Storage</span><span style="color: #800000;">"</span> volumev2</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">创建块设备存储服务的</span> API <span style="font-family: '微软雅黑',sans-serif;">入口点。<span style="color: red;">注意：</span>需要注册两个版本</span>
</p>

<p style="text-indent: 15.75pt;">
  # v1<span style="font-family: '微软雅黑',sans-serif;">版本注册</span>
</p>

<div class="cnblogs_code">
  <pre>openstack endpoint create --region RegionOne volume public http://controller:8776/v1/%<span style="color: #000000;">\(tenant_id\)s
openstack endpoint create </span>--region RegionOne volume internal http://controller:8776/v1/%<span style="color: #000000;">\(tenant_id\)s
openstack endpoint create </span>--region RegionOne volume admin http://controller:8776/v1/%\(tenant_id\)s</pre>
</div>

<p style="text-indent: 15.75pt;">
   # v2<span style="font-family: '微软雅黑',sans-serif;">版本注册</span>
</p>

<div class="cnblogs_code">
  <pre> openstack endpoint create --region RegionOne volumev2 public http://controller:8776/v2/%<span style="color: #000000;">\(tenant_id\)s
 openstack endpoint create </span>--region RegionOne volumev2 internal http://controller:8776/v2/%<span style="color: #000000;">\(tenant_id\)s
openstack endpoint create </span>--region RegionOne volumev2 admin http://controller:8776/v2/%\(tenant_id\)s</pre>
</div>

<span style="background: yellow;">4</span><span style="font-family: '微软雅黑',sans-serif;">）安装软件包</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-cinder</pre>
</div>

<span style="background: yellow;">5</span><span style="font-family: '微软雅黑',sans-serif;">）修改配置文件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span> /etc/cinder/cinder.conf<span style="font-family: '微软雅黑',sans-serif;">，同时完成如下动作</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [database] <span style="font-family: '微软雅黑',sans-serif;">部分，配置数据库访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[database]
...
connection </span>= mysql+pymysql://cinder:CINDER_DBPASS@controller/cinder</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[oslo_messaging_rabbit]<span style="font-family: '微软雅黑',sans-serif;">”部分，配置</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>RabbitMQ<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">消息队列访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
rpc_backend </span>=<span style="color: #000000;"> rabbit

[oslo_messaging_rabbit]
...
rabbit_host </span>=<span style="color: #000000;"> controller
rabbit_userid </span>=<span style="color: #000000;"> openstack
rabbit_password </span>= RABBIT_PASS</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[keystone_authtoken]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
auth_strategy </span>=<span style="color: #000000;"> keystone

[keystone_authtoken]
...
auth_uri </span>= http://controller:5000<span style="color: #000000;">
auth_url </span>= http://controller:35357<span style="color: #000000;">
memcached_servers </span>= controller:11211<span style="color: #000000;">
auth_type </span>=<span style="color: #000000;"> password
project_domain_name </span>=<span style="color: #000000;"> default
user_domain_name </span>=<span style="color: #000000;"> default
project_name </span>=<span style="color: #000000;"> service
username </span>=<span style="color: #000000;"> cinder
password </span>= CINDER_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT <span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>``my_ip`` <span style="font-family: '微软雅黑',sans-serif;">来使用控制节点的管理接口的</span>IP <span style="font-family: '微软雅黑',sans-serif;">地址</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
my_ip </span>= 10.0.0.11</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [oslo_concurrency] <span style="font-family: '微软雅黑',sans-serif;">部分，配置锁路径</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[oslo_concurrency]
...
lock_path </span>= /var/lib/cinder/tmp</pre>
</div>

<p style="text-indent: 15.75pt;">
  <strong><span style="font-family: 微软雅黑, sans-serif; background: lime;">配置计算服务使用块设备存储</span></strong>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑文件</span> /etc/nova/nova.conf <span style="font-family: '微软雅黑',sans-serif;">并添加如下到其中</span>
</p>

<div class="cnblogs_code">
  <pre>vim /etc/nova/<span style="color: #000000;">nova.conf
[cinder]
os_region_name </span>= RegionOne</pre>
</div>

<span style="background: yellow;">6</span><span style="font-family: '微软雅黑',sans-serif;">）同步数据库</span>

<div class="cnblogs_code">
  <pre>su -s /bin/sh -c <span style="color: #800000;">"</span><span style="color: #800000;">cinder-manage db sync</span><span style="color: #800000;">"</span><span style="color: #000000;"> cinder
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 忽略输出中任何不推荐使用的信息。</span></pre>
</div>

<span style="background: yellow;">7</span><span style="font-family: '微软雅黑',sans-serif;">）启动服务</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">重启计算</span>API <span style="font-family: '微软雅黑',sans-serif;">服务</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl restart openstack-nova-<span style="color: #000000;">api.service
systemctl status openstack</span>-nova-api.service</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">启动块设备存储服务，并将其配置为开机自启</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl enable openstack-cinder-api.service openstack-cinder-<span style="color: #000000;">scheduler.service
systemctl start openstack</span>-cinder-api.service openstack-cinder-<span style="color: #000000;">scheduler.service
systemctl status openstack</span>-cinder-api.service openstack-cinder-scheduler.service</pre>
</div>

### <span id="1103">1.10.3 <span style="font-family: '微软雅黑',sans-serif;">安装并配置一个存储节点</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/cinder-storage-install.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/cinder-storage-install.html</a>
</p>

<span style="background: yellow;">1</span><span style="font-family: '微软雅黑',sans-serif;">）安装</span><span style="background: yellow;">lvm</span><span style="font-family: '微软雅黑',sans-serif;">软件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">安装支持的工具包</span>
</p>

<div class="cnblogs_code">
  <pre>yum -y install lvm2</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">启动</span>LVM<span style="font-family: '微软雅黑',sans-serif;">的</span>metadata<span style="font-family: '微软雅黑',sans-serif;">服务并且设置该服务随系统启动</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl enable lvm2-<span style="color: #000000;">lvmetad.service
systemctl start lvm2</span>-<span style="color: #000000;">lvmetad.service
systemctl status lvm2</span>-lvmetad.service</pre>
</div>

<span style="background: yellow;">2</span><span style="font-family: '微软雅黑',sans-serif;">）创建物理卷</span>

<span style="font-family: '微软雅黑',sans-serif;">将之前添加的两块硬盘创建物理卷</span>

<div class="cnblogs_code">
  <pre>pvcreate /dev/<span style="color: #000000;">sdb
pvcreate </span>/dev/sdc</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">执行过程</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;">  pvcreate /dev/sdb</span>
  Physical volume <span style="color: #800000;">"</span><span style="color: #800000;">/dev/sdb</span><span style="color: #800000;">"</span><span style="color: #000000;"> successfully created.
[root@compute1 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;">  pvcreate /dev/sdc</span>
  Physical volume <span style="color: #800000;">"</span><span style="color: #800000;">/dev/sdc</span><span style="color: #800000;">"</span> successfully created.</pre>
</div>

<span style="background: yellow;">3</span><span style="font-family: '微软雅黑',sans-serif;">）创建</span> <span style="background: yellow;">LVM </span><span style="font-family: '微软雅黑',sans-serif;">卷组</span>

<div class="cnblogs_code">
  <pre>vgcreate cinder-volumes-sata /dev/<span style="color: #000000;">sdb 
vgcreate cinder</span>-volumes-ssd /dev/sdc</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看创建出来的卷组</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vgs</span>
  VG                  <span style="color: #008000;">#</span><span style="color: #008000;">PV #LV #SN Attr   VSize  VFree </span>
  cinder-volumes-sata   1   0   0 wz--n- 30.00g 30<span style="color: #000000;">.00g
  cinder</span>-volumes-ssd    1   0   0 wz--n- 20.00g 20.00g</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">删除卷组方法</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> vgremove vg-name</span></pre>
</div>

<span style="background: yellow;">4</span><span style="font-family: '微软雅黑',sans-serif;">）修改配置文件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">只有实例可以访问块存储卷组。不过，底层的操作系统管理这些设备并将其与卷关联。</span>
</p>

   <span style="font-family: '微软雅黑',sans-serif;">默认情况下，</span>LVM<span style="font-family: '微软雅黑',sans-serif;">卷扫描工具会扫描</span>\`\`/dev\`\` <span style="font-family: '微软雅黑',sans-serif;">目录，查找包含卷的块存储设备。</span>

<span style="font-family: '微软雅黑',sans-serif;">如果项目在他们的卷上使用</span>LVM<span style="font-family: '微软雅黑',sans-serif;">，扫描工具检测到这些卷时会尝试缓存它们，可能会在底层操作系统和项目卷上产生各种问题。</span>

<p style="text-indent: 21.0pt;">
  <em><span style="font-family: '微软雅黑',sans-serif;">编辑</span>``/etc/lvm/lvm.conf``</em><em><span style="font-family: '微软雅黑',sans-serif;">文件并完成下面的操作</span></em>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">devices {
...
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 在130行下增加如下行</span>
filter = [ <span style="color: #800000;">"</span><span style="color: #800000;">a/sdb/</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">a/sdc/</span><span style="color: #800000;">"</span>, <span style="color: #800000;">"</span><span style="color: #800000;">r/.*/</span><span style="color: #800000;">"</span>]</pre>
</div>

<span style="background: yellow;">5</span><span style="font-family: '微软雅黑',sans-serif;">）安装软件并配置组件</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-cinder targetcli python-keystone</pre>
</div>

<span style="background: yellow;">6</span><span style="font-family: '微软雅黑',sans-serif;">）配置文件修改</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span> /etc/cinder/cinder.conf<span style="font-family: '微软雅黑',sans-serif;">，同时完成如下动作</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [database] <span style="font-family: '微软雅黑',sans-serif;">部分，配置数据库访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[database]
...
connection </span>= mysql+pymysql://cinder:CINDER_DBPASS@controller/cinder</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[oslo_messaging_rabbit]<span style="font-family: '微软雅黑',sans-serif;">”部分，配置</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>RabbitMQ<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">消息队列访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
rpc_backend </span>=<span style="color: #000000;"> rabbit

[oslo_messaging_rabbit]
...
rabbit_host </span>=<span style="color: #000000;"> controller
rabbit_userid </span>=<span style="color: #000000;"> openstack
rabbit_password </span>= RABBIT_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[DEFAULT]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">和</span> <span style="font-family: '微软雅黑',sans-serif;">“</span>[keystone_authtoken]<span style="font-family: '微软雅黑',sans-serif;">”</span> <span style="font-family: '微软雅黑',sans-serif;">部分，配置认证服务访问</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
auth_strategy </span>=<span style="color: #000000;"> keystone

[keystone_authtoken]
...
auth_uri </span>= http://controller:5000<span style="color: #000000;">
auth_url </span>= http://controller:35357<span style="color: #000000;">
memcached_servers </span>= controller:11211<span style="color: #000000;">
auth_type </span>=<span style="color: #000000;"> password
project_domain_name </span>=<span style="color: #000000;"> default
user_domain_name </span>=<span style="color: #000000;"> default
project_name </span>=<span style="color: #000000;"> service
username </span>=<span style="color: #000000;"> cinder
password </span>= CINDER_PASS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT] <span style="font-family: '微软雅黑',sans-serif;">部分，配置</span> my_ip <span style="font-family: '微软雅黑',sans-serif;">选项</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
my_ip </span>= MANAGEMENT_INTERFACE_IP_ADDRESS</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意：将其中的</span><span style="color: red;">``MANAGEMENT_INTERFACE_IP_ADDRESS``</span><span style="font-family: '微软雅黑',sans-serif;">替换为存储节点上的管理网络接口的</span><span style="color: red;">IP </span><span style="font-family: '微软雅黑',sans-serif;">地址</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[lvm]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>LVM<span style="font-family: '微软雅黑',sans-serif;">后端以</span>LVM<span style="font-family: '微软雅黑',sans-serif;">驱动结束，卷组</span>``cinder-volumes`` <span style="font-family: '微软雅黑',sans-serif;">，</span>iSCSI <span style="font-family: '微软雅黑',sans-serif;">协议和正确的</span> iSCSI<span style="font-family: '微软雅黑',sans-serif;">服务</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[lvm]
...
volume_driver </span>=<span style="color: #000000;"> cinder.volume.drivers.lvm.LVMVolumeDriver
volume_group </span>= cinder-<span style="color: #000000;">volumes
iscsi_protocol </span>=<span style="color: #000000;"> iscsi
iscsi_helper </span>= lioadm</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT] <span style="font-family: '微软雅黑',sans-serif;">部分，启用</span> LVM <span style="font-family: '微软雅黑',sans-serif;">后端</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
enabled_backends </span>= lvm</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [DEFAULT] <span style="font-family: '微软雅黑',sans-serif;">区域，配置镜像服务</span> API <span style="font-family: '微软雅黑',sans-serif;">的位置</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
glance_api_servers </span>= http://controller:9292</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span> [oslo_concurrency] <span style="font-family: '微软雅黑',sans-serif;">部分，配置锁路径</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[oslo_concurrency]
...
lock_path </span>= /var/lib/cinder/tmp</pre>
</div>

<p style="text-indent: 15.75pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">配置文件最终内容</span></strong>
</p>

<div class="cnblogs_code">
  <p>
    <img id="code_img_closed_12b768c5-f2f8-4e18-aef5-0e5fc6f3e44d" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-2.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" /><img id="code_img_opened_12b768c5-f2f8-4e18-aef5-0e5fc6f3e44d" class="code_img_opened" style="display: none;" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-2.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
  </p>
  
  <div id="cnblogs_code_open_12b768c5-f2f8-4e18-aef5-0e5fc6f3e44d" class="cnblogs_code_hide">
    <pre><span style="color: #008080;"> 1</span> [root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/cinder/cinder.conf</span>
<span style="color: #008080;"> 2</span> <span style="color: #000000;">[DEFAULT]
</span><span style="color: #008080;"> 3</span> glance_api_servers = http://10.0.0.32:9292
<span style="color: #008080;"> 4</span> enabled_backends =<span style="color: #000000;"> lvm
</span><span style="color: #008080;"> 5</span> rpc_backend =<span style="color: #000000;"> rabbit
</span><span style="color: #008080;"> 6</span> auth_strategy =<span style="color: #000000;"> keystone
</span><span style="color: #008080;"> 7</span> my_ip = 10.0.0.31
<span style="color: #008080;"> 8</span> <span style="color: #000000;">[BACKEND]
</span><span style="color: #008080;"> 9</span> <span style="color: #000000;">[BRCD_FABRIC_EXAMPLE]
</span><span style="color: #008080;">10</span> <span style="color: #000000;">[CISCO_FABRIC_EXAMPLE]
</span><span style="color: #008080;">11</span> <span style="color: #000000;">[COORDINATION]
</span><span style="color: #008080;">12</span> [FC-ZONE-<span style="color: #000000;">MANAGER]
</span><span style="color: #008080;">13</span> <span style="color: #000000;">[KEYMGR]
</span><span style="color: #008080;">14</span> <span style="color: #000000;">[cors]
</span><span style="color: #008080;">15</span> <span style="color: #000000;">[cors.subdomain]
</span><span style="color: #008080;">16</span> <span style="color: #000000;">[database]
</span><span style="color: #008080;">17</span> connection = mysql+pymysql://cinder:CINDER_DBPASS@controller/<span style="color: #000000;">cinder
</span><span style="color: #008080;">18</span> <span style="color: #000000;">[keystone_authtoken]
</span><span style="color: #008080;">19</span> auth_uri = http://controller:5000
<span style="color: #008080;">20</span> auth_url = http://controller:35357
<span style="color: #008080;">21</span> memcached_servers = controller:11211
<span style="color: #008080;">22</span> auth_type =<span style="color: #000000;"> password
</span><span style="color: #008080;">23</span> project_domain_name =<span style="color: #000000;"> default
</span><span style="color: #008080;">24</span> user_domain_name =<span style="color: #000000;"> default
</span><span style="color: #008080;">25</span> project_name =<span style="color: #000000;"> service
</span><span style="color: #008080;">26</span> username =<span style="color: #000000;"> cinder
</span><span style="color: #008080;">27</span> password =<span style="color: #000000;"> CINDER_PASS
</span><span style="color: #008080;">28</span> <span style="color: #000000;">[matchmaker_redis]
</span><span style="color: #008080;">29</span> <span style="color: #000000;">[oslo_concurrency]
</span><span style="color: #008080;">30</span> lock_path = /var/lib/cinder/<span style="color: #000000;">tmp
</span><span style="color: #008080;">31</span> <span style="color: #000000;">[oslo_messaging_amqp]
</span><span style="color: #008080;">32</span> <span style="color: #000000;">[oslo_messaging_notifications]
</span><span style="color: #008080;">33</span> <span style="color: #000000;">[oslo_messaging_rabbit]
</span><span style="color: #008080;">34</span> rabbit_host =<span style="color: #000000;"> controller
</span><span style="color: #008080;">35</span> rabbit_userid =<span style="color: #000000;"> openstack
</span><span style="color: #008080;">36</span> rabbit_password =<span style="color: #000000;"> RABBIT_PASS
</span><span style="color: #008080;">37</span> <span style="color: #000000;">[oslo_middleware]
</span><span style="color: #008080;">38</span> <span style="color: #000000;">[oslo_policy]
</span><span style="color: #008080;">39</span> <span style="color: #000000;">[oslo_reports]
</span><span style="color: #008080;">40</span> <span style="color: #000000;">[oslo_versionedobjects]
</span><span style="color: #008080;">41</span> <span style="color: #000000;">[ssl]
</span><span style="color: #008080;">42</span> <span style="color: #000000;">[lvm]
</span><span style="color: #008080;">43</span> volume_driver =<span style="color: #000000;"> cinder.volume.drivers.lvm.LVMVolumeDriver
</span><span style="color: #008080;">44</span> volume_group = cinder-volumes-<span style="color: #000000;">sata
</span><span style="color: #008080;">45</span> iscsi_protocol =<span style="color: #000000;"> iscsi
</span><span style="color: #008080;">46</span> iscsi_helper = lioadm</pre>
  </div>
  
  <p>
    <span class="cnblogs_code_collapse">View Code <strong>配置文件最终内容</strong></span>
  </p>
</div>

<span style="background: yellow;">7</span><span style="font-family: '微软雅黑',sans-serif;">）启动服务</span>

<div class="cnblogs_code">
  <pre>systemctl enable openstack-cinder-<span style="color: #000000;">volume.service target.service
systemctl start openstack</span>-cinder-<span style="color: #000000;">volume.service target.service
systemctl status openstack</span>-cinder-volume.service target.service</pre>
</div>

<span style="background: yellow;">8</span><span style="font-family: '微软雅黑',sans-serif;">）验证检查状态</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;">  cinder service-list</span>
+------------------+--------------+------+---------+-------+----------------------------+-----------------+
|      Binary      |     Host     | Zone |  Status | State |         Updated_at         | Disabled Reason |
+------------------+--------------+------+---------+-------+----------------------------+-----------------+
| cinder-scheduler |  controller  | nova | enabled |   up  | 2018-01-25T11:01:41.000000 |        -        |
|  cinder-volume   | compute1@lvm | nova | enabled |   up  | 2018-01-25T11:01:40.000000 |        -        |
+------------------+--------------+------+---------+-------+----------------------------+-----------------+</pre>
</div>

### <span id="1104_ssd">1.10.4 <span style="font-family: '微软雅黑',sans-serif;">添加</span>ssd<span style="font-family: '微软雅黑',sans-serif;">盘配置信息</span></span>

<span style="font-family: '微软雅黑',sans-serif;">修改配置文件</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;">  vim /etc/cinder/cinder.conf</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 修改内容如下</span>
<span style="color: #000000;">[DEFAULT]
···
enabled_backends </span>=<span style="color: #000000;"> lvm,ssd

[lvm]
···
volume_backend_name </span>=<span style="color: #000000;"> sata

[ssd]
volume_driver </span>=<span style="color: #000000;"> cinder.volume.drivers.lvm.LVMVolumeDriver
volume_group </span>= cinder-volumes-<span style="color: #000000;">ssd
iscsi_protocol </span>=<span style="color: #000000;"> iscsi
iscsi_helper </span>=<span style="color: #000000;"> lioadm
volume_backend_name </span>= ssd</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">重启服务</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl restart openstack-cinder-volume.service</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">检查</span>cinder<span style="font-family: '微软雅黑',sans-serif;">服务状态</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;">  cinder service-list</span>
+------------------+--------------+------+---------+-------+----------------------------+-----------------+
|      Binary      |     Host     | Zone |  Status | State |         Updated_at         | Disabled Reason |
+------------------+--------------+------+---------+-------+----------------------------+-----------------+
| cinder-scheduler |  controller  | nova | enabled |   up  | 2018-01-25T11:45:42.000000 |        -        |
|  cinder-volume   | compute1@lvm | nova | enabled |   up  | 2018-01-25T11:45:21.000000 |        -        |
|  cinder-volume   | compute1@ssd | nova | enabled |   up  | 2018-01-25T11:45:42.000000 |        -        |
+------------------+--------------+------+---------+-------+----------------------------+-----------------+</pre>
</div>

### <span id="1105_Dashboard">1.10.5 <span style="font-family: '微软雅黑',sans-serif;">在</span>Dashboard<span style="font-family: '微软雅黑',sans-serif;">中如何创建硬盘</span></span>

1<span style="font-family: '微软雅黑',sans-serif;">、登陆浏览器</span>dashboard<span style="font-family: '微软雅黑',sans-serif;">，</span>http://10.0.0.31/dashboard

<span style="font-family: '微软雅黑',sans-serif;">选择创建卷</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192331131-237454478.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

2<span style="font-family: '微软雅黑',sans-serif;">）创建一个</span>sata<span style="font-family: '微软雅黑',sans-serif;">类型的卷</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192344678-1797523691.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

3<span style="font-family: '微软雅黑',sans-serif;">）创建过程</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192416865-555769969.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   <span style="font-family: '微软雅黑',sans-serif;">创建完成</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192422944-1896724939.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

4<span style="font-family: '微软雅黑',sans-serif;">）床啊进</span>ssd<span style="font-family: '微软雅黑',sans-serif;">类型卷</span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192434272-1913396240.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

5<span style="font-family: '微软雅黑',sans-serif;">）在查看创建的硬盘</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192442006-2113467402.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在命令行中查看添加的块存储</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> lvs</span>
  LV                                          VG                  Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%<span style="color: #000000;">Sync Convert
  volume</span>-0ea47012-c0fb-4dc4-90e7-89427fe9e675 cinder-volumes-sata -wi-a----- 1<span style="color: #000000;">.00g                                                    
  volume</span>-288efecb-6bf0-4409-9564-81b0a6edc9b8 cinder-volumes-sata -wi-a----- 1<span style="color: #000000;">.00g                                                    
  volume</span>-ab347594-6402-486d-87a1-19358aa92a08 cinder-volumes-sata -wi-a----- 1<span style="color: #000000;">.00g                                                    
  volume</span>-33ccbb43-8bd3-4006-849d-73fe6176ea90 cinder-volumes-ssd  -wi-a----- 1<span style="color: #000000;">.00g                                                    
  volume</span>-cfd0ac03-f03f-4fe2-b369-76dba946934d cinder-volumes-ssd  -wi-a----- 1.00g</pre>
</div>

### <span id="1106">1.10.6 <span style="font-family: '微软雅黑',sans-serif;">添加硬盘到虚拟机</span></span>

<p style="text-align: center;">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192514459-1388267652.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<span style="font-family: '微软雅黑',sans-serif;">连接到一个实例</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127192526490-776538225.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<span style="font-family: '微软雅黑',sans-serif;">登陆虚拟机</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ssh cirros@172.16.1.101</span>
<span style="color: #000000;">$ lsblk 
NAME   MAJ:MIN RM    SIZE RO TYPE MOUNTPOINT
vda    </span>253<span style="color: #000000;">:0    0      1G  0 disk 
`</span>-vda1 253:1    0 1011.9M  0 part /<span style="color: #000000;">
vdb    </span>253:16   0      1G  0 disk</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">格式化磁盘</span>

<div class="cnblogs_code">
  <pre>$ sudo mkfs.ext3  /dev/<span style="color: #000000;">vdb  
$ sudo mount </span>/dev/vdb  /mnt/</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">创建文件测试</span>

<div class="cnblogs_code">
  <pre>$ cd /mnt/<span style="color: #000000;">
$ sudo touch clsn
$ ls
clsn        lost</span>+found</pre>
</div>

## <span id="111-2">1.11 <span style="font-family: '微软雅黑',sans-serif;">添加一台新的计算节点</span></span>

### <span id="1111">1.11.1 <span style="font-family: '微软雅黑',sans-serif;">主机基础环境配置</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">要求：主机的配置与之前的系统相同配置相同，推荐</span>4G<span style="font-family: '微软雅黑',sans-serif;">以上内存。</span>
</p>

<span style="background: lime;">1</span><span style="font-family: '微软雅黑',sans-serif;">）配置本地</span><span style="background: lime;">yum</span><span style="font-family: '微软雅黑',sans-serif;">仓库（提高安装速度）</span>

<div class="cnblogs_code">
  <pre>cd /opt/ && wget http://10.0.0.1:8080/openstack/<span style="color: #000000;">openstack_rpm.tar.gz
tar xf openstack_rpm.tar.gz
echo  </span><span style="color: #800000;">'</span><span style="color: #800000;">mount /dev/cdrom /mnt</span><span style="color: #800000;">'</span>  &gt;&gt;/etc/rc.d/<span style="color: #000000;">rc.local
mount </span>/dev/cdrom /<span style="color: #000000;">mnt
chmod </span>+x /etc/rc.d/<span style="color: #000000;">rc.local
cat </span>&gt;/etc/yum.repos.d/local.repo&lt;&lt;-<span style="color: #800000;">'</span><span style="color: #800000;">EOF</span><span style="color: #800000;">'</span><span style="color: #000000;">
[local]
name</span>=<span style="color: #000000;">local
baseurl</span>=file:///<span style="color: #000000;">mnt
gpgcheck</span>=<span style="color: #000000;">0

[openstack]
name</span>=openstack-<span style="color: #000000;">mitaka
baseurl</span>=file:///opt/<span style="color: #000000;">repo
gpgcheck</span>=<span style="color: #000000;">0
EOF</span></pre>
</div>

<span style="background: lime;">2</span><span style="font-family: '微软雅黑',sans-serif;">）配置</span><span style="background: lime;">NTP</span><span style="font-family: '微软雅黑',sans-serif;">时间服务</span>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 安装软件</span>
yum install chrony -<span style="color: #000000;">y 
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 修改配置信息，同步chrony服务</span>
sed -ri.bak <span style="color: #800000;">'</span><span style="color: #800000;">/server/s/^/#/g;2a server 10.0.0.11 iburst</span><span style="color: #800000;">'</span> /etc/<span style="color: #000000;">chrony.conf
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 启动，设置自启动</span>
<span style="color: #000000;">systemctl enable chronyd.service
systemctl start chronyd.service</span></pre>
</div>

<span style="background: lime;">3</span><span style="font-family: '微软雅黑',sans-serif;">）安装</span><span style="background: lime;">OpenStack</span><span style="font-family: '微软雅黑',sans-serif;">的包操作</span>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;">安装 OpenStack 客户端：</span>
yum -y install python-<span style="color: #000000;">openstackclient
</span><span style="color: #008000;">#</span><span style="color: #008000;">安装 openstack-selinux 软件包</span>
yum -y install openstack-selinux</pre>
</div>

### <span id="1112">1.11.2 <span style="font-family: '微软雅黑',sans-serif;">安装配置计算服务</span></span>

<span style="font-family: '微软雅黑',sans-serif;">安装</span>nova<span style="font-family: '微软雅黑',sans-serif;">软件包</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-nova-compute</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命令集修改配置文件</span>

<div class="cnblogs_code">
  <pre>yum install openstack-utils -<span style="color: #000000;">y
cp </span>/etc/nova/<span style="color: #000000;">nova.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/nova/nova.conf.bak &gt;/etc/nova/<span style="color: #000000;">nova.conf
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT enabled_apis  osapi_compute,metadata
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT rpc_backend  rabbit
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT auth_strategy  keystone
openstack</span>-config --set /etc/nova/nova.conf  DEFAULT my_ip  10.0.0.32<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT use_neutron  True
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  DEFAULT firewall_driver  nova.virt.firewall.NoopFirewallDriver
openstack</span>-config --set /etc/nova/nova.conf  glance api_servers  http://controller:9292<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  keystone_authtoken  memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  auth_type  password
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  project_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  user_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  project_name  service
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  username  nova
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  keystone_authtoken  password  NOVA_PASS
openstack</span>-config --set /etc/nova/nova.conf  oslo_concurrency lock_path  /var/lib/nova/<span style="color: #000000;">tmp
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_host  controller
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_userid  openstack
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  oslo_messaging_rabbit   rabbit_password  RABBIT_PASS
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  vnc enabled  True
openstack</span>-config --set /etc/nova/nova.conf  vnc vncserver_listen  0.0.0.0<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  vnc vncserver_proxyclient_address  <span style="color: #800000;">'</span><span style="color: #800000;">$my_ip</span><span style="color: #800000;">'</span><span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  vnc novncproxy_base_url  http://controller:6080/vnc_auto.html</pre>
</div>

### <span id="1113_neutron">1.11.3 <span style="font-family: '微软雅黑',sans-serif;">配置</span>neutron<span style="font-family: '微软雅黑',sans-serif;">网络</span></span>

<span style="font-family: '微软雅黑',sans-serif;">安装</span>neutron<span style="font-family: '微软雅黑',sans-serif;">相关组件</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-neutron-linuxbridge ebtables ipset</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">修改</span>neutron<span style="font-family: '微软雅黑',sans-serif;">配置</span>

<div class="cnblogs_code">
  <pre>cp /etc/neutron/<span style="color: #000000;">neutron.conf{,.bak}
grep </span>-Ev <span style="color: #800000;">'</span><span style="color: #800000;">^$|#</span><span style="color: #800000;">'</span> /etc/neutron/neutron.conf.bak &gt;/etc/neutron/<span style="color: #000000;">neutron.conf
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT rpc_backend  rabbit
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  DEFAULT auth_strategy  keystone
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/neutron.conf  keystone_authtoken memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken auth_type  password
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken project_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken user_domain_name  default
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken project_name  service
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken username  neutron
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  keystone_authtoken password  NEUTRON_PASS
openstack</span>-config --set /etc/neutron/neutron.conf  oslo_concurrency lock_path  /var/lib/neutron/<span style="color: #000000;">tmp
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  oslo_messaging_rabbit rabbit_host  controller
openstack</span>-config --set /etc/neutron/<span style="color: #000000;">neutron.conf  oslo_messaging_rabbit rabbit_userid  openstack
openstack</span>-config --set /etc/neutron/neutron.conf  oslo_messaging_rabbit rabbit_password  RABBIT_PASS</pre>
</div>

<p style="margin-left: 12.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">代理</span>
</p>

<div class="cnblogs_code">
  <pre>cp /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/neutron/plugins/ml2/linuxbridge_agent.ini.bak &gt;/etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  linux_bridge physical_interface_mappings  provider:eth0
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  securitygroup enable_security_group  True
openstack</span>-config --set /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini  securitygroup firewall_driver  neutron.agent.linux.iptables_firewall.IptablesFirewallDriver
openstack</span>-config --set /etc/neutron/plugins/ml2/linuxbridge_agent.ini  vxlan enable_vxlan  False</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">再次配置</span> nova <span style="font-family: '微软雅黑',sans-serif;">服务</span>

<div class="cnblogs_code">
  <pre>openstack-config --set /etc/nova/nova.conf  neutron url  http://controller:9696<span style="color: #000000;">
openstack</span>-config --set /etc/nova/nova.conf  neutron auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron auth_type  password
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron project_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron user_domain_name  default
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron region_name  RegionOne
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron project_name  service
openstack</span>-config --set /etc/nova/<span style="color: #000000;">nova.conf  neutron username  neutron
openstack</span>-config --set /etc/nova/nova.conf  neutron password  NEUTRON_PASS</pre>
</div>

### <span id="1114">1.11.4 <span style="font-family: '微软雅黑',sans-serif;">启动计算节点</span></span>

#<span style="font-family: '微软雅黑',sans-serif;">启动</span>nova<span style="font-family: '微软雅黑',sans-serif;">服务，设置开机自启动</span>

<div class="cnblogs_code">
  <pre>systemctl enable libvirtd.service openstack-nova-<span style="color: #000000;">compute.service
systemctl start libvirtd.service openstack</span>-nova-compute.service</pre>
</div>

\# <span style="font-family: '微软雅黑',sans-serif;">启动</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">代理并配置它开机自启动</span>

<div class="cnblogs_code">
  <pre>systemctl enable neutron-linuxbridge-<span style="color: #000000;">agent.service
systemctl start neutron</span>-linuxbridge-agent.service</pre>
</div>

\# <span style="font-family: '微软雅黑',sans-serif;">查看状态</span>

<div class="cnblogs_code">
  <pre>systemctl status libvirtd.service openstack-nova-<span style="color: #000000;">compute.service
systemctl stauts neutron</span>-linuxbridge-agent.service</pre>
</div>

### <span id="1115">1.11.5 <span style="font-family: '微软雅黑',sans-serif;">验证之前的操作</span></span>

<span style="font-family: '微软雅黑',sans-serif;">在控制节点验证配置</span>

<div class="cnblogs_code">
  <pre>neutron agent-list</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">验证网络配置</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> neutron agent-list </span>
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| id                   | agent_type         | host       | availability_zone | alive | admin_state_up | binary                  |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| 3ab2f17f-737e-4c3f-  | DHCP agent         | controller | nova              | :-)   | True           | neutron-dhcp-agent      |
| 86f0-2289c56a541b    |                    |            |                   |       |                |                         |
| 4f64caf6-a9b0-4742-b | Linux bridge agent | controller |                   | :-)   | True           | neutron-linuxbridge-    |
| 0d1-0d961063200a     |                    |            |                   |       |                | agent                   |
| 630540de-d0a0-473b-  | Linux bridge agent | compute1   |                   | :-)   | True           | neutron-linuxbridge-    |
| 96b5-757afc1057de    |                    |            |                   |       |                | agent                   |
| 9989ddcb-6aba-4b7f-  | Metadata agent     | controller |                   | :-)   | True           | neutron-metadata-agent  |
| 9bd7-7d61f774f2bb    |                    |            |                   |       |                |                         |
| af40d1db-ff24-4201-b | Linux bridge agent | compute2   |                   | :-)   | True           | neutron-linuxbridge-    |
| 0f2-175fc1542f26     |                    |            |                   |       |                | agent                   |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">验证计算节点</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack compute service list </span>
+----+------------------+------------+----------+---------+-------+----------------------------+
| Id | Binary           | Host       | Zone     | Status  | State | Updated At                 |
+----+------------------+------------+----------+---------+-------+----------------------------+
|  1 | nova-scheduler   | controller | internal | enabled | up    | 2018-01-24T06:06:02.000000 |
|  2 | nova-conductor   | controller | internal | enabled | up    | 2018-01-24T06:06:04.000000 |
|  3 | nova-consoleauth | controller | internal | enabled | up    | 2018-01-24T06:06:03.000000 |
|  6 | nova-compute     | compute1   | nova     | enabled | up    | 2018-01-24T06:06:05.000000 |
|  7 | nova-compute     | compute2   | nova     | enabled | up    | 2018-01-24T06:06:00.000000 |
+----+------------------+------------+----------+---------+-------+----------------------------+</pre>
</div>

## <span id="112_Glance">1.12 Glance<span style="font-family: '微软雅黑',sans-serif;">镜像服务迁移</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">将</span>glance<span style="font-family: '微软雅黑',sans-serif;">服务迁移到其他节点上，减轻控制节点压力，提高性能。</span>
</p>

### <span id="1121">1.12.1 <span style="font-family: '微软雅黑',sans-serif;">数据库迁移</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">本次</span>glance<span style="font-family: '微软雅黑',sans-serif;">迁移到</span>compute2<span style="font-family: '微软雅黑',sans-serif;">节点上</span>
</p>

<span style="font-family: '微软雅黑',sans-serif;">安装数据库</span>

<div class="cnblogs_code">
  <pre>yum -y install mariadb mariadb-server python2-PyMySQL</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">修改数据库配置文件</span>

<div class="cnblogs_code">
  <pre>[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/my.cnf.d/openstack.cnf</span>
<span style="color: #000000;">[mysqld]
bind</span>-address = 10.0.0.32<span style="color: #000000;">
default</span>-storage-engine =<span style="color: #000000;"> innodb
innodb_file_per_table
max_connections </span>= 4096<span style="color: #000000;">
collation</span>-server =<span style="color: #000000;"> utf8_general_ci
character</span>-set-server = utf8</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动数据库，并设置开机自启动</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl enable mariadb.service
systemctl start mariadb.service</span></pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif;">【重要】</span>**<span style="font-family: '微软雅黑',sans-serif;">为了保证数据库服务的安全性，运行</span>\`\`mysql\_secure\_installation\`\`<span style="font-family: '微软雅黑',sans-serif;">脚本</span>

<div class="cnblogs_code">
  <pre>mysql_secure_installation</pre>
</div>

### <span id="1122_glance">1.12.2 <span style="font-family: '微软雅黑',sans-serif;">镜像</span>glance <span style="font-family: '微软雅黑',sans-serif;">数据库迁移</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在控制节点的数据库将</span>glance<span style="font-family: '微软雅黑',sans-serif;">库导出，文件传到计算节点</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysqldump -B glance  &gt; glance.sql</span>
[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rsync -avz  glance.sql  10.0.0.32:/opt/</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">以下操作在</span>compute2<span style="font-family: '微软雅黑',sans-serif;">节点上进行操作</span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">导入数据库：</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute2  ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql </span>
MariaDB [(none)]&gt; source /opt/glance.sql</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">重新创建</span>glance<span style="font-family: '微软雅黑',sans-serif;">授权用户</span>

<div class="cnblogs_code">
  <pre>GRANT ALL PRIVILEGES ON glance.* TO <span style="color: #800000;">'</span><span style="color: #800000;">glance</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">GLANCE_DBPASS</span><span style="color: #800000;">'</span><span style="color: #000000;">;
GRANT ALL PRIVILEGES ON glance.</span>* TO <span style="color: #800000;">'</span><span style="color: #800000;">glance</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span>  IDENTIFIED BY <span style="color: #800000;">'</span><span style="color: #800000;">GLANCE_DBPASS</span><span style="color: #800000;">'</span>;</pre>
</div>

### <span id="1123_glance">1.12.3 <span style="font-family: '微软雅黑',sans-serif;">安装</span>glance<span style="font-family: '微软雅黑',sans-serif;">服务</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考文档</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/glance.html</a>
</p>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">安装</span>glance<span style="font-family: '微软雅黑',sans-serif;">相关软件包</span>
</p>

<div class="cnblogs_code">
  <pre>yum -y install openstack-glance</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑配置文件</span> /etc/glance/glance-api.conf
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意</span><span style="font-family: '微软雅黑',sans-serif;">：修改其中的数据库指向地址，修改为</span>copmute2<span style="font-family: '微软雅黑',sans-serif;">上的数据库。</span>
</p>

   <span style="font-family: '微软雅黑',sans-serif;">批量修改命令集：</span>

<div class="cnblogs_code">
  <pre>yum install openstack-utils -<span style="color: #000000;">y
cp </span>/etc/glance/glance-<span style="color: #000000;">api.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/glance/glance-api.conf.bak &gt;/etc/glance/glance-<span style="color: #000000;">api.conf
openstack</span>-config --set /etc/glance/glance-api.conf  database  connection  mysql+pymysql://glance:GLANCE_DBPASS<span style="color: #ffff00;">@10.0.0.32</span>/<span style="color: #000000;">glance
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  glance_store stores  file,http
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  glance_store default_store  file
openstack</span>-config --set /etc/glance/glance-api.conf  glance_store filesystem_store_datadir  /var/lib/glance/images/<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-api.conf  keystone_authtoken auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-api.conf  keystone_authtoken auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-api.conf  keystone_authtoken memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken auth_type  password
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken project_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken user_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken project_name  service
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken username  glance
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">api.conf  keystone_authtoken password  GLANCE_PASS
openstack</span>-config --set /etc/glance/glance-api.conf  paste_deploy flavor  keystone</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">编辑配置文件</span> /etc/glance/glance-registry.conf

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意</span><span style="font-family: '微软雅黑',sans-serif;">：修改其中的数据库指向地址，修改为</span>copmute2<span style="font-family: '微软雅黑',sans-serif;">上的数据库。</span>
</p>

   <span style="font-family: '微软雅黑',sans-serif;">批量修改命令集：</span>

<div class="cnblogs_code">
  <pre>yum install openstack-utils -<span style="color: #000000;">y
cp </span>/etc/glance/glance-<span style="color: #000000;">registry.conf{,.bak}
grep </span><span style="color: #800000;">'</span><span style="color: #800000;">^[a-Z\[]</span><span style="color: #800000;">'</span> /etc/glance/glance-registry.conf.bak &gt; /etc/glance/glance-<span style="color: #000000;">registry.conf
openstack</span>-config --set /etc/glance/glance-registry.conf  database  connection  mysql+pymysql://glance:GLANCE_DBPASS@<span style="color: #ff0000;">10.0.0.32</span>/<span style="color: #000000;">glance
openstack</span>-config --set /etc/glance/glance-registry.conf  keystone_authtoken auth_uri  http://controller:5000<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-registry.conf  keystone_authtoken auth_url  http://controller:35357<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-registry.conf  keystone_authtoken memcached_servers  controller:11211<span style="color: #000000;">
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken auth_type  password
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken project_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken user_domain_name  default
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken project_name  service
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken username  glance
openstack</span>-config --set /etc/glance/glance-<span style="color: #000000;">registry.conf  keystone_authtoken password  GLANCE_PASS
openstack</span>-config --set /etc/glance/glance-registry.conf  paste_deploy flavor  keystone</pre>
</div>

### <span id="1124">1.12.4 <span style="font-family: '微软雅黑',sans-serif;">迁移原有镜像文件</span></span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">将原</span>glance<span style="font-family: '微软雅黑',sans-serif;">上的镜像文件，传输到</span>compute2<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd  /var/lib/glance/images/</span>
[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rsync -avz `pwd`/ 10.0.0.32:`pwd`/ </span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">【注意权限】</span><span style="font-family: '微软雅黑',sans-serif;">传输过后，在</span>compute2<span style="font-family: '微软雅黑',sans-serif;">上查看权限</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd  /var/lib/glance/images/</span>
[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chown glance:glance *</span></pre>
</div>

### <span id="1125_keystone_glance">1.12.5 <span style="font-family: '微软雅黑',sans-serif;">修改现有</span>keystone<span style="font-family: '微软雅黑',sans-serif;">中</span> glance<span style="font-family: '微软雅黑',sans-serif;">服务注册信息</span></span>

<span style="font-family: '微软雅黑',sans-serif;">备份数据库</span>endpoint<span style="font-family: '微软雅黑',sans-serif;">表数据</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysqldump keystone endpoint &gt; endpoint.sql</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">修改</span>keystone<span style="font-family: '微软雅黑',sans-serif;">注册信息</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">cp endpoint.sql{,.bak}
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s#http://controller:9292#http://10.0.0.32:9292#g</span><span style="color: #800000;">'</span> endpoint.sql</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">重新将修改后的</span>sql<span style="font-family: '微软雅黑',sans-serif;">文件导入数据库</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mysql keystone &lt; endpoint.sql</span></pre>
</div>

### <span id="1126_nova">1.12.6 <span style="font-family: '微软雅黑',sans-serif;">修改</span>nova<span style="font-family: '微软雅黑',sans-serif;">节点配置文件</span></span>

<p style="margin-left: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">将<span style="color: red;">所有的节点</span>上的配置文件都进行修改</span>
</p>

<div class="cnblogs_code">
  <pre>sed -i <span style="color: #800000;">'</span><span style="color: #800000;">s#api_servers = http://controller:9292#api_servers = http://10.0.0.32:9292#g</span><span style="color: #800000;">'</span> /etc/nova/nova.conf</pre>
</div>

<p style="text-indent: 24.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">控制节点重启</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl restart openstack-nova-api.service openstack-nova-consoleauth.service openstack-nova-scheduler.service openstack-nova-conductor.service openstack-nova-novncproxy.service</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">计算节点重启</span>

<div class="cnblogs_code">
  <pre>systemctl restart   openstack-nova-compute.service</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">停掉</span>glance<span style="font-family: '微软雅黑',sans-serif;">原节点的服务</span>

<div class="cnblogs_code">
  <pre>systemctl stop openstack-glance-api.service  openstack-glance-registry.service</pre>
</div>

### <span id="1127">1.12.7 <span style="font-family: '微软雅黑',sans-serif;">验证操作</span></span>

<span style="font-family: '微软雅黑',sans-serif;">在</span>copmute2<span style="font-family: '微软雅黑',sans-serif;">节点启动</span>glance<span style="font-family: '微软雅黑',sans-serif;">服务</span>

<div class="cnblogs_code">
  <pre>systemctl start openstack-glance-api.service  openstack-glance-registry.service</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看镜像列表</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack image list </span>
+--------------------------------------+----------+--------+
| ID                                   | Name     | Status |
+--------------------------------------+----------+--------+
| 68222030-a808-4d05-978f-1d4a6f85f7dd | clsn-img | active |
| 9d92c601-0824-493a-bc6e-cecb10e9a4c6 | cirros   | active |
+--------------------------------------+----------+--------+</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看</span>web<span style="font-family: '微软雅黑',sans-serif;">界面中的镜像信息</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193456365-946295776.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

## <span id="113-2">1.13 <span style="font-family: '微软雅黑',sans-serif;">添加一个新的网段并让它能够上网</span></span>

### <span id="1131">1.13.1 <span style="font-family: '微软雅黑',sans-serif;">环境准备</span></span>

<p style="margin-left: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）为</span>openstack<span style="font-family: '微软雅黑',sans-serif;">服务机器机器添加一块新的网卡（所有机器操作）。</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">网卡选择</span>LAN<span style="font-family: '微软雅黑',sans-serif;">区段，并保证所有的机器在同一个</span>LAN<span style="font-family: '微软雅黑',sans-serif;">区段当中。</span>
</p>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193510194-1815801421.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   2<span style="font-family: '微软雅黑',sans-serif;">）主机修改配置，启动</span>eth1<span style="font-family: '微软雅黑',sans-serif;">网卡（所有节点操作）</span>

<span style="font-family: '微软雅黑',sans-serif;">查看网卡设备</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ls /proc/sys/net/ipv4/conf/</span>
all  brq2563bcef-c6  brq54f942f7-<span style="color: #000000;">cc  default  eth0  eth1  lo
[root@compute1 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> cp /etc/sysconfig/network-scripts/ifcfg-eth{0,1}</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">修改网卡配置</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/sysconfig/network-scripts/ifcfg-eth1</span>
TYPE=<span style="color: #000000;">Ethernet
BOOTPROTO</span>=<span style="color: #000000;">none
NAME</span>=<span style="color: #000000;">eth1
DEVICE</span>=<span style="color: #000000;">eth1
ONBOOT</span>=<span style="color: #000000;">yes
IPADDR</span>=172.16.1.31<span style="color: #000000;">
NETMASK</span>=255.255.255.0</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动网卡</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ifup eth0</span></pre>
</div>

### <span id="1132_neutron">1.13.2 <span style="font-family: '微软雅黑',sans-serif;">配置</span>neutron<span style="font-family: '微软雅黑',sans-serif;">服务</span></span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">再增加一个</span>faulte<span style="font-family: '微软雅黑',sans-serif;">网络，这里添加的名为</span>net172
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/neutron/plugin.ini </span>
<span style="color: #000000;">[DEFAULT]
[ml2]
type_drivers </span>=<span style="color: #000000;"> flat,vlan
tenant_network_types </span>=<span style="color: #000000;">
mechanism_drivers </span>=<span style="color: #000000;"> linuxbridge
extension_drivers </span>=<span style="color: #000000;"> port_security
[ml2_type_flat]
flat_networks </span>=<span style="color: #000000;"> provider,<span style="color: #ff0000;">net172</span>
[ml2_type_geneve]
[ml2_type_gre]
[ml2_type_vlan]
[ml2_type_vxlan]
[securitygroup]
enable_ipset </span>= True</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">修改桥接配置，添加</span>eth1<span style="font-family: '微软雅黑',sans-serif;">信息</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/neutron/plugins/ml2/linuxbridge_agent.ini</span>
<span style="color: #000000;">[DEFAULT]
[agent]
[linux_bridge]
physical_interface_mappings </span>=<span style="color: #000000;"> provider:eth0,<span style="color: #ff0000;">net172:eth1</span>
[securitygroup]
enable_security_group </span>=<span style="color: #000000;"> True
firewall_driver </span>=<span style="color: #000000;"> neutron.agent.linux.iptables_firewall.IptablesFirewallDriver
[vxlan]
enable_vxlan </span>= False</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">将桥接配置文件发往各个节点</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rsync -avz /etc/neutron/plugins/ml2/linuxbridge_agent.ini 10.0.0.31:/etc/neutron/plugins/ml2/linuxbridge_agent.ini</span>
····</pre>
</div>

### <span id="1133">1.13.3 <span style="font-family: '微软雅黑',sans-serif;">重启服务</span></span>

<p style="margin-left: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在<strong><span style="color: red;">控制节点</span></strong>重启网络服务</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl restart  neutron-server.service  neutron-linuxbridge-agent.service</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在其他<strong><span style="color: red;">计算节点</span></strong>重启网络服务</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl restart neutron-linuxbridge-agent.service </span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">查看当前网络状态</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> neutron agent-list</span>
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| id                   | agent_type         | host       | availability_zone | alive | admin_state_up | binary                  |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| 3ab2f17f-737e-4c3f-  | DHCP agent         | controller | nova              | :-)   | True           | neutron-dhcp-agent      |
| 86f0-2289c56a541b    |                    |            |                   |       |                |                         |
| 4f64caf6-a9b0-4742-b | Linux bridge agent | controller |                   | :-)   | True           | neutron-linuxbridge-    |
| 0d1-0d961063200a     |                    |            |                   |       |                | agent                   |
| 630540de-d0a0-473b-  | Linux bridge agent | compute1   |                   | :-)   | True           | neutron-linuxbridge-    |
| 96b5-757afc1057de    |                    |            |                   |       |                | agent                   |
| 9989ddcb-6aba-4b7f-  | Metadata agent     | controller |                   | :-)   | True           | neutron-metadata-agent  |
| 9bd7-7d61f774f2bb    |                    |            |                   |       |                |                         |
| af40d1db-ff24-4201-b | Linux bridge agent | compute2   |                   | :-)   | True           | neutron-linuxbridge-    |
| 0f2-175fc1542f26     |                    |            |                   |       |                | agent                   |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+</pre>
</div>

### <span id="1134_iptables">1.13.4 <span style="font-family: '微软雅黑',sans-serif;">配置</span>iptables<span style="font-family: '微软雅黑',sans-serif;">服务器作<span style="background: yellow;">子网网关</span></span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">主机信息</span>
</p>

<div class="cnblogs_code">
  <pre>[root@route ~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r </span>
3.10.0-327<span style="color: #000000;">.el7.x86_64
[root@route </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> hostname -I </span>
10.0.0.2 172.16.1.2</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置内核转发</span>

<div class="cnblogs_code">
  <pre>[root@route ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo 'net.ipv4.ip_forward=1' &gt;&gt;/etc/sysctl.conf</span>
[root@route ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sysctl -p </span>
net.ipv4.ip_forward = 1</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置</span>iptables<span style="font-family: '微软雅黑',sans-serif;">转发规则</span>

<div class="cnblogs_code">
  <pre>iptables -t nat -A POSTROUTING -s 172.16.1.0/24 -o eth0 -j MASQUERADE</pre>
</div>

### <span id="1135_web">1.13.5 web<span style="font-family: '微软雅黑',sans-serif;">界面创建子网</span></span>

<p style="margin-left: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）选择创建网络</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193727006-2061774592.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   2<span style="font-family: '微软雅黑',sans-serif;">）配置在子网</span>

<span style="font-family: '微软雅黑',sans-serif;">网关选择搭建的</span>iptables<span style="font-family: '微软雅黑',sans-serif;">服务器，经由</span>iptables<span style="font-family: '微软雅黑',sans-serif;">服务器进行代理上网</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193737553-1341473394.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   3<span style="font-family: '微软雅黑',sans-serif;">）配置子网</span>IP<span style="font-family: '微软雅黑',sans-serif;">地范围，配置完成子网创建成功</span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193747209-838095327.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   4<span style="font-family: '微软雅黑',sans-serif;">）创建一个新的实例测试子网</span>

<span style="font-family: '微软雅黑',sans-serif;">注意：在创建时，网络选择刚刚创建的</span>net172<span style="font-family: '微软雅黑',sans-serif;">网络</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193754537-1288439442.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   <span style="font-family: '微软雅黑',sans-serif;">实例创建完成</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193801928-976672424.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   5<span style="font-family: '微软雅黑',sans-serif;">）登陆控制台</span>

<span style="font-family: '微软雅黑',sans-serif;">查看网关信息</span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193810162-2005435871.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   <span style="font-family: '微软雅黑',sans-serif;">检测网络连通性</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127193817772-2116885693.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   **<span style="font-family: '微软雅黑',sans-serif;">至此一个新的子网创建成功</span>**

## <span id="114_CinderNFS">1.14 Cinder<span style="font-family: '微软雅黑',sans-serif;">服务对接</span>NFS<span style="font-family: '微软雅黑',sans-serif;">配置</span></span>

<p style="margin-left: 21.0pt;">
  NFS<span style="font-family: '微软雅黑',sans-serif;">服务介绍参考文档：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/7694456.html" >http://www.cnblogs.com/clsn/p/7694456.html</a>
</p>

### <span id="1141_NFS">1.14.1 NFS<span style="font-family: '微软雅黑',sans-serif;">服务部署</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">注意：</span><span style="font-family: '微软雅黑',sans-serif;">实验环境使用控制节点做</span>nfs<span style="font-family: '微软雅黑',sans-serif;">服务器，在生产环境中，需配置高性能存储服务器。</span>
</p>

<span style="font-family: '微软雅黑',sans-serif;">安装</span>nfs<span style="font-family: '微软雅黑',sans-serif;">相关软件包</span>

<div class="cnblogs_code">
  <pre>yum install nfs-utils rpcbind -y</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">配置</span>nfs<span style="font-family: '微软雅黑',sans-serif;">服务</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/exports</span>
/data 10.0.0.0/24<span style="color: #000000;">(rw,async,no_root_squash,no_all_squash)
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 创建目录</span>
[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir /data</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动</span>nfs<span style="font-family: '微软雅黑',sans-serif;">服务，并设置开机自启动</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl restart rpcbind 
systemctl restart nfs
systemctl enable rpcbind  nfs
systemctl status rpcbind  nfs</span></pre>
</div>

### <span id="1142_NFS">1.14.2 <span style="font-family: '微软雅黑',sans-serif;">测试</span>NFS<span style="font-family: '微软雅黑',sans-serif;">的可用性</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在计算节点查看</span>nfs<span style="font-family: '微软雅黑',sans-serif;">信息</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> showmount -e 10.0.0.11</span>
Export list <span style="color: #0000ff;">for</span> 10.0.0.11<span style="color: #000000;">:
</span>/data 10.0.0.0/24</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">进行挂载测试</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mount 10.0.0.11:/data /srv</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">写入文件</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd /srv/</span>
[root@compute1 srv]<span style="color: #008000;">#</span><span style="color: #008000;"> touch clsn </span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">在服务端查看文件是否写入成功。</span>

<div class="cnblogs_code">
  <pre>[root@controller data]<span style="color: #008000;">#</span><span style="color: #008000;"> ll</span>
<span style="color: #000000;">total 0
</span>-rw-r--r-- 1 root root 0 Jan 26 15:35 clsn</pre>
</div>

### <span id="1143_Cinder">1.14.3 <span style="font-family: '微软雅黑',sans-serif;">修改</span>Cinder<span style="font-family: '微软雅黑',sans-serif;">节点配置文件</span></span>

<span style="font-family: '微软雅黑',sans-serif;">首先我们需要知道，</span>cinder<span style="font-family: '微软雅黑',sans-serif;">是通过在</span>cinder.conf<span style="font-family: '微软雅黑',sans-serif;">配置文件来配置驱动从而使用不同的存储介质的，</span> <span style="font-family: '微软雅黑',sans-serif;">所以如果我们使用</span>NFS<span style="font-family: '微软雅黑',sans-serif;">作为存储介质，那么就需要配置成</span>NFS<span style="font-family: '微软雅黑',sans-serif;">的驱动，</span>

<span style="font-family: '微软雅黑',sans-serif;">那么问题来了，如何找到</span>NFS<span style="font-family: '微软雅黑',sans-serif;">的驱动呢？请看下面查找步骤：</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd /usr/lib/python2.7/site-packages/cinder   # 切换到cinder的模块包里</span>
[root@controller cinder]<span style="color: #008000;">#</span><span style="color: #008000;"> cd volume/drivers/   # 找到卷的驱动</span>
[root@controller drivers]<span style="color: #008000;">#</span><span style="color: #008000;"> grep Nfs nfs.py   # 过滤下Nfs就能找到</span>
<span style="color: #0000ff;">class</span> NfsDriver(driver.ExtendVD, remotefs.RemoteFSDriver):  <span style="color: #008000;">#</span><span style="color: #008000;"> 这个class定义的类就是Nfs的驱动名字了</span></pre>
</div>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">驱动找到了，现在修改</span>cinder<span style="font-family: '微软雅黑',sans-serif;">配置添加</span>nfs<span style="font-family: '微软雅黑',sans-serif;">服务器信息</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/cinder/cinder.conf</span>
<span style="color: #000000;">[DEFAULT]
···
enabled_backends </span>=<span style="color: #000000;"> lvm,ssd,nfs

[nfs]
volume_driver </span>=<span style="color: #000000;"> cinder.volume.drivers.nfs.NfsDriver
nfs_shares_config </span>= /etc/cinder/<span style="color: #000000;">nfs_shares
volume_backend_name </span>= nfs</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">写</span>nfs<span style="font-family: '微软雅黑',sans-serif;">信息文件</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/cinder/nfs_shares</span>
10.0.0.11:/<span style="color: #000000;">data
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 修改权限</span>
chown root:cinder /etc/cinder/<span style="color: #000000;">nfs_shares
chmod </span>640 /etc/cinder/nfs_shares</pre>
</div>

### <span id="1144">1.14.4 <span style="font-family: '微软雅黑',sans-serif;">重启服务</span></span>

<p style="margin-left: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">重启</span>cinder-volume<span style="font-family: '微软雅黑',sans-serif;">服务</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl restart openstack-cinder-volume</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看挂载信息</span>

<div class="cnblogs_code">
  <pre>[root@compute1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> df -h</span>
Filesystem       Size  Used Avail Use%<span style="color: #000000;"> Mounted on
</span>/dev/sda2         48G  4.0G   45G   9% /<span style="color: #000000;">
devtmpfs         480M     0  480M   0</span>% /<span style="color: #000000;">dev
tmpfs            489M     0  489M   0</span>% /dev/<span style="color: #000000;">shm
tmpfs            489M   13M  477M   </span>3% /<span style="color: #000000;">run
tmpfs            489M     0  489M   0</span>% /sys/fs/<span style="color: #000000;">cgroup
</span>/dev/sr0         4.1G  4.1G     0 100% /<span style="color: #000000;">mnt
tmpfs             98M     0   98M   0</span>% /run/user/<span style="color: #000000;">0
</span>10.0.0.11:/data   48G  2.9G   46G   6% <span style="color: #ff0000;">/var/lib/cinder/mnt/490717a467bd12d34ec324c86a4f35b3</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在控制节点验证服务是否正常</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;">  cinder service-list</span>
+------------------+--------------+------+---------+-------+----------------------------+-----------------+
|      Binary      |     Host     | Zone |  Status | State |         Updated_at         | Disabled Reason |
+------------------+--------------+------+---------+-------+----------------------------+-----------------+
| cinder-scheduler |  controller  | nova | enabled |   up  | 2018-01-26T13:18:45.000000 |        -        |
|  cinder-volume   | compute1@lvm | nova | enabled |   up  | 2018-01-26T13:18:42.000000 |        -        |
| <span style="color: #ff0000;"> cinder-volume   | compute1@nfs | nova | enabled |   up  | 2018-01-26T13:18:42.000000</span> |        -        |
|  cinder-volume   | compute1@ssd | nova | enabled |   up  | 2018-01-26T13:18:42.000000 |        -        |
|  cinder-volume   | compute2@lvm | nova | enabled |   up  | 2018-01-26T13:18:50.000000 |        -        |
+------------------+--------------+------+---------+-------+----------------------------+-----------------+</pre>
</div>

### <span id="1145_NFS">1.14.5 <span style="font-family: '微软雅黑',sans-serif;">添加</span>NFS<span style="font-family: '微软雅黑',sans-serif;">存储卷</span></span>

<p style="margin-left: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）创建</span>nfs<span style="font-family: '微软雅黑',sans-serif;">类型卷</span>
</p>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194058553-836722711.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   2<span style="font-family: '微软雅黑',sans-serif;">）创建成功</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194107006-409148844.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   3<span style="font-family: '微软雅黑',sans-serif;">）查看卷的详细信息</span>

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194115459-593792341.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   <span style="font-family: '微软雅黑',sans-serif;">在</span>nfs<span style="font-family: '微软雅黑',sans-serif;">服务端，查找到标识一致的文件</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /data/</span>
<span style="color: #000000;">total 0
</span>-rw-r--r-- 1 root root          0 Jan 26 15:35<span style="color: #000000;"> clsn
</span>-rw-rw-rw- 1 root root 1073741824 Jan 26 21:23 volume-8c55c9bf-6ab2-4828-a14e-76bd525ba4ad</pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif;">至此</span><span style="color: red; background: yellow;">Cinder</span>****<span style="font-family: '微软雅黑',sans-serif;">对接</span><span style="color: red; background: yellow;">NFS</span>****<span style="font-family: '微软雅黑',sans-serif;">就完成了</span>**

## <span id="115_OpenStackVXLAN">1.15 OpenStack<span style="font-family: '微软雅黑',sans-serif;">中的</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">网络</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">本次的配置时基于</span>" <a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-controller-install-option1.html"><span style="font-family: '微软雅黑',sans-serif;">网络选项1：公共网络</span></a>" <span style="font-family: '微软雅黑',sans-serif;">进行配置。配置参考</span> "<a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-controller-install-option2.html"><span style="font-family: '微软雅黑',sans-serif;">网络选项2：私有网络</span></a>"<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

### <span id="1151">1.15.1 <span style="font-family: '微软雅黑',sans-serif;">前期准备</span></span>

<p style="text-indent: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）添加网卡</span>eth2 (<span style="font-family: '微软雅黑',sans-serif;">所有节点操作</span>)
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194134194-554487846.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  2<span style="font-family: '微软雅黑',sans-serif;">）配置网卡，配置网段</span>172.16.0.x<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<div class="cnblogs_code">
  <pre>cp /etc/sysconfig/network-scripts/ifcfg-eth{1,2<span style="color: #000000;">}
vim  </span>/etc/sysconfig/network-scripts/ifcfg-<span style="color: #000000;">eth2
TYPE</span>=<span style="color: #000000;">Ethernet
BOOTPROTO</span>=<span style="color: #000000;">none
NAME</span>=<span style="color: #000000;">eth2
DEVICE</span>=<span style="color: #000000;">eth2
ONBOOT</span>=<span style="color: #000000;">yes
IPADDR</span>=172.16<span style="color: #000000;">.0.X
NETMASK</span>=255.255.255.0</pre>
</div>

<p style="text-indent: 15.75pt;">
  3<span style="font-family: '微软雅黑',sans-serif;">）启动网卡</span>
</p>

<div class="cnblogs_code">
  <pre>ifup eth2</pre>
</div>

### <span id="1152">1.15.2 <span style="font-family: '微软雅黑',sans-serif;">修改控制节点配置</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考文档：</span><a href="https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-controller-install-option2.html">https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/neutron-controller-install-option2.html</a>
</p>

<p style="text-indent: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）安装组件</span>
</p>

<div class="cnblogs_code">
  <pre>yum -y install openstack-neutron openstack-neutron-ml2 openstack-neutron-linuxbridge ebtables</pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">）修改配置文件</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">修改</span><strong><span style="background: yellow;"> /etc/neutron/neutron.conf</span></strong>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
core_plugin </span>=<span style="color: #000000;"> ml2
service_plugins </span>=<span style="color: #000000;"> router
allow_overlapping_ips </span>= True</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置</span> Modular Layer 2 (ML2) <span style="font-family: '微软雅黑',sans-serif;">插件，<span style="background: yellow;">修改</span></span><strong><span style="background: yellow;">/etc/neutron/plugins/ml2/ml2_conf.ini</span></strong>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用</span>flat<span style="font-family: '微软雅黑',sans-serif;">，</span>VLAN<span style="font-family: '微软雅黑',sans-serif;">以及</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">网络</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[ml2]
...
type_drivers </span>= flat,vlan,vxlan</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">私有网络</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[ml2]
...
tenant_network_types </span>= vxlan</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2]``<span style="font-family: '微软雅黑',sans-serif;">部分，启用</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">和</span>layer<span style="font-family: '微软雅黑',sans-serif;">－</span>2<span style="font-family: '微软雅黑',sans-serif;">机制：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[ml2]
...
mechanism_drivers </span>= linuxbridge,l2population</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[ml2_type_vxlan]``<span style="font-family: '微软雅黑',sans-serif;">部分，为私有网络配置</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">网络识别的网络范围</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[ml2_type_vxlan]
...
vni_ranges </span>= 1:1000</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">代理，<span style="background: yellow;">修改</span></span><strong><span style="background: yellow;"> /etc/neutron/plugins/ml2/linuxbridge_agent.ini</span></strong>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[vxlan]
enable_vxlan </span>=<span style="color: #000000;"> True
local_ip </span>= 172.16.0.11<span style="color: #000000;">
l2_population </span>= True</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置</span>layer<span style="font-family: '微软雅黑',sans-serif;">－</span>3<span style="font-family: '微软雅黑',sans-serif;">代理，<span style="background: yellow;">编辑</span></span><span style="background: yellow;">``/etc/neutron/l3_agent.ini``</span><span style="font-family: '微软雅黑',sans-serif;">文件并完成以下操作</span><span style="font-family: '微软雅黑',sans-serif;">：</span>
</p>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>``[DEFAULT]``<span style="font-family: '微软雅黑',sans-serif;">部分，配置</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">接口驱动和外部网络网桥</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">[DEFAULT]
...
interface_driver </span>=<span style="color: #000000;"> neutron.agent.linux.interface.BridgeInterfaceDriver
external_network_bridge </span>=</pre>
</div>

<p style="text-indent: 15.75pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">同步数据库</span></strong>
</p>

<div class="cnblogs_code">
  <pre>su -s /bin/sh -c <span style="color: #800000;">"</span><span style="color: #800000;">neutron-db-manage --config-file /etc/neutron/neutron.conf \
  --config-file /etc/neutron/plugins/ml2/ml2_conf.ini upgrade head</span><span style="color: #800000;">"</span> neutron</pre>
</div>

<p style="text-indent: 15.75pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">启动服务</span></strong>
</p>

<div class="cnblogs_code">
  <pre>systemctl restart neutron-<span style="color: #000000;">server.service \
neutron</span>-linuxbridge-agent.service neutron-dhcp-<span style="color: #000000;">agent.service \
neutron</span>-metadata-<span style="color: #000000;">agent.service
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 启动l3网络</span>
systemctl enable neutron-l3-<span style="color: #000000;">agent.service
systemctl start neutron</span>-l3-agent.service</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">检查网络状态</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> neutron agent-list</span>
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| id                   | agent_type         | host       | availability_zone | alive | admin_state_up | binary                  |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| 3ab2f17f-737e-4c3f-  | DHCP agent         | controller | nova              | :-)   | True           | neutron-dhcp-agent      |
| 86f0-2289c56a541b    |                    |            |                   |       |                |                         |
| 4f64caf6-a9b0-4742-b | Linux bridge agent | controller |                   | :-)   | True           | neutron-linuxbridge-    |
| 0d1-0d961063200a     |                    |            |                   |       |                | agent                   |
| 630540de-d0a0-473b-  | Linux bridge agent | compute1   |                   | :-)   | True           | neutron-linuxbridge-    |
| 96b5-757afc1057de    |                    |            |                   |       |                | agent                   |
| 9989ddcb-6aba-4b7f-  | Metadata agent     | controller |                   | :-)   | True           | neutron-metadata-agent  |
| 9bd7-7d61f774f2bb    |                    |            |                   |       |                |                         |
| af40d1db-ff24-4201-b | Linux bridge agent | compute2   |                   | :-)   | True           | neutron-linuxbridge-    |
| 0f2-175fc1542f26     |                    |            |                   |       |                | agent                   |
| b08be87c-4abe-48ce-  | L3 agent           | controller | nova              | :-)   | True           | neutron-l3-agent        |
| 983f-0bb08208f6de    |                    |            |                   |       |                |                         |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+</pre>
</div>

### <span id="1153">1.15.3 <span style="font-family: '微软雅黑',sans-serif;">修改配置计算节点文件</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">配置</span>Linuxbridge<span style="font-family: '微软雅黑',sans-serif;">代理，添加配置</span>
</p>

<div class="cnblogs_code">
  <pre>vim /etc/neutron/plugins/ml2/<span style="color: #000000;">linuxbridge_agent.ini
[vxlan]
enable_vxlan </span>=<span style="color: #000000;"> True
local_ip </span>=<span style="color: #000000;"><span style="color: #ff0000;"> OVERLAY_INTERFACE_IP_ADDRESS</span>
l2_population </span>= True</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">重启服务</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl restart neutron-linuxbridge-agent.service</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">再次检查网络状态</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> neutron agent-list</span>
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| id                   | agent_type         | host       | availability_zone | alive | admin_state_up | binary                  |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+
| 3ab2f17f-737e-4c3f-  | DHCP agent         | controller | nova              | :-)   | True           | neutron-dhcp-agent      |
| 86f0-2289c56a541b    |                    |            |                   |       |                |                         |
| 4f64caf6-a9b0-4742-b | Linux bridge agent | controller |                   | :-)   | True           | neutron-linuxbridge-    |
| 0d1-0d961063200a     |                    |            |                   |       |                | agent                   |
| 630540de-d0a0-473b-  | Linux bridge agent | compute1   |                   | :-)   | True           | neutron-linuxbridge-    |
| 96b5-757afc1057de    |                    |            |                   |       |                | agent                   |
| 9989ddcb-6aba-4b7f-  | Metadata agent     | controller |                   | :-)   | True           | neutron-metadata-agent  |
| 9bd7-7d61f774f2bb    |                    |            |                   |       |                |                         |
| af40d1db-ff24-4201-b | Linux bridge agent | compute2   |                   | :-)   | True           | neutron-linuxbridge-    |
| 0f2-175fc1542f26     |                    |            |                   |       |                | agent                   |
| b08be87c-4abe-48ce-  | L3 agent           | controller | nova              | :-)   | True           | neutron-l3-agent        |
| 983f-0bb08208f6de    |                    |            |                   |       |                |                         |
+----------------------+--------------------+------------+-------------------+-------+----------------+-------------------------+</pre>
</div>

### <span id="1154_dashboard">1.15.4 <span style="font-family: '微软雅黑',sans-serif;">修改</span>dashboard<span style="font-family: '微软雅黑',sans-serif;">开启路由界面显示</span></span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">该操作是在</span>web<span style="font-family: '微软雅黑',sans-serif;">界面开启</span>route<span style="font-family: '微软雅黑',sans-serif;">功能</span>
</p>

<div class="cnblogs_code">
  <pre>vim /etc/openstack-dashboard/<span style="color: #000000;">local_settings
OPENSTACK_NEUTRON_NETWORK </span>=<span style="color: #000000;"> {
    </span><span style="color: #800000;">'</span><span style="color: #800000;">enable_router</span><span style="color: #800000;">'</span><span style="color: #000000;">: True,
····</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">重启</span>dashboard<span style="font-family: '微软雅黑',sans-serif;">服务</span>
</p>

<div class="cnblogs_code">
  <pre>systemctl restart httpd.service</pre>
</div>

### <span id="1155_VXLAN">1.15.5 <span style="font-family: '微软雅黑',sans-serif;">配置</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">网络</span></span>

<p style="margin-left: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）查看现在网络拓扑</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194445490-98320466.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   2<span style="font-family: '微软雅黑',sans-serif;">）编辑网络配置，开启外部网络</span>

<p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; layout-grid-mode: both;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194457397-40298296.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   3<span style="font-family: '微软雅黑',sans-serif;">）配置网络</span>

<p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; layout-grid-mode: both;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194505397-744089759.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   4<span style="font-family: '微软雅黑',sans-serif;">）配置子网</span>

<p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; layout-grid-mode: both;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194514428-2079928145.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   5<span style="font-family: '微软雅黑',sans-serif;">）创建路由器</span>

<span style="font-family: '微软雅黑',sans-serif;">创建路由时，注意配置外部网络连接</span>.

<p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; layout-grid-mode: both;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194523319-1433152884.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="margin: 0cm; margin-bottom: .0001pt; text-indent: 21.0pt; layout-grid-mode: both;">
  <strong><span style="font-family: 宋体; background: yellow;">路由器实质为创建命名空间</span></strong>
</p>

<p style="margin: 0cm; margin-bottom: .0001pt; layout-grid-mode: both;">
  <span style="font-family: 宋体;">查看命名空间列表</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ip netns</span>
qdhcp-ac1f482b-5c37-4da2-8922-<span style="color: #000000;">c8d02e3ad27b
qrouter</span>-546678a3-7277-42a6-9ddd-<span style="color: #000000;">a060e3d3198d
qdhcp</span>-2563bcef-c6b0-43f1-9b17-<span style="color: #000000;">1eca15472893
qdhcp</span>-54f942f7-cc28-4292-a4d6-e37b8833e35f</pre>
</div>

<p style="margin: 0cm; margin-bottom: .0001pt; layout-grid-mode: both;">
  <span style="font-family: 宋体;">    </span><span style="font-family: 宋体;">进入命名空间</span>
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ip netns exec qrouter-546678a3-7277-42a6-9ddd-a060e3d3198d /bin/bash</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  6<span style="font-family: '微软雅黑',sans-serif;">）为路由器添加接口连接子网</span>
</p>

<p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; layout-grid-mode: both;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194549506-1106040010.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   7<span style="font-family: '微软雅黑',sans-serif;">）创建一台实例，使用配置的</span>VXLAN<span style="font-family: '微软雅黑',sans-serif;">网络</span>

<span style="font-family: '微软雅黑',sans-serif;">注意选择配置</span>vxlan<span style="font-family: '微软雅黑',sans-serif;">的网络配置</span>

<p style="margin: 0cm; margin-bottom: .0001pt; text-align: center; layout-grid-mode: both;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194557162-464684758.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   8<span style="font-family: '微软雅黑',sans-serif;">）为创建的实例配置浮动</span>IP

<p style="text-align: center;" align="center">
   <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194604334-1474928604.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   <span style="font-family: '微软雅黑',sans-serif;">配置浮动</span>IP<span style="font-family: '微软雅黑',sans-serif;">后的实例</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194611865-1955137906.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

### <span id="1156_IP">1.15.6 <span style="font-family: '微软雅黑',sans-serif;">连接浮动</span>IP<span style="font-family: '微软雅黑',sans-serif;">测试</span></span>

<p style="margin-left: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">使用</span>ssh<span style="font-family: '微软雅黑',sans-serif;">连接主机，由于之前定制的进行</span>root<span style="font-family: '微软雅黑',sans-serif;">密码进行修改可以使用</span>root<span style="font-family: '微软雅黑',sans-serif;">用户直接进行</span> <span style="font-family: '微软雅黑',sans-serif;">连接。</span>
</p>

<div class="cnblogs_code">
  <pre>[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #ff0000;"> ssh root@10.0.0.115</span>
root@10.0.0.115<span style="color: #800000;">'</span><span style="color: #800000;">s password: </span>
<span style="color: #008000;">#</span><span style="color: #008000;"> ip a</span>
1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 16436<span style="color: #000000;"> qdisc noqueue 
    link</span>/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00<span style="color: #000000;">
    inet </span>127.0.0.1/8<span style="color: #000000;"> scope host lo
    inet6 ::</span>1/128<span style="color: #000000;"> scope host 
       valid_lft forever preferred_lft forever
</span>2: eth0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1450 qdisc pfifo_fast qlen 1000<span style="color: #000000;">
    link</span>/ether fa:16:3e:fc:70:31<span style="color: #000000;"> brd ff:ff:ff:ff:ff:ff
    ine<span style="color: #ff0000;">t </span></span><span style="color: #ff0000;">1.1.1.101/24</span> brd 1.1.1.255 scope <span style="color: #0000ff;">global</span><span style="color: #000000;"> eth0
    inet6 fe80::f816:3eff:fefc:</span>7031/64<span style="color: #000000;"> scope link 
       valid_lft forever preferred_lft forever
</span><span style="color: #008000;">#</span><span style="color: #008000;"> ping baidu.com -c1</span>
PING baidu.com (111.13.101.208): 56<span style="color: #000000;"> data bytes
</span><span style="color: #ff0000;">64 bytes from 111.13.101.208: seq=0 ttl=127 time=5.687 ms

</span>--- baidu.com ping statistics ---
1 packets transmitted, 1 packets received, 0%<span style="color: #000000;"> packet loss
round</span>-trip min/avg/max = 5.687/5.687/5.687 ms</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看当前网络拓扑</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194649959-324856902.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   **<span style="font-family: '微软雅黑',sans-serif;">到此</span><span style="color: red; background: yellow;">VXLAN</span>****<span style="font-family: '微软雅黑',sans-serif;">网络已实现</span>**

## <span id="116_openstack_API">1.16 openstack API<span style="font-family: '微软雅黑',sans-serif;">应用用</span></span>

<span style="font-family: '微软雅黑',sans-serif;">官方</span>API<span style="font-family: '微软雅黑',sans-serif;">列表：</span>https://docs.openstack.org/pike/api/

<span style="font-family: '微软雅黑',sans-serif;">官方提供了丰富的</span>API<span style="font-family: '微软雅黑',sans-serif;">接口，方便用户的使用。可以使用</span>curl<span style="font-family: '微软雅黑',sans-serif;">命令调用</span>API

<p style="text-indent: 21.0pt;">
  curl<span style="font-family: '微软雅黑',sans-serif;">命令是</span>Linux<span style="font-family: '微软雅黑',sans-serif;">下一个可以使用多种协议收发数据的工具，包括</span>http<span style="font-family: '微软雅黑',sans-serif;">协议。</span>openstack<span style="font-family: '微软雅黑',sans-serif;">的</span>API<span style="font-family: '微软雅黑',sans-serif;">接口都是</span>URL<span style="font-family: '微软雅黑',sans-serif;">地址：</span>http://controller:35357/v3<span style="font-family: '微软雅黑',sans-serif;">可以使用</span>curl<span style="font-family: '微软雅黑',sans-serif;">命令进行调用。</span>
</p>

### <span id="1161_token">1.16.1 <span style="font-family: '微软雅黑',sans-serif;">获取</span>token<span style="font-family: '微软雅黑',sans-serif;">方法</span></span>

<p style="margin-left: 7.1pt; text-indent: 8.65pt;">
  <span style="font-family: '微软雅黑',sans-serif;">获取</span>token
</p>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack token issue |awk '/ id /{print $4}' </span>
gAAAAABaa0MpXNGCHgaytnvyPMbIF3IecIu9jA4WeMaL1kLWueNYs_Q1APXwdXDU7K34wdLg0I1spUIzDhAkst-Qdrizn_L3N5YBlApUrkY7gSw96MkKpTTDjUhIgm0eAD85Ayi6TL_1HmJJQIhm5ERY91zcKi9dvl73jj0dFNDWRqD9Cc9_oPA</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">将获取</span>token<span style="font-family: '微软雅黑',sans-serif;">给变量复制</span>
</p>

<div class="cnblogs_code">
  <pre>token=` token=`openstack token issue |awk <span style="color: #800000;">'</span><span style="color: #800000;">/ id /{print $4}</span><span style="color: #800000;">'</span>`</pre>
</div>

### <span id="1162">1.16.2 <span style="font-family: '微软雅黑',sans-serif;">常用获取命令</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考：</span>http://www.qstack.com.cn/archives/168.html
</p>

<span style="font-family: '微软雅黑',sans-serif;">使用</span>api<span style="font-family: '微软雅黑',sans-serif;">端口查看镜像列表</span>

<div class="cnblogs_code">
  <pre>curl -H <span style="color: #800000;">"</span><span style="color: #800000;">X-Auth-Token:$token</span><span style="color: #800000;">"</span>  -H <span style="color: #800000;">"</span><span style="color: #800000;">Content-Type: application/json</span><span style="color: #800000;">"</span>  http://10.0.0.32:9292/v2/images</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">获取</span>roles<span style="font-family: '微软雅黑',sans-serif;">列表</span>

<div class="cnblogs_code">
  <pre>curl -H <span style="color: #800000;">"</span><span style="color: #800000;">X-Auth-Token:$token</span><span style="color: #800000;">"</span> -H <span style="color: #800000;">"</span><span style="color: #800000;">Content-Type: application/json</span><span style="color: #800000;">"</span> http://10.0.0.11:35357/v3/roles</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">获取主机列表</span>

<div class="cnblogs_code">
  <pre>curl -H <span style="color: #800000;">"</span><span style="color: #800000;">X-Auth-Token:$token</span><span style="color: #800000;">"</span> -H <span style="color: #800000;">"</span><span style="color: #800000;">Content-Type: application/json</span><span style="color: #800000;">"</span> http://10.0.0.11:8774/v2.1/servers</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">获取网络列表</span>

<div class="cnblogs_code">
  <pre>curl -H <span style="color: #800000;">"</span><span style="color: #800000;">X-Auth-Token:$token</span><span style="color: #800000;">"</span> -H <span style="color: #800000;">"</span><span style="color: #800000;">Content-Type: application/json</span><span style="color: #800000;">"</span> http://10.0.0.11:9696/v2.0/networks</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">获取子网列表</span>

<div class="cnblogs_code">
  <pre>curl -H <span style="color: #800000;">"</span><span style="color: #800000;">X-Auth-Token:$token</span><span style="color: #800000;">"</span> -H <span style="color: #800000;">"</span><span style="color: #800000;">Content-Type: application/json</span><span style="color: #800000;">"</span> http://10.0.0.11:9696/v2.0/subnets</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">下载一个镜像</span>

<div class="cnblogs_code">
  <pre>curl -o clsn.qcow2 -H <span style="color: #800000;">"</span><span style="color: #800000;">X-Auth-Token:$token</span><span style="color: #800000;">"</span> -H <span style="color: #800000;">"</span><span style="color: #800000;">Content-Type: application/json</span><span style="color: #800000;">"</span> http://10.0.0.11:9292/v2/images/eb9e7015-d5ef-48c7-bd65-88a144c59115/file</pre>
</div>

## <span id="117">1.17 <span style="font-family: '微软雅黑',sans-serif;">附录</span></span>

### <span id="1171">1.17.1 <span style="font-family: '微软雅黑',sans-serif;">附录</span>-<span style="font-family: '微软雅黑',sans-serif;">常见错误</span></span>

1<span style="font-family: '微软雅黑',sans-serif;">、配置用户时的错误</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">【错误】</span>Multiple service matches found for 'identity', use an ID to be more specific.
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">解决办法：</span>
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    openstack endpoint list # <span style="font-family: '微软雅黑',sans-serif;">查看列表</span>
  </p>
  
  <p>
        openstack endpoint delete  'id'  # <span style="font-family: '微软雅黑',sans-serif;">利用</span>ID<span style="font-family: '微软雅黑',sans-serif;">删除</span>API <span style="font-family: '微软雅黑',sans-serif;">端点</span>
  </p>
  
  <p>
    openstack service list  # <span style="font-family: '微软雅黑',sans-serif;">查看服务列表</span>
  </p>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、用户管理时错误</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    HTTP 503<span style="font-family: '微软雅黑',sans-serif;">错误：</span>
  </p>
  
  <p>
    glance<span style="font-family: '微软雅黑',sans-serif;">日志位置：</span> /var/log/glance/
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">用户删除后，重新重建用户后，再关联次角色</span>
  </p>
  
  <p>
    openstack role add --project service --user glance admin
  </p>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">、未加载环境变量时出错</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@controller ~]# openstack user list
  </p>
  
  <p>
    Missing parameter(s):
  </p>
  
  <p>
    Set a username with --os-username, OS_USERNAME, or auth.username
  </p>
  
  <p>
    Set an authentication URL, with --os-auth-url, OS_AUTH_URL or auth.auth_url
  </p>
  
  <p>
    Set a scope, such as a project or domain, set a project scope with --os-project-name, OS_PROJECT_NAME or auth.project_name, set a domain scope with --os-domain-name, OS_DOMAIN_NAME or auth.domain_name
  </p>
</div>

### <span id="1172_-OpenStack">1.17.2 <span style="font-family: '微软雅黑',sans-serif;">附录</span>-OpenStack<span style="font-family: '微软雅黑',sans-serif;">组件使用的默认端口号</span></span>

<table style="width: 100%; border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
  <tr style="height: 1.15pt;">
    <td style="width: 52.84%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt; height: 1.15pt;" valign="top" width="52%">
      <p style="text-align: center; layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;" align="center">
        <strong><span style="font-size: 9.0pt; font-family: 'Verdana',sans-serif; color: white;">OpenStack service</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: solid #9BBB59 1.0pt; border-left: none; border-bottom: solid #9BBB59 1.0pt; border-right: none; background: #9BBB59; padding: 0cm 5.4pt 0cm 5.4pt; height: 1.15pt;" valign="top" width="17%">
      <p style="text-align: center; layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;" align="center">
        <strong><span style="font-size: 9.0pt; font-family: 'Verdana',sans-serif; color: white;">Default ports</span></strong>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt; height: 1.15pt;" valign="top" width="29%">
      <p style="text-align: center; layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;" align="center">
        <strong><span style="font-size: 9.0pt; font-family: 'Verdana',sans-serif; color: white;">Port type</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Block Storage (</span></strong><strong><span style="font-family: 宋体;">cinder</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8776</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl and adminurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Compute (</span></strong><strong><span style="font-family: 宋体;">nova</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">) endpoints</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8774</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl and adminurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Compute API (</span></strong><strong><span style="font-family: 宋体;">nova-api</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8773, 8775</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Compute ports for access to virtual machine consoles</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">5900-5999</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Compute VNC proxy for browsers ( </span></strong><strong><span style="font-family: 宋体;">openstack-nova-novncproxy</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">6080</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Compute VNC proxy for traditional VNC clients (</span></strong><strong><span style="font-family: 宋体;">openstack-nova-xvpvncproxy</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">6081</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Proxy port for HTML5 console used by Compute service</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">6082</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Data processing service (</span></strong><strong><span style="font-family: 宋体;">sahara</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">) endpoint</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8386</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl and adminurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Identity service (</span></strong><strong><span style="font-family: 宋体;">keystone</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">) administrative endpoint</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">35357</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">adminurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Identity service public endpoint</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">5000</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Image service (</span></strong><strong><span style="font-family: 宋体;">glance</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">) API</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">9292</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl and adminurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Image service registry</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">9191</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Networking (</span></strong><strong><span style="font-family: 宋体;">neutron</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">9696</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl and adminurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Object Storage (</span></strong><strong><span style="font-family: 宋体;">swift</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">6000, 6001, 6002</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Orchestration (</span></strong><strong><span style="font-family: 宋体;">heat</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">) endpoint</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8004</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl and adminurl</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Orchestration AWS CloudFormation-compatible API (</span></strong><strong><span style="font-family: 宋体;">openstack-heat-api-cfn</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8000</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Orchestration AWS CloudWatch-compatible API (</span></strong><strong><span style="font-family: 宋体;">openstack-heat-api-cloudwatch</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8003</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
    </td>
  </tr>
  
  <tr>
    <td style="width: 52.84%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="52%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Telemetry (</span></strong><strong><span style="font-family: 宋体;">ceilometer</span></strong><strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">)</span></strong>
      </p>
    </td>
    
    <td style="width: 17.62%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="17%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8777</span>
      </p>
    </td>
    
    <td style="width: 29.54%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="29%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">publicurl and adminurl</span>
      </p>
    </td>
  </tr>
</table>

### <span id="1173_-openstack">1.17.3 <span style="font-family: '微软雅黑',sans-serif;">附录</span>-openstack<span style="font-family: '微软雅黑',sans-serif;">组件使用的默认端口号</span></span>

<table style="width: 100%; border-collapse: collapse; border: initial none initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 25.02%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="text-align: center; layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;" align="center">
        <span style="font-size: 9.0pt; font-family: 'Verdana',sans-serif; color: white;">Service</span>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: solid #9BBB59 1.0pt; border-left: none; border-bottom: solid #9BBB59 1.0pt; border-right: none; background: #9BBB59; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="text-align: center; layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;" align="center">
        <span style="font-size: 9.0pt; font-family: 'Verdana',sans-serif; color: white;">Default port</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" valign="top" width="59%">
      <p style="text-align: center; layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;" align="center">
        <span style="font-size: 9.0pt; font-family: 'Verdana',sans-serif; color: white;">Used by</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">HTTP</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">80</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">OpenStack dashboard (</span><span style="font-family: 宋体;">Horizon</span><span style="font-size: 10pt; font-family: Verdana, sans-serif;">) when it is not configured to use secure access.</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">HTTP alternate</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">8080</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">OpenStack Object Storage (</span><span style="font-family: 宋体;">swift</span><span style="font-size: 10pt; font-family: Verdana, sans-serif;">) service.</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">HTTPS</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">443</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">Any OpenStack service that is enabled for SSL, especially secure-access dashboard.</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">rsync</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">873</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">OpenStack Object Storage. Required.</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">iSCSI target</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">3260</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">OpenStack Block Storage. Required.</span>
      </p>
    </td>
  </tr>
  
  <tr style="height: 35.15pt;">
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt; height: 35.15pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">MySQL database service</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 35.15pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">3306</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 35.15pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">Most OpenStack components.</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">Message Broker (AMQP traffic)</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">5672</span>
      </p>
      
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">25672</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">OpenStack Block Storage, Networking, Orchestration, and Compute.</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">NTP(chrony)</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">123,323</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: 宋体;">时间同步</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 25.02%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="25%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <strong><span style="font-size: 10pt; font-family: Verdana, sans-serif;">memcached</span></strong>
      </p>
    </td>
    
    <td style="width: 15.6%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="15%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: Verdana, sans-serif;">11211</span>
      </p>
    </td>
    
    <td style="width: 59.38%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="59%">
      <p style="layout-grid-mode: both; margin: 0cm 0cm 15.0pt 0cm;">
        <span style="font-size: 10pt; font-family: 宋体;">缓存服务器</span>
      </p>
    </td>
  </tr>
</table>

### <span id="1174_-openstack">1.17.4 <span style="font-family: '微软雅黑',sans-serif;">附录</span>-openstack<span style="font-family: '微软雅黑',sans-serif;">新建云主机流程图</span></span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127194843240-1945202755.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">虚拟机启动过程文字表述如下：</span>
</p>

<div class="cnblogs_Highlighter">
  <pre class="brush:Perl;gutter:true;">1.	界面或命令行通过RESTful API向keystone获取认证信息。
2.	keystone通过用户请求认证信息，并生成auth-token返回给对应的认证请求。
3.	界面或命令行通过RESTful API向nova-api发送一个boot instance的请求（携带auth-token）。
4.	nova-api接受请求后向keystone发送认证请求，查看token是否为有效用户和token。
5.	keystone验证token是否有效，如有效则返回有效的认证和对应的角色（注：有些操作需要有角色权限才能操作）。
6.	通过认证后nova-api和数据库通讯。
7.	初始化新建虚拟机的数据库记录。
8.	nova-api通过rpc.call向nova-scheduler请求是否有创建虚拟机的资源(Host ID)。
9.	nova-scheduler进程侦听消息队列，获取nova-api的请求。
10.	nova-scheduler通过查询nova数据库中计算资源的情况，并通过调度算法计算符合虚拟机创建需要的主机。
11.	对于有符合虚拟机创建的主机，nova-scheduler更新数据库中虚拟机对应的物理主机信息。
12.	nova-scheduler通过rpc.cast向nova-compute发送对应的创建虚拟机请求的消息。
13.	nova-compute会从对应的消息队列中获取创建虚拟机请求的消息。
14.	nova-compute通过rpc.call向nova-conductor请求获取虚拟机消息。（Flavor）
15.	nova-conductor从消息队队列中拿到nova-compute请求消息。
16.	nova-conductor根据消息查询虚拟机对应的信息。
17.	nova-conductor从数据库中获得虚拟机对应信息。
18.	nova-conductor把虚拟机信息通过消息的方式发送到消息队列中。
19.	nova-compute从对应的消息队列中获取虚拟机信息消息。
20.	nova-compute通过keystone的RESTfull API拿到认证的token，并通过HTTP请求glance-api获取创建虚拟机所需要镜像。
21.	glance-api向keystone认证token是否有效，并返回验证结果。
22.	token验证通过，nova-compute获得虚拟机镜像信息(URL)。
23.	nova-compute通过keystone的RESTfull API拿到认证k的token，并通过HTTP请求neutron-server获取创建虚拟机所需要的网络信息。
24.	neutron-server向keystone认证token是否有效，并返回验证结果。
25.	token验证通过，nova-compute获得虚拟机网络信息。
26.	nova-compute通过keystone的RESTfull API拿到认证的token，并通过HTTP请求cinder-api获取创建虚拟机所需要的持久化存储信息。
27.	cinder-api向keystone认证token是否有效，并返回验证结果。
28.	token验证通过，nova-compute获得虚拟机持久化存储信息。
29.	nova-compute根据instance的信息调用配置的虚拟化驱动来创建虚拟机。</pre>
</div>

### <span id="1175_-MetaData_IP_169254169254">1.17.5 <span style="font-family: '微软雅黑',sans-serif;">附录</span>-MetaData IP 169.254.169.254<span style="font-family: '微软雅黑',sans-serif;">说明</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">查考文献：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://server.51cto.com/sVirtual-516706.htm" >http://server.51cto.com/sVirtual-516706.htm</a>
</p>

**<span style="background: yellow;">OpenStack metadata</span>**

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">要理解如何实现的，我们需要先了解</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">的</span>metadata<span style="font-family: '微软雅黑',sans-serif;">。</span>metadata<span style="font-family: '微软雅黑',sans-serif;">字面上是元数据，主要用来给客户提供一个可以修改设置</span>OpenStack instence(<span style="font-family: '微软雅黑',sans-serif;">云主机</span>)<span style="font-family: '微软雅黑',sans-serif;">的机制，就像我们想在虚拟机放置一个公钥这样的需求，或者设置主机名等都可以通过</span>metadata<span style="font-family: '微软雅黑',sans-serif;">来实现。让我来梳理一下思路：</span>
</p>

> <p style="text-indent: 21.0pt;">
>    1.OpenStack有一个叫做Metadata的东东。
> </p>
> 
> <p style="text-indent: 21.0pt;">
>    2.我们创建虚拟机时候设置的主机名、密钥对，都保存在Metadata中。
> </p>
> 
> <p style="text-indent: 21.0pt;">
>    3.虚拟机创建后，在启动的时候获取Metadata，并进行系统配置。
> </p>

<span style="font-family: '微软雅黑',sans-serif;">虚拟机如何取到</span>Metadata?

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">那么虚拟机到底是怎么取到这个</span>metadata<span style="font-family: '微软雅黑',sans-serif;">呢</span>?<span style="font-family: '微软雅黑',sans-serif;">让我们在虚拟机试试这个。</span>
</p>

<div class="cnblogs_code">
  <pre>$ curl http://169.254.169.254
1.0
2007-01-19
2007-03-01
2007-08-29
2007-10-10
2007-12-15
2008-02-01
2008-09-01
2009-04-04<span style="color: #000000;">
latest</span></pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif;">为啥是</span><span style="background: yellow;">169.254.169.254?</span>**

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">或许你和我有一样的疑问，为啥这个</span>meatadata<span style="font-family: '微软雅黑',sans-serif;">的</span>ip<span style="font-family: '微软雅黑',sans-serif;">地址是</span>169.254.169.254<span style="font-family: '微软雅黑',sans-serif;">呢</span>?
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">这个就要提到</span>Amazon<span style="font-family: '微软雅黑',sans-serif;">了。因为</span>metadata<span style="font-family: '微软雅黑',sans-serif;">是亚马逊提出来的。然后大家再给亚马逊定制各种操作系统镜像的时候获取</span>metadata<span style="font-family: '微软雅黑',sans-serif;">的</span>api<span style="font-family: '微软雅黑',sans-serif;">地址就写的是</span>169.254.169.254<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">为了这些镜像也能在</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">上运行，为了兼容它。</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">就保留了这个地址。其实早期的</span>OpenStack<span style="font-family: '微软雅黑',sans-serif;">版本是通过</span>iptables NAT<span style="font-family: '微软雅黑',sans-serif;">来映射</span>169.254.169.254<span style="font-family: '微软雅黑',sans-serif;">到真实</span>API<span style="font-family: '微软雅黑',sans-serif;">的</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址上。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">不过现在更灵活了，直接在虚拟机里面增加了一条路由条目来实现，让虚拟机顺利的访问到这个</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址。关于这个</span>IP<span style="font-family: '微软雅黑',sans-serif;">的产生需要了解到<strong>‘命名空间’</strong>的概念</span>,<span style="font-family: '微软雅黑',sans-serif;">关于命名空间可以参考这篇博文</span>: <a href="/wp-content/themes/clsn-003/inc/go.php?url=http://blog.csdn.net/preterhuman_peak/article/details/40857117" >http://blog.csdn.net/preterhuman_peak/article/details/40857117</a>
</p>

<span style="font-family: '微软雅黑',sans-serif;">进入命名空间</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ip  netns  exec qdhcp-54f942f7-cc28-4292-a4d6-e37b8833e35f  /bin/bash </span>
[root@controller ~]<span style="color: #008000;">#</span> 
[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ifconfig </span>
lo: flags=73&lt;UP,LOOPBACK,RUNNING&gt;  mtu 65536<span style="color: #000000;">
        inet </span>127.0.0.1  netmask 255.0.0.0<span style="color: #000000;">
        inet6 ::</span>1  prefixlen 128  scopeid 0x10<span style="color: #000000;">
        loop  txqueuelen 0  (Local Loopback)
        RX packets </span>3  bytes 1728 (1.6<span style="color: #000000;"> KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets </span>3  bytes 1728 (1.6<span style="color: #000000;"> KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ns</span>-432508f9-da: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST&gt;  mtu 1500<span style="color: #000000;">
        inet </span>10.0.0.101  netmask 255.255.255.0  broadcast 10.0.0.255<span style="color: #000000;">
        inet6 fe80::f816:3eff:fedb:5a54  prefixlen </span>64  scopeid 0x20<span style="color: #000000;">
        ether fa:</span>16:3e:db:5a:54  txqueuelen 1000<span style="color: #000000;">  (Ethernet)
        RX packets </span>3609  bytes 429341 (419.2<span style="color: #000000;"> KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets </span>777  bytes 89302 (87.2<span style="color: #000000;"> KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">命名空间中的进程</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> netstat  -lntup </span>
<span style="color: #000000;">Active Internet connections (only servers)
Proto Recv</span>-Q Send-Q Local Address           Foreign Address         State       PID/<span style="color: #000000;">Program name    
tcp        0      0 </span>0.0.0.0:80              0.0.0.0:*               LISTEN      31094/<span style="color: #000000;">python2       
tcp        0      0 </span>10.0.0.101:53           0.0.0.0:*               LISTEN      41418/<span style="color: #000000;">dnsmasq       
tcp        0      0 </span>169.254.169.254:53      0.0.0.0:*               LISTEN      41418/<span style="color: #000000;">dnsmasq       
tcp6       0      0 fe80::f816:3eff:fedb:</span>53 :::*                    LISTEN      41418/<span style="color: #000000;">dnsmasq       
udp        0      0 </span>10.0.0.101:53           0.0.0.0:*                           41418/<span style="color: #000000;">dnsmasq       
udp        0      0 </span>169.254.169.254:53      0.0.0.0:*                           41418/<span style="color: #000000;">dnsmasq       
udp        0      0 </span>0.0.0.0:67              0.0.0.0:*                           41418/<span style="color: #000000;">dnsmasq       
udp6       0      0 fe80::f816:3eff:fedb:</span>53 :::*                                41418/dnsmasq</pre>
</div>

### <span id="1176">1.17.6 <span style="font-family: '微软雅黑',sans-serif;">附录</span>-<span style="font-family: '微软雅黑',sans-serif;">将控制节点秒变计算节点</span></span>

1<span style="font-family: '微软雅黑',sans-serif;">）在控制节点操作</span>

<div class="cnblogs_code">
  <pre>yum -y install openstack-nova-compute</pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">）修改</span>nova<span style="font-family: '微软雅黑',sans-serif;">配置文件</span>

<div class="cnblogs_code">
  <pre>[root@controller ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/nova/nova.conf</span>
<span style="color: #000000;">[vnc]
...
enabled </span>=<span style="color: #000000;"> True
vncserver_listen </span>= 0.0.0.0<span style="color: #000000;">
vncserver_proxyclient_address </span>=<span style="color: #000000;"> $my_ip
novncproxy_base_url </span>= http://controller:6080/vnc_auto.html</pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">）启动计算节点服务</span>

<div class="cnblogs_code">
  <pre>systemctl enable libvirtd.service openstack-nova-<span style="color: #000000;">compute.service
systemctl start libvirtd.service openstack</span>-nova-compute.service</pre>
</div>

### <span id="1177">1.17.7 <span style="font-family: '微软雅黑',sans-serif;">附录</span>-<span style="font-family: '微软雅黑',sans-serif;">如何把实例转换为镜像</span></span>

<p style="margin-left: 7.1pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif;">需求说明：</span></strong><span style="font-family: '微软雅黑',sans-serif;">将一台配置好的服务器，做成镜像，利用该镜像创建新的实例</span>
</p>

<p style="margin-left: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">）对实例进行拍摄快照</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127195044334-1678216576.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   <span style="font-family: '微软雅黑',sans-serif;">设置快照名称</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127195053772-680697591.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   <span style="font-family: '微软雅黑',sans-serif;">快照创建文件</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127195101428-582997936.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

<p style="text-indent: 21.0pt;">
     <span style="font-family: '微软雅黑',sans-serif;">但是这里显示的<strong>快照</strong>名字让人很不爽，下面就将他<strong>改为映像</strong>。</span>
</p>

<p style="text-indent: 21.0pt;">
  2<span style="font-family: '微软雅黑',sans-serif;">）查看进行上的标识信息</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127195108600-1487839964.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   3<span style="font-family: '微软雅黑',sans-serif;">）在</span>glace<span style="font-family: '微软雅黑',sans-serif;">服务端查看镜像文件</span>

<div class="cnblogs_code">
  <pre>[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /var/lib/glance/images/ -h </span>
total 1<span style="color: #000000;">.9G
</span>-rw-r----- 1 glance glance 1.1G Jan 26 16:27 1473524b-df75-45f5-afc2-<span style="color: #000000;">83ab3e6915cc
</span>-rw-r----- 1 glance glance  22M Jan 26 21:33 1885a4c7-d400-4d97-964c-<span style="color: #000000;">eddcbeb245a3
</span>-rw-r----- 1 glance glance 857M Jan 26 09:37 199bae53-fc7b-4eeb-a02a-<span style="color: #000000;">83e17ae73e20
</span>-rw-r----- 1 glance glance  13M Jan 25 11:31 68222030-a808-4d05-978f-<span style="color: #000000;">1d4a6f85f7dd
</span>-rw-r----- 1 glance glance  13M Jan 23 18:20 9d92c601-0824-493a-bc6e-cecb10e9a4c6</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">将生成的镜像文件移动到其他目录</span>

<div class="cnblogs_code">
  <pre>[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mv /var/lib/glance/images/1885a4c7-d400-4d97-964c-eddcbeb245a3  /root</span></pre>
</div>

4<span style="font-family: '微软雅黑',sans-serif;">）在</span>web<span style="font-family: '微软雅黑',sans-serif;">界面删除刚刚生成的快照</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127195134162-240660782.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   5<span style="font-family: '微软雅黑',sans-serif;">）将镜像文件重新上传</span>

<div class="cnblogs_code">
  <pre>[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> . admin-openrc </span>
[root@compute2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openstack image create "clsn-image-upload"   --file 1885a4c7-d400-4d97-964c-eddcbeb245a3   --disk-format qcow2 --container-format bare   --public</span>
+------------------+------------------------------------------------------+
| Field            | Value                                                |
+------------------+------------------------------------------------------+
| checksum         | 45fdc3a04021042855890712f31de1f9                     |
| container_format | bare                                                 |
| created_at       | 2018-01-26T13:46:15Z                                 |
| disk_format      | qcow2                                                |
| file             | /v2/images/ab30d820-94e5-4567-8110-605759745112/file |
| id               | ab30d820-94e5-4567-8110-605759745112                 |
| min_disk         | 0                                                    |
| min_ram          | 0                                                    |
| name             | clsn-image-upload                                    |
| owner            | d0dfbdbc115b4a728c24d28bc1ce1e62                     |
| protected        | False                                                |
| schema           | /v2/schemas/image                                    |
| size             | 22085632                                             |
| status           | active                                               |
| tags             |                                                      |
| updated_at       | 2018-01-26T13:46:40Z                                 |
| virtual_size     | None                                                 |
| visibility       | public                                               |
+------------------+------------------------------------------------------+</pre>
</div>

6<span style="font-family: '微软雅黑',sans-serif;">）在查看刚才创建的镜像</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127195200225-869466180.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   7<span style="font-family: '微软雅黑',sans-serif;">）使用新镜像创建一台实例</span>

<p style="text-align: center;" align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201801/1190037-20180127195204850-898378338.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="OpenStack云计算之路-Mitaka 版本" alt="" />
</p>

   **<span style="font-family: '微软雅黑',sans-serif;">至此实例转换为镜像完成</span>**

## <span id="118">1.18 <span style="font-family: '微软雅黑',sans-serif;">参考文献</span></span>

> [1] [openstack官方参考文档] <https://docs.openstack.org/mitaka/zh_CN/install-guide-rdo/>
> 
> [2] <https://zh.wikipedia.org/wiki/%e9%9b%b2%e7%ab%af%e9%81%8b%e7%ae%97>
> 
> [3] [http://www.ruanyifeng.com/blog/2017/07/iaas-paas-saas.html][2]
> 
> [4] <https://wiki.openstack.org/wiki/Main_Page>
> 
> [5] <https://zh.wikipedia.org/wiki/OpenStack>
> 
> [6] <https://www.cnblogs.com/pythonxiaohu/p/5861409.html>
> 
> [7] <https://linux.cn/article-5019-1.html>
> 
> [8] <https://www.cnblogs.com/endoresu/p/5018688.html>
> 
> [9] <https://developer.openstack.org/api-ref/compute/>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11">1.1 云计算简介</a><ul>
        <li>
          <a href="#111">1.1.1 云计算的特点</a>
        </li>
        <li>
          <a href="#112">1.1.2 云计算服务模式</a>
        </li>
        <li>
          <a href="#113">1.1.3 云计算的类型</a>
        </li>
        <li>
          <a href="#114">1.1.4 为什么要选择云计算</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_OpenStack">1.2 OpenStack简介</a><ul>
        <li>
          <a href="#121">1.2.1 市场趋向</a>
        </li>
        <li>
          <a href="#122">1.2.2 大型用户</a>
        </li>
        <li>
          <a href="#123_OpenStack">1.2.3 OpenStack项目介绍</a>
        </li>
        <li>
          <a href="#124">1.2.4 系统环境说明</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_OpenStack">1.3 OpenStack基础配置服务</a><ul>
        <li>
          <a href="#131_OpenStack">1.3.1 OpenStack服务部署顺序</a>
        </li>
        <li>
          <a href="#132_yum">1.3.2 配置本地yum源</a>
        </li>
        <li>
          <a href="#133_NTP">1.3.3 安装NTP时间服务</a>
        </li>
        <li>
          <a href="#134_OpenStack">1.3.4 OpenStack的包操作（添加新的计算节点时需要安装）</a>
        </li>
        <li>
          <a href="#135_SQL">1.3.5 SQL数据库安装（在控制节点操作）</a>
        </li>
        <li>
          <a href="#136_NoSQL">1.3.6 NoSQL 数据库</a>
        </li>
        <li>
          <a href="#137">1.3.7 消息队列部署</a>
        </li>
        <li>
          <a href="#138_Memcached">1.3.8 Memcached服务部署</a>
        </li>
        <li>
          <a href="#139">1.3.9 验证以上部署的服务是否正常</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14_Keystone">1.4 Keystone认证服务配置</a><ul>
        <li>
          <a href="#141">1.4.1 创建数据库</a>
        </li>
        <li>
          <a href="#142_keystone">1.4.2 安装keystone</a>
        </li>
        <li>
          <a href="#143">1.4.3 修改配置文件</a>
        </li>
        <li>
          <a href="#144">1.4.4 初始化身份认证服务的数据库(同步数据库)</a>
        </li>
        <li>
          <a href="#145_Fernet_keys">1.4.5 初始化Fernet keys</a>
        </li>
        <li>
          <a href="#146_Apache_HTTP">1.4.6 配置 Apache HTTP 服务器</a>
        </li>
        <li>
          <a href="#147_Apache_HTTP">1.4.7 启动 Apache HTTP 服务并配置其随系统启动</a>
        </li>
        <li>
          <a href="#148_API">1.4.8 创建服务实体和API端点</a>
        </li>
        <li>
          <a href="#149">1.4.9 创建域、项目、用户和角色</a>
        </li>
        <li>
          <a href="#1410_OpenStack">1.4.10 创建 OpenStack 客户端环境脚本</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15_glance">1.5 镜像服务glance部署</a><ul>
        <li>
          <a href="#151">1.5.1 创库授权</a>
        </li>
        <li>
          <a href="#152_glance">1.5.2 创建glance用户和授权</a>
        </li>
        <li>
          <a href="#153_API">1.5.3 创建镜像服务的 API 端点，并注册</a>
        </li>
        <li>
          <a href="#154_glance">1.5.4 安装glance软件包</a>
        </li>
        <li>
          <a href="#155_glance">1.5.5 修改glance相关配置文件</a>
        </li>
        <li>
          <a href="#156">1.5.6 同步数据库</a>
        </li>
        <li>
          <a href="#157_glance">1.5.7 启动glance服务</a>
        </li>
        <li>
          <a href="#158_glance">1.5.8 验证glance服务操作</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#16_nova">1.6 计算服务(nova)部署</a><ul>
        <li>
          <a href="#161">1.6.1 在控制节点安装并配置</a>
        </li>
        <li>
          <a href="#162">1.6.2 在计算节点安装和配置</a>
        </li>
        <li>
          <a href="#163">1.6.3 验证服务</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#17_Networkingneutron">1.7 Networking(neutron)服务</a><ul>
        <li>
          <a href="#171">1.7.1 安装并配置控制节点</a>
        </li>
        <li>
          <a href="#172">1.7.2 安装和配置计算节点</a>
        </li>
        <li>
          <a href="#173">1.7.3 验证操作</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#18_Dashboardhorizon-web">1.8 Dashboard（horizon-web界面）安装</a><ul>
        <li>
          <a href="#181">1.8.1 安全并配置组件（单独主机安装）</a>
        </li>
        <li>
          <a href="#182">1.8.2 修改配置文件</a>
        </li>
        <li>
          <a href="#183">1.8.3 启动服务</a>
        </li>
        <li>
          <a href="#184">1.8.4 验证操作</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#19">1.9 启动第一台实例</a><ul>
        <li>
          <a href="#191">1.9.1 创建虚拟网络</a>
        </li>
        <li>
          <a href="#192_m1nano">1.9.2 创建m1.nano规格的主机</a>
        </li>
        <li>
          <a href="#193">1.9.3 生成一个键值对，创建密钥对</a>
        </li>
        <li>
          <a href="#194">1.9.4 增加安全组规则</a>
        </li>
        <li>
          <a href="#195">1.9.5 启动第一台云主机</a>
        </li>
        <li>
          <a href="#196_WEB">1.9.6 在WEB端进行查看</a>
        </li>
        <li>
          <a href="#197_web">1.9.7 使用web界面创建一个实例</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#110_cinder">1.10 cinder块存储服务</a><ul>
        <li>
          <a href="#1101">1.10.1 环境准备</a>
        </li>
        <li>
          <a href="#1102">1.10.2 安装并配置控制节点</a>
        </li>
        <li>
          <a href="#1103">1.10.3 安装并配置一个存储节点</a>
        </li>
        <li>
          <a href="#1104_ssd">1.10.4 添加ssd盘配置信息</a>
        </li>
        <li>
          <a href="#1105_Dashboard">1.10.5 在Dashboard中如何创建硬盘</a>
        </li>
        <li>
          <a href="#1106">1.10.6 添加硬盘到虚拟机</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#111-2">1.11 添加一台新的计算节点</a><ul>
        <li>
          <a href="#1111">1.11.1 主机基础环境配置</a>
        </li>
        <li>
          <a href="#1112">1.11.2 安装配置计算服务</a>
        </li>
        <li>
          <a href="#1113_neutron">1.11.3 配置neutron网络</a>
        </li>
        <li>
          <a href="#1114">1.11.4 启动计算节点</a>
        </li>
        <li>
          <a href="#1115">1.11.5 验证之前的操作</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#112_Glance">1.12 Glance镜像服务迁移</a><ul>
        <li>
          <a href="#1121">1.12.1 数据库迁移</a>
        </li>
        <li>
          <a href="#1122_glance">1.12.2 镜像glance 数据库迁移</a>
        </li>
        <li>
          <a href="#1123_glance">1.12.3 安装glance服务</a>
        </li>
        <li>
          <a href="#1124">1.12.4 迁移原有镜像文件</a>
        </li>
        <li>
          <a href="#1125_keystone_glance">1.12.5 修改现有keystone中 glance服务注册信息</a>
        </li>
        <li>
          <a href="#1126_nova">1.12.6 修改nova节点配置文件</a>
        </li>
        <li>
          <a href="#1127">1.12.7 验证操作</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#113-2">1.13 添加一个新的网段并让它能够上网</a><ul>
        <li>
          <a href="#1131">1.13.1 环境准备</a>
        </li>
        <li>
          <a href="#1132_neutron">1.13.2 配置neutron服务</a>
        </li>
        <li>
          <a href="#1133">1.13.3 重启服务</a>
        </li>
        <li>
          <a href="#1134_iptables">1.13.4 配置iptables服务器作子网网关</a>
        </li>
        <li>
          <a href="#1135_web">1.13.5 web界面创建子网</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#114_CinderNFS">1.14 Cinder服务对接NFS配置</a><ul>
        <li>
          <a href="#1141_NFS">1.14.1 NFS服务部署</a>
        </li>
        <li>
          <a href="#1142_NFS">1.14.2 测试NFS的可用性</a>
        </li>
        <li>
          <a href="#1143_Cinder">1.14.3 修改Cinder节点配置文件</a>
        </li>
        <li>
          <a href="#1144">1.14.4 重启服务</a>
        </li>
        <li>
          <a href="#1145_NFS">1.14.5 添加NFS存储卷</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#115_OpenStackVXLAN">1.15 OpenStack中的VXLAN网络</a><ul>
        <li>
          <a href="#1151">1.15.1 前期准备</a>
        </li>
        <li>
          <a href="#1152">1.15.2 修改控制节点配置</a>
        </li>
        <li>
          <a href="#1153">1.15.3 修改配置计算节点文件</a>
        </li>
        <li>
          <a href="#1154_dashboard">1.15.4 修改dashboard开启路由界面显示</a>
        </li>
        <li>
          <a href="#1155_VXLAN">1.15.5 配置VXLAN网络</a>
        </li>
        <li>
          <a href="#1156_IP">1.15.6 连接浮动IP测试</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#116_openstack_API">1.16 openstack API应用用</a><ul>
        <li>
          <a href="#1161_token">1.16.1 获取token方法</a>
        </li>
        <li>
          <a href="#1162">1.16.2 常用获取命令</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#117">1.17 附录</a><ul>
        <li>
          <a href="#1171">1.17.1 附录-常见错误</a>
        </li>
        <li>
          <a href="#1172_-OpenStack">1.17.2 附录-OpenStack组件使用的默认端口号</a>
        </li>
        <li>
          <a href="#1173_-openstack">1.17.3 附录-openstack组件使用的默认端口号</a>
        </li>
        <li>
          <a href="#1174_-openstack">1.17.4 附录-openstack新建云主机流程图</a>
        </li>
        <li>
          <a href="#1175_-MetaData_IP_169254169254">1.17.5 附录-MetaData IP 169.254.169.254说明</a>
        </li>
        <li>
          <a href="#1176">1.17.6 附录-将控制节点秒变计算节点</a>
        </li>
        <li>
          <a href="#1177">1.17.7 附录-如何把实例转换为镜像</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#118">1.18 参考文献</a>
    </li>
  </ul>
</div>

 [1]: /wp-content/themes/clsn-003/inc/go.php?url=http://10.0.0.31/dashboard
 [2]: /wp-content/themes/clsn-003/inc/go.php?url=http://www.ruanyifeng.com/blog/2017/07/iaas-paas-saas.html