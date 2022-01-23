---
title: inotify+rsync实现实时同步
author: 惨绿少年
type: post
date: 2017-12-10T21:20:00+00:00
url: /clsn/lx490.html
Baidusubmit:
  - 1
views:
  - 170
specs_zan:
  - 1
categories:
  - Linux运维
  - 安全
  - 运维基本功

---
<div id="log-box">
  <div id="catalog">
    <ul id="catalog-ul">
      <li>
        <i class="be be-arrowright"></i> <a href="#title-0" title="1.5.6.1&nbsp; 修改输出的日期格式">1.5.6.1&nbsp; 修改输出的日期格式</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-1" title="2.2.2.1&nbsp; inotifywait和inotifywatch的作用：">2.2.2.1&nbsp; inotifywait和inotifywatch的作用：</a>
      </li>
    </ul>
    
    <span class="log-zd"><span class="log-close"><a title="隐藏目录"><i class="be be-cross"></i><strong>目录</strong></a></span></span>
  </div>
</div>

## <span id="11">1.1 什么是实时同步：如何实现实时同步</span>

  1. 要利用监控服务（inotify），监控同步数据服务器目录中信息的变化
  2. 发现目录中数据产生变化，就利用rsync服务推送到备份服务器上

## <span id="12">1.2 实现实时同步的方法</span>

　　&nbsp;inotify+rsync 方式实现数据同步

　　&nbsp;sersync 方式实现实时数据同步 详情参照：http://www.cnblogs.com/clsn/p/7707828.html

### <span id="121">1.2.1 实时同步原理介绍</span>

<p style="text-align: center;">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211132107790-537190684.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="inotify+rsync实现实时同步" alt="" />
</p>

## <span id="13_inotifyrsync">1.3 inotify+rsync 方式实现数据同步</span>

### <span id="131_Inotify">1.3.1 Inotify简介</span>

　　Inotify是一种强大的，细粒度的。异步的文件系统事件监控机制，linux内核从2.6.13起，加入了 Inotify支持，通过Inotify可以监控文件系统中添加、删除，修改、移动等各种事件,利用这个内核接口，第三方软件就可以监控文件系统下文件的各种变化情况，而 **inotify-tools** 正是实施这样监控的软件。国人周洋在金山公司也开发了类似的实时同步软件sersync。

<div>
  <div style="mso-element: para-border-div; border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; mso-border-shadow: yes; background: #F2F2F2; mso-shading: windowtext; mso-pattern: gray-5 auto;">
    <p class="a">
      <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">提示信息：</span>
    </p>
    
    <p class="a" style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">sersync</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">软件实际上就是在</span><span lang="EN-US"> inotify</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">软件基础上进行开发的，功能要更加强大些</span> <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">，多了定时重传机制，过滤机制了提供接口做</span><span lang="EN-US"> CDN</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">，支持多线程橾作。</span>
    </p>
  </div>
</div>

　　Inotify实际是一种事件驱动机制，它为应用程序监控文件系统事件提供了实时响应事件的机制，而无须通过诸如cron等的轮询机制来获取事件。cron等机制不仅无法做到实时性，而且消耗大量系统资源。相比之下，inotify基于事件驱动，可以做到对事件处理的实时响应，也没有轮询造成的系统资源消耗，是非常自然的事件通知接口，也与自然世界事件机制相符合。

　　inotify的实现有几款软件：

<div>
  <div class="cnblogs_code">
    <pre>　　inotify-tools，sersync，lrsyncd</pre>
  </div>
</div>

### <span id="132_inotifyrsync">1.3.2 inotify+rsync使用方式</span>

　　inotify 对同步数据目录信息的监控

　　rsync&nbsp; 完成对数据信息的实时同步

　　利用脚本进行结合

## <span id="14_inotify">1.4 部署inotify软件的前提</span>

　　需要2.6.13以后内核版本才能支持inotify软件。2.6.13内核之后版本，在没有安装inotify软件之前，应该有这三个文件。

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# ll /proc/sys/fs/inotify/<span style="color: #000000;">
total </span><span style="color: #800080;"></span>
-rw-r--r-- <span style="color: #800080;">1</span> root root <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">10</span>:<span style="color: #800080;">12</span><span style="color: #000000;"> max_queued_events
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">10</span>:<span style="color: #800080;">12</span><span style="color: #000000;"> max_user_instances
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">10</span>:<span style="color: #800080;">12</span> max_user_watches</pre>
  </div>
</div>

### <span id="141_nbsp">1.4.1 三个重要文件的说明&nbsp;</span>

<table class="MsoTableGrid" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 140.45pt; border-width: 1pt; border-color: windowtext; background: #a6a6a6; padding: 0cm 5.4pt;" width="187">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件</span></strong>
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #a6a6a6; padding: 0cm 5.4pt;" width="77">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">默认值</span></strong>
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #a6a6a6; padding: 0cm 5.4pt;" width="433">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">作用说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 140.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="187">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">max_user_watches</span>
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="77">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">8192</span>
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="433">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">设置</span><span lang="EN-US">inotifywait</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">或</span><span lang="EN-US">inotifywatch</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">命令可以监视的文件数量（单进程）</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 140.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="187">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">max_user_instances</span>
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="77">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">128</span>
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="433">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">设置每个用户可以运行的</span><span lang="EN-US">inotifywait</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">或</span><span lang="EN-US">inotifywatch</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">命令的进程数</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 140.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="187">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">max_queued_events</span>
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="77">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">16384</span>
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="433">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">设置</span><span lang="EN-US">inotify</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">实例事件（</span><span lang="EN-US">event</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">）队列可容纳的事件数量</span>
      </p>
    </td>
  </tr>
</table>

### <span id="143_142"><span style="font-size: 1.17em;">1.4.3 【官方说明】三个重要文件</span>1.4.2 【服务优化】可以将三个文件的数值调大，监听更大的范围</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 ~]# <span style="color: #0000ff;">man</span><span style="color: #000000;"> proc
</span>/proc/sys/fs/inotify (since Linux <span style="color: #800080;">2.6</span>.<span style="color: #800080;">13</span><span style="color: #000000;">)
       This   directory   contains    files    max_queued_events,
       max_user_instances, and max_user_watches, that can be used
       to limit the amount of kernel memory consumed by the  inotify interface.  
</span><span style="color: #0000ff;">for</span> further details, see inotify(<span style="color: #800080;">7</span>).</pre>
  </div>
</div>

<div>
  <p class="a">
    通过man手册的第7级别中查到 inotify的默认文件的详细说明。
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 ~]# <span style="color: #0000ff;">man</span> <span style="color: #800080;">7</span><span style="color: #000000;"> inotify
</span>/proc/sys/fs/inotify/<span style="color: #000000;">max_queued_events
       The  value  </span><span style="color: #0000ff;">in</span> this <span style="color: #0000ff;">file</span><span style="color: #000000;"> is used when an application calls
       inotify_init(</span><span style="color: #800080;">2</span><span style="color: #000000;">) to set an upper limit  on  the  number  of
       events  that  can  be  queued to the corresponding inotify
       instance.  Events </span><span style="color: #0000ff;">in</span><span style="color: #000000;"> excess of this limit are dropped, but
       an IN_Q_OVERFLOW event is always generated.

