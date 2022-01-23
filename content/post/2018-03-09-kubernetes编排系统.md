---
title: Kubernetes 编排系统
author: 惨绿少年
type: post
date: 2018-03-08T23:56:00+00:00
url: /clsn/lx8.html
Baidusubmit:
  - 1
views:
  - 681
specs_zan:
  - 2
categories:
  - Linux运维
  - 云计算
  - 虚拟化

---
## <span id="11_Kubernetes">1.1 <span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">Kubernetes</span><span style="font-family: 微软雅黑, sans-serif; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">简介</span></span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180203175245328-1183720416.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Kubernetes 编排系统" alt="" />
</p>

### <span id="111_Kubernetes">1.1.1 <span style="font-family: '微软雅黑',sans-serif;">什么是</span>Kubernetes</span>

<p style="text-indent: 21.0pt;">
  Kubernetes (<span style="font-family: '微软雅黑',sans-serif;">通常称为</span>K8s<span style="font-family: '微软雅黑',sans-serif;">，</span>K8s<span style="font-family: '微软雅黑',sans-serif;">是将</span>8<span style="font-family: '微软雅黑',sans-serif;">个字母&ldquo;</span>ubernete<span style="font-family: '微软雅黑',sans-serif;">&rdquo;替换为&ldquo;</span>8<span style="font-family: '微软雅黑',sans-serif;">&rdquo;的缩写</span>) <span style="font-family: '微软雅黑',sans-serif;">是用于<strong><span style="text-decoration: underline;">自动部署、扩展和管理容器化（</span></strong></span><strong><span style="text-decoration: underline;">containerized</span></strong><strong><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">）应用程序的开源系统</span></span></strong><span style="font-family: '微软雅黑',sans-serif;">。</span>Google<span style="font-family: '微软雅黑',sans-serif;">设计并捐赠给</span>Cloud Native Computing Foundation<span style="font-family: '微软雅黑',sans-serif;">（今属</span>Linux<span style="font-family: '微软雅黑',sans-serif;">基金会）来使用的。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">它旨在提供&ldquo;<span style="background: yellow;">跨主机集群的自动部署、扩展以及运行应用程序容器的平台</span>&rdquo;。它支持一系列容器工具</span>, <span style="font-family: '微软雅黑',sans-serif;">包括</span>Docker<span style="font-family: '微软雅黑',sans-serif;">等。</span>CNCF<span style="font-family: '微软雅黑',sans-serif;">于</span>2017<span style="font-family: '微软雅黑',sans-serif;">年宣布首批</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">认证服务提供商（</span>KCSPs<span style="font-family: '微软雅黑',sans-serif;">），包含</span>IBM<span style="font-family: '微软雅黑',sans-serif;">、</span>MIRANTIS<span style="font-family: '微软雅黑',sans-serif;">、华为、</span>inwinSTACK<span style="font-family: '微软雅黑',sans-serif;">迎栈科技等服务商。</span>
</p>

### <span id="112_Kubernetes">1.1.2 Kubernetes<span style="font-family: '微软雅黑',sans-serif;">发展史</span></span>

<p style="text-indent: 21.0pt;">
  Kubernetes (<span style="font-family: '微软雅黑',sans-serif;">希腊语</span>"<span style="font-family: '微软雅黑',sans-serif;">舵手</span>" <span style="font-family: '微软雅黑',sans-serif;">或</span> "<span style="font-family: '微软雅黑',sans-serif;">飞行员</span>") <span style="font-family: '微软雅黑',sans-serif;">由</span>Joe Beda<span style="font-family: '微软雅黑',sans-serif;">，</span>Brendan Burns<span style="font-family: '微软雅黑',sans-serif;">和</span>Craig McLuckie<span style="font-family: '微软雅黑',sans-serif;">创立，并由其他谷歌工程师，包括</span>Brian Grant<span style="font-family: '微软雅黑',sans-serif;">和</span>Tim Hockin<span style="font-family: '微软雅黑',sans-serif;">进行加盟创作，并由谷歌在</span>2014<span style="font-family: '微软雅黑',sans-serif;">年首次对外宣布</span> <span style="font-family: '微软雅黑',sans-serif;">。它的开发和设计都深受谷歌的</span>Borg<span style="font-family: '微软雅黑',sans-serif;">系统的影响，它的许多顶级贡献者之前也是</span>Borg<span style="font-family: '微软雅黑',sans-serif;">系统的开发者。在谷歌内部，</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">的原始代号曾经是</span>Seven<span style="font-family: '微软雅黑',sans-serif;">，即星际迷航中友好的</span>Borg(<span style="font-family: '微软雅黑',sans-serif;">博格人</span>)<span style="font-family: '微软雅黑',sans-serif;">角色。</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">标识中舵轮有七个轮辐就是对该项目代号的致意。</span>
</p>

<p style="text-indent: 21.0pt;">
  Kubernetes v1.0<span style="font-family: '微软雅黑',sans-serif;">于</span>2015<span style="font-family: '微软雅黑',sans-serif;">年</span>7<span style="font-family: '微软雅黑',sans-serif;">月</span>21<span style="font-family: '微软雅黑',sans-serif;">日发布。随着</span>v1.0<span style="font-family: '微软雅黑',sans-serif;">版本发布，谷歌与</span>Linux <span style="font-family: '微软雅黑',sans-serif;">基金会合作组建了</span>Cloud Native Computing Foundation (CNCF)<span style="font-family: '微软雅黑',sans-serif;">并把</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">作为种子技术来提供。</span>
</p>

<p style="text-indent: 21.0pt;">
  Rancher Labs<span style="font-family: '微软雅黑',sans-serif;">在其</span>Rancher<span style="font-family: '微软雅黑',sans-serif;">容器管理平台中包含了</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">的发布版。</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">也在很多其他公司的产品中被使用，比如</span>Red Hat<span style="font-family: '微软雅黑',sans-serif;">在</span>OpenShift<span style="font-family: '微软雅黑',sans-serif;">产品中，</span>CoreOS<span style="font-family: '微软雅黑',sans-serif;">的</span>Tectonic<span style="font-family: '微软雅黑',sans-serif;">产品中，</span> <span style="font-family: '微软雅黑',sans-serif;">以及</span>IBM<span style="font-family: '微软雅黑',sans-serif;">的</span>IBM<span style="font-family: '微软雅黑',sans-serif;">云私有产品中。</span>
</p>

### <span id="113_Kubernetes">1.1.3 Kubernetes <span style="font-family: '微软雅黑',sans-serif;">特点</span></span>

<p style="text-indent: 21.0pt;">
  1<span style="font-family: '微软雅黑',sans-serif;">、可移植</span>: <span style="font-family: '微软雅黑',sans-serif;">支持公有云，私有云，混合云，多重云（</span>multi-cloud<span style="font-family: '微软雅黑',sans-serif;">）</span>
</p>

<p style="text-indent: 21.0pt;">
  2<span style="font-family: '微软雅黑',sans-serif;">、可扩展</span>: <span style="font-family: '微软雅黑',sans-serif;">模块化</span>, <span style="font-family: '微软雅黑',sans-serif;">插件化</span>, <span style="font-family: '微软雅黑',sans-serif;">可挂载</span>, <span style="font-family: '微软雅黑',sans-serif;">可组合</span>
</p>

<p style="text-indent: 21.0pt;">
  3<span style="font-family: '微软雅黑',sans-serif;">、自动化</span>: <span style="font-family: '微软雅黑',sans-serif;">自动部署，自动重启，自动复制，自动伸缩</span>/<span style="font-family: '微软雅黑',sans-serif;">扩展</span>
</p>

<p style="text-indent: 21.0pt;">
  4<span style="font-family: '微软雅黑',sans-serif;">、快速部署应用，快速扩展应用</span>
</p>

<p style="text-indent: 21.0pt;">
  5<span style="font-family: '微软雅黑',sans-serif;">、无缝对接新的应用功能</span>
</p>

<p style="text-indent: 21.0pt;">
  6<span style="font-family: '微软雅黑',sans-serif;">、节省资源，优化硬件资源的使用</span>
</p>

### <span id="114_Kubernetes">1.1.4 Kubernetes<span style="font-family: '微软雅黑',sans-serif;">规划组件</span></span>

<p style="margin-left: 42.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考文档：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://docs.kubernetes.org.cn/249.html" >http://docs.kubernetes.org.cn/249.html</a>
</p>

<p style="text-indent: 21.0pt;">
  Kubernetes<span style="font-family: '微软雅黑',sans-serif;">定义了一组构建块，它们可以共同提供部署、维护和扩展应用程序的机制。组成</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">的组件设计为松耦合和可扩展的，这样可以满足多种不同的工作负载。可扩展性在很大程度上由</span><strong>Kubernetes API</strong><span style="font-family: '微软雅黑',sans-serif;">提供&mdash;&mdash;它被作为扩展的内部组件以及</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">上运行的容器等使用。</span>
