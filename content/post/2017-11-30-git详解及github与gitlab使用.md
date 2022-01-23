---
title: Git详解及 github与gitlab使用
author: 惨绿少年
type: post
date: 2017-11-30T01:39:00+00:00
url: /clsn/lx565.html
Baidusubmit:
  - 1
views:
  - 180
categories:
  - Linux运维
  - 敏捷开发
  - 玩转Linux
  - 自动化
  - 运维基本功

---
## <span id="11">1.1 <span style="font-family: '微软雅黑',sans-serif;">关于版本控制</span></span>

### <span id="111">1.1.1 <span style="font-family: '微软雅黑',sans-serif;">本地版本控制</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">本地版本控制系统</span> <span style="font-family: 等线;">许多人习惯用复制整个项目目录的方式来保存不同的版本，或许还会改名加上备份时间以示区别。这么做唯一的</span> <span style="font-family: 等线;">好处就是简单，但是特别容易犯错。有时候会混淆所在的工作目录，一不小心会写错文件或者覆盖意想外的文件。</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130170717667-909081145.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

### <span id="112">1.1.2 <span style="font-family: '微软雅黑',sans-serif;">集中化的版本控制系统</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">如何让在不同系统上的开发者协同工作？于是，集中化的版本控制系统（</span>Centralized Version &nbsp;Control Systems<span style="font-family: 等线;">，简称</span> CVCS<span style="font-family: 等线;">）应运而生。这类系统，诸如</span> CVS<span style="font-family: 等线;">、</span>Subversion <span style="font-family: 等线;">以及</span>Perforce <span style="font-family: 等线;">等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130170728011-1466806727.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

### <span id="113">1.1.3 <span style="font-family: '微软雅黑',sans-serif;">分布式版本控制系统</span></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">在这类系统中，像</span>Git<span style="font-family: 等线;">、</span>Mercurial<span style="font-family: 等线;">、</span>Bazaar <span style="font-family: 等线;">以及</span> Darcs <span style="font-family: 等线;">等，客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130170739729-1366084886.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

## <span id="12_Git">1.2 Git<span style="font-family: '微软雅黑',sans-serif;">简介</span></span>

<span style="font-family: 等线;"><img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130170749808-269525315.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" /></span>

&nbsp;

<span style="font-family: 等线;">　　　　官网：</span>https://git-scm.com

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; git<span style="font-family: 等线;">是一个分布式版本控制软件，最初由林纳斯&middot;托瓦兹（</span>Linus Torvalds<span style="font-family: 等线;">）创作，于</span>2005<span style="font-family: 等线;">年以</span>GPL<span style="font-family: 等线;">发布。最初目的是为更好地管理</span>Linux<span style="font-family: 等线;">内核开发而设计。</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    Git <span style="font-family: '微软雅黑',sans-serif;">官方中文手册</span> https://git-scm.com/book/zh/v2
  </p>
</div>

### <span id="121_Git">1.2.1 Git<span style="font-family: '微软雅黑',sans-serif;">历史</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">自</span>2002<span style="font-family: 等线;">年开始，林纳斯&middot;托瓦兹决定使用</span>BitKeeper<span style="font-family: 等线;">作为</span>Linux<span style="font-family: 等线;">内核主要的版本控制系统用以维护代码。因为</span>BitKeeper<span style="font-family: 等线;">为专有软件，这个决定在社区中长期遭受质疑。在</span>Linux<span style="font-family: 等线;">社区中，特别是理查德&middot;斯托曼与自由软件基金会的成员，主张应该使用开放源代码的软件来作为</span>Linux<span style="font-family: 等线;">核心的版本控制系统。林纳斯&middot;托瓦兹曾考虑过采用现成软件作为版本控制系统（例如</span>Monotone<span style="font-family: 等线;">），但这些软件都存在一些问题，特别是性能不佳。现成的方案，如</span>CVS<span style="font-family: 等线;">的架构，受到林纳斯&middot;托瓦兹的批评。</span>
</p>

<p style="text-indent: 21.0pt;">
  2005<span style="font-family: 等线;">年，安德鲁&middot;垂鸠写了一个简单程序，可以连接</span>BitKeeper<span style="font-family: 等线;">的存储库，</span>BitKeeper<span style="font-family: 等线;">著作权拥有者拉里&middot;麦沃伊认为安德鲁&middot;垂鸠对</span>BitKeeper<span style="font-family: 等线;">内部使用的协议进行逆向工程，决定收回无偿使用</span>BitKeeper<span style="font-family: 等线;">的授权。</span>Linux<span style="font-family: 等线;">内核开发团队与</span>BitMover<span style="font-family: 等线;">公司进行蹉商，但无法解决他们之间的歧见。林纳斯&middot;托瓦兹决定自行开发版本控制系统替代</span>BitKeeper<span style="font-family: 等线;">，以十天的时间，编写出第一个</span>git<span style="font-family: 等线;">版本</span>
</p>

## <span id="13_git">1.3 <span style="font-family: '微软雅黑',sans-serif;">安装</span>git</span>