</span>/proc/sys/fs/inotify/<span style="color: #000000;">max_user_instances
       This  specifies  an  upper  limit on the number of inotify
       instances that can be created per real user ID.

</span>/proc/sys/fs/inotify/<span style="color: #000000;">max_user_watches
       This specifies an upper limit on  the  number  of  watches
       that can be created per real user ID.</span></pre>
  </div>
</div>

## <span id="15_inotify">1.5 inotify软件介绍及参数说明</span>

### <span id="151">1.5.1 两种安装方式</span>

<div>
  <p class="a">
    　　1） yum install -y inotify-tools
  </p>
  
  <p class="a">
    　　2） 手工编译安装
  </p>
</div>

_**注：**_

&nbsp;&nbsp; <span style="background-color: #00ffff;">&nbsp;YUM 安装需要有epel源</span>

<div>
  <p class="a1">
    　　http://mirrors.aliyun.com
  </p>
</div>

手工编译安装方式需要到github上进行下载软件包

&nbsp;&nbsp;&nbsp; <span style="background-color: #00ffff;">inotify软件的参考资料链接：</span>

<div>
  <p class="a1">
    　　https://github.com/rvoicilas/inotify-tools/wiki
  </p>
</div>

### <span id="152_inotify">1.5.2 inotify主要安装的两个软件</span>

<div>
  <p class="a">
    <strong>inotifywait</strong><strong>： </strong><strong>（主要）</strong>
  </p>
  
  <p class="a">
    &nbsp;　　在被监控的文件或目录上等待特定文件系统事件（open close delete等）发生，执行后处于阻塞状态，适合在shell脚本中使用
  </p>
  
  <p class="a">
    <strong>inotifywatch：</strong>
  </p>
  
  <p class="a">
    　　收集被监控的文件系统使用的统计数据，指文件系统事件发生的次数统计。
  </p>
</div>

　　说明：在实时实时同步的时候，主要是利用inotifywait对目录进行监控

### <span id="153_inotifywaitnbsp">1.5.3 inotifywait命令参数说明&nbsp;</span>

<table class="MsoTableGrid" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 162.8pt; border-width: 1pt; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">参数</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">含义</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">-m, --monitor</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Keep listening for events forever.&nbsp; Without this option, inotifywait will exit after one event is received.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">始终保持事件监听。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">-d, --daemon</span>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">111</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">-r, --recursive</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Watch all subdirectories of any directories passed as arguments.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">递归监控目录数据信息变化</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">-o, --outfile <file></span>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Print events to <file> rather than stdout.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">打印事件到文件中，相当于标准正确输出</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">-s, --syslog</span>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Output errors to syslog(3) system log module rather than stderr.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">发送错误到</span><span lang="EN-US">syslog</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">相当于标准错误输出</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">-q, --quiet</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">If specified once, the program will be less verbose.&nbsp; Specifically, it will not state&nbsp; when&nbsp; it&nbsp; has&nbsp; completed establishing all inotify watches.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">输出信息少（只打印事件信息）</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">--exclude <pattern></span>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Exclude all events on files matching the extended regular expression <pattern>.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">排除文件或目录</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">--excludei <pattern></span>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Like --exclude but case insensitive.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">排除文件或目录时，不区分大小写</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">--timefmt <fmt></span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Print using a specified printf-like format string; read the man page for more details.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指定时间输出格式</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">--format <fmt></span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Print using a specified printf-like formatstring; read the man page for more details.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">打印使用指定的输出类似格式字符串；即实际监控输出内容</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span lang="EN-US">-e</span></strong>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="480">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">Listen for specific event(s).&nbsp; If omitted, all events are listened for.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指定监听指定的事件，如果省略，表示所有事件都进行监听</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" width="697">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">以上的信息可以通过</span> <span lang="EN-US">inotifywait --help&nbsp; </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">获得</span>
      </p>
    </td>
  </tr>
</table>

<h3 style="text-align: left;">
  <span id="154_-e_nbspnbsp">1.5.4 -e[参数] &nbsp;可以指定的事件类型&nbsp;</span>
</h3>

<table class="MsoTableGrid" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 106.1pt; border-width: 1pt; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">事件名称</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">事件说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">access</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">file or directory contents were read</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录内容被读取</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">modify</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">file or directory contents were writterv</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录内容被写入</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">attrib</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">file or directory attributes changed</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录属性改变</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">close_write</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">file or directory closed, after being opened in writeable mode.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录关闭，在写入模式打开之后关闭的。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">close_nowrite</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">file or directory closed, after being opened in read-only mode.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录关闭，在只读模式打开之后关闭的</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">close</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">file or directory closed, regardless of read/write mode </span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录关闭，不管读或是写模式</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">open</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; tab-stops: 47.55pt;">
        <span lang="EN-US">file or directory opened </span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; tab-stops: 47.55pt;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录被打开</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">moved_to</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">拉</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; tab-stops: 44.15pt;">
        <span lang="EN-US">file or directory moved to watched directory</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; tab-stops: 44.15pt;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录被移动到监控的目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p class="MsoNormal">
        <span lang="EN-US">moved_from</span>
      </p>
      
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">推</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" valign="top" width="556">
      <p class="MsoNormal">
        <span lang="EN-US">file or directory moved from watched directory</span>
      </p>
      
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录被移动从监控的目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p class="MsoNormal">
        <span lang="EN-US">move</span>
      </p>
      
      <p class="MsoNormal">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" valign="top" width="556">
      <p class="MsoNormal">
        <span lang="EN-US">file or directory moved to or from watched directory</span>
      </p>
      
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录不管移动到或是移出监控目录都触发事件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p class="MsoNormal">
        <span lang="EN-US">create</span>
      </p>
      
      <p class="MsoNormal">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" valign="top" width="556">
      <p class="MsoNormal">
        <span lang="EN-US">file or directory created within watched directory</span>
      </p>
      
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录创建在监控目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p class="MsoNormal">
        <span lang="EN-US">delete</span>
      </p>
      
      <p class="MsoNormal">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" valign="top" width="556">
      <p class="MsoNormal">
        <span lang="EN-US">file or directory deleted within watched directory</span>
      </p>
      
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录被删除在监控目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p class="MsoNormal">
        <span lang="EN-US">delete_self</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" valign="top" width="556">
      <p class="MsoNormal">
        <span lang="EN-US">file or directory was deleted</span>
      </p>
      
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录被删除，目录本身被删除</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p class="MsoNormal">
        <span lang="EN-US">unmount</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" valign="top" width="556">
      <p class="MsoNormal">
        <span lang="EN-US">file system containing file or directory unmounted</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" valign="top" width="697">
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">以上的信息可以通过</span> <span lang="EN-US">inotifywait --help&nbsp; </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">获得</span>
      </p>
    </td>
  </tr>