</p>

**<span style="background: yellow;">Pod</span>**

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;">Kubernetes</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">的基本调度单元称为&ldquo;</span>pod</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">&rdquo;</span></span><span style="font-family: '微软雅黑',sans-serif;">。它可以把更高级别的抽象内容增加到容器化组件。一个</span>pod<span style="font-family: '微软雅黑',sans-serif;">一般包含一个或多个容器，这样可以保证它们一直位于主机上，并且可以共享资源。</span><span style="text-decoration: underline;">Kubernetes</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">中的每个</span>pod</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">都被分配一个唯一的（在集群内的）</span>IP</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">地址这样就可以允许应用程序使用端口，而不会有冲突的风险。</span></span>
</p>

<p style="text-indent: 21.0pt;">
  Pod<span style="font-family: '微软雅黑',sans-serif;">可以定义一个卷，例如本地磁盘目录或网络磁盘，并将其暴露在</span>pod<span style="font-family: '微软雅黑',sans-serif;">中的一个容器之中。</span>pod<span style="font-family: '微软雅黑',sans-serif;">可以通过</span>Kubernetes API<span style="font-family: '微软雅黑',sans-serif;">手动管理，也可以委托给控制器来管理。</span>
</p>

**<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">标签和选择器</span>**

<p style="text-indent: 21.0pt;">
  Kubernetes<span style="font-family: '微软雅黑',sans-serif;">使客户端（用户或内部组件）将称为&ldquo;标签&rdquo;的键值对附加到系统中的任何</span>API<span style="font-family: '微软雅黑',sans-serif;">对象，如</span>pod<span style="font-family: '微软雅黑',sans-serif;">和节点。相应地，<span style="text-decoration: underline;">&ldquo;标签选择器&rdquo;是针对匹配对象的标签的查询。</span></span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">标签和选择器是</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">中的主要分组机制，用于确定操作适用的组件。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">例如，如果应用程序的</span>Pods<span style="font-family: '微软雅黑',sans-serif;">具有系统的标签</span> tier ("front-end", "back-end", for example) <span style="font-family: '微软雅黑',sans-serif;">和一个</span> release_track ("canary", "production", for example)<span style="font-family: '微软雅黑',sans-serif;">，那么对所有</span>"back-end" <span style="font-family: '微软雅黑',sans-serif;">和</span> "canary" <span style="font-family: '微软雅黑',sans-serif;">节点的操作可以使用如下所示的标签选择器：</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">&nbsp;<span class="cnblogs_code">tier=back-end AND release_track=canary</span>&nbsp;</span>
</p>

**<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">控制器</span>**

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">控制器是将实际集群状态转移到所需集群状态的对帐循环。它通过管理一组</span><strong>pod</strong></span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">来实现。</span></span><span style="font-family: '微软雅黑',sans-serif;">一种控制器是一个&ldquo;复制控制器&rdquo;，它通过在集群中运行指定数量的</span>pod<span style="font-family: '微软雅黑',sans-serif;">副本来处理复制和缩放。如果基础节点出现故障，它还可以处理创建替换</span>pod<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">其它控制器，是核心</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">系统的一部分包括一个&ldquo;</span><span style="text-decoration: underline;">DaemonSet</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">控制器</span></span><span style="font-family: '微软雅黑',sans-serif;">&rdquo;为每一台机器（或机器的一些子集）上运行的恰好一个</span>pod<span style="font-family: '微软雅黑',sans-serif;">，和一个&ldquo;作业控制器&rdquo;用于运行</span>pod<span style="font-family: '微软雅黑',sans-serif;">运行到完成，例如作为批处理作业的一部分。控制器管理的一组</span>pod<span style="font-family: '微软雅黑',sans-serif;">由作为控制器定义的一部分的标签选择器确定。</span>
</p>

**<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">服务</span>**

<p style="text-indent: 21.0pt;">
  Kubernetes<span style="font-family: '微软雅黑',sans-serif;">服务是一组协同工作的</span>pod<span style="font-family: '微软雅黑',sans-serif;">，就像多层架构应用中的一层。构成服务的</span>pod<span style="font-family: '微软雅黑',sans-serif;">组通过标签选择器来定义。</span>
</p>

<p style="text-indent: 21.0pt;">
  Kubernetes<span style="font-family: '微软雅黑',sans-serif;">通过给服务分配静态</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址和域名来提供服务发现机制，并且以轮询调度的方式将流量负载均衡到能与选择器匹配的</span>pod<span style="font-family: '微软雅黑',sans-serif;">的</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址的网络连接上（即使是故障导致</span>pod<span style="font-family: '微软雅黑',sans-serif;">从一台机器移动到另一台机器）。默认情况下，一个服务会暴露在集群中（例如，多个后端</span>pod<span style="font-family: '微软雅黑',sans-serif;">可能被分组成一个服务，前端</span>pod<span style="font-family: '微软雅黑',sans-serif;">的请求在它们之间负载平衡）；但是，一个服务也可以暴露在集群外部（例如，从客户端访问前端</span>pod<span style="font-family: '微软雅黑',sans-serif;">）。</span>
</p>

### <span id="115_Kubernetes">1.1.5 Kubernetes<span style="font-family: '微软雅黑',sans-serif;">核心组件</span></span>

<p style="text-indent: 21.0pt;">
  Kubernetes<span style="font-family: '微软雅黑',sans-serif;">遵循</span><em>master-slave architecture</em><span style="font-family: '微软雅黑',sans-serif;">。</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">的组件可以分为管理单个的</span> node <span style="font-family: '微软雅黑',sans-serif;">组件和控制平面的一部分的组件。</span>
</p>

<p style="text-indent: 21.0pt;">
  Kubernetes Master<span style="font-family: '微软雅黑',sans-serif;">是集群的主要控制单元，用于管理其工作负载并指导整个系统的通信。</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">控制平面由各自的进程组成，每个组件都可以在单个主节点上运行，也可以在支持</span><em>high-availability clusters</em><span style="font-family: '微软雅黑',sans-serif;">的多个主节点上运行。</span>
</p>

Kubernetes<span style="font-family: '微软雅黑',sans-serif;">主要由以下几个核心组件组成：</span>

<table style="width: 100%; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 27.06%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">组件名称</span></strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="72%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 27.06%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>etcd </strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="72%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">保存了整个集群的状态；</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 27.06%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>apiserver</strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="72%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">提供了资源操作的唯一入口，并提供认证、授权、访问控制、</span>API<span style="font-family: '微软雅黑',sans-serif;">注册和发现等机制；</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 27.06%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>controller manager</strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="72%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">负责维护集群的状态，比如故障检测、自动扩展、滚动更新等；</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 27.06%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>scheduler</strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="72%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">负责资源的调度，按照预定的调度策略将</span>Pod<span style="font-family: '微软雅黑',sans-serif;">调度到相应的机器上；</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 27.06%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>kubelet</strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="72%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">负责维护容器的生命周期，同时也负责</span>Volume<span style="font-family: '微软雅黑',sans-serif;">（</span>CVI<span style="font-family: '微软雅黑',sans-serif;">）和网络（</span>CNI<span style="font-family: '微软雅黑',sans-serif;">）的管理；</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 27.06%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Container runtime</strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="72%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">负责镜像管理以及</span>Pod<span style="font-family: '微软雅黑',sans-serif;">和容器的真正运行（</span>CRI<span style="font-family: '微软雅黑',sans-serif;">）；</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 27.06%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="27%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>kube-proxy</strong>
      </p>
    </td>
    
    <td style="width: 72.94%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="72%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">负责为</span>Service<span style="font-family: '微软雅黑',sans-serif;">提供</span>cluster<span style="font-family: '微软雅黑',sans-serif;">内部的服务发现和负载均衡；</span>
      </p>
    </td>
  </tr>
</table>

<span style="font-family: '微软雅黑',sans-serif;">核心组件结构图</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180203175259843-780547270.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Kubernetes 编排系统" alt="" />&nbsp;
</p>

<span style="font-family: '微软雅黑',sans-serif;">除了核心组件，还有一些推荐的</span>Add-ons<span style="font-family: '微软雅黑',sans-serif;">：</span>

<table style="width: 100%; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 31.14%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="31%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">组件名称</span></strong>
      </p>
    </td>
    
    <td style="width: 68.86%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="68%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 31.14%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="31%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>kube-dns</strong>
      </p>
    </td>
    
    <td style="width: 68.86%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="68%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">负责为整个集群提供</span>DNS<span style="font-family: '微软雅黑',sans-serif;">服务</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 31.14%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="31%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Ingress Controller</strong>
      </p>
    </td>
    
    <td style="width: 68.86%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="68%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">为服务提供外网入口</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 31.14%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="31%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Heapster</strong>
      </p>
    </td>
    
    <td style="width: 68.86%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="68%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">提供资源监控</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 31.14%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="31%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Dashboard</strong>
      </p>
    </td>
    
    <td style="width: 68.86%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="68%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">提供</span>GUI
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 31.14%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="31%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Federation</strong>
      </p>
    </td>
    
    <td style="width: 68.86%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="68%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">提供跨可用区的集群</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 31.14%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="31%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Fluentd-elasticsearch</strong>
      </p>
    </td>
    
    <td style="width: 68.86%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="68%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">提供集群日志采集、存储与查询</span>
      </p>
    </td>
  </tr>