### <span id="131">1.3.1 <span style="font-family: '微软雅黑',sans-serif;">环境说明</span></span>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -qa centos-release</span>
centos-release-7-4.1708<span style="color: #000000;">.el7.centos.x86_64
[root@gitlab </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -a</span>
Linux gitlab 3.10.0-693.el7.x86_64 <span style="color: #008000;">#</span><span style="color: #008000;">1 SMP Tue Aug 22 21:09:27 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux</span>
[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> getenforce </span>
<span style="color: #000000;">Disabled
[root@gitlab </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status firewalld.service </span>
● firewalld.service - firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(</span>1)</pre>
</div>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    # <span style="font-family: '微软雅黑',sans-serif;">本文使用的</span>linux<span style="font-family: '微软雅黑',sans-serif;">系统均为该系统</span>
  </p>
  
  <p>
    # <span style="font-family: '微软雅黑',sans-serif;">本文使用的</span>windows<span style="font-family: '微软雅黑',sans-serif;">系统为</span> Microsoft Windows [<span style="font-family: '微软雅黑',sans-serif;">版本</span> 10.0.15063]
  </p>
</div>

### <span id="132_YumGit">1.3.2 Yum<span style="font-family: '微软雅黑',sans-serif;">安装</span>Git</span>

<p style="margin-left: 7.1pt;">
  # centos <span style="font-family: 等线;">自带</span>git
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -qa git</span>
git-1.8.3.1-11.el7.x86_64</pre>
</div>

&nbsp;# <span style="font-family: 等线;">安装方法</span>

<div class="cnblogs_code">
  <pre>yum install git -y</pre>
</div>

### <span id="133">1.3.3 <span style="font-family: '微软雅黑',sans-serif;">编译安装</span></span>

<p style="text-indent: 11.0pt;">
  <span style="font-family: 等线;">编译安装可以安装较新版本的</span>git
</p>

<p style="text-indent: 10.5pt;">
  Git<span style="font-family: 等线;">下载地址：</span> https://github.com/git/git/releases
</p>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 安装依赖关系</span>
yum install curl-devel expat-devel gettext-devel  openssl-devel zlib-<span style="color: #000000;">devel
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 编译安装</span>
tar -zxf git-2.0<span style="color: #000000;">.0.tar.gz
cd git</span>-2.0<span style="color: #000000;">.0
make configure
.</span>/configure --prefix=/<span style="color: #000000;">usr
make  
make install  </span></pre>
</div>

## <span id="14_Git">1.4 <span style="font-family: '微软雅黑',sans-serif;">初次运行</span> Git <span style="font-family: '微软雅黑',sans-serif;">前的配置</span></span>

### <span id="141_git">1.4.1 <span style="font-family: '微软雅黑',sans-serif;">配置</span>git</span>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">命令集</span></span>_

<div class="cnblogs_code">
  <pre>git config --<span style="color: #0000ff;">global</span> user.name <span style="color: #800000;">"</span><span style="color: #800000;">clsn</span><span style="color: #800000;">"</span>  <span style="color: #008000;">#</span><span style="color: #008000;">配置git使用用户</span>
git config --<span style="color: #0000ff;">global</span> user.email <span style="color: #800000;">"</span><span style="color: #800000;">admin@znix.top</span><span style="color: #800000;">"</span>  <span style="color: #008000;">#</span><span style="color: #008000;">配置git使用邮箱</span>
git config --<span style="color: #0000ff;">global</span> color.ui true  <span style="color: #008000;">#</span><span style="color: #008000;">语法高亮</span>
git config --list <span style="color: #008000;">#</span><span style="color: #008000;"> 查看全局配置</span></pre>
</div>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">配置过程</span></span>_

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> git config --global user.name "clsn"  #配置git使用用户</span>
[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> git config --global user.email "admin@znix.top"  #配置git使用邮箱</span>
[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> git config --global color.ui true  #语法高亮</span>
[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> git config --list # 查看全局配置</span>
user.name=<span style="color: #000000;">clsn
user.email</span>=<span style="color: #000000;">admin@znix.top
color.ui</span>=true</pre>
</div>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">生成的配置文件</span></span>_

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat .gitconfig </span>
<span style="color: #000000;">[user]
    name </span>=<span style="color: #000000;"> clsn
    email </span>=<span style="color: #000000;"> admin@znix.top
[color]
    ui </span>= true</pre>
</div>

### <span id="142">1.4.2 <span style="font-family: '微软雅黑',sans-serif;">获取帮助</span></span>

<p style="text-indent: 15.75pt;">
  <span style="font-family: 等线;">使用</span>Git<span style="font-family: 等线;">时需要获取帮助，有三种方法可以找到</span>Git<span style="font-family: 等线;">命令的使用手册：</span>
</p>

<div class="cnblogs_code">
  <pre>git help &lt;verb><span style="color: #000000;">
git </span>&lt;verb> --<span style="color: #000000;">help
man git</span>-&lt;verb></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: 等线;">例如，要想获得配置命令的手册，执行</span>
</p>

<div class="cnblogs_code">
  <pre>git help config</pre>
</div>

## <span id="15_Git">1.5 <span style="font-family: '微软雅黑',sans-serif;">获取</span> Git <span style="font-family: '微软雅黑',sans-serif;">仓库</span><em><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";font-weight: normal;">（初始化仓库）</span></em></span>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 创建目录</span>
<span style="color: #000000;">mkdir git_data
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 进入目录</span>
cd git_data/
<span style="color: #008000;">#</span><span style="color: #008000;"> 初始化</span>
<span style="color: #000000;">git init
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 查看工作区状态</span>
git status</pre>
</div>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">操作过程</span></span>_

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir git_data</span>
[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd git_data/</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git init</span>
初始化空的 Git 版本库于 /root/git_data/.git/<span style="color: #000000;">
[root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> 初始提交</span><span style="color: #008000;">
#
</span>无文件要提交（创建/拷贝文件并使用 <span style="color: #800000;">"</span><span style="color: #800000;">git add</span><span style="color: #800000;">"</span> 建立跟踪）</pre>
</div>

## <span id="16_Git">1.6 Git<span style="font-family: '微软雅黑',sans-serif;">命令常规操作</span></span>

<span style="font-family: 等线;">常用命令说明</span>

<table style="width: 99%; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 20.32%; border-top: 1pt solid #4472c4; border-bottom: 1pt solid #4472c4; border-left: 1pt solid #4472c4; border-right: none; background: #4472c4; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: center;" align="center">
        <strong><span style="font-size: 10.0pt; font-family: 宋体; times new roman"4times new roman";times new roman";color: white;">命令</span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: 1pt solid #4472c4; border-right: 1pt solid #4472c4; border-bottom: 1pt solid #4472c4; border-left: none; background: #4472c4; padding: 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: center;" align="center">
        <strong><span style="font-size: 10.0pt; font-family: 宋体; times new roman"4times new roman";times new roman";color: white;">命令说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">add&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">添加文件内容至索引</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">bisect&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">通过二分查找定位引入</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;"> bug </span><span style="font-size: 10.0pt; font-family: 宋体;">的变更</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">branch&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">列出、创建或删除分支</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">checkout</span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">检出一个分支或路径到工作区</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">clone&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">克隆一个版本库到一个新目录</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">commit&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">记录变更到版本库</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">diff&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">显示提交之间、提交和工作区之间等的差异</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">fetch&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">从另外一个版本库下载对象和引用</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">grep&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">输出和模式匹配的行</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">init&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">创建一个空的</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">Git&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">版本库或重新初始化一个已存在的版本库</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">log&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">显示提交日志</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">merge&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">合并两个或更多开发历史</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">mv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">移动或重命名一个文件、目录或符号链接</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">pull&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">获取并合并另外的版本库或一个本地分支</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">push&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">更新远程引用和相关的对象</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">rebase&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">本地提交转移至更新后的上游分支中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">reset&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">重置当前</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">HEAD</span><span style="font-size: 10.0pt; font-family: 宋体;">到指定状态</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">rm&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">从工作区和索引中删除文件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">show&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">显示各种类型的对象</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">status&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">显示工作区状态</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 20.32%; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" width="20%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">tag</span></strong>
      </p>
    </td>
    
    <td style="width: 79.68%; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="79%">
      <p style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph;">
        <span style="font-size: 10.0pt; font-family: 宋体;">创建、列出、删除或校验一个</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">GPG</span><span style="font-size: 10.0pt; font-family: 宋体;">签名的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;"> tag </span><span style="font-size: 10.0pt; font-family: 宋体;">对象</span>
      </p>
    </td>
  </tr>
</table>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">常用操作示意图</span></span>_**

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130170959948-2117269594.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">文件的状态变化周期</span></span>_**

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130171009651-1964332055.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

### <span id="161">1.6.1 <span style="font-family: '微软雅黑',sans-serif;">创建文件</span></span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> touch README</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> 初始提交</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> 未跟踪的文件:</span><span style="color: #008000;">
#</span><span style="color: #008000;">   （使用 "git add &lt;file>..." 以包含要提交的内容）</span><span style="color: #008000;">
#
#</span><span style="color: #ff0000;">    README</span>
提交为空，但是存在尚未跟踪的文件（使用 <span style="color: #800000;">"</span><span style="color: #800000;">git add</span><span style="color: #800000;">"</span> 建立跟踪）</pre>
</div>

&nbsp;

<span style="font-family: 等线;">添加文件跟踪</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git add ./*</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> 初始提交</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> 要提交的变更：</span><span style="color: #008000;">
#</span><span style="color: #008000;">   （使用 "git rm --cached &lt;file>..." 撤出暂存区）</span><span style="color: #008000;">
#
#</span><span style="color: #008000;">    新文件：    README</span><span style="color: #008000;">
#</span></pre>
</div>

<span style="font-family: 等线;">文件会添加到</span>.git<span style="font-family: 等线;">的隐藏目录</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> tree  .git/</span>
.git/<span style="color: #000000;">
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch</span>-<span style="color: #000000;">msg.sample
│   ├── commit</span>-<span style="color: #000000;">msg.sample
│   ├── post</span>-<span style="color: #000000;">update.sample
│   ├── pre</span>-<span style="color: #000000;">applypatch.sample
│   ├── pre</span>-<span style="color: #000000;">commit.sample
│   ├── prepare</span>-commit-<span style="color: #000000;">msg.sample
│   ├── pre</span>-<span style="color: #000000;">push.sample
│   ├── pre</span>-<span style="color: #000000;">rebase.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── objects
│   ├── e6
│   │  <span style="color: #00ff00;"> └── 9de29bb2d1d6434b8b29ae775ad8c2e48c5391</span>
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags</span></pre>
</div>

<span style="font-family: 等线;">由工作区提交到本地仓库</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git commit  -m 'first commit'  </span>
<span style="color: #000000;">[master（根提交） bb963eb] first commit
 </span>1 file changed, 0 insertions(+), 0 deletions(-<span style="color: #000000;">)
 create mode </span>100644 README</pre>
</div>

<span style="font-family: 等线;">查看</span>git<span style="font-family: 等线;">的状态</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span>
无文件要提交，干净的工作区</pre>
</div>

<span style="font-family: 等线;">提交后的</span>git<span style="font-family: 等线;">目录状态</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> tree  .git/</span>
.git/<span style="color: #000000;">
├── branches
├── COMMIT_EDITMSG
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch</span>-<span style="color: #000000;">msg.sample
│   ├── commit</span>-<span style="color: #000000;">msg.sample
│   ├── post</span>-<span style="color: #000000;">update.sample
│   ├── pre</span>-<span style="color: #000000;">applypatch.sample
│   ├── pre</span>-<span style="color: #000000;">commit.sample
│   ├── prepare</span>-commit-<span style="color: #000000;">msg.sample
│   ├── pre</span>-<span style="color: #000000;">push.sample
│   ├── pre</span>-<span style="color: #000000;">rebase.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           └── master
├── objects
│   ├── </span>54<span style="color: #000000;">
│   │   <span style="color: #00ff00;">└── 3b9bebdc6bd5c4b22136034a95dd097a57d3dd</span>
│   ├── bb
│   │  <span style="color: #00ff00;"> └── 963eb32ad93a72d9ce93e4bb55105087f1227d</span>
│   ├── e6
│   │   <span style="color: #00ff00;">└── 9de29bb2d1d6434b8b29ae775ad8c2e48c5391</span>
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags</span></pre>
</div>

### <span id="162">1.6.2 <span style="font-family: '微软雅黑',sans-serif;">添加新文件</span></span>

<div class="cnblogs_code">
  <pre>git add  *<span style="color: #000000;"> 添加到暂存区域
git commit  提交git仓库 </span>-m 后面接上注释信息，内容关于本次提交的说明，方便自己或他人查看</pre>
</div>

<span style="font-family: 等线;">修改或删除原有文件</span>

<span style="font-family: 等线;">常规方法</span>

<div class="cnblogs_code">
  <pre>git add  *<span style="color: #000000;">
git commit</span></pre>
</div>

<span style="font-family: 等线;">简便方法</span>

<div class="cnblogs_code">
  <pre>git commit -a  -m <span style="color: #800000;">"</span><span style="color: #800000;">注释信息</span><span style="color: #800000;">"</span></pre>
</div>

-a <span style="font-family: 等线;">表示直接提交</span>

<div class="cnblogs_code">
  <pre>Tell the command to automatically stage files that have been modified <span style="color: #0000ff;">and</span> deleted, but new files you have <span style="color: #0000ff;">not</span><span style="color: #000000;"> told Git about are
</span><span style="color: #0000ff;">not</span> affected.</pre>
</div>

### <span id="163_git">1.6.3 <span style="font-family: '微软雅黑',sans-serif;">删除</span>git<span style="font-family: '微软雅黑',sans-serif;">内的文件</span></span>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">命令说明：</span></span>_**

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> <span style="font-family: '微软雅黑',sans-serif;">没有添加到暂存区的数据直接</span>rm<span style="font-family: '微软雅黑',sans-serif;">删除即可。</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> <span style="font-family: '微软雅黑',sans-serif;">已经添加到暂存区数据：</span>
  </p>
  
  <p>
    git rm --cached database&nbsp;
  </p>
  
  <p>
    #<span style="font-family: '微软雅黑',sans-serif;">&rarr;将文件从</span>git<span style="font-family: '微软雅黑',sans-serif;">暂存区域的追踪列表移除</span>(<span style="font-family: '微软雅黑',sans-serif;">并不会删除当前工作目录内的数据文件</span>)
  </p>
  
  <p>
    git rm -f database
  </p>
  
  <p>
    #<span style="font-family: '微软雅黑',sans-serif;">&rarr;将文件数据从</span>git<span style="font-family: '微软雅黑',sans-serif;">暂存区和工作目录一起删除</span>
  </p>
</div>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">命令实践：</span></span>_**

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 创建新文件</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> touch 123</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 未跟踪的文件:</span><span style="color: #008000;">
#</span><span style="color: #008000;">   （使用 "git add &lt;file>..." 以包含要提交的内容）</span><span style="color: #008000;">
#
#</span><span style="color: #008000;">    123</span>
提交为空，但是存在尚未跟踪的文件（使用 <span style="color: #800000;">"</span><span style="color: #800000;">git add</span><span style="color: #800000;">"</span> 建立跟踪）</pre>
</div>

\# <span style="font-family: 等线;">将文件添加到暂存区域</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git add 123</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 要提交的变更：</span><span style="color: #008000;">
#</span><span style="color: #008000;">   （使用 "git reset HEAD &lt;file>..." 撤出暂存区）</span><span style="color: #008000;">
#
#</span><span style="color: #008000;">    新文件：    123</span><span style="color: #008000;"><br /></span></pre>
</div>

#&nbsp; <span style="font-family: 等线;">删除文件</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> rm 123 -f</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> ls</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 要提交的变更：</span><span style="color: #008000;">
#</span><span style="color: #008000;">   （使用 "git reset HEAD &lt;file>..." 撤出暂存区）</span><span style="color: #008000;">
#
#</span><span style="color: #008000;">    新文件：    123</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> 尚未暂存以备提交的变更：</span><span style="color: #008000;">
#</span><span style="color: #008000;">   （使用 "git add/rm &lt;file>..." 更新要提交的内容）</span><span style="color: #008000;">
#</span><span style="color: #008000;">   （使用 "git checkout -- &lt;file>..." 丢弃工作区的改动）</span><span style="color: #008000;">
#
#</span><span style="color: #008000;">    删除：      123</span><span style="color: #008000;">
#</span>&nbsp;</pre>
</div>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git reset HEAD  ./* </span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git status</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 位于分支 master</span>
无文件要提交，干净的工作区</pre>
</div>

### <span id="164">1.6.4 <span style="font-family: '微软雅黑',sans-serif;">重命名暂存区数据</span></span>

<span style="font-family: 等线;">&bull;</span> <span style="font-family: 等线;">没有添加到暂存区的数据直接</span>mv/rename<span style="font-family: 等线;">改名即可。</span>

<span style="font-family: 等线;">&bull;</span> <span style="font-family: 等线;">已经添加到暂存区数据：</span>

<div class="cnblogs_code">
  <pre>git mv README NOTICE</pre>
</div>

### <span id="165">1.6.5 <span style="font-family: '微软雅黑',sans-serif;">查看历史记录</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> git log&nbsp;&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;查看提交历史记录</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> git log -2&nbsp;&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;查看最近几条记录</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> git log -p -1&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;</span>-p<span style="font-family: '微软雅黑',sans-serif;">显示每次提交的内容差异</span>,<span style="font-family: '微软雅黑',sans-serif;">例如仅查看最近一次差异</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> git log --stat -2 #<span style="font-family: '微软雅黑',sans-serif;">&rarr;</span>--stat<span style="font-family: '微软雅黑',sans-serif;">简要显示数据增改行数，这样能够看到提交中修改过的内容，对文件添加或移动的行数，并在最后列出所有增减行的概要信息</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> git log --pretty=oneline #<span style="font-family: '微软雅黑',sans-serif;">&rarr;</span>--pretty<span style="font-family: '微软雅黑',sans-serif;">根据不同的格式展示提交的历史信息</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> git log --pretty=fuller -2 #<span style="font-family: '微软雅黑',sans-serif;">&rarr;以更详细的模式输出提交的历史记录</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">&bull;</span> git log --pretty=fomat:"%h %cn"&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;查看当前所有提交记录的简短</span>SHA-1<span style="font-family: '微软雅黑',sans-serif;">哈希字串与提交着的姓名。</span>
  </p>
</div>

<span style="font-family: 等线;">使用</span>format<span style="font-family: 等线;">参数来指定具体的输出格式</span>

<div align="center">
  <table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
    <tr>
      <td style="width: 141.5pt; border-top: 1pt solid #4472c4; border-bottom: 1pt solid #4472c4; border-left: 1pt solid #4472c4; border-right: none; background: #4472c4; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 宋体; times new roman"4times new roman";times new roman";color: white;">格式</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: 1pt solid #4472c4; border-right: 1pt solid #4472c4; border-bottom: 1pt solid #4472c4; border-left: none; background: #4472c4; padding: 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 宋体; times new roman"4times new roman";times new roman";color: white;">说明</span></strong>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%s</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">提交说明。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%cd</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">提交日期。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%an</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">作者的名字。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%cn</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">提交者的姓名。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%ce</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">提交者的电子邮件。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%H</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">提交对象的完整</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">SHA-1</span><span style="font-size: 10.0pt; font-family: 宋体;">哈希字串。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%h</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">提交对象的简短</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">SHA-1</span><span style="font-size: 10.0pt; font-family: 宋体;">哈希字串。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%T</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">树对象的完整</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">SHA-1</span><span style="font-size: 10.0pt; font-family: 宋体;">哈希字串。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%t</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">树对象的简短</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">SHA-1</span><span style="font-size: 10.0pt; font-family: 宋体;">哈希字串。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%P</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">父对象的完整</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">SHA-1</span><span style="font-size: 10.0pt; font-family: 宋体;">哈希字串。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; background: #d9e2f3; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%p</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; background: #D9E2F3; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">父对象的简短</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">SHA-1</span><span style="font-size: 10.0pt; font-family: 宋体;">哈希字串。</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 141.5pt; border-right: 1pt solid #8eaadb; border-bottom: 1pt solid #8eaadb; border-left: 1pt solid #8eaadb; border-top: none; padding: 0cm 5.4pt;" valign="top" width="189">
        <p style="margin-bottom: .0001pt; text-align: center;" align="center">
          <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif;">%ad</span></strong>
        </p>
      </td>
      
      <td style="width: 290.6pt; border-top: none; border-left: none; border-bottom: solid #8EAADB 1.0pt; border-right: solid #8EAADB 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="387">
        <p style="margin-bottom: .0001pt;">
          <span style="font-size: 10.0pt; font-family: 宋体;">作者的修订时间。</span>
        </p>
      </td>
    </tr>
  </table>
</div>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">命令实践</span></span>_

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git log</span>
<span style="color: #000000;">commit a409fc46f792228a8119705e9cc97c2a013534ab
Author: clsn </span>&lt;admin@znix.top><span style="color: #000000;">
Date:   Wed Nov </span>29 11:44:14 2017 +0800<span style="color: #000000;">

    test

commit bb963eb32ad93a72d9ce93e4bb55105087f1227d
Author: clsn </span>&lt;admin@znix.top><span style="color: #000000;">
Date:   Wed Nov </span>29 10:57:02 2017 +0800<span style="color: #000000;">

    first commit</span></pre>
</div>

### <span id="166">1.6.6 <span style="font-family: '微软雅黑',sans-serif;">还原历史数据</span></span>

<p style="text-indent: 21.0pt;">
  Git<span style="font-family: 等线;">服务程序中有一个叫做</span>HEAD<span style="font-family: 等线;">的版本指针，当用户申请还原数据时，其实就是将</span>HEAD<span style="font-family: 等线;">指针指向到某个特定的提交版本，但是因为</span>Git<span style="font-family: 等线;">是分布式版本控制系统，为了避免历史记录冲突，故使用了</span>SHA-1<span style="font-family: 等线;">计算出十六进制的哈希字串来区分每个提交版本，另外默认的</span>HEAD<span style="font-family: 等线;">版本指针会指向到最近的一次提交版本记录，而上一个提交版本会叫</span>HEAD^<span style="font-family: 等线;">，上上一个版本则会叫做</span>HEAD^^<span style="font-family: 等线;">，当然一般会用</span>HEAD~5<span style="font-family: 等线;">来表示往上数第五个提交版本。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    git reset --hard&nbsp;&nbsp; hash
  </p>
  
  <p>
    git reset --hard HEAD^&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;还原历史提交版本上一次</span>
  </p>
  
  <p>
    git reset --hard 3de15d4 #<span style="font-family: '微软雅黑',sans-serif;">&rarr;找到历史还原点的</span>SHA-1<span style="font-family: '微软雅黑',sans-serif;">值后，就可以还原</span>(<span style="font-family: '微软雅黑',sans-serif;">值不写全</span>,<span style="font-family: '微软雅黑',sans-serif;">系统</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">会自动匹配</span>)
  </p>
</div>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">测试命令</span></span>_

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git log</span>
<span style="color: #000000;">commit a409fc46f792228a8119705e9cc97c2a013534ab
Author: clsn </span>&lt;13835544305@163.com><span style="color: #000000;">
Date:   Wed Nov </span>29 11:44:14 2017 +0800<span style="color: #000000;">

    test

commit bb963eb32ad93a72d9ce93e4bb55105087f1227d
Author: clsn </span>&lt;13835544305@163.com><span style="color: #000000;">
Date:   Wed Nov </span>29 10:57:02 2017 +0800<span style="color: #000000;">

    first commit</span></pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">还原数据</span>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git reset --hard  bb963</span>
<span style="color: #000000;">HEAD 现在位于 bb963eb first commit
</span><span style="color: #008000;">#</span><span style="color: #008000;"> 查看数据</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> ls</span>
README</pre>
</div>

### <span id="167">1.6.7 <span style="font-family: '微软雅黑',sans-serif;">还原未来数据</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">什么是未来数据？就是你还原到历史数据了，但是你后悔了，想撤销更改，但是</span>git log<span style="font-family: 等线;">已经找不到这个版本了。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    git reflog #<span style="font-family: '微软雅黑',sans-serif;">&rarr;查看未来历史更新点</span>
  </p>
</div>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">测试命令</span></span>_

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git reflog</span>
<span style="color: #000000;">bb963eb HEAD@{0}: reset: moving to bb963
a409fc4 HEAD@{</span>1<span style="color: #000000;">}: reset: moving to a409fc4
bb963eb HEAD@{</span>2<span style="color: #000000;">}: reset: moving to bb963
a409fc4 HEAD@{</span>3<span style="color: #000000;">}: commit: test
bb963eb HEAD@{</span>4<span style="color: #000000;">}: commit (initial): first commit
[root@gitlab git_data]</span><span style="color: #008000;">#</span> &nbsp;</pre>
</div>

### <span id="168">1.6.8 <span style="font-family: '微软雅黑',sans-serif;">标签使用</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">前面回滚使用的是一串字符串，又长又难记。</span>
  </p>
  
  <p>
    git tag v1.0&nbsp;&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;当前提交内容打一个标签</span>(<span style="font-family: '微软雅黑',sans-serif;">方便快速回滚</span>)<span style="font-family: '微软雅黑',sans-serif;">，每次提交都可以打个</span>tag<span style="font-family: '微软雅黑',sans-serif;">。</span>
  </p>
  
  <p>
    git tag&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;查看当前所有的标签</span>
  </p>
  
  <p>
    git show v1.0&nbsp;&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;查看当前</span>1.0<span style="font-family: '微软雅黑',sans-serif;">版本的详细信息</span>
  </p>
  
  <p>
    git tag v1.2 -m "version 1.2 release is test"&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;创建带有说明的标签</span>,-a<span style="font-family: '微软雅黑',sans-serif;">指定标签名字，</span>-m<span style="font-family: '微软雅黑',sans-serif;">指定说明文字</span>
  </p>
  
  <p>
    git tag -d v1.0&nbsp;&nbsp; #<span style="font-family: '微软雅黑',sans-serif;">&rarr;我们为同一个提交版本设置了两次标签</span>,<span style="font-family: '微软雅黑',sans-serif;">删除之前的</span>v1.0
  </p>
</div>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">测试命令</span></span>_

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git reset --hard 0bdf2e7</span>
HEAD <span style="color: #0000ff;">is</span><span style="color: #000000;"> now at 0bdf2e7 modified README file
[root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git reset --hard V1.0</span>
HEAD <span style="color: #0000ff;">is</span><span style="color: #000000;"> now at a66370a add test dir

[root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git tag  v20171129</span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git tag </span>
v20171129</pre>
</div>

### <span id="169">1.6.9 <span style="font-family: '微软雅黑',sans-serif;">对比数据</span></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; git diff<span style="font-family: 等线;">可以对比当前文件与仓库已保存文件的区别，知道了对</span>README<span style="font-family: 等线;">作了什么修改</span>

<span style="font-family: 等线;">后，再把它提交到仓库就放⼼多了。</span>

<div class="cnblogs_code">
  <pre>git diff README</pre>
</div>

## <span id="17">1.7 <span style="font-family: '微软雅黑',sans-serif;">分支结构</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">在实际的项目开发中，尽量保证</span>master<span style="font-family: 等线;">分支稳定，仅用于发布新版本，平时不要随便直接修改里面的数据文件。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">那在哪干活呢？干活都在</span>dev<span style="font-family: 等线;">分支上。每个人从</span>dev<span style="font-family: 等线;">分支创建自己个人分支，开发完合并到</span>dev<span style="font-family: 等线;">分支，最后</span>dev<span style="font-family: 等线;">分支合并到</span>master<span style="font-family: 等线;">分支。所以团队的合作分支看起来会像下图那样。</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130171522167-746209354.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

### <span id="171">1.7.1 <span style="font-family: '微软雅黑',sans-serif;">分支切换</span></span>

<div class="cnblogs_code">
  <pre>    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git branch linux</span>
    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git branch </span>
<span style="color: #000000;">      linux
    </span>*<span style="color: #000000;"> master
    [root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git checkout linux </span>
    切换到分支 <span style="color: #800000;">'</span><span style="color: #800000;">linux</span><span style="color: #800000;">'</span><span style="color: #000000;">
    [root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git branch </span>
    *<span style="color: #000000;"> linux
      master</span></pre>
</div>

**<span style="font-family: 等线; background: yellow;">在</span><span style="background: yellow;">linux</span>****<span style="font-family: 等线; background: yellow;">分支进行修改</span>**

<div class="cnblogs_code">
  <pre>    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> cat README </span>
    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> echo "2017年11月30日" >> README </span>
    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git add .</span>
    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git commit -m "2017年11月30日09点10分"</span>
<span style="color: #000000;">    [linux 5a6c037] 2017年11月30日09点10分
     </span>1 file changed, 1 insertion(+<span style="color: #000000;">)
    [root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git status </span>
    <span style="color: #008000;">#</span><span style="color: #008000;"> 位于分支 linux</span>
    无文件要提交，干净的工作区</pre>
</div>

**<span style="font-family: 等线; background: yellow;">回到</span><span style="background: yellow;">master</span>****<span style="font-family: 等线; background: yellow;">分支</span>**

<div class="cnblogs_code">
  <pre>    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git checkout master </span>
    切换到分支 <span style="color: #800000;">'</span><span style="color: #800000;">master</span><span style="color: #800000;">'</span><span style="color: #000000;">
    [root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> cat README </span>
    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git log  -1</span>
<span style="color: #000000;">    commit 7015bc7b316cc95e2dfe6c53e06e3900b2edf427
    Author: clsn </span>&lt;admin@znix.top><span style="color: #000000;">
    Date:   Wed Nov </span>29 19:30:57 2017 +0800

        123</pre>
</div>

**<span style="font-family: 等线; background: yellow;">合并代码</span>**

<div class="cnblogs_code">
  <pre>    [root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git merge linux </span>
<span style="color: #000000;">    更新 7015bc7..5a6c037
    Fast</span>-<span style="color: #000000;">forward
     README </span>| 1 +
     1 file changed, 1 insertion(+<span style="color: #000000;">)
    [root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git status </span>
    <span style="color: #008000;">#</span><span style="color: #008000;"> 位于分支 master</span>
<span style="color: #000000;">    无文件要提交，干净的工作区
    [root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> cat README </span>
    2017年11月30日</pre>
</div>

### <span id="172">1.7.2 <span style="font-family: '微软雅黑',sans-serif;">合并失败解决</span></span>

_<span style="text-decoration: underline;"><span style="font-family: 等线;">模拟冲突，在文件的同一行做不同修改</span></span>_

**<span style="font-family: 等线; background: yellow;">在</span><span style="background: yellow;">master </span>****<span style="font-family: 等线; background: yellow;">分支进行修改</span>**

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> cat README </span>
<span style="color: #000000;">2017年11月30日
[root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> echo  "clsn in master">> README </span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git commit -a -m "clsn 2017年11月30日 09点20分 "</span>
<span style="color: #000000;">[master 7ab71d4] clsn 2017年11月30日 09点20分
 </span>1 file changed, 1 insertion(+)</pre>
</div>

**<span style="font-family: 等线; background: yellow;">切换到</span><span style="background: yellow;">linux</span>****<span style="font-family: 等线; background: yellow;">分支</span>**

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git checkout linux </span>
切换到分支 <span style="color: #800000;">'</span><span style="color: #800000;">linux</span><span style="color: #800000;">'</span><span style="color: #000000;">
[root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> cat README </span>
<span style="color: #000000;">2017年11月30日
[root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> echo "clsn in linux" >> README </span>
[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git commit -a -m "2017年11月30日 03"</span>
[linux 20f1a13] 2017年11月30日 03
 1 file changed, 1 insertion(+)</pre>
</div>

**<span style="font-family: 等线; background: yellow;">回到</span><span style="background: yellow;">master</span>****<span style="font-family: 等线; background: yellow;">分区，进行合并，出现冲突</span>**

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git checkout master </span>
切换到分支 <span style="color: #800000;">'</span><span style="color: #800000;">master</span><span style="color: #800000;">'</span><span style="color: #000000;">
[root@gitlab git_data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git merge linux</span>
<span style="color: #000000;">自动合并 README
冲突（内容）：合并冲突于 README
自动合并失败，修正冲突然后提交修正的结果。</span></pre>
</div>

**<span style="font-family: 等线; background: yellow;">解决冲突</span>**

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> vim README </span>
<span style="color: #000000;">2017年11月30日
clsn </span><span style="color: #0000ff;">in</span><span style="color: #000000;"> master
clsn </span><span style="color: #0000ff;">in</span> linux</pre>
</div>

<p style="text-indent: 15.75pt;">
  <em><span style="text-decoration: underline;"># </span></em><em><span style="text-decoration: underline;"><span style="font-family: 等线;">手工解决冲突</span></span></em>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab git_data]<span style="color: #008000;">#</span><span style="color: #008000;"> git commit -a -m "2017年11月30日 03"</span>
[master b6a097f] 2017年11月30日 03</pre>
</div>

### <span id="173">1.7.3 <span style="font-family: '微软雅黑',sans-serif;">删除分支</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">因为之前已经合并了</span>linux<span style="font-family: 等线;">分支，所以现在看到它在列表中。</span> <span style="font-family: 等线;">在这个列表中分支名字前没有</span> * <span style="font-family: 等线;">号的分支通常可以使用</span> git branch -d <span style="font-family: 等线;">删除掉；你已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">查看所有包含未合并工作的分支，可以运行</span><em><span style="text-decoration: underline;"> git branch --no-merged</span></em><em><span style="text-decoration: underline;"><span style="font-family: 等线;">：</span></span></em>
</p>

<div class="cnblogs_code">
  <pre>git branch --no-<span style="color: #000000;">merged
  testing</span></pre>
</div>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">这里显示了其他分支。</span> <span style="font-family: 等线;">因为它包含了还未合并的工作，尝试使用</span> git branch -d <span style="font-family: 等线;">命令删除它时会失败：</span>
</p>

<div class="cnblogs_code">
  <pre>git branch -<span style="color: #000000;">d testing
error: The branch </span><span style="color: #800000;">'</span><span style="color: #800000;">testing</span><span style="color: #800000;">'</span> <span style="color: #0000ff;">is</span> <span style="color: #0000ff;">not</span><span style="color: #000000;"> fully merged.
If you are sure you want to delete it, run </span><span style="color: #800000;">'</span><span style="color: #800000;">git branch -D testing</span><span style="color: #800000;">'</span>.<span style="text-indent: 21pt;">&nbsp;</span></pre>
</div>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用</span> <strong><span style="background: yellow;">-D</span></strong> <span style="font-family: 等线;">选项强制删除它。</span>
</p>

## <span id="18_windwosGit">1.8 windwos<span style="font-family: '微软雅黑',sans-serif;">上</span>Git<span style="font-family: '微软雅黑',sans-serif;">的使用</span></span>

<p style="text-indent: 21.0pt;">
  windows <span style="font-family: 等线;">上</span>git<span style="font-family: 等线;">软件网站</span> <em><span style="text-decoration: underline;">https://git-for-windows.github.io</span></em>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    &nbsp;&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">软件下载地址：</span>https://github.com/git-for-windows/git/releases/download/v2.15.1.windows.2/Git-2.15.1.2-64-bit.exe
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">软件安装默认即可。</span>

### <span id="181">1.8.1 <span style="font-family: '微软雅黑',sans-serif;">软件使用</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 等线;">创建新的仓库</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130171826511-65282446.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">定义仓库的路径</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130171836073-38741033.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">添加用户信息（在</span> git bash<span style="font-family: 等线;">中）</span>

<div class="cnblogs_code">
  <pre>default@Hzs-Desktop MINGW64 /i/<span style="color: #000000;">git_data (master)
$ git config  </span>--<span style="color: #0000ff;">global</span> user.email <span style="color: #800000;">"</span><span style="color: #800000;">admin@znix.top</span><span style="color: #800000;">"</span><span style="color: #000000;">

default@Hzs</span>-Desktop MINGW64 /i/<span style="color: #000000;">git_data (master)
$ git config  </span>--<span style="color: #0000ff;">global</span> user.name <span style="color: #800000;">"</span><span style="color: #800000;">clsn</span><span style="color: #800000;">"</span></pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">在</span>git Gui <span style="font-family: 等线;">中添加用户信息，添加一次就可</span>

<p style="margin-bottom: .0001pt; text-align: center; text-autospace: none;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130171858448-43416173.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">在页面中将数据配置好即可使用</span>

<p style="margin-bottom: .0001pt; text-align: center; text-autospace: none;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130171907558-1392348319.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">查看历史数据</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130171916026-822562163.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

## <span id="19_gitlab"><span style="courier new"4courier new";background: lime;">1.9 </span><span style="background: lime;">gitlab</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">的使用</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">前面我们已经知道</span>Git<span style="font-family: 等线;">人人都是中心，那他们怎么交互数据呢？</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: 等线;">&bull;</span> <span style="font-family: 等线;">使用</span><strong><em>GitHub</em></strong><span style="font-family: 等线;">或者<strong><em>码云</em></strong>等公共代码仓库</span>
</p>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: 等线;">&bull;</span> <span style="font-family: 等线;">使用</span>GitLab<span style="font-family: 等线;">私有仓库</span>
</p>

### <span id="191_gitlab">1.9.1 <span style="font-family: '微软雅黑',sans-serif;">安装配置</span>gitlab</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">官方安装文档</span> &nbsp;&nbsp;https://about.gitlab.com/installation/
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">国内软件镜像站</span> https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/
  </p>
</div>

<p style="text-indent: 15.75pt;">
  <strong><em><span style="font-family: 等线; background: yellow;">安装</span></em></strong>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum localinstall gitlab-ce-9.1.4-ce.0.el7.x86_64.rpm</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: 等线;">初始化</span>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> gitlab-ctl reconfigure</span></pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: 等线;">状态</span>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;">  gitlab-ctl status</span>
run: gitaly: (pid 4491) 48s; run: log: (pid 4087<span style="color: #000000;">) 279s
run: gitlab</span>-monitor: (pid 4539) 44s; run: log: (pid 4251<span style="color: #000000;">) 207s
run: gitlab</span>-workhorse: (pid 4501) 47s; run: log: (pid 4099<span style="color: #000000;">) 273s
run: logrotate: (pid </span>4125) 265s; run: log: (pid 4124<span style="color: #000000;">) 265s
run: nginx: (pid </span>4112) 271s; run: log: (pid 4111<span style="color: #000000;">) 271s
run: node</span>-exporter: (pid 4175) 243s; run: log: (pid 4174<span style="color: #000000;">) 243s
run: postgres</span>-exporter: (pid 4528) 45s; run: log: (pid 4223<span style="color: #000000;">) 219s
run: postgresql: (pid </span>3933) 343s; run: log: (pid 3932<span style="color: #000000;">) 343s
run: prometheus: (pid </span>4514) 46s; run: log: (pid 4156<span style="color: #000000;">) 259s
run: redis: (pid </span>3876) 355s; run: log: (pid 3875<span style="color: #000000;">) 355s
run: redis</span>-exporter: (pid 4186) 237s; run: log: (pid 4185<span style="color: #000000;">) 237s
run: sidekiq: (pid </span>4078) 281s; run: log: (pid 4077<span style="color: #000000;">) 281s
run: unicorn: (pid </span>4047) 287s; run: log: (pid 4046) 287s</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: 等线;">检查端口</span>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> netstat -lntup|grep 80</span>
tcp        0      0 127.0.0.1:8080      0.0.0.0:*    LISTEN     4073/<span style="color: #000000;">unicorn master 
tcp        0      0 </span>0.0.0.0:80      0.0.0.0:*         LISTEN      4112/<span style="color: #000000;">nginx: master  
tcp        0      0 </span>0.0.0.0:8060       0.0.0.0:*      LISTEN      4112/nginx: master  </pre>
</div>

### <span id="192_web">1.9.2 <span style="font-family: '微软雅黑',sans-serif;">使用浏览器访问，进行</span>web<span style="font-family: '微软雅黑',sans-serif;">界面操作</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 等线;">第一次访问，创建密码</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172003370-795962407.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">进行登陆，用户名为</span>root<span style="font-family: 等线;">，密码为</span>12345678

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172009104-1047161099.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">创建一个新的项目</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172017526-15521339.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">定义项目的名称</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172024214-328349596.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">创建完成后会提示没有添加</span>ssh<span style="font-family: 等线;">密钥</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172030761-29430481.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">在服务器上创建</span>ssh<span style="font-family: 等线;">密钥</span> <span style="font-family: 等线;">使用</span>ssh-ketgen <span style="font-family: 等线;">命令</span>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ssh-keygen </span>
Generating public/<span style="color: #000000;">private rsa key pair.
Enter file </span><span style="color: #0000ff;">in</span> which to save the key (/root/.ssh/<span style="color: #000000;">id_rsa): 
Created directory </span><span style="color: #800000;">'</span><span style="color: #800000;">/root/.ssh</span><span style="color: #800000;">'</span><span style="color: #000000;">.
Enter passphrase (empty </span><span style="color: #0000ff;">for</span><span style="color: #000000;"> no passphrase): 
Enter same passphrase again: 
Your identification has been saved </span><span style="color: #0000ff;">in</span> /root/.ssh/<span style="color: #000000;">id_rsa.
Your public key has been saved </span><span style="color: #0000ff;">in</span> /root/.ssh/<span style="color: #000000;">id_rsa.pub.
The key fingerprint </span><span style="color: #0000ff;">is</span><span style="color: #000000;">:
SHA256:n</span>/<span style="color: #000000;">V2kCiwwm2UfBsnQLm17eXUCBiBByyPbefmz5oQvfU root@gitlab
The key</span><span style="color: #800000;">'</span><span style="color: #800000;">s randomart image is:</span>
+---[RSA 2048]----+
|       o++o+     |
|      ..+o+ .    |
|       ==++o.. o |
|     ..o==o=..+..|
|      o.So+.++o  |
|       o oo*.o.. |
|        .o+   E .|
|         ..o . . |
|          ooo    |
+----[SHA256]-----+</pre>
</div>

<div class="cnblogs_code">
  <pre>[root@gitlab .ssh]<span style="color: #008000;">#</span><span style="color: #008000;"> cat id_rsa.pub </span>
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDSVdBypha/ALMmvIiZGXxYGz7FJ5TC+hYWo7QGBJ+J6JVinp9yH851fwxln5TWGBrtEousoVHXTTJfFRy8LV+Ho7OfaksYt+5TPxEjf5XX53Z3ZX70PYH3DQFmgzl0QpWw1PYIjrD7kBeLhUg+R/ZePS+HzPvbRCb6gOlkdx46vX4Olr7YbAO5lzAarhaZcE2Q702kPXGeuZbR7KcwVhtoiueyHwyj94bccMfKq7qSskXGbpWuCwcaKQ6uqGap1rP5Viqqv0xeO7Vq0dIZ/YnPL2vPDUvNa36nHosiZGkn4thpPh63KjXaFIfKOuPemLzvDZY0A+88P8gwmAYiPoxp root@gitlab</pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">将密钥添加到</span>web<span style="font-family: 等线;">界面的用户中</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172111261-1864984064.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

**_<span style="text-decoration: underline;"><span style="background: yellow;">gitlab</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">自带的命令集</span></span>_**

<div class="cnblogs_code">
  <pre><span style="color: #000000;">Command line instructions
<span style="text-decoration: underline;"><em><strong>Git </strong></em></span></span><span style="text-decoration: underline;"><em><strong><span style="color: #0000ff; text-decoration: underline;">global</span></strong></em></span><span style="color: #000000;"><span style="text-decoration: underline;"><em><strong> setup</strong></em></span>
git config </span>--<span style="color: #0000ff;">global</span> user.name <span style="color: #800000;">"</span><span style="color: #800000;">Administrator</span><span style="color: #800000;">"</span><span style="color: #000000;">
git config </span>--<span style="color: #0000ff;">global</span> user.email <span style="color: #800000;">"</span><span style="color: #800000;">admin@example.com</span><span style="color: #800000;">"</span><span style="color: #000000;"><em><span style="text-decoration: underline;"><strong>
Create a new repository</strong></span></em>
git clone git@gitlab.example.com:root</span>/<span style="color: #000000;">clsn.git
cd clsn
touch README.md
git add README.md
git commit </span>-m <span style="color: #800000;">"</span><span style="color: #800000;">add README</span><span style="color: #800000;">"</span><span style="color: #000000;">
git push </span>-<span style="color: #000000;">u origin master
<em><span style="text-decoration: underline;"><strong>Existing folder</strong></span></em>
cd existing_folder
git init
git remote add origin git@gitlab.example.com:root</span>/<span style="color: #000000;">clsn.git
git add .
git commit </span>-m <span style="color: #800000;">"</span><span style="color: #800000;">Initial commit</span><span style="color: #800000;">"</span><span style="color: #000000;">
git push </span>-<span style="color: #000000;">u origin master
<em><span style="text-decoration: underline;"><strong>Existing Git repository</strong></span></em>
cd existing_repo
git remote rename origin old</span>-<span style="color: #000000;">origin
git remote add origin git@gitlab.example.com:root</span>/<span style="color: #000000;">clsn.git
git push </span>-u origin --<span style="color: #000000;">all
git push </span>-u origin --tags</pre>
</div>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">创建行的</span><span style="background: yellow;">git</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">仓库</span></span>_**

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> git clone git@gitlab:root/Test1.git</span>
正克隆到 <span style="color: #800000;">'</span><span style="color: #800000;">Test1</span><span style="color: #800000;">'</span><span style="color: #000000;">...
The authenticity of host </span><span style="color: #800000;">'</span><span style="color: #800000;">gitlab (10.0.0.63)</span><span style="color: #800000;">'</span> can<span style="color: #800000;">'</span><span style="color: #800000;">t be established.</span>
ECDSA key fingerprint <span style="color: #0000ff;">is</span> SHA256:yOrzs0W+R//s8VDEN9nko6r6wW+<span style="color: #000000;">8gwJl3Ut7ac0i5SY.
ECDSA key fingerprint </span><span style="color: #0000ff;">is</span> MD5:21:33:dd:4d:01:00:eb:71:a4:4e:2d:2b:bf:37:48<span style="color: #000000;">:ed.
Are you sure you want to </span><span style="color: #0000ff;">continue</span> connecting (yes/<span style="color: #000000;">no)? yes
Warning: Permanently added </span><span style="color: #800000;">'</span><span style="color: #800000;">gitlab</span><span style="color: #800000;">'</span><span style="color: #000000;"> (ECDSA) to the list of known hosts.
warning: 您似乎克隆了一个空版本库。</span></pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">创建文件并推到远端</span>git<span style="font-family: 等线;">仓库</span>

<div class="cnblogs_code">
  <pre>[root@gitlab Test1]<span style="color: #008000;">#</span><span style="color: #008000;"> echo "clsn" >> clsn</span>
[root@gitlab Test1]<span style="color: #008000;">#</span><span style="color: #008000;"> git push -u origin master</span>
<span style="color: #000000;">分支 master 设置为跟踪来自 origin 的远程分支 master。
Everything up</span>-to-date</pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线; background: yellow;">推送完成后能够在</span><span style="background: yellow;">web</span><span style="font-family: 等线; background: yellow;">界面中查看</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172239792-1540256709.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    # md <span style="font-family: '微软雅黑',sans-serif;">语法的使用方法</span>
  </p>
  
  <p>
    <a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.zyops.com/markdown-syntax" >http://www.zyops.com/markdown-syntax</a>
  </p>
</div>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">至此</span><span style="background: yellow;">gitlab</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">的使用结束了</span> </span>_**

## <span id="110_GitHub">1.10 GitHub<span style="font-family: '微软雅黑',sans-serif;">托管服务</span></span>

<p style="text-indent: 21.0pt;">
  Github<span style="font-family: 等线;">顾名思义是一个</span>Git<span style="font-family: 等线;">版本库的托管服务，是目前全球最大的软件仓库，拥有上百万的开发者用户，也是软件开发和寻找资源的最佳途径，</span>Github<span style="font-family: 等线;">不仅可以托管各种</span>Git<span style="font-family: 等线;">版本仓库，还拥有了更美观的</span>Web<span style="font-family: 等线;">界面，您的代码文件可以被任何人克隆，使得开发者为开源项贡献代码变得更加容易，当然也可以付费购买私有库，这样高性价比的私有库真的是帮助到了很多团队和企业。</span>
</p>

### <span id="1101_GitHub">1.10.1 <span style="font-family: '微软雅黑',sans-serif;">注册</span>GitHub</span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 等线;">浏览器访问</span>github<span style="font-family: 等线;">官网</span> <span style="font-family: 等线;">：</span> <a href="https://github.com/">https://github.com/</a> <span style="font-family: 等线;">，点击</span><strong>Sign up </strong><span style="font-family: 等线;">进行注册</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172252558-286760021.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">填写个人信息，进行注册</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172258245-1338891185.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">选择仓库类型</span>,<span style="font-family: 等线;">默认免费</span>,<span style="font-family: 等线;">点击底下</span>Continue<span style="font-family: 等线;">注册</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172304542-1447880412.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">描述一下你自己，当然，这一步可以跳过</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172317448-490179651.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">用户创建完成，可以创建新的项目</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172323761-1590278362.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

<span style="font-family: 等线;">注意：创建新的项目之前要现验证邮箱</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172329339-889835729.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

### <span id="1102">1.10.2 <span style="font-family: '微软雅黑',sans-serif;">添加密钥</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 等线;">在</span>github<span style="font-family: 等线;">上添加一个新的</span>ssh<span style="font-family: 等线;">密钥</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172539964-1266551504.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />&nbsp;
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">获取主机（</span>linux<span style="font-family: 等线;">）上的密钥</span>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ssh-keygen </span>
Generating public/<span style="color: #000000;">private rsa key pair.
Enter file </span><span style="color: #0000ff;">in</span> which to save the key (/root/.ssh/<span style="color: #000000;">id_rsa): 
Created directory </span><span style="color: #800000;">'</span><span style="color: #800000;">/root/.ssh</span><span style="color: #800000;">'</span><span style="color: #000000;">.
Enter passphrase (empty </span><span style="color: #0000ff;">for</span><span style="color: #000000;"> no passphrase): 
Enter same passphrase again: 
Your identification has been saved </span><span style="color: #0000ff;">in</span> /root/.ssh/<span style="color: #000000;">id_rsa.
Your public key has been saved </span><span style="color: #0000ff;">in</span> /root/.ssh/<span style="color: #000000;">id_rsa.pub.
The key fingerprint </span><span style="color: #0000ff;">is</span><span style="color: #000000;">:
SHA256:n</span>/<span style="color: #000000;">V2kCiwwm2UfBsnQLm17eXUCBiBByyPbefmz5oQvfU root@gitlab
The key</span><span style="color: #800000;">'</span><span style="color: #800000;">s randomart image is:</span>
+---[RSA 2048]----+
|       o++o+     |
|      ..+o+ .    |
|       ==++o.. o |
|     ..o==o=..+..|
|      o.So+.++o  |
|       o oo*.o.. |
|        .o+   E .|
|         ..o . . |
|          ooo    |
+----[SHA256]-----+</pre>
</div>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat .ssh/id_rsa.pub </span>
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCmv4aEEEpbUyzv1r6SN0JqOfeyQ7sZZbXxWFv4xflIJeK/rl8cF7UYCzjLEvwJlrkIjKSs5uW1x0zWEcZFiv5tGCiO7DeMR6pKUAn7NzNjKiCcElCXiqHVew84iTbxX4MWKlbFoJYO9/wQ1NlrQfqcSgZwJTLKBMVoMXvTWPPGXf6AwdSp68guFwwGDIV8BiHZiy61bKiWYSVKSDP47Y7VUV/bdwGaxG7tAfalWVpe6xXXRtsj58sENyIWbRI7/9XWqs+eV+CgI74YjOanMvHnHFlfg0tb+MewRb4tFGVmroFBRsvfI3Sl2fez2zHG0qh3f34/0KF1kitlWkgcBJqN root@gitlab</pre>
</div>

windwos<span style="font-family: 等线;">上获得密钥的方法（需要安装</span>git for windows<span style="font-family: 等线;">）</span>

<div class="cnblogs_code">
  <pre>default@Hzs-Desktop MINGW64 /i/<span style="color: #000000;">Desktop
$ ssh</span>-<span style="color: #000000;">keygen.exe
Generating public</span>/<span style="color: #000000;">private rsa key pair.
Enter file </span><span style="color: #0000ff;">in</span> which to save the key (/c/Users/default.DESKTOP-U9D5JP4/.ssh/<span style="color: #000000;">id_rs                                                                                                                                  a):
Created directory </span><span style="color: #800000;">'</span><span style="color: #800000;">/c/Users/default.DESKTOP-U9D5JP4/.ssh</span><span style="color: #800000;">'</span><span style="color: #000000;">.
Enter passphrase (empty </span><span style="color: #0000ff;">for</span><span style="color: #000000;"> no passphrase):
Enter same passphrase again:
Your identification has been saved </span><span style="color: #0000ff;">in</span> /c/Users/default.DESKTOP-U9D5JP4/.ssh/<span style="color: #000000;">id_r                                                                                                                                  sa.
Your public key has been saved </span><span style="color: #0000ff;">in</span> /c/Users/default.DESKTOP-U9D5JP4/.ssh/<span style="color: #000000;">id_rsa.p                                                                                                                                  ub.
The key fingerprint </span><span style="color: #0000ff;">is</span><span style="color: #000000;">:
SHA256:aqnHq</span>/xNn159jBX4o2L2ZJdtiwu4ietvKRT2fL9igZo default@Hzs-<span style="color: #000000;">Desktop
The key</span><span style="color: #800000;">'</span><span style="color: #800000;">s randomart image is:</span>
+---[RSA 2048]----+
|                 |
|              .  |
|             . . |
|       o      . .|
|      . S .    o.|
|       + +.o ..++|
|     .= +.o=++oo=|
|   . ooE.+==*.oo.|
|    +++=*== .=o. |
+----[SHA256]-----+<span style="color: #000000;">

default@Hzs</span>-Desktop MINGW64 /i/<span style="color: #000000;">Desktop
$

default@Hzs</span>-Desktop MINGW64 /i/<span style="color: #000000;">Desktop
$ cat </span>/c/Users/default.DESKTOP-U9D5JP4/.ssh/<span style="color: #000000;">id_rsa.pub
ssh</span>-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC15+1oQBzvgzQP3p0Lb9FsTKFfIIws9WxGBVl2B9d2                                                                                                                                  qT2eKFgXoEDNCF/OrhhXvbMDhORxXHf9RG0Aqj+/vJddbaQpCawHhP6VUG1X885xhY4OohDOkFQiWD1s                                                                                                                                  DCMkX7OHNW5ake6P8AdNwI6eSpKYKYCxRMGkRiBa1KDRtG8CvsG8VN0iTSW0UZ3s4Ps+S31pBYlNjOMv                                                                                                                                  Lp0HRAMVhYimLLi0Wz2mBffPOeNjPX1FfJdr+hO7TIRNdyAEGIhSbckkAnVEIASAhI0Re/19v1RnSkk2                                                                                                                                  VtBvc5rVeGxFMNuEIl9WDMSTcedhEGXyRlW2N9TtXlvF1eNflzUg2BtCaCFZ default@Hzs-Desktop</pre>
</div>

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172336636-985818438.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" /> 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">密钥创建完成后进行添加</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172742761-924253187.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">密钥添加成功</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172749558-1088172890.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;

### <span id="1103">1.10.3 <span style="font-family: '微软雅黑',sans-serif;">创建仓库</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 等线;">准备工作已经完毕</span>,<span style="font-family: 等线;">右上角点击创建一个新的仓库</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172756151-1248422868.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">创建仓库，输入个人信息</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172806573-563002521.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">根据上面的提示，创建一个代码仓库</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172813573-635220233.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">在个人主机上进行推送测试</span>

<div class="cnblogs_code">
  <pre>[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir -p clsn </span>
[root@gitlab ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cd clsn/</span>
[root@gitlab clsn]<span style="color: #008000;">#</span><span style="color: #008000;"> echo "# test" >> README.md</span>
[root@gitlab clsn]<span style="color: #008000;">#</span><span style="color: #008000;"> git init</span>
初始化空的 Git 版本库于 /root/clsn/.git/<span style="color: #000000;">
[root@gitlab clsn]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git add README.md</span>
[root@gitlab clsn]<span style="color: #008000;">#</span><span style="color: #008000;"> git commit -m "first commit"</span>
<span style="color: #000000;">[master（根提交） 089ae47] first commit
 </span>1 file changed, 1 insertion(+<span style="color: #000000;">)
 create mode </span>100644<span style="color: #000000;"> README.md
[root@gitlab clsn]</span><span style="color: #008000;">#</span><span style="color: #008000;"> git remote add origin git@github.com:clsn-git/test.git</span>
[root@gitlab clsn]<span style="color: #008000;">#</span><span style="color: #008000;"> git push -u origin master</span>
The authenticity of host <span style="color: #800000;">'</span><span style="color: #800000;">github.com (192.30.255.113)</span><span style="color: #800000;">'</span> can<span style="color: #800000;">'</span><span style="color: #800000;">t be established.</span>
RSA key fingerprint <span style="color: #0000ff;">is</span><span style="color: #000000;"> SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint </span><span style="color: #0000ff;">is</span> MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48<span style="color: #000000;">.
Are you sure you want to </span><span style="color: #0000ff;">continue</span> connecting (yes/<span style="color: #000000;">no)? yes
Warning: Permanently added </span><span style="color: #800000;">'</span><span style="color: #800000;">github.com,192.30.255.113</span><span style="color: #800000;">'</span><span style="color: #000000;"> (RSA) to the list of known hosts.
Counting objects: </span>3<span style="color: #000000;">, done.
Writing objects: </span>100% (3/3), 212 bytes | 0 bytes/<span style="color: #000000;">s, done.
Total </span>3<span style="color: #000000;"> (delta 0), reused 0 (delta 0)
To git@github.com:clsn</span>-git/<span style="color: #000000;">test.git
 </span>* [new branch]      master -><span style="color: #000000;"> master
分支 master 设置为跟踪来自 origin 的远程分支 master。</span></pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">推送完成，刷新界面就可以发现，推送上去的</span>README.md<span style="font-family: 等线;">文件</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172835651-1162220378.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">创建新文件，进行拉取测试</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172843276-848969039.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">创建好后点击下面的</span>commit<span style="font-family: 等线;">即可</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172849808-1420080040.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; github<span style="font-family: 等线;">添加成功，进行拉取测试</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172855542-1363801232.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;

### <span id="1104">1.10.4 <span style="font-family: '微软雅黑',sans-serif;">拉取文件测试</span></span>

<p style="margin-left: 7.1pt; text-indent: 8.65pt;">
  <span style="font-family: 等线;">查看目录内容</span>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab clsn]<span style="color: #008000;">#</span><span style="color: #008000;"> ls</span>
README.md</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: 等线;">进行拉取</span>
</p>

<div class="cnblogs_code">
  <pre>[root@gitlab clsn]<span style="color: #008000;">#</span><span style="color: #008000;"> git pull</span>
remote: Counting objects: 3<span style="color: #000000;">, done.
remote: Compressing objects: </span>100% (3/3<span style="color: #000000;">), done.
Unpacking objects: </span>100% (3/3<span style="color: #000000;">), done.
remote: Total </span>3 (delta 0), reused 0 (delta 0), pack-<span style="color: #000000;">reused 0
来自 github.com:clsn</span>-git/<span style="color: #000000;">test
   089ae47..a16be65  master     </span>-> origin/<span style="color: #000000;">master
更新 089ae47..a16be65
Fast</span>-<span style="color: #000000;">forward
 clsn.txt </span>| 3 +++
 1 file changed, 3 insertions(+<span style="color: #000000;">)
 create mode </span>100644 clsn.txt<span style="text-indent: 15.75pt;">&nbsp;</span></pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _<span style="font-family: 等线;">检查文件</span>_

<div class="cnblogs_code">
  <pre>[root@gitlab clsn]<span style="color: #008000;">#</span><span style="color: #008000;"> ls</span>
<span style="color: #000000;">clsn.txt  README.md
[root@gitlab clsn]</span><span style="color: #008000;">#</span><span style="color: #008000;"> cat clsn.txt </span><span style="color: #008000;">
#</span><span style="color: #008000;"> 这是惨绿少年的文档</span><span style="color: #008000;">
#</span><span style="color: #008000;"> clsn blog  : http://blog.znix.top</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 惨绿少年的博客为 ： http://blog.znix.top</span></pre>
</div>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">至此</span><span style="background: yellow;">github</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">的使用就介绍完了</span></span>_**

## <span id="111_JetBrains_PyCharm_github">1.11 JetBrains PyCharm <span style="font-family: '微软雅黑',sans-serif;">使用</span>github</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    PyCharm <span style="font-family: '微软雅黑',sans-serif;">下载：</span> <a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.jetbrains.com/pycharm/download/#section=windows" >http://www.jetbrains.com/pycharm/download/#section=windows</a>
  </p>
</div>

### <span id="1111_PyCharm_github">1.11.1 PyCharm <span style="font-family: '微软雅黑',sans-serif;">上</span>github<span style="font-family: '微软雅黑',sans-serif;">设置</span></span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172928558-525807597.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">选择</span>github<span style="font-family: 等线;">进行连接</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172936620-209187871.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">用户密码准确后会生产</span>token

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172942854-609274746.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">然后点击</span>ok<span style="font-family: 等线;">即可</span>

### <span id="1112">1.11.2 <span style="font-family: '微软雅黑',sans-serif;">推送代码</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 等线;">现确保有之前安装的</span>windwos<span style="font-family: 等线;">端</span>git<span style="font-family: 等线;">，测试路径</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172949995-846642301.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">共享代码</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130172956479-949503333.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">输入信息，进行共享</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130173003089-706424074.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">代码发布成功</span> https://github.com/clsn-git/test1

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130173009104-332127144.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">至此</span><span style="background: yellow;">pycharm</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">使用</span><span style="background: yellow;">github</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">就完成了</span></span>_**

## <span id="112_pycharmgitlab">1.12 pycharm<span style="font-family: '微软雅黑',sans-serif;">使用</span>gitlab</span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 等线;">使用</span>pycharm<span style="font-family: 等线;">是的</span>vcs<span style="font-family: 等线;">，现在</span>git
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130173017073-1107328223.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">输入</span>gitlab<span style="font-family: 等线;">地址</span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  <span style="font-family: 等线;">然后输入用户名及密码</span>
</p>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130173024261-1849548232.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">添加一些注释信息</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130173031183-49085239.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 等线;">在</span>gitlab<span style="font-family: 等线;">的界面中就能查看到长传的代码</span>

<p style="text-align: center;" align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171130173037511-380711709.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Git详解及 github与gitlab使用" alt="" />
</p>

**_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">至此</span><span style="background: yellow;">pycharm</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">使用</span><span style="background: yellow;">gitlab</span></span>_****_<span style="text-decoration: underline;"><span style="font-family: 等线; background: yellow;">就结束了</span></span>_**

## <span id="113-2">1.13 <span style="font-family: '微软雅黑',sans-serif;">参考文档</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">猴子都能懂的</span>GIT <a href="https://backlog.com/git-tutorial/cn/">https://backlog.com/git-tutorial/cn/</a>
  </p>
  
  <p>
    Pro Git<span style="font-family: '微软雅黑',sans-serif;">书籍</span> &nbsp;&nbsp;&nbsp;https://git-scm.com/book/zh/v2
  </p>
</div>

<p style="text-align: right;">
  &nbsp;<strong>&nbsp;本文出自&ldquo;惨绿少年&rdquo;，欢迎转载，转载请注明出处！http://blog.znix.top</strong>
</p>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11">1.1 关于版本控制</a><ul>
        <li>
          <a href="#111">1.1.1 本地版本控制</a>
        </li>
        <li>
          <a href="#112">1.1.2 集中化的版本控制系统</a>
        </li>
        <li>
          <a href="#113">1.1.3 分布式版本控制系统</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_Git">1.2 Git简介</a><ul>
        <li>
          <a href="#121_Git">1.2.1 Git历史</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_git">1.3 安装git</a><ul>
        <li>
          <a href="#131">1.3.1 环境说明</a>
        </li>
        <li>
          <a href="#132_YumGit">1.3.2 Yum安装Git</a>
        </li>
        <li>
          <a href="#133">1.3.3 编译安装</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14_Git">1.4 初次运行 Git 前的配置</a><ul>
        <li>
          <a href="#141_git">1.4.1 配置git</a>
        </li>
        <li>
          <a href="#142">1.4.2 获取帮助</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15_Git">1.5 获取 Git 仓库（初始化仓库）</a>
    </li>
    <li>
      <a href="#16_Git">1.6 Git命令常规操作</a><ul>
        <li>
          <a href="#161">1.6.1 创建文件</a>
        </li>
        <li>
          <a href="#162">1.6.2 添加新文件</a>
        </li>
        <li>
          <a href="#163_git">1.6.3 删除git内的文件</a>
        </li>
        <li>
          <a href="#164">1.6.4 重命名暂存区数据</a>
        </li>
        <li>
          <a href="#165">1.6.5 查看历史记录</a>
        </li>
        <li>
          <a href="#166">1.6.6 还原历史数据</a>
        </li>
        <li>
          <a href="#167">1.6.7 还原未来数据</a>
        </li>
        <li>
          <a href="#168">1.6.8 标签使用</a>
        </li>
        <li>
          <a href="#169">1.6.9 对比数据</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#17">1.7 分支结构</a><ul>
        <li>
          <a href="#171">1.7.1 分支切换</a>
        </li>
        <li>
          <a href="#172">1.7.2 合并失败解决</a>
        </li>
        <li>
          <a href="#173">1.7.3 删除分支</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#18_windwosGit">1.8 windwos上Git的使用</a><ul>
        <li>
          <a href="#181">1.8.1 软件使用</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#19_gitlab">1.9 gitlab的使用</a><ul>
        <li>
          <a href="#191_gitlab">1.9.1 安装配置gitlab</a>
        </li>
        <li>
          <a href="#192_web">1.9.2 使用浏览器访问，进行web界面操作</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#110_GitHub">1.10 GitHub托管服务</a><ul>
        <li>
          <a href="#1101_GitHub">1.10.1 注册GitHub</a>
        </li>
        <li>
          <a href="#1102">1.10.2 添加密钥</a>
        </li>
        <li>
          <a href="#1103">1.10.3 创建仓库</a>
        </li>
        <li>
          <a href="#1104">1.10.4 拉取文件测试</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#111_JetBrains_PyCharm_github">1.11 JetBrains PyCharm 使用github</a><ul>
        <li>
          <a href="#1111_PyCharm_github">1.11.1 PyCharm 上github设置</a>
        </li>
        <li>
          <a href="#1112">1.11.2 推送代码</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#112_pycharmgitlab">1.12 pycharm使用gitlab</a>
    </li>
    <li>
      <a href="#113-2">1.13 参考文档</a>
    </li>
  </ul>
</div>