</table>

#### <span id="1541nbsp_inotifywait">1.5.4.1&nbsp; 【<strong>实例</strong>】inotifywait监控中的事件测试</span>

1、创建事件

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">touch</span><span style="color: #000000;"> test2.txt
[root@nfs01 </span>~]# inotifywait -mrq  /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d-%m-%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e create
</span><span style="color: #800080;">17</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">19</span> /data/test2.txt 事件信息: CREATE</pre>
  </div>
</div>

2、删除事件

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# \<span style="color: #0000ff;">rm</span> -<span style="color: #000000;">f test1.txt
[root@nfs01 </span>~]# inotifywait -mrq  /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d-%m-%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e delete
</span><span style="color: #800080;">17</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">28</span> /data/test1.txt 事件信息: DELETE</pre>
  </div>
</div>

3、修改事件

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">echo</span> <span style="color: #800000;">"</span><span style="color: #800000;">132</span><span style="color: #800000;">"</span> ><span style="color: #000000;"> test.txt
[root@nfs01 </span>~]# inotifywait -mrq  /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d-%m-%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e close_write
</span><span style="color: #800080;">17</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/test.txt 事件信息: CLOSE_WRITE,CLOSE</pre>
  </div>
</div>

4、移动事件 moved_to

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">mv</span> /etc/<span style="color: #000000;">hosts .
[root@nfs01 </span>~]# inotifywait -mrq  /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d-%m-%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e moved_to
</span><span style="color: #800080;">17</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">33</span> /data/hosts 事件信息: MOVED_TO</pre>
  </div>
</div>

5、移动事件 moved_from

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">mv</span> ./hosts  /tmp/<span style="color: #000000;">
[root@nfs01 </span>~]# inotifywait -mrq  /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d-%m-%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e moved_from
</span><span style="color: #800080;">17</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">34</span> /data/hosts 事件信息: MOVED_FROM</pre>
  </div>
</div>

### <span id="155_inotifywait_--format_nbsp">1.5.5 inotifywait 参数 --format <fmt>格式定义参数&nbsp;</span>

<table class="MsoTableGrid" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 134.45pt; border-width: 1pt; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="179">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">命令参数</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="518">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">参数说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%w</span><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="518">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">事件出现时，监控文件或目录的名称信息</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%f</span><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="518">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">事件出现时，将显示监控目录下触发事件的文件或目录信息，否则为空</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%e</span><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="518">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">显示发生的事件信息，不同的事件信息用逗号进行分隔</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%Xe</span>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="518">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">显示发生的事件信息，不同的事件信息有</span><span lang="EN-US">x</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">进行分隔，可以修改</span><span lang="EN-US">X</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">为指定分隔符</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%T</span><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="518">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">输出时间格式中定义的时间格式信息，通过</span><span lang="EN-US"> --timefmt option </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">语法格式指定时间信息</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">这个格式是通过</span><span lang="EN-US">strftime</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">函数进行匹配时间格式信息的</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" valign="top" width="697">
      <p class="MsoNormal">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">以上的信息可以通过</span> <span lang="EN-US">inotifywait --help&nbsp; </span></strong><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">获得</span></strong>
      </p>
    </td>
  </tr>
</table>

### <span id="156_inotifywait_--timefmt">1.5.6 inotifywait 参数--timefmt <fmt>时间格式参数</span>

<table class="MsoTableGrid" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 106.1pt; border-width: 1pt; border-color: windowtext; background: #a6a6a6; padding: 0cm 5.4pt;" valign="top" width="141">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">命令参数</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #a6a6a6; padding: 0cm 5.4pt;" valign="top" width="556">
      <p class="MsoNormal" style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">参数说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%d</span><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">The day of the month as a decimal number(range 01 to 31)</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">每月的第几天，显示倍息为十进制数（范围是</span><span lang="EN-US"> 01-31 )</span>
      </p>
    </td>
  </tr>
  
  <tr style="mso-yfti-irow: 2; height: 39.15pt;">
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt; height: 39.15pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%m</span><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt; height: 39.15pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; tab-stops: 178.35pt;">
        <span lang="EN-US">The month as a decimal number (range 01 to 12).</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; tab-stops: 178.35pt;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">显示月份，显示信息为十进制（范围</span><span lang="EN-US"> 01-12 )</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%M</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">The minute as a decimal number (range 00 to 59).</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">显示分钟，显示信息为十进制（范围</span><span lang="EN-US"> 00-59 )</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%y</span><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">The year as a decimal number without a century (range 00 to 99).</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">年份信息，显示信息为十进制，并且没有世纪信息</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%Y</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">The year as a decimal number including the century.</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">年份信息，显示信息为十进制，并且包含世纪信息</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">%H</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="556">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">The hour as a decimal number using a 24-hour clock (range 00 to 23).</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">小时信息，显示信息为十进制，使用</span><span lang="EN-US"> 24</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">小时制（范围</span><span lang="EN-US"> 00-23 )</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" valign="top" width="697">
      <p class="MsoNormal">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">说明：以上信息可以通过</span><span lang="EN-US"> man strftime</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">信息获取</span></strong>
      </p>
    </td>
  </tr>
</table>

<span class="directory"></span>

#### <span id="1561nbsp">1.5.6.1&nbsp; 修改输出的日期格式</span> {#title-0}

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 ~]# inotifywait -mrq  /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d/%m/%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f</span><span style="color: #800000;">"</span>
<span style="color: #800080;">17</span>/<span style="color: #800080;">10</span>/<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">12</span> /data/test1.txt</pre>
  </div>
</div>

### <span id="157_-e_nbsp">1.5.7 -e[参数] 重要监控事件参数汇总表：&nbsp;</span>

<table class="MsoTableGrid" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 99pt; border-width: 1pt; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="132">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; tab-stops: 49.4pt;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">重要事件</span></strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="161">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">包含事件</span></strong>
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: windowtext; border-right-color: windowtext; border-bottom-color: windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="404">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">备注说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="132">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span lang="EN-US">close</span></strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="161">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">close_write&nbsp;&nbsp; </span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">close_nowrite</span>
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="404">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录关闭，不管读或是写模式</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">即包含写关闭与读关闭</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="132">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span lang="EN-US">close_write</span></strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="161">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">create</span>
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="404">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">包含文件创建事件，但不包含目录创建事件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; padding: 0cm 5.4pt;" width="132">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span lang="EN-US">move</span></strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="161">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">moved_to&nbsp;&nbsp; </span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">moved_from</span>
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: windowtext; border-right-width: 1pt; border-right-color: windowtext; padding: 0cm 5.4pt;" width="404">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">文件或目录不管移动到或是移动出监控目录都触发事件</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">即包含信息移入或移出监控目录事件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: windowtext; border-bottom-color: windowtext; border-left-color: windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="3" width="697">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">重要参数汇总：根据以上说明，在实际使用时，只要监控以下事件即可</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span lang="EN-US">create </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">创建、</span><span lang="EN-US"> delete </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">删除、</span><span lang="EN-US"> movedjto </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">移入、</span><span lang="EN-US"> close_write </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">修</span> <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">改</span>
      </p>
      
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">&nbsp;<span class="cnblogs_code">inotifywait -mrq /data --format <span style="color: #800000;">"</span><span style="color: #800000;">%w%f</span><span style="color: #800000;">"</span> -e create,delete,moved_to,close_write</span>&nbsp;</span>
      </p>
    </td>
  </tr>