</table>

### <span id="116">1.1.6 <span style="font-family: '微软雅黑',sans-serif;">分层架构</span></span>

<p style="text-indent: 21.0pt;">
  Kubernetes<span style="font-family: '微软雅黑',sans-serif;">设计理念和功能其实就是一个类似</span>Linux<span style="font-family: '微软雅黑',sans-serif;">的分层架构，如下图所示：</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180203175310937-357350103.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Kubernetes 编排系统" alt="" />&nbsp;
</p>

<span style="font-family: '微软雅黑',sans-serif;">分层说明：</span>

<table style="width: 100%; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 14.88%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="14%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">分层结构</span></strong>
      </p>
    </td>
    
    <td style="width: 85.12%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" colspan="2" width="85%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 14.88%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="14%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">核心层</span></strong>
      </p>
    </td>
    
    <td style="width: 85.12%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" colspan="2" width="85%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Kubernetes<span style="font-family: '微软雅黑',sans-serif;">最核心的功能，对外提供</span>API<span style="font-family: '微软雅黑',sans-serif;">构建高层的应用，对内提供插件式应用执行环境</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 14.88%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="14%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">应用层</span></strong>
      </p>
    </td>
    
    <td style="width: 85.12%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" colspan="2" width="85%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">部署（无状态应用、有状态应用、批处理任务、集群应用等）和路由（服务发现、</span>DNS<span style="font-family: '微软雅黑',sans-serif;">解析等）</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 14.88%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="14%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">管理层</span></strong>
      </p>
    </td>
    
    <td style="width: 85.12%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" colspan="2" width="85%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">系统度量（如基础设施、容器和网络的度量），自动化（如自动扩展、动态</span>Provision<span style="font-family: '微软雅黑',sans-serif;">等）以及策略管理（</span>RBAC<span style="font-family: '微软雅黑',sans-serif;">、</span>Quota<span style="font-family: '微软雅黑',sans-serif;">、</span>PSP<span style="font-family: '微软雅黑',sans-serif;">、</span>NetworkPolicy<span style="font-family: '微软雅黑',sans-serif;">等）</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 14.88%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="14%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">接口层</span></strong>
      </p>
    </td>
    
    <td style="width: 85.12%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" colspan="2" width="85%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        kubectl<span style="font-family: '微软雅黑',sans-serif;">命令行工具、客户端</span>SDK<span style="font-family: '微软雅黑',sans-serif;">以及集群联邦</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 14.88%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" rowspan="3" width="14%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">生态系统</span></strong>
      </p>
    </td>
    
    <td style="width: 85.12%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #92D050; padding: 0cm 5.4pt 0cm 5.4pt;" colspan="2" width="85%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">在接口层之上的庞大容器集群管理调度的生态系统，可以划分为两个范畴</span>
      </p>
    </td>
  </tr>
  
  <tr style="height: 17.7pt;">
    <td style="width: 23.04%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 17.7pt;" width="23%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Kubernetes</strong><strong><span style="font-family: '微软雅黑',sans-serif;">外部</span></strong>
      </p>
    </td>
    
    <td style="width: 62.08%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 17.7pt;" width="62%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">日志、监控、配置管理、</span>CI<span style="font-family: '微软雅黑',sans-serif;">、</span>CD<span style="font-family: '微软雅黑',sans-serif;">、</span>Workflow<span style="font-family: '微软雅黑',sans-serif;">、</span>FaaS<span style="font-family: '微软雅黑',sans-serif;">、</span>OTS<span style="font-family: '微软雅黑',sans-serif;">应用、</span>ChatOps<span style="font-family: '微软雅黑',sans-serif;">等</span>
      </p>
    </td>
  </tr>
  
  <tr style="height: 17.65pt;">
    <td style="width: 23.04%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt; height: 17.65pt;" width="23%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>Kubernetes</strong><strong><span style="font-family: '微软雅黑',sans-serif;">内部</span></strong>
      </p>
    </td>
    
    <td style="width: 62.08%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt; height: 17.65pt;" width="62%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        CRI<span style="font-family: '微软雅黑',sans-serif;">、</span>CNI<span style="font-family: '微软雅黑',sans-serif;">、</span>CVI<span style="font-family: '微软雅黑',sans-serif;">、镜像仓库、</span>Cloud Provider<span style="font-family: '微软雅黑',sans-serif;">、集群自身的配置和管理等</span>
      </p>
    </td>
  </tr>
</table>

## <span id="12_Kubernetes">1.2 <span style="font-family: '微软雅黑',sans-serif;">部署</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">集群</span></span>

### <span id="121">1.2.1 <span style="font-family: '微软雅黑',sans-serif;">主机环境说明</span></span>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">系统版本说明</span>
</p>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/redhat-release </span>
CentOS Linux release 7.2.1511<span style="color: #000000;"> (Core) 
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r </span>
3.10.0-327<span style="color: #000000;">.el7.x86_64
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> getenforce </span>
<span style="color: #000000;">Disabled
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status  firewalld.service </span>
● firewalld.service - firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">主机</span>IP<span style="font-family: '微软雅黑',sans-serif;">规划</span>

<table style="width: 100%; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 23%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">主机名</span></strong>
      </p>
    </td>
    
    <td style="width: 39.32%; border-top: solid #9BBB59 1.0pt; border-left: none; border-bottom: solid #9BBB59 1.0pt; border-right: none; background: #9BBB59; padding: 0cm 5.4pt 0cm 5.4pt;" width="39%">
      <p style="text-align: center;" align="center">
        <strong><span style="color: white;">IP</span></strong>
      </p>
    </td>
    
    <td style="width: 37.68%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="37%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">功能</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>k8s-master</strong>
      </p>
    </td>
    
    <td style="width: 39.32%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="39%">
      <p style="text-align: center;" align="center">
        10.0.0.11/172.16.1.11
      </p>
    </td>
    
    <td style="width: 37.68%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="37%">
      <p style="text-align: center;" align="center">
        Master<span style="font-family: '微软雅黑',sans-serif;">、</span>etcd<span style="font-family: '微软雅黑',sans-serif;">、</span>registry
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>k8s-node-1</strong>
      </p>
    </td>
    
    <td style="width: 39.32%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="39%">
      <p style="text-align: center;" align="center">
        10.0.0.12/172.16.1.12
      </p>
    </td>
    
    <td style="width: 37.68%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="37%">
      <p style="text-align: center;" align="center">
        node1
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 23%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="23%">
      <p style="text-align: center;" align="center">
        <strong>k8s-node-2</strong>
      </p>
    </td>
    
    <td style="width: 39.32%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="39%">
      <p style="text-align: center;" align="center">
        10.0.0.13/172.16.1.13
      </p>
    </td>
    
    <td style="width: 37.68%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="37%">
      <p style="text-align: center;" align="center">
        node2
      </p>
    </td>
  </tr>
</table>

<span style="font-family: '微软雅黑',sans-serif;">设置</span>hosts<span style="font-family: '微软雅黑',sans-serif;">解析</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/hosts</span>
127.0.0.1<span style="color: #000000;">   localhost localhost.localdomain localhost4 localhost4.localdomain4
::</span>1<span style="color: #000000;">         localhost localhost.localdomain localhost6 localhost6.localdomain6
</span>10.0.0.11   k8s-<span style="color: #000000;">master
</span>10.0.0.12   k8s-node-1
10.0.0.13   k8s-node-2</pre>
</div>

### <span id="122">1.2.2 <span style="font-family: '微软雅黑',sans-serif;">安装软件包</span></span>