</table>

## <span id="16_inotifywait">1.6 对inotifywait命令的测试</span>

**<span style="font-size: 14px;">对inotifywait命令测试的说明：</span>**

&nbsp;&nbsp; 需要打开两个连接窗口

<div>
  <div style="mso-element: para-border-div; border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; mso-border-shadow: yes; background: #F2F2F2; mso-shading: windowtext; mso-pattern: gray-5 auto;">
    <p class="a">
      <strong><span style="color: #00b0f0;" lang="EN-US">1</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体; color: #00b0f0;">窗口</span></strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">运行</span><span lang="EN-US">inotifywait</span>
    </p>
    
    <p class="a">
      <strong><span style="color: yellow;" lang="EN-US">2</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体; color: yellow;">窗口</span></strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">对文件夹进行操作，可在一窗口中查看出</span><span lang="EN-US">inotifywait</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">的监控记录</span>
    </p>
  </div>
</div>

### <span id="161_darr">1.6.1 、创建文件的逻辑&darr;</span>

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]# inotifywait /<span style="color: #000000;">data
Setting up watches.
Watches established.
</span>/data/<span style="color: #000000;"> CREATE test1.txt
</span>/data/<span style="color: #000000;"> OPEN test1.txt
</span>/data/<span style="color: #000000;"> ATTRIB test1.txt
</span>/data/<span style="color: #000000;"> CLOSE_WRITE,CLOSE test1.txt
创建文件，inotifywait显示创建文件的过程&uarr;
[root@nfs01 data]# </span><span style="color: #0000ff;">touch</span> test1.txt</pre>
</div>

### <span id="162_darr">1.6.2 创建目录逻辑&darr;</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">mkdir</span><span style="color: #000000;"> testdir
[root@nfs01 </span>~<span style="color: #000000;">]#
</span>/data/ CREATE,ISDIR testdir</pre>
  </div>
</div>

### <span id="163_darr">1.6.3 监控子目录下的文件&darr;</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">touch</span>  testdir/<span style="color: #000000;">test01.txt
[root@nfs01 </span>~]# inotifywait -mrq  /<span style="color: #000000;">data 
</span>/data/testdir/<span style="color: #000000;"> OPEN test01.txt
</span>/data/testdir/<span style="color: #000000;"> ATTRIB test01.txt
</span>/data/testdir/ CLOSE_WRITE,CLOSE test01.txt</pre>
  </div>
</div>

### <span id="164_sed">1.6.4 sed命令修改逻辑</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">sed</span> <span style="color: #800000;">'</span><span style="color: #800000;">s#132#123#g</span><span style="color: #800000;">'</span> test.txt -<span style="color: #000000;">i

[root@nfs01 </span>~]# inotifywait -mrq  /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d-%m-%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e moved_from
 </span>/data/<span style="color: #000000;">test.txt 事件信息: OPEN
 </span>/data/<span style="color: #000000;">sedDh5R8v 事件信息: CREATE
 </span>/data/<span style="color: #000000;">sedDh5R8v 事件信息: OPEN
 </span>/data/<span style="color: #000000;">test.txt 事件信息: ACCESS
 </span>/data/<span style="color: #000000;">sedDh5R8v 事件信息: MODIFY
 </span>/data/<span style="color: #000000;">sedDh5R8v 事件信息: ATTRIB
 </span>/data/<span style="color: #000000;">sedDh5R8v 事件信息: ATTRIB
 </span>/data/<span style="color: #000000;">test.txt 事件信息: CLOSE_NOWRITE,CLOSE
 </span>/data/<span style="color: #000000;">sedDh5R8v 事件信息: CLOSE_WRITE,CLOSE
 </span>/data/<span style="color: #000000;">sedDh5R8v 事件信息: MOVED_FROM
 </span>/data/test.txt 事件信息: MOVED_TO</pre>
  </div>
</div>

<span style="background-color: #00ff00;"><strong>sed</strong><strong>命令替换逻辑 </strong><strong>：</strong></span>

　　01. 创建临时文件

　　02. 将原文件内容放置到临时文件中，修改替换临时文件中的内容，原有文件不做改动

　　03. 重命名临时文件，覆盖原文件

### <span id="165_inotifywait_-e">1.6.5 inotifywait监控中 -e 的参数使用</span>

<div>
  <div class="cnblogs_code">
    <pre>inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d/%m/%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -e create</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp; 说明：表示只监听create事件

<div>
  <div class="cnblogs_code">
    <pre>inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d/%m/%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp; 说明：不指定-e参数，表示监听所有事件

02. 删除事件delete

<div>
  <div class="cnblogs_code">
    <pre>    # inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%F %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %@e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e delete
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">28</span> /data/<span style="color: #800080;">02</span><span style="color: #000000;">.txt 事件信息: DELETE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">28</span> /data/<span style="color: #800080;">03</span><span style="color: #000000;">.txt 事件信息: DELETE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">28</span> /data/<span style="color: #800080;">04</span>.txt 事件信息: DELETE</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp; 03. 修改事件close_write

<div>
  <div class="cnblogs_code">
    <pre>  # inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%F %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %@e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e delete,close_write
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/<span style="color: #000000;">oldgirl.txt 事件信息: CLOSE_WRITE@CLOSE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/<span style="color: #000000;">.oldgirl.txt.swx 事件信息: CLOSE_WRITE@CLOSE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/<span style="color: #000000;">.oldgirl.txt.swx 事件信息: DELETE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/<span style="color: #000000;">.oldgirl.txt.swp 事件信息: CLOSE_WRITE@CLOSE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/<span style="color: #000000;">.oldgirl.txt.swp 事件信息: DELETE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/<span style="color: #000000;">.oldgirl.txt.swp 事件信息: CLOSE_WRITE@CLOSE
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">30</span> /data/.oldgirl.txt.swp 事件信息: DELETE</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp; 04. 移动事件moved_to

<div>
  <div class="cnblogs_code">
    <pre>inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%F %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %@e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e delete,close_write,moved_to
    </span><span style="color: #800080;">2017</span>-<span style="color: #800080;">10</span>-<span style="color: #800080;">17</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">34</span> /data/hosts 事件信息: MOVED_TO</pre>
  </div>
</div>

## <span id="17">1.7 实时同步命令参数示意图</span>

<p style="text-align: center;">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211130603134-1187017146.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="inotify+rsync实现实时同步" alt="" />&nbsp;
</p>

## <span id="2_inotifyrsync">第2章 inotify+rsync实时同步服务部署</span>

## <span id="21_rsync">2.1 第一个里程碑：部署rsync服务</span>

### <span id="211_rsync">2.1.1 rsync服务端部署</span>

1)软件是否存在

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# rpm -qa |<span style="color: #0000ff;">grep</span><span style="color: #000000;"> rsync
rsync</span>-<span style="color: #800080;">3.0</span>.<span style="color: #800080;">6</span>-<span style="color: #800080;">12</span>.el6.x86_64</pre>
  </div>
</div>

<div>
  <p class="a">
    <span style="background-color: #ffff00;">需求：查询到某个命令非常有用。但是不知道属于哪个软件包</span>
  </p>
  
  <p class="a">
    &nbsp;<span class="cnblogs_code"> <span style="color: #0000ff;">yum</span><span style="color: #000000;"> provides rysnc </span></span>
  </p>
  
  <p class="a">
    <span class="cnblogs_code"><span style="color: #000000;">provides Find what package provides the given value</span></span>&nbsp;
  </p>
</div>

2)进行软件服务配置

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# vim /etc/<span style="color: #000000;">rsyncd.conf 
uid </span>=<span style="color: #000000;"> rsync
gid </span>=<span style="color: #000000;"> rsync
use </span><span style="color: #0000ff;">chroot</span> =<span style="color: #000000;"> no
max connections </span>= <span style="color: #800080;">200</span><span style="color: #000000;">
timeout </span>= <span style="color: #800080;">300</span><span style="color: #000000;">
pid </span><span style="color: #0000ff;">file</span> = /var/run/<span style="color: #000000;">rsyncd.pid
lock </span><span style="color: #0000ff;">file</span> = /var/run/<span style="color: #000000;">rsync.lock
log </span><span style="color: #0000ff;">file</span> = /var/log/<span style="color: #000000;">rsyncd.log
ignore errors
read only </span>= <span style="color: #0000ff;">false</span><span style="color: #000000;">
list </span>= <span style="color: #0000ff;">false</span><span style="color: #000000;">
hosts allow </span>= <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.0</span>/<span style="color: #800080;">24</span><span style="color: #000000;">
auth users </span>=<span style="color: #000000;"> rsync_backup
secrets </span><span style="color: #0000ff;">file</span> = /etc/<span style="color: #000000;">rsync.password
[backup]
comment </span>= <span style="color: #800000;">"</span><span style="color: #800000;">backup dir by oldboy</span><span style="color: #800000;">"</span><span style="color: #000000;">
path </span>= /<span style="color: #000000;">backup
[nfsbackup]
comment </span>= <span style="color: #800000;">"</span><span style="color: #800000;">nfsbackup dir by hzs</span><span style="color: #800000;">"</span><span style="color: #000000;">
path </span>= /nfsbackup</pre>
  </div>
</div>

3)创建rsync管理用户

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# useradd -s /sbin/nologin -M rsync</pre>
  </div>
</div>

4)创建数据备份储存目录,目录修改属主

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# <span style="color: #0000ff;">mkdir</span> /nfsbackup/<span style="color: #000000;">
[root@backup </span>~]# <span style="color: #0000ff;">chown</span> -R rsync.rsync /nfsbackup/</pre>
  </div>
</div>

5)创建认证用户密码文件并进行授权600

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">echo</span> <span style="color: #800000;">"</span><span style="color: #800000;">rsync_backup:clsn123</span><span style="color: #800000;">"</span> >>/etc/<span style="color: #000000;">rsync.password
</span><span style="color: #0000ff;">chmod</span> <span style="color: #800080;">600</span> /etc/rsync.password</pre>
  </div>
</div>

6)启动rsync服务

<div>
  <div class="cnblogs_code">
    <pre>rsync --daemon</pre>
  </div>
</div>

至此服务端配置完成

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# <span style="color: #0000ff;">ps</span> -ef |<span style="color: #0000ff;">grep</span><span style="color: #000000;"> rsync 
root       </span><span style="color: #800080;">2076</span>      <span style="color: #800080;">1</span>  <span style="color: #800080;"></span> <span style="color: #800080;">17</span>:<span style="color: #800080;">05</span> ?        <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span> rsync --<span style="color: #000000;">daemon
root       </span><span style="color: #800080;">2163</span>   <span style="color: #800080;">1817</span>  <span style="color: #800080;"></span> <span style="color: #800080;">17</span>:<span style="color: #800080;">38</span> pts/<span style="color: #800080;">1</span>    <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span> <span style="color: #0000ff;">grep</span> --color=auto rsync</pre>
  </div>
</div>

### <span id="212_rsync">2.1.2 rsync客户端配置</span>

1)软件是否存在

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# rpm -qa |<span style="color: #0000ff;">grep</span><span style="color: #000000;"> rsync
rsync</span>-<span style="color: #800080;">3.0</span>.<span style="color: #800080;">6</span>-<span style="color: #800080;">12</span>.el6.x86_64</pre>
  </div>
</div>

2)创建安全认证文件，并进行修改权限600

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">echo</span> <span style="color: #800000;">"clsn</span><span style="color: #800000;">123</span><span style="color: #800000;">"</span> >>/etc/<span style="color: #000000;">rsync.password
</span><span style="color: #0000ff;">chmod</span> <span style="color: #800080;">600</span> /etc/rsync.password</pre>
  </div>
</div>

3) 测试数据传输

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 sersync]# rsync -avz /data  rsync_backup@<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.41</span>::nfsbackup  --password-<span style="color: #0000ff;">file</span>=/etc/<span style="color: #000000;">rsync.password
sending incremental </span><span style="color: #0000ff;">file</span><span style="color: #000000;"> list
data</span>/<span style="color: #000000;">
data</span>/<span style="color: #000000;">.hzs
data</span>/.<span style="color: #0000ff;">tar</span><span style="color: #000000;">.gz
data</span>/.txt</pre>
  </div>
</div>

## <span id="22_inotify">2.2 第二个里程碑：部署inotify服务</span>

首先先确认是否有**epel****源**用来安装inotify-tools软件

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 ~]# <span style="color: #0000ff;">yum</span><span style="color: #000000;"> repolist
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
 </span>*<span style="color: #000000;"> base: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> epel: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> extras: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> updates: mirrors.aliyun.com