<span style="font-family: '微软雅黑',sans-serif;">在三个节点上分别操作</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install etcd docker kubernetes flannel  -y </span>
[root@k8s-node-1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install docker kubernetes flannel  -y </span>
[root@k8s-node-2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install docker kubernetes flannel  -y</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">安装的软件版本说明</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -qa  etcd docker kubernetes flannel</span>
flannel-0.7.1-2<span style="color: #000000;">.el7.x86_64
docker</span>-1.12.6-71.git3e8e77d.el7.centos.1<span style="color: #000000;">.x86_64
kubernetes</span>-1.5.2-0.7<span style="color: #000000;">.git269f928.el7.x86_64
etcd</span>-3.2.11-1.el7.x86_64</pre>
</div>

### <span id="123_etcd">1.2.3 <span style="font-family: '微软雅黑',sans-serif;">修改配置</span>etcd</span>

&nbsp;&nbsp; yum<span style="font-family: '微软雅黑',sans-serif;">安装的</span>etcd<span style="font-family: '微软雅黑',sans-serif;">默认配置文件在</span>/etc/etcd/etcd.conf<span style="font-family: '微软雅黑',sans-serif;">。</span>

<span style="font-family: '微软雅黑',sans-serif;">最终配置文件</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep -Ev '^$|#' /etc/etcd/etcd.conf</span>
ETCD_DATA_DIR=<span style="color: #800000;">"</span><span style="color: #800000;">/var/lib/etcd/default.etcd</span><span style="color: #800000;">"</span><span style="color: #000000;">
ETCD_LISTEN_CLIENT_URLS</span>=<span style="color: #800000;">"</span><span style="color: #800000;">http://0.0.0.0:2379</span><span style="color: #800000;">"</span><span style="color: #000000;">
ETCD_NAME</span>=<span style="color: #800000;">"</span><span style="color: #800000;">default</span><span style="color: #800000;">"</span><span style="color: #000000;">
ETCD_ADVERTISE_CLIENT_URLS</span>=<span style="color: #800000;">"</span><span style="color: #800000;">http://10.0.0.11:2379</span><span style="color: #800000;">"</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动</span>etcd

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl enable etcd</span>
[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl start etcd</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">测试</span>etcd

<div class="cnblogs_code">
  <pre>etcdctl set testdir/<span style="color: #000000;">testkey0 0
etcdctl set testdir</span>/<span style="color: #000000;">testkey0 0
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> etcdctl -C http://10.0.0.11:2379 cluster-health</span>
member 8e9e05c52164694d <span style="color: #0000ff;">is</span> healthy: got healthy result <span style="color: #0000ff;">from</span> http://10.0.0.11:2379<span style="color: #000000;">
cluster </span><span style="color: #0000ff;">is</span> healthy</pre>
</div>

### <span id="124_kubernetes">1.2.4 <span style="font-family: '微软雅黑',sans-serif;">配置并启动</span>kubernetes</span>

/etc/kubernetes/apiserver<span style="font-family: '微软雅黑',sans-serif;">配置文件内容</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;">  grep -Ev '^$|#'  /etc/kubernetes/apiserver</span>
KUBE_API_ADDRESS=<span style="color: #800000;">"</span><span style="color: #800000;">--insecure-bind-address=0.0.0.0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_API_PORT</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--port=8080</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ETCD_SERVERS</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--etcd-servers=http://10.0.0.11:2379</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_SERVICE_ADDRESSES</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--service-cluster-ip-range=10.254.0.0/16</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ADMISSION_CONTROL</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ResourceQuota</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_API_ARGS</span>=<span style="color: #800000;">""</span></pre>
</div>

/etc/kubernetes/config<span style="font-family: '微软雅黑',sans-serif;">配置文件</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;">  grep -Ev '^$|#' /etc/kubernetes/config</span>
KUBE_LOGTOSTDERR=<span style="color: #800000;">"</span><span style="color: #800000;">--logtostderr=true</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_LOG_LEVEL</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--v=0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ALLOW_PRIV</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--allow-privileged=false</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_MASTER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--master=http://10.0.0.11:8080</span><span style="color: #800000;">"</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动服务</span>

<div class="cnblogs_code">
  <pre>systemctl enable kube-<span style="color: #000000;">apiserver.service
systemctl start kube</span>-<span style="color: #000000;">apiserver.service
systemctl enable kube</span>-controller-<span style="color: #000000;">manager.service
systemctl start kube</span>-controller-<span style="color: #000000;">manager.service
systemctl enable kube</span>-<span style="color: #000000;">scheduler.service
systemctl start kube</span>-scheduler.service</pre>
</div>

### <span id="125_node">1.2.5 <span style="font-family: '微软雅黑',sans-serif;">部署配置</span>node</span>

/etc/kubernetes/config<span style="font-family: '微软雅黑',sans-serif;">配置文件</span>

<div class="cnblogs_code">
  <pre>[root@k8s-node-1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep -Ev '^$|#'  /etc/kubernetes/config</span>
KUBE_LOGTOSTDERR=<span style="color: #800000;">"</span><span style="color: #800000;">--logtostderr=true</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_LOG_LEVEL</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--v=0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ALLOW_PRIV</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--allow-privileged=false</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_MASTER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--master=http://10.0.0.11:8080</span><span style="color: #800000;">"</span><span style="color: #000000;">
[root@k8s</span>-node-1 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep -Ev '^$|#'  /etc/kubernetes/kubelet</span>
KUBELET_ADDRESS=<span style="color: #800000;">"</span><span style="color: #800000;">--address=0.0.0.0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_HOSTNAME</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--hostname-override=10.0.0.12</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_API_SERVER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--api-servers=http://10.0.0.11:8080</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_POD_INFRA_CONTAINER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--pod-infra-container-image=registry.access.redhat.com/rhel7/pod-infrastructure:latest</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_ARGS</span>=<span style="color: #800000;">""</span></pre>
</div>

/etc/kubernetes/config<span style="font-family: '微软雅黑',sans-serif;">配置文件</span>

<div class="cnblogs_code">
  <pre>[root@k8s-node-2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep -Ev '^$|#'  /etc/kubernetes/config</span>
KUBE_LOGTOSTDERR=<span style="color: #800000;">"</span><span style="color: #800000;">--logtostderr=true</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_LOG_LEVEL</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--v=0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ALLOW_PRIV</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--allow-privileged=false</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_MASTER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--master=http://10.0.0.11:8080</span><span style="color: #800000;">"</span><span style="color: #000000;">
[root@k8s</span>-node-2 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep -Ev '^$|#'  /etc/kubernetes/kubelet</span>
KUBELET_ADDRESS=<span style="color: #800000;">"</span><span style="color: #800000;">--address=0.0.0.0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_HOSTNAME</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--hostname-override=10.0.0.13</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_API_SERVER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--api-servers=http://10.0.0.11:8080</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_POD_INFRA_CONTAINER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--pod-infra-container-image=registry.access.redhat.com/rhel7/pod-infrastructure:latest</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBELET_ARGS</span>=<span style="color: #800000;">""</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">systemctl enable kubelet.service
systemctl start kubelet.service
systemctl enable kube</span>-<span style="color: #000000;">proxy.service
systemctl start kube</span>-proxy.service</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">在</span>master<span style="font-family: '微软雅黑',sans-serif;">上查看集群中节点及节点状态</span>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> kubectl -s http://10.0.0.11:8080 get node</span>
[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl -s http://10.0.0.11:8080 get node</span>
<span style="color: #000000;">NAME        STATUS    AGE
</span>10.0.0.12<span style="color: #000000;">   Ready     49s
</span>10.0.0.13<span style="color: #000000;">   Ready     56s
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get nodes</span>
<span style="color: #000000;">NAME        STATUS    AGE
</span>10.0.0.12<span style="color: #000000;">   Ready     1m
</span>10.0.0.13   Ready     1m</pre>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">至此</span>Kubernetes<span style="font-family: '微软雅黑',sans-serif;">基础部署完成</span>

### <span id="126_Kubernetes">1.2.6 Kubernetes<span style="font-family: '微软雅黑',sans-serif;">其他安装方法</span></span>

> &nbsp;&nbsp; &nbsp;二进制安装
> 
> &nbsp;&nbsp; &nbsp; kubuadm&nbsp;安装
> 
> &nbsp;&nbsp; &nbsp; minikube&nbsp;安装
> 
> &nbsp;&nbsp; &nbsp; ansible部署：<https://github.com/gjmzj/kubeasz>

## <span id="13_--Flannel">1.3 <span style="font-family: '微软雅黑',sans-serif;">创建覆盖网络</span>--Flannel</span>

### <span id="131_Flannel">1.3.1 <span style="font-family: '微软雅黑',sans-serif;">配置</span>Flannel<span style="font-family: '微软雅黑',sans-serif;">（所有节点操作）</span></span>

<span style="font-family: '微软雅黑',sans-serif;">安装软件包</span>

<div class="cnblogs_code">
  <pre>yum install flannel -y</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">修改配置文件</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep "^[a-Z]" /etc/sysconfig/flanneld</span>
FLANNEL_ETCD_ENDPOINTS=<span style="color: #800000;">"</span><span style="color: #800000;">http://10.0.0.11:2379</span><span style="color: #800000;">"</span><span style="color: #000000;">
FLANNEL_ETCD_PREFIX</span>=<span style="color: #800000;">"</span><span style="color: #800000;">/atomic.io/network</span><span style="color: #800000;">"</span></pre>
</div>

### <span id="132_etcdflannelkey">1.3.2 <span style="font-family: '微软雅黑',sans-serif;">配置</span>etcd<span style="font-family: '微软雅黑',sans-serif;">中关于</span>flannel<span style="font-family: '微软雅黑',sans-serif;">的</span>key</span>

<p style="text-indent: 21.0pt;">
  Flannel<span style="font-family: '微软雅黑',sans-serif;">使用</span>Etcd<span style="font-family: '微软雅黑',sans-serif;">进行配置，来保证多个</span>Flannel<span style="font-family: '微软雅黑',sans-serif;">实例之间的配置一致性，所以需要在</span>etcd<span style="font-family: '微软雅黑',sans-serif;">上进行如下配置：（&lsquo;</span>/atomic.io/network/config<span style="font-family: '微软雅黑',sans-serif;">&rsquo;这个</span>key<span style="font-family: '微软雅黑',sans-serif;">与上文</span>/etc/sysconfig/flannel<span style="font-family: '微软雅黑',sans-serif;">中的配置项</span>FLANNEL_ETCD_PREFIX<span style="font-family: '微软雅黑',sans-serif;">是相对应的，错误的话启动就会出错）</span>
</p>

<span style="font-family: '微软雅黑',sans-serif;">配置网络范围</span>

<div class="cnblogs_code">
  <pre>etcdctl mk  /atomic.io/network/config <span style="color: #800000;">'</span><span style="color: #800000;">{ "Network": "172.16.0.0/16" }</span><span style="color: #800000;">'</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">操作创建网络</span>
</p>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> etcdctl mk /atomic.io/network/config '{ "Network": "172.16.0.0/16" }'</span>
{ <span style="color: #800000;">"</span><span style="color: #800000;">Network</span><span style="color: #800000;">"</span>: <span style="color: #800000;">"</span><span style="color: #800000;">172.16.0.0/16</span><span style="color: #800000;">"</span> }</pre>
</div>

master<span style="font-family: '微软雅黑',sans-serif;">节点操作</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    systemctl enable flanneld.service 
    systemctl start flanneld.service 
    service docker restart
    systemctl restart kube</span>-<span style="color: #000000;">apiserver.service
    systemctl restart kube</span>-controller-<span style="color: #000000;">manager.service
    systemctl restart kube</span>-scheduler.service</pre>
</div>

node<span style="font-family: '微软雅黑',sans-serif;">节点操作</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    systemctl enable flanneld.service 
    systemctl start flanneld.service 
    service docker restart
    systemctl restart kubelet.service
    systemctl restart kube</span>-<span style="color: #000000;">proxy.service 
    </span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">修改配置文件</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  /etc/kubernetes/apiserver </span>
KUBE_API_ADDRESS=<span style="color: #800000;">"</span><span style="color: #800000;">--insecure-bind-address=0.0.0.0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_API_PORT</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--port=8080</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ETCD_SERVERS</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--etcd-servers=http://10.0.0.11:2379</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_SERVICE_ADDRESSES</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--service-cluster-ip-range=10.254.0.0/16</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ADMISSION_CONTROL</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ResourceQuota</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_API_ARGS</span>=<span style="color: #800000;">""</span></pre>
</div>

&nbsp;&nbsp; **<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red; background: yellow;">至此</span><span style="color: red; background: yellow;">Flannel</span>****<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red; background: yellow;">网络配置完成</span>**

## <span id="14_pod">1.4 <span style="font-family: '微软雅黑',sans-serif;">创建一个简单的</span>pod</span>

<p style="text-indent: 21.0pt;">
  Pod<span style="font-family: '微软雅黑',sans-serif;">是</span>K8s<span style="font-family: '微软雅黑',sans-serif;">集群中所有业务类型的基础</span>
</p>

<p style="text-indent: 21.0pt;">
  Pod<span style="font-family: '微软雅黑',sans-serif;">是在</span>K8s<span style="font-family: '微软雅黑',sans-serif;">集群中运行部署应用或服务的最小单元，它是可以支持多容器的。</span>
</p>

<p style="text-indent: 21.0pt;">
  Pod<span style="font-family: '微软雅黑',sans-serif;">的设计理念是支持多个容器在一个</span>Pod<span style="font-family: '微软雅黑',sans-serif;">中共享网络地址和文件系统。</span>
</p>

<p style="text-indent: 21.0pt;">
  POD<span style="font-family: '微软雅黑',sans-serif;">控制器</span>Deployment<span style="font-family: '微软雅黑',sans-serif;">、</span>Job<span style="font-family: '微软雅黑',sans-serif;">、</span>DaemonSet<span style="font-family: '微软雅黑',sans-serif;">和</span>PetSet
</p>

### <span id="141_yaml">1.4.1 <span style="font-family: '微软雅黑',sans-serif;">写一个编排</span>yaml<span style="font-family: '微软雅黑',sans-serif;">格式</span></span>

<p style="text-indent: 7.1pt;">
  kubenetes<span style="font-family: '微软雅黑',sans-serif;">里面的创建</span>service<span style="font-family: '微软雅黑',sans-serif;">、</span>rc<span style="font-family: '微软雅黑',sans-serif;">、</span>pod<span style="font-family: '微软雅黑',sans-serif;">都是这种形式</span>(<span style="font-family: '微软雅黑',sans-serif;">另外一种是</span>json)
</p>

<p style="text-indent: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">关于</span>yaml<span style="font-family: '微软雅黑',sans-serif;">参考：</span><a href="/wp-content/themes/clsn-003/inc/go.php?url=http://t.cn/RK0Jlwu" >http://t.cn/RK0Jlwu</a>
</p>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  /etc/kubernetes/apiserver </span>
KUBE_API_ADDRESS=<span style="color: #800000;">"</span><span style="color: #800000;">--insecure-bind-address=0.0.0.0</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_API_PORT</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--port=8080</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ETCD_SERVERS</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--etcd-servers=http://10.0.0.11:2379</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_SERVICE_ADDRESSES</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--service-cluster-ip-range=10.254.0.0/16</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_ADMISSION_CONTROL</span>=<span style="color: #800000;">"</span><span style="color: #800000;">--admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ResourceQuota</span><span style="color: #800000;">"</span><span style="color: #000000;">
KUBE_API_ARGS</span>=<span style="color: #800000;">""</span></pre>
</div>

### <span id="142_pod">1.4.2 <span style="font-family: '微软雅黑',sans-serif;">启动一个</span>pod</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl create -f hello.yaml </span>
pod <span style="color: #800000;">"</span><span style="color: #800000;">hello-world</span><span style="color: #800000;">"</span> created</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看默认</span>namespace<span style="font-family: '微软雅黑',sans-serif;">下的</span>pods

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get pods</span>
<span style="color: #000000;">NAME          READY     STATUS              RESTARTS   AGE
hello</span>-world   0/1       ContainerCreating   0          8s</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看</span>pod<span style="font-family: '微软雅黑',sans-serif;">的详细信息</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl  describe pod  hello-world </span>
<span style="color: #000000;">Events:
  FirstSeen    LastSeen    Count    From            SubObjectPath    Type       Reason        Message
  </span>---------    --------    -----    ----            -------------    --------   ------        -------<span style="color: #000000;">
  4m        4m        </span>1    {default-scheduler}      Normal         Scheduled    Successfully assigned hello-world to 10.0.0.13<span style="color: #000000;">
  4m        1m        </span>5    {kubelet 10.0.0.13}      Warning        FailedSync    Error syncing pod, skipping: failed to <span style="color: #800000;">"</span><span style="color: #800000;">StartContainer</span><span style="color: #800000;">"</span> <span style="color: #0000ff;">for</span> <span style="color: #800000;">"</span><span style="color: #800000;">POD</span><span style="color: #800000;">"</span> with ErrImagePull: <span style="color: #800000;">"</span><span style="color: #800000;">image pull failed for registry.access.redhat.com/rhel7/pod-infrastructure:latest, this may be because there are no credentials on this request.  details: (open /etc/docker/certs.d/registry.access.redhat.com/redhat-ca.crt: no such file or directory)</span><span style="color: #800000;">"</span><span style="color: #000000;">
  3m        14s       </span>13   {kubelet 10.0.0.13}      Warning        FailedSync    Error syncing pod, skipping: failed to <span style="color: #800000;">"</span><span style="color: #800000;">StartContainer</span><span style="color: #800000;">"</span> <span style="color: #0000ff;">for</span> <span style="color: #800000;">"</span><span style="color: #800000;">POD</span><span style="color: #800000;">"</span> with ImagePullBackOff: <span style="color: #800000;">"</span><span style="color: #800000;">Back-off pulling image \"registry.access.redhat.com/rhel7/pod-infrastructure:latest\"</span><span style="color: #800000;">"</span></pre>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">该错误的解决方法</span><span style="font-family: '微软雅黑',sans-serif;">：&nbsp;<span class="cnblogs_code">yum install python-rhsm* -y</span>&nbsp;</span>

<span style="font-family: '微软雅黑',sans-serif;">获取指定</span>pods<span style="font-family: '微软雅黑',sans-serif;">详细信息</span>

<div class="cnblogs_code">
  <pre>kubectl describe pods yourpodname</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">获取已运行</span>pod<span style="font-family: '微软雅黑',sans-serif;">状态</span>

<div class="cnblogs_code">
  <pre>kubectl get pods -o wide</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">下载</span>pod-infrastructure<span style="font-family: '微软雅黑',sans-serif;">镜像包</span>

<div class="cnblogs_code">
  <pre>docker tag docker.io/tianyebj/pod-infrastructure:latest registry.access.redhat.com/rhel7/pod-infrastructure:lates</pre>
</div>

### <span id="143_pod">1.4.3 pod<span style="font-family: '微软雅黑',sans-serif;">其他操作</span></span>

<span style="font-family: '微软雅黑',sans-serif;">删除</span>pod<span style="font-family: '微软雅黑',sans-serif;">，重新创建</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl delete -f hello.yaml </span>
pod <span style="color: #800000;">"</span><span style="color: #800000;">hello-world</span><span style="color: #800000;">"</span><span style="color: #000000;"> deleted
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl create -f hello.yaml </span>
pod <span style="color: #800000;">"</span><span style="color: #800000;">hello-world</span><span style="color: #800000;">"</span> created</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">查看状态</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get pods -o wide</span>
<span style="color: #000000;">NAME        READY     STATUS             RESTARTS   AGE       IP            NODE
nginx</span>-web   1/1       ImagePullBackOff   0          1m        172.16.53.2   10.0.0.13</pre>
</div>

## <span id="15_Replication_Controller">1.5 Replication Controller</span>

&nbsp;&nbsp; RC<span style="font-family: '微软雅黑',sans-serif;">是</span>K8s<span style="font-family: '微软雅黑',sans-serif;">集群中最早的保证</span>Pod<span style="font-family: '微软雅黑',sans-serif;">高可用的</span>API<span style="font-family: '微软雅黑',sans-serif;">对象。通过监控运行中的</span>Pod<span style="font-family: '微软雅黑',sans-serif;">来保证集群中运行指定数目的</span>Pod<span style="font-family: '微软雅黑',sans-serif;">副本。指定的数目可以是多个也可以是</span>1<span style="font-family: '微软雅黑',sans-serif;">个；少于指定数目，</span>RC<span style="font-family: '微软雅黑',sans-serif;">就会启动运行新的</span>Pod<span style="font-family: '微软雅黑',sans-serif;">副本；多于指定数目，</span>RC<span style="font-family: '微软雅黑',sans-serif;">就会杀死多余的</span>Pod<span style="font-family: '微软雅黑',sans-serif;">副本。</span>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">即使在指定数目为</span>1<span style="font-family: '微软雅黑',sans-serif;">的情况下，通过</span>RC<span style="font-family: '微软雅黑',sans-serif;">运行</span>Pod<span style="font-family: '微软雅黑',sans-serif;">也比直接运行</span>Pod<span style="font-family: '微软雅黑',sans-serif;">更明智，因为</span>RC<span style="font-family: '微软雅黑',sans-serif;">也可以发挥它高可用的能力，保证永远有</span>1<span style="font-family: '微软雅黑',sans-serif;">个</span>Pod<span style="font-family: '微软雅黑',sans-serif;">在运行。</span>

### <span id="151_rc">1.5.1 <span style="font-family: '微软雅黑',sans-serif;">简单</span>rc<span style="font-family: '微软雅黑',sans-serif;">配置</span></span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get  rc</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">始终保证有一个在活着</span>

<span style="font-family: '微软雅黑',sans-serif;">更新</span>rc<span style="font-family: '微软雅黑',sans-serif;">文件</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl replace -f  nginx.yml</span></pre>
</div>

&nbsp;&nbsp; nginx.yml<span style="font-family: '微软雅黑',sans-serif;">文件信息</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim nginx.yml</span>
<span style="color: #000000;">apiVersion: v1
kind: Pod
metadata:
  name: nginx</span>-2<span style="color: #000000;">
spec:
  restartPolicy: Never
  containers:
  </span>-<span style="color: #000000;"> name: nginx
    image: </span><span style="color: #800000;">"</span><span style="color: #800000;">docker.io/nginx:latest</span><span style="color: #800000;">"</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">对现有已创建资源直进行修改</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl edit rc nginx</span></pre>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">可以调整数量即使生效</span>

### <span id="152_rs">1.5.2 rs<span style="font-family: '微软雅黑',sans-serif;">实现灰度发布</span></span>

<p style="text-indent: 21.0pt;">
  RS<span style="font-family: '微软雅黑',sans-serif;">是新一代</span>RC<span style="font-family: '微软雅黑',sans-serif;">，提供同样的高可用能力，区别主要在于</span>RS<span style="font-family: '微软雅黑',sans-serif;">后来居上，能支持更多中的匹配模式。副本集对象一般不单独使用，而是作为部署的理想状态参数使用。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">是</span>K8S 1.2<span style="font-family: '微软雅黑',sans-serif;">中出现的概念，是</span>RC<span style="font-family: '微软雅黑',sans-serif;">的升级。一般和</span>Deployment<span style="font-family: '微软雅黑',sans-serif;">共同使用。</span>
</p>

> <p style="text-indent: 21.0pt;">
>   &nbsp;部署表示用户对K8s集群的一次更新操作。部署是一个比RS应用模式更广的API对象，可以是创建一个新的服务，更新一个新的服务，也可以是滚动升级一个服务。滚动升级一个服务，实际是创建一个新的RS，然后逐渐将新RS中副本数增加到理想状态，将旧RS中的副本数减小到0的复合操作；
> </p>
> 
> <p style="text-indent: 21.0pt;">
>   &nbsp;　　这样一个复合操作用一个RS是不太好描述的，所以用一个更通用的Deployment来描述。
> </p>
> 
> <p style="text-indent: 21.0pt;">
>   <span style="text-indent: 0px; background-color: initial;">　　以K8s的发展方向，未来对所有长期伺服型的的业务的管理，都会通过Deployment来管理。</span>
> </p>
> 
> <p style="text-indent: 21.0pt;">
>   &nbsp;　　Deployment是对RC的升级，与RC的相似度超过90%。
> </p>

web-rc.yaml<span style="font-family: '微软雅黑',sans-serif;">文件内容</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat web-rc.yaml </span>
<span style="color: #000000;">apiVersion: v1
kind: ReplicationController
metadata:
  name: myweb
spec:
  replicas: </span>3<span style="color: #000000;">
  selector:
    app: myweb
  template:
    metadata:
      labels:
        app: myweb
    spec:
      containers:
      </span>-<span style="color: #000000;"> name: myweb
        image: kubeguide</span>/tomcat-<span style="color: #000000;">app:v1
        ports:
        </span>- containerPort: 8080<span style="color: #000000;">
        env:
        </span>-<span style="color: #000000;"> name: MYSQL_SERVICE_HOST
          value: </span><span style="color: #800000;">'</span><span style="color: #800000;">mysql</span><span style="color: #800000;">'</span>
        -<span style="color: #000000;"> name: MYSQL_SERVICE_PORT
          value: </span><span style="color: #800000;">'</span><span style="color: #800000;">3306</span><span style="color: #800000;">'</span></pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">创建集群</span>**

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl create -f web-rc.yaml</span></pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">对集群进行升级操作</span>**

**&nbsp;&nbsp;** <span style="font-family: '微软雅黑',sans-serif;">将集群内容器自动升级到新版本的容器</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl rolling-update  myweb  -f web-rc2.yaml </span></pre>
</div>

web-rc2.yaml<span style="font-family: '微软雅黑',sans-serif;">配置文件内容</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat web-rc2.yaml </span>
<span style="color: #000000;">apiVersion: v1
kind: ReplicationController
metadata:
  name: myweb</span>-2<span style="color: #000000;">
spec:
  replicas: </span>3<span style="color: #000000;">
  selector:
    app: myweb</span>-2<span style="color: #000000;">
  template:
    metadata:
      labels:
        app: myweb</span>-2<span style="color: #000000;">
    spec:
      containers:
      </span>- name: myweb-2<span style="color: #000000;">
        image: kubeguide</span>/tomcat-<span style="color: #000000;">app:v2
        ports:
        </span>- containerPort: 8080<span style="color: #000000;">
        env:
        </span>-<span style="color: #000000;"> name: MYSQL_SERVICE_HOST
          value: </span><span style="color: #800000;">'</span><span style="color: #800000;">mysql</span><span style="color: #800000;">'</span>
        -<span style="color: #000000;"> name: MYSQL_SERVICE_PORT
          value: </span><span style="color: #800000;">'</span><span style="color: #800000;">3306</span><span style="color: #800000;">'</span></pre>
</div>

**<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: yellow;">升级后的回滚</span>**

**&nbsp;&nbsp;** <span style="font-family: '微软雅黑',sans-serif;">使用新的文件，进行升级操作可达到回滚的目的，参考：https://github.com/kubeguide/samplecode</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl rolling-update  myweb-2  -f web-rc.yaml </span></pre>
</div>

### <span id="153_rc">1.5.3 rc<span style="font-family: '微软雅黑',sans-serif;">小结</span></span>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> RC<span style="font-family: '微软雅黑',sans-serif;">里包括完整的</span>POD<span style="font-family: '微软雅黑',sans-serif;">定义模板</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> RC<span style="font-family: '微软雅黑',sans-serif;">通过</span>Label Selector<span style="font-family: '微软雅黑',sans-serif;">机制实现对</span>POD<span style="font-family: '微软雅黑',sans-serif;">副本的自动控制。</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">通过改变</span>RC<span style="font-family: '微软雅黑',sans-serif;">里的</span>POD<span style="font-family: '微软雅黑',sans-serif;">副本以实现</span>POD<span style="font-family: '微软雅黑',sans-serif;">的扩容和缩容</span>
</p>

<p style="margin-left: 24.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">?</span> <span style="font-family: '微软雅黑',sans-serif;">通过改变</span>RC<span style="font-family: '微软雅黑',sans-serif;">里</span>POD<span style="font-family: '微软雅黑',sans-serif;">模块中的镜像版本，可以实现</span>POD<span style="font-family: '微软雅黑',sans-serif;">的滚动升级。</span>
</p>

## <span id="16_Service">1.6 <span style="font-family: '微软雅黑',sans-serif;">服务（</span>Service<span style="font-family: '微软雅黑',sans-serif;">）</span></span>

### <span id="161_Service">1.6.1 Service<span style="font-family: '微软雅黑',sans-serif;">作用</span></span>

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;">RC</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">、</span>RS</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">和</span>Deployment</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">只是保证了支撑服务的</span>POD</span><span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">的数量</span></span><span style="font-family: '微软雅黑',sans-serif;">，但是没有解决如何访问这些服务的问题。一个</span>Pod<span style="font-family: '微软雅黑',sans-serif;">只是一个运行服务的实例，随时可能在一个节点上停止，在另一个节点以一个新的</span>IP<span style="font-family: '微软雅黑',sans-serif;">启动一个新的</span>Pod<span style="font-family: '微软雅黑',sans-serif;">，因此不能以确定的</span>IP<span style="font-family: '微软雅黑',sans-serif;">和端口号提供服务。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="text-decoration: underline;"><span style="font-family: '微软雅黑',sans-serif;">要稳定地提供服务需要服务发现和负载均衡能力</span></span><span style="font-family: '微软雅黑',sans-serif;">。服务发现完成的工作，是针对客户端访问的服务，找到对应的的后端服务实例。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>K8<span style="font-family: '微软雅黑',sans-serif;">集群中，客户端需要访问的服务就是</span>Service<span style="font-family: '微软雅黑',sans-serif;">对象。每个</span>Service<span style="font-family: '微软雅黑',sans-serif;">会对应一个集群内部有效的<strong><span style="color: red;">虚拟</span></strong></span><strong><span style="color: red;">IP</span></strong><span style="font-family: '微软雅黑',sans-serif;">，集群内部通过虚拟</span>IP<span style="font-family: '微软雅黑',sans-serif;">访问一个服务。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>K8s<span style="font-family: '微软雅黑',sans-serif;">集群中微服务的负载均衡是由</span>Kube-proxy<span style="font-family: '微软雅黑',sans-serif;">实现的。</span>Kube-proxy<span style="font-family: '微软雅黑',sans-serif;">是</span>K8s<span style="font-family: '微软雅黑',sans-serif;">集群内部的负载均衡器。它是一个分布式代理服务器，在</span>K8s<span style="font-family: '微软雅黑',sans-serif;">的每个节点上都有一个；这一设计体现了它的<strong><em>伸缩性</em></strong>优势，需要访问服务的节点越多，提供负载均衡能力的</span>Kube-proxy<span style="font-family: '微软雅黑',sans-serif;">就越多，高可用节点也随之增多。</span>
</p>

### <span id="162_service">1.6.2 <span style="font-family: '微软雅黑',sans-serif;">测试</span>service</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim myweb-svc.yaml </span>
<span style="color: #000000;">apiVersion: v1
kind: Service
metadata:
  name: myweb
spec:
  type: NodePort
  ports:
    </span>- port: 8080<span style="color: #000000;">
      nodePort: </span>30001<span style="color: #000000;">
  selector:
    app: myweb</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">启动集群</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl create -f myweb-svc.yaml </span>
service <span style="color: #800000;">"</span><span style="color: #800000;">myweb</span><span style="color: #800000;">"</span><span style="color: #000000;"> created
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get svc</span>
NAME         CLUSTER-IP      EXTERNAL-<span style="color: #000000;">IP   PORT(S)          AGE
kubernetes   </span>10.254.0.1      &lt;none>        443/<span style="color: #000000;">TCP          6h
myweb        </span>10.254.247.21   &lt;nodes>       8080:30001/TCP   12s</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">浏览器访问测试</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180203175348406-53755867.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Kubernetes 编排系统" alt="" />&nbsp;
</p>

### <span id="163_service">1.6.3 service<span style="font-family: '微软雅黑',sans-serif;">原理图</span></span>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180203175356062-1640106637.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Kubernetes 编排系统" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">网访问</span>node ip <span style="font-family: '微软雅黑',sans-serif;">转到</span>cluster ip<span style="font-family: '微软雅黑',sans-serif;">上</span> <span style="font-family: '微软雅黑',sans-serif;">在进行</span>pod <span style="font-family: '微软雅黑',sans-serif;">分发</span>&nbsp; rr<span style="font-family: '微软雅黑',sans-serif;">轮询</span>
</p>

<div class="cnblogs_code">
  <pre>kubectl create -f web-<span style="color: #000000;">svc.yaml
    [root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get service</span>
<span style="color: #000000;">    
    NAME         CLUSTER</span>-IP      EXTERNAL-<span style="color: #000000;">IP   PORT(S)          AGE
    kubernetes   </span>10.254.0.1      &lt;none>        443/<span style="color: #000000;">TCP          4h
    myweb        </span>10.254.168.71   &lt;nodes>       8080:30001/TCP   15s</pre>
</div>

### <span id="164_K8SIP">1.6.4 K8S<span style="font-family: '微软雅黑',sans-serif;">三种</span>IP</span>

<table style="width: 100%; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 21.64%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="21%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">类型</span></strong>
      </p>
    </td>
    
    <td style="width: 78.36%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="78%">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: white;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 21.64%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="21%">
      <p style="text-align: center;" align="center">
        <strong>Node IP</strong>
      </p>
    </td>
    
    <td style="width: 78.36%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="78%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">节点设备的</span>IP<span style="font-family: '微软雅黑',sans-serif;">，如物理机，虚拟机等容器宿主的实际</span>IP<span style="font-family: '微软雅黑',sans-serif;">。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 21.64%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="21%">
      <p style="text-align: center;" align="center">
        <strong>Pod IP</strong>
      </p>
    </td>
    
    <td style="width: 78.36%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="78%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Pod <span style="font-family: '微软雅黑',sans-serif;">的</span>IP<span style="font-family: '微软雅黑',sans-serif;">地址，是根据</span>docker0<span style="font-family: '微软雅黑',sans-serif;">网格</span>IP<span style="font-family: '微软雅黑',sans-serif;">段进行分配的。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 21.64%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="21%">
      <p style="text-align: center;" align="center">
        <strong>Cluster IP</strong>
      </p>
    </td>
    
    <td style="width: 78.36%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="78%">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        &nbsp;Service<span style="font-family: '微软雅黑',sans-serif;">的</span>IP<span style="font-family: '微软雅黑',sans-serif;">，是一个虚拟</span>IP<span style="font-family: '微软雅黑',sans-serif;">，仅作用于</span>service<span style="font-family: '微软雅黑',sans-serif;">对象，由</span>k8s<span style="font-family: '微软雅黑',sans-serif;">管理和分配，需要结合</span>service port<span style="font-family: '微软雅黑',sans-serif;">才能使用，单独的</span>IP<span style="font-family: '微软雅黑',sans-serif;">没有通信功能，集群外访问需要一些修改。</span>
      </p>
    </td>
  </tr>
</table>

## <span id="17_DashBoard"><span style="font-size: 1.5em;">1.7 </span><span style="font-family: '微软雅黑',sans-serif;">部署</span><span style="font-size: 1.5em;">DashBoard</span></span>

<p style="margin-left: 25.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">参考文档：</span>http://www.cnblogs.com/zhenyuyaodidiao/p/6500897.html
</p>

### <span id="171">1.7.1 <span style="font-family: '微软雅黑',sans-serif;">修改配置文件</span></span>

<span style="font-family: '微软雅黑',sans-serif;">编辑</span>dashboard.yaml<span style="font-family: '微软雅黑',sans-serif;">，注意或更改以下部分：</span>

<div class="cnblogs_code">
  <pre>    image: index.tenxcloud.com/google_containers/kubernetes-dashboard-amd64:v1.4.1<span style="color: #000000;">
            args:
         </span>-  --apiserver-host=http://10.0.0.11:8080</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">编辑</span>dashboardsvc.yaml<span style="font-family: '微软雅黑',sans-serif;">文件：</span>
</p>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">apiVersion: v1
kind: Service
metadata:
  name: kubernetes</span>-<span style="color: #000000;">dashboard
  namespace: kube</span>-<span style="color: #000000;">system
  labels:
    k8s</span>-app: kubernetes-<span style="color: #000000;">dashboard
    kubernetes.io</span>/cluster-service: <span style="color: #800000;">"</span><span style="color: #800000;">true</span><span style="color: #800000;">"</span><span style="color: #000000;">
spec:
  selector:
    k8s</span>-app: kubernetes-<span style="color: #000000;">dashboard
  ports:
  </span>- port: 80<span style="color: #000000;">
targetPort: </span>9090</pre>
</div>

### <span id="172">1.7.2 <span style="font-family: '微软雅黑',sans-serif;">镜像准备</span></span>

> 在dashboard.yaml中定义了dashboard所用的镜像
> 
> gcr.io/google_containers/kubernetes-dashboard-amd64:v1.5.1（当然你可以选择其他的版本）

<span style="font-family: '微软雅黑',sans-serif;">下载地址</span>

<div class="cnblogs_code">
  <pre>docker pull registry.cn-hangzhou.aliyuncs.com/google-containers/kubernetes-dashboard-amd64:v1.4.1</pre>
</div>

### <span id="173_dashboard">1.7.3 <span style="font-family: '微软雅黑',sans-serif;">启动</span>dashboard</span>

<span style="font-family: '微软雅黑',sans-serif;">在</span>master<span style="font-family: '微软雅黑',sans-serif;">执行如下命令：</span>

<div class="cnblogs_code">
  <pre>kubectl create -<span style="color: #000000;">f dashboard.yaml
kubectl create </span>-f dashboardsvc.yaml</pre>
</div>

<p style="text-indent: 15.75pt;">
  <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red; background: yellow;">到此</span><span style="color: red; background: yellow;">dashboard</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red; background: yellow;">搭建完成</span></strong>
</p>

### <span id="174">1.7.4 <span style="font-family: '微软雅黑',sans-serif;">验证</span></span>

<span style="font-family: '微软雅黑',sans-serif;">　　命令验证，</span>master<span style="font-family: '微软雅黑',sans-serif;">上执行如下命令：</span>

<div class="cnblogs_code">
  <pre>[root@k8s-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get deployment --all-namespaces</span>
NAMESPACE     NAME                          DESIRED   CURRENT   UP-TO-<span style="color: #000000;">DATE   AVAILABLE   AGE
kube</span>-system   kubernetes-dashboard-latest   1         1         1            1<span style="color: #000000;">           42m
[root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get svc  --all-namespaces</span>
NAMESPACE     NAME                   CLUSTER-IP      EXTERNAL-<span style="color: #000000;">IP   PORT(S)          AGE
default       kubernetes             </span>10.254.0.1      &lt;none>        443/<span style="color: #000000;">TCP          5h
default       myweb                  </span>10.254.168.71   &lt;nodes>       8080:30001/<span style="color: #000000;">TCP   1h
kube</span>-system   kubernetes-dashboard   10.254.90.78    &lt;none>        80/<span style="color: #000000;">TCP           41m
    [root@k8s</span>-master ~]<span style="color: #008000;">#</span><span style="color: #008000;"> kubectl get pod  -o wide  --all-namespaces</span>
<span style="color: #000000;">    NAMESPACE     NAME                                           READY     STATUS    RESTARTS   AGE       IP            NODE
    default       myweb</span>-c2dfj                                    1/1       Running   0          1h        172.16.57.2   10.0.0.13<span style="color: #000000;">
    default       myweb</span>-h7rkb                                    1/1       Running   0          1h        172.16.76.2   10.0.0.12<span style="color: #000000;">
    default       myweb</span>-l48b3                                    1/1       Running   0          1h        172.16.57.3   10.0.0.13<span style="color: #000000;">
    kube</span>-system   kubernetes-dashboard-latest-1395490986-1t37v   1/1       Running   0          43m       172.16.76.3   10.0.0.12</pre>
</div>

### <span id="175_http1000118080ui">1.7.5 <span style="font-family: '微软雅黑',sans-serif;">浏览器访问：</span>http://10.0.0.11:8080/ui</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20180203175411906-1279802068.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Kubernetes 编排系统" alt="" />&nbsp;
</p>

### <span id="176">1.7.6 <span style="font-family: '微软雅黑',sans-serif;">销毁应用</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red; font-weight: normal;">（测试）</span></span>

<span style="font-family: '微软雅黑',sans-serif;">在</span>master<span style="font-family: '微软雅黑',sans-serif;">上执行：</span>

<div class="cnblogs_code">
  <pre>kubectl delete deployment kubernetes-dashboard-latest --namespace=kube-<span style="color: #000000;">system
kubectl delete svc  kubernetes</span>-dashboard --namespace=kube-system</pre>
</div>

## <span id="18">1.8 <span style="font-family: '微软雅黑',sans-serif;">参考文献</span></span>

> &nbsp;[1]&nbsp;[http://docs.kubernetes.org.cn/227.html][1]
> 
> &nbsp;[2]&nbsp;[http://www.cnblogs.com/zhenyuyaodidiao/p/6500830.html][2]

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11_Kubernetes">1.1 Kubernetes简介</a><ul>
        <li>
          <a href="#111_Kubernetes">1.1.1 什么是Kubernetes</a>
        </li>
        <li>
          <a href="#112_Kubernetes">1.1.2 Kubernetes发展史</a>
        </li>
        <li>
          <a href="#113_Kubernetes">1.1.3 Kubernetes 特点</a>
        </li>
        <li>
          <a href="#114_Kubernetes">1.1.4 Kubernetes规划组件</a>
        </li>
        <li>
          <a href="#115_Kubernetes">1.1.5 Kubernetes核心组件</a>
        </li>
        <li>
          <a href="#116">1.1.6 分层架构</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_Kubernetes">1.2 部署Kubernetes集群</a><ul>
        <li>
          <a href="#121">1.2.1 主机环境说明</a>
        </li>
        <li>
          <a href="#122">1.2.2 安装软件包</a>
        </li>
        <li>
          <a href="#123_etcd">1.2.3 修改配置etcd</a>
        </li>
        <li>
          <a href="#124_kubernetes">1.2.4 配置并启动kubernetes</a>
        </li>
        <li>
          <a href="#125_node">1.2.5 部署配置node</a>
        </li>
        <li>
          <a href="#126_Kubernetes">1.2.6 Kubernetes其他安装方法</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_--Flannel">1.3 创建覆盖网络--Flannel</a><ul>
        <li>
          <a href="#131_Flannel">1.3.1 配置Flannel（所有节点操作）</a>
        </li>
        <li>
          <a href="#132_etcdflannelkey">1.3.2 配置etcd中关于flannel的key</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14_pod">1.4 创建一个简单的pod</a><ul>
        <li>
          <a href="#141_yaml">1.4.1 写一个编排yaml格式</a>
        </li>
        <li>
          <a href="#142_pod">1.4.2 启动一个pod</a>
        </li>
        <li>
          <a href="#143_pod">1.4.3 pod其他操作</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15_Replication_Controller">1.5 Replication Controller</a><ul>
        <li>
          <a href="#151_rc">1.5.1 简单rc配置</a>
        </li>
        <li>
          <a href="#152_rs">1.5.2 rs实现灰度发布</a>
        </li>
        <li>
          <a href="#153_rc">1.5.3 rc小结</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#16_Service">1.6 服务（Service）</a><ul>
        <li>
          <a href="#161_Service">1.6.1 Service作用</a>
        </li>
        <li>
          <a href="#162_service">1.6.2 测试service</a>
        </li>
        <li>
          <a href="#163_service">1.6.3 service原理图</a>
        </li>
        <li>
          <a href="#164_K8SIP">1.6.4 K8S三种IP</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#17_DashBoard">1.7 部署DashBoard</a><ul>
        <li>
          <a href="#171">1.7.1 修改配置文件</a>
        </li>
        <li>
          <a href="#172">1.7.2 镜像准备</a>
        </li>
        <li>
          <a href="#173_dashboard">1.7.3 启动dashboard</a>
        </li>
        <li>
          <a href="#174">1.7.4 验证</a>
        </li>
        <li>
          <a href="#175_http1000118080ui">1.7.5 浏览器访问：http://10.0.0.11:8080/ui</a>
        </li>
        <li>
          <a href="#176">1.7.6 销毁应用（测试）</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#18">1.8 参考文献</a>
    </li>
  </ul>
</div>

 [1]: /wp-content/themes/clsn-003/inc/go.php?url=http://docs.kubernetes.org.cn/227.html
 [2]: /wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/zhenyuyaodidiao/p/6500830.html