repo </span><span style="color: #0000ff;">id</span><span style="color: #000000;">  repo name                                       status
base     CentOS</span>-<span style="color: #800080;">6</span> - Base - mirrors.aliyun.com             <span style="color: #800080;">6</span>,<span style="color: #800080;">706</span><span style="color: #000000;">
epel     Extra Packages </span><span style="color: #0000ff;">for</span> Enterprise Linux <span style="color: #800080;">6</span> - x86_64  <span style="color: #800080;">12</span>,<span style="color: #800080;">401</span><span style="color: #000000;">
extras   CentOS</span>-<span style="color: #800080;">6</span> - Extras - mirrors.aliyun.com              <span style="color: #800080;">46</span><span style="color: #000000;">
updates  CentOS</span>-<span style="color: #800080;">6</span> - Updates - mirrors.aliyun.com            <span style="color: #800080;">722</span><span style="color: #000000;">
repolist: </span><span style="color: #800080;">19</span>,<span style="color: #800080;">875</span></pre>
  </div>
</div>

### <span id="221_inotify">2.2.1 安装inotify软件</span>

两种安装方式

<div>
  <p class="a">
    　　1） yum install -y inotify-tools
  </p>
  
  <p class="a">
    　　2） 手工编译安装
  </p>
</div>

注：

手工编译安装方式需要到github上进行下载软件包

&nbsp;&nbsp;&nbsp; inotify软件的参考资料链接：

　　　　https://github.com/rvoicilas/inotify-tools/wiki

### <span id="222_inotifyinotifywaitinotifywatch">2.2.2 查看inotify安装上的两个命令(inotifywait,inotifywatch)</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 ~]# rpm -ql inotify-<span style="color: #000000;">tools
</span>/usr/bin/<span style="color: #000000;">inotifywait      #主要
</span>/usr/bin/inotifywatch</pre>
  </div>
</div>

<span class="directory"></span>

#### <span id="2221nbsp_inotifywaitinotifywatch">2.2.2.1&nbsp; inotifywait和inotifywatch的作用：</span> {#title-1}

<div>
  <p class="a1">
    一共安装了2个工具（命令），即inotifywait和inotifywatch
  </p>
  
  <p class="a1">
    inotifywait : 在被监控的文件或目录上等待特定文件系统事件（open close delete等）发生，
  </p>
  
  <p class="a1">
    　　　　　 执行后处于阻塞状态，适合在shell脚本中使用
  </p>
  
  <p class="a1">
    inotifywatch :收集被监控的文件系统使用的统计数据,指文件系统事件发生的次数统计。
  </p>
  
  <p class="a1">
    <strong>　　说明：</strong>yum安装后可以直接使用，如果编译安装需要进入到相应软件目录的bin目录下使用
  </p>
  
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">#命令 man手册说明
# </span><span style="color: #0000ff;">man</span><span style="color: #000000;"> inotifywait
inotifywait </span>- <span style="color: #0000ff;">wait</span> <span style="color: #0000ff;">for</span><span style="color: #000000;"> changes to files using inotify

使用inotify进行监控，等待产生变化的文件信息

# </span><span style="color: #0000ff;">man</span><span style="color: #000000;"> inotifywatch
inotifywatch </span>-<span style="color: #000000;"> gather filesystem access statistics using inotify
使用inotify进行监控，收集文件系统访问统计佶息</span></pre>
  </div>
</div>

## <span id="23_rsyncinotify">2.3 第三个里程碑：编写脚本，实现rsync+inotify软件功能结合</span>

### <span id="231_rsync">2.3.1 rsync服务命令：</span>

<div>
  <div class="cnblogs_code">
    <pre>rsync -avz --delete /data/ rsync_backup@<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.41</span>::nfsbackup --password-<span style="color: #0000ff;">file</span>=/etc/rsync.password</pre>
  </div>
</div>

### <span id="232_inotify">2.3.2 inotify服务命令：</span>

<div>
  <div class="cnblogs_code">
    <pre>inotifywait -mrq /data -format <span style="color: #800000;">"</span><span style="color: #800000;">%w%f</span><span style="color: #800000;">"</span>  -e create,delete,move_to,close_write</pre>
  </div>
</div>

### <span id="233">2.3.3 编写脚本：</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 sersync]# vim /server/scripts/inotify.<span style="color: #0000ff;">sh</span><span style="color: #000000;">
#</span>!/bin/<span style="color: #000000;">bash
inotifywait </span>-mrq /data --format <span style="color: #800000;">"</span><span style="color: #800000;">%w%f</span><span style="color: #800000;">"</span>  -e create,delete,moved_to,close_write|<span style="color: #000000;">\
</span><span style="color: #0000ff;">while</span><span style="color: #000000;"> read line
</span><span style="color: #0000ff;">do</span><span style="color: #000000;">
        rsync </span>-az --delete /data/ rsync_backup@<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.41</span>::nfsbackup --password-
<span style="color: #0000ff;">file</span>=/etc/<span style="color: #000000;">rsync.password
</span><span style="color: #0000ff;">done</span></pre>
  </div>
</div>

脚本说明：

<div>
  <p class="a">
    　　for循环会定义一个条件，当条件不满足时停止循环
  </p>
  
  <p class="a">
    　　while循环：只要条件满足就一直循环下去
  </p>
</div>

### <span id="234">2.3.4 对脚本进行优化</span>

<div>
  <div class="cnblogs_code">
    <pre>#!/bin/<span style="color: #000000;">bash

Path</span>=/<span style="color: #000000;">data
backup_Server</span>=<span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.41</span>

/usr/bin/inotifywait -mrq --format <span style="color: #800000;">'</span><span style="color: #800000;">%w%f</span><span style="color: #800000;">'</span> -e create,close_write,delete /data  | <span style="color: #0000ff;">while</span><span style="color: #000000;"> read line  
</span><span style="color: #0000ff;">do</span>
    <span style="color: #0000ff;">if</span> [ -f $line ];<span style="color: #0000ff;">then</span><span style="color: #000000;">
        rsync </span>-az $line --delete rsync_backup@$backup_Server::nfsbackup --password-<span style="color: #0000ff;">file</span>=/etc/<span style="color: #000000;">rsync.password
    </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
        cd $Path </span>&&<span style="color: #000000;">\
        rsync </span>-az ./ --delete rsync_backup@$backup_Server::nfsbackup --password-<span style="color: #0000ff;">file</span>=/etc/<span style="color: #000000;">rsync.password
    </span><span style="color: #0000ff;">fi</span>

<span style="color: #0000ff;">done</span></pre>
  </div>
</div>

## <span id="24">2.4 第四个里程碑：测试编写的脚本</span>

### <span id="241">2.4.1 让脚本在后台运行</span>

　　在/data 目录先创建6个文件

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# <span style="color: #0000ff;">sh</span>  /server/scripts/inotify.<span style="color: #0000ff;">sh</span> &<span style="color: #000000;">
[root@nfs01 data]# </span><span style="color: #0000ff;">touch</span> {<span style="color: #800080;">1</span>..<span style="color: #800080;">6</span>}.txt</pre>
  </div>
</div>

在backup服务器上，已经时候同步过去了6个文件。

<div>
  <div class="cnblogs_code">
    <pre>[root@backup ~]# ll /nfsbackup/<span style="color: #000000;">
total </span><span style="color: #800080;">8</span>
-rw-r--r-- <span style="color: #800080;">1</span> rsync rsync <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">12</span>:<span style="color: #800080;">06</span> <span style="color: #800080;">1</span><span style="color: #000000;">.txt
</span>-rw-r--r-- <span style="color: #800080;">1</span> rsync rsync <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">12</span>:<span style="color: #800080;">06</span> <span style="color: #800080;">2</span><span style="color: #000000;">.txt
</span>-rw-r--r-- <span style="color: #800080;">1</span> rsync rsync <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">12</span>:<span style="color: #800080;">06</span> <span style="color: #800080;">3</span><span style="color: #000000;">.txt
</span>-rw-r--r-- <span style="color: #800080;">1</span> rsync rsync <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">12</span>:<span style="color: #800080;">06</span> <span style="color: #800080;">4</span><span style="color: #000000;">.txt
</span>-rw-r--r-- <span style="color: #800080;">1</span> rsync rsync <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">12</span>:<span style="color: #800080;">06</span> <span style="color: #800080;">5</span><span style="color: #000000;">.txt
</span>-rw-r--r-- <span style="color: #800080;">1</span> rsync rsync <span style="color: #800080;"></span> Oct <span style="color: #800080;">17</span> <span style="color: #800080;">12</span>:<span style="color: #800080;">06</span> <span style="color: #800080;">6</span>.txt</pre>
  </div>
</div>

## <span id="25_whilekill">2.5 利用while循环语句编写的脚本停止方法（kill）</span>

<div>
  <p class="a1">
    　　01. ctrl+z暂停程序运行，kill -9杀死
  </p>
  
  <p class="a1">
    &nbsp; &nbsp; &nbsp; &nbsp;02. 不要暂停程序，直接利用杀手三剑客进行杀进程
  </p>
</div>

**　　　　&nbsp;****说明：**kill三个杀手不是万能的，在进程暂停时，无法杀死；<span style="color: #ff0000;"><strong>kill -9 </strong><strong>（危险）</strong></span>

### <span id="251">2.5.1 查看后台都要哪些程序在运行</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">[root@nfs01 data]# jobs
[</span><span style="color: #800080;">1</span>]+  Running                 <span style="color: #0000ff;">sh</span> /server/scripts/inotify.<span style="color: #0000ff;">sh</span> &</pre>
  </div>
</div>

### <span id="252_fg">2.5.2 fg将后台的程序调到前台来</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@nfs01 data]# fg <span style="color: #800080;">1</span>
<span style="color: #0000ff;">sh</span> /server/scripts/inotify.<span style="color: #0000ff;">sh</span></pre>
  </div>
</div>

## <span id="26">2.6 进程的前台和后台运行方法：</span>

<div>
  <p class="a">
    &nbsp;&nbsp;&nbsp;　　 fg &nbsp;&nbsp;&nbsp;-- 前台
  </p>
  
  <p class="a">
    &nbsp;&nbsp;　　&nbsp; bg&nbsp;&nbsp;&nbsp; -- 后台
  </p>
</div>

### <span id="261">2.6.1 脚本后台运行方法</span>

<div>
  <p class="a1">
    <span class="cnblogs_code">&nbsp;<span style="color: #800080;">01</span>. <span style="color: #0000ff;">sh</span> inotify.<span style="color: #0000ff;">sh</span> & </span>
  </p>
  
  <p class="a1">
    <span class="cnblogs_code"><span style="color: #800080;">&nbsp;02</span>. nohup <span style="color: #0000ff;">sh</span> inotify.<span style="color: #0000ff;">sh</span> & </span>
  </p>
  
  <p class="a1">
    <span class="cnblogs_code"><span style="color: #800080;">&nbsp;03</span>. screen实现脚本程序后台运行</span>&nbsp;
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">sh</span> /server/scripts/inotify.<span style="color: #0000ff;">sh</span> &<span style="color: #000000;">
nohup
nohup </span><span style="color: #0000ff;">sh</span> inotify.<span style="color: #0000ff;">sh</span> &</pre>
  </div>
</div>

## <span id="27_screen">2.7 screen实现脚本程序后台运行</span>

### <span id="271_yumscreenscreen">2.7.1 经过yum查找发现screen命令属于screen包</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# <span style="color: #0000ff;">yum</span><span style="color: #000000;"> provides screen
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
 </span>*<span style="color: #000000;"> base: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> epel: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> extras: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> updates: mirrors.aliyun.com
base                                                      </span>| <span style="color: #800080;">3.7</span> kB     <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span><span style="color: #000000;">     
epel                                                      </span>| <span style="color: #800080;">4.3</span> kB     <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span><span style="color: #000000;">     
extras                                                    </span>| <span style="color: #800080;">3.4</span> kB     <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span><span style="color: #000000;">     
updates                                                   </span>| <span style="color: #800080;">3.4</span> kB     <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span><span style="color: #000000;">     
screen</span>-<span style="color: #800080;">4.0</span>.<span style="color: #800080;">3</span>-<span style="color: #800080;">19</span><span style="color: #000000;">.el6.x86_64 : A screen manager that supports multiple logins on
                           : one terminal
Repo        : base
Matched from:</span></pre>
  </div>
</div>

### <span id="272_screen">2.7.2 安装screen软件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> -y  screen</pre>
  </div>
</div>

### <span id="273_screen">2.7.3 screen命令的参数</span>

在shell中输入 screen即可进入screen 视图

<div>
  <div class="cnblogs_code">
    <pre>[root@test ~]# screen</pre>
  </div>
</div>

**Screen****实现后台运行程序的简单步骤:**

<div>
  <p class="a">
    &nbsp; 　　screen -ls ：可看screen会话
  </p>
  
  <p class="a">
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;screen +a d&nbsp; ：退出当前的screen，保持其后台运行
  </p>
  
  <p class="a">
    &nbsp;　　 screen -r ID :指定进入哪个screen会话
  </p>
</div>

**Screen****命令中用到的快捷键**

<div>
  <p class="a">
    　　&nbsp; Ctrl+a c ：创建窗口
  </p>
  
  <p class="a">
    &nbsp;　　 Ctrl+a w ：窗口列表
  </p>
  
  <p class="a">
    　　&nbsp; Ctrl+a n ：下一个窗口
  </p>
  
  <p class="a">
    &nbsp;　　 Ctrl+a p ：上一个窗口
  </p>
  
  <p class="a">
    &nbsp; 　　Ctrl+a 0-9 ：在第0个窗口和第9个窗口之间切换
  </p>
  
  <p class="a">
    &nbsp;　　 Ctrl+a K(大写) ：关闭当前窗口，并且切换到下一个窗口 ，
  </p>
  
  <p class="a">
    　　　　（当退出最后一个窗口时，该终端自动终止，并且退回到原始shell状态）
  </p>
  
  <p class="a">
    &nbsp;　　 exit ：关闭当前窗口，并且切换到下一个窗口
  </p>
  
  <p class="a">
    　　　　（当退出最后一个窗口时，该终端自动终止，并且退回到原始shell状态）
  </p>
  
  <p class="a">
    &nbsp;　　<span style="background-color: #ffff00;"> Ctrl+a d</span> ：退出当前终端，返回加载screen前的shell命令状态
  </p>
  
  <p class="a">
    &nbsp;　　 Ctrl+a " : 窗口列表不同于w
  </p>
</div>

<div style="mso-element: para-border-div; border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; mso-border-shadow: yes; background: #F2F2F2; mso-shading: windowtext; mso-pattern: gray-5 auto;">
  <p class="a">
    <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">提示信息：</span>
  </p>
  
  <p class="a" style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span lang="EN-US">sersync</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">软件实际上就是在</span><span lang="EN-US"> inotify</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">软件基础上进行开发的，功能要更加强大些</span> <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">，多了定时重传机制，过滤机制了提供接口做</span><span lang="EN-US"> CDN</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">，支持多线程橾作。</span>
  </p>
  
  <p class="a" style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; text-align: right;">
    <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">&nbsp;<strong>&nbsp;本文出自&ldquo;惨绿少年&rdquo;，欢迎转载，转载请注明出处！http://blog.znix.top</strong></span>
  </p>
</div>

我的博客即将同步至腾讯云+社区，邀请大家一同入驻。

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11">1.1 什么是实时同步：如何实现实时同步</a>
    </li>
    <li>
      <a href="#12">1.2 实现实时同步的方法</a><ul>
        <li>
          <a href="#121">1.2.1 实时同步原理介绍</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_inotifyrsync">1.3 inotify+rsync 方式实现数据同步</a><ul>
        <li>
          <a href="#131_Inotify">1.3.1 Inotify简介</a>
        </li>
        <li>
          <a href="#132_inotifyrsync">1.3.2 inotify+rsync使用方式</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14_inotify">1.4 部署inotify软件的前提</a><ul>
        <li>
          <a href="#141_nbsp">1.4.1 三个重要文件的说明&nbsp;</a>
        </li>
        <li>
          <a href="#143_142">1.4.3 【官方说明】三个重要文件1.4.2 【服务优化】可以将三个文件的数值调大，监听更大的范围</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15_inotify">1.5 inotify软件介绍及参数说明</a><ul>
        <li>
          <a href="#151">1.5.1 两种安装方式</a>
        </li>
        <li>
          <a href="#152_inotify">1.5.2 inotify主要安装的两个软件</a>
        </li>
        <li>
          <a href="#153_inotifywaitnbsp">1.5.3 inotifywait命令参数说明&nbsp;</a>
        </li>
        <li>
          <a href="#154_-e_nbspnbsp">1.5.4 -e[参数] &nbsp;可以指定的事件类型&nbsp;</a><ul>
            <li>
              <a href="#1541nbsp_inotifywait">1.5.4.1&nbsp; 【实例】inotifywait监控中的事件测试</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#155_inotifywait_--format_nbsp">1.5.5 inotifywait 参数 --format 格式定义参数&nbsp;</a>
        </li>
        <li>
          <a href="#156_inotifywait_--timefmt">1.5.6 inotifywait 参数--timefmt 时间格式参数</a><ul>
            <li>
              <a href="#1561nbsp">1.5.6.1&nbsp; 修改输出的日期格式</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#157_-e_nbsp">1.5.7 -e[参数] 重要监控事件参数汇总表：&nbsp;</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#16_inotifywait">1.6 对inotifywait命令的测试</a><ul>
        <li>
          <a href="#161_darr">1.6.1 、创建文件的逻辑&darr;</a>
        </li>
        <li>
          <a href="#162_darr">1.6.2 创建目录逻辑&darr;</a>
        </li>
        <li>
          <a href="#163_darr">1.6.3 监控子目录下的文件&darr;</a>
        </li>
        <li>
          <a href="#164_sed">1.6.4 sed命令修改逻辑</a>
        </li>
        <li>
          <a href="#165_inotifywait_-e">1.6.5 inotifywait监控中 -e 的参数使用</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#17">1.7 实时同步命令参数示意图</a>
    </li>
    <li>
      <a href="#2_inotifyrsync">第2章 inotify+rsync实时同步服务部署</a>
    </li>
    <li>
      <a href="#21_rsync">2.1 第一个里程碑：部署rsync服务</a><ul>
        <li>
          <a href="#211_rsync">2.1.1 rsync服务端部署</a>
        </li>
        <li>
          <a href="#212_rsync">2.1.2 rsync客户端配置</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#22_inotify">2.2 第二个里程碑：部署inotify服务</a><ul>
        <li>
          <a href="#221_inotify">2.2.1 安装inotify软件</a>
        </li>
        <li>
          <a href="#222_inotifyinotifywaitinotifywatch">2.2.2 查看inotify安装上的两个命令(inotifywait,inotifywatch)</a><ul>
            <li>
              <a href="#2221nbsp_inotifywaitinotifywatch">2.2.2.1&nbsp; inotifywait和inotifywatch的作用：</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#23_rsyncinotify">2.3 第三个里程碑：编写脚本，实现rsync+inotify软件功能结合</a><ul>
        <li>
          <a href="#231_rsync">2.3.1 rsync服务命令：</a>
        </li>
        <li>
          <a href="#232_inotify">2.3.2 inotify服务命令：</a>
        </li>
        <li>
          <a href="#233">2.3.3 编写脚本：</a>
        </li>
        <li>
          <a href="#234">2.3.4 对脚本进行优化</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#24">2.4 第四个里程碑：测试编写的脚本</a><ul>
        <li>
          <a href="#241">2.4.1 让脚本在后台运行</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#25_whilekill">2.5 利用while循环语句编写的脚本停止方法（kill）</a><ul>
        <li>
          <a href="#251">2.5.1 查看后台都要哪些程序在运行</a>
        </li>
        <li>
          <a href="#252_fg">2.5.2 fg将后台的程序调到前台来</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#26">2.6 进程的前台和后台运行方法：</a><ul>
        <li>
          <a href="#261">2.6.1 脚本后台运行方法</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#27_screen">2.7 screen实现脚本程序后台运行</a><ul>
        <li>
          <a href="#271_yumscreenscreen">2.7.1 经过yum查找发现screen命令属于screen包</a>
        </li>
        <li>
          <a href="#272_screen">2.7.2 安装screen软件</a>
        </li>
        <li>
          <a href="#273_screen">2.7.3 screen命令的参数</a>
        </li>
      </ul>
    </li>
  </ul>
</div>