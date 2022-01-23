---
title: inotify+rsync实现实时同步
author: 惨绿少年
type: post
date: 2017-11-22T18:04:00+00:00
url: /clsn/lx754.html
views:
  - 113
Baidusubmit:
  - 1
categories:
  - Linux运维
  - 安全
  - 运维基本功

---
# <span id="1">第1章 <span style="font-family: '微软雅黑',sans-serif;">数据实时同步</span><span style="font-family: '微软雅黑',sans-serif;">介绍</span></span>

## <span id="11">1.1 <span style="font-family: '微软雅黑',sans-serif;">什么是实时同步：如何实现实时同步</span></span>

<p style="margin-left: 46.1pt; text-indent: -21.0pt;">
  A.&nbsp;<span style="font-family: '微软雅黑',sans-serif;">要利用监控服务（</span>inotify<span style="font-family: '微软雅黑',sans-serif;">），监控同步数据服务器目录中信息的变化</span>
</p>

<p style="margin-left: 46.1pt; text-indent: -21.0pt;">
  B.&nbsp;<span style="font-family: '微软雅黑',sans-serif;">发现目录中数据产生变化，就利用</span>rsync<span style="font-family: '微软雅黑',sans-serif;">服务推送到备份服务器上</span>
</p>

## <span id="12">1.2 <span style="font-family: '微软雅黑',sans-serif;">实现实时同步的方法</span></span>

<p style="margin-left: 24.0pt;">
  &nbsp;inotify+rsync <span style="font-family: '微软雅黑',sans-serif;">方式实现数据同步</span>
</p>

<p style="margin-left: 24.0pt;">
  &nbsp;sersync <span style="font-family: '微软雅黑',sans-serif;">方式实现实时数据同步</span>
</p>

### <span id="121">1.2.1 <span style="font-family: '微软雅黑',sans-serif;">实时同步原理介绍</span></span>

<p style="text-align: center;">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201802/1190037-20180208094604529-2106245770.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="inotify+rsync实现实时同步" alt="" />
</p>

## <span id="13_inotifyrsync">1.3 inotify+rsync <span style="font-family: '微软雅黑',sans-serif;">方式实现数据同步</span></span>

### <span id="131_Inotify">1.3.1 Inotify<span style="font-family: '微软雅黑',sans-serif;">简介</span></span>

<p style="margin-left: 7.1pt; text-indent: 21.0pt;">
  Inotify<span style="font-family: '微软雅黑',sans-serif;">是一种强大的，细粒度的。异步的文件系统事件监控机制，</span>linux<span style="font-family: '微软雅黑',sans-serif;">内核从</span>2.6.13<span style="font-family: '微软雅黑',sans-serif;">起，加入了</span> Inotify<span style="font-family: '微软雅黑',sans-serif;">支持，通过</span>Inotify<span style="font-family: '微软雅黑',sans-serif;">可以监控文件系统中<span style="background: yellow;">添加、删除，修改、移动</span>等各种事件</span>,<span style="font-family: '微软雅黑',sans-serif;">利用这个内核接口，第三方软件就可以监控文件系统下文件的各种变化情况，而</span> <strong><span style="background: lime;">inotify-tools</span></strong> <span style="font-family: '微软雅黑',sans-serif;">正是实施这样监控的软件。国人周洋在金山公司也开发了类似的实时同步软件</span>sersync<span style="font-family: '微软雅黑',sans-serif;">。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">提示信息：</span>
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    sersync<span style="font-family: '微软雅黑',sans-serif;">软件实际上就是在</span> inotify<span style="font-family: '微软雅黑',sans-serif;">软件基础上进行开发的，功能要更加强大些</span> <span style="font-family: '微软雅黑',sans-serif;">，多了定时重传机制，过滤机制了提供接口做</span> CDN<span style="font-family: '微软雅黑',sans-serif;">，支持多线程橾作。</span>
  </p>
</div>

<p style="text-indent: 21.0pt;">
  Inotify<span style="font-family: '微软雅黑',sans-serif;">实际是一种事件驱动机制，它为应用程序监控文件系统事件提供了实时响应事件的机制，而无须通过诸如</span>cron<span style="font-family: '微软雅黑',sans-serif;">等的轮询机制来获取事件。</span>cron<span style="font-family: '微软雅黑',sans-serif;">等机制不仅无法做到实时性，而且消耗大量系统资源。相比之下，</span>inotify<span style="font-family: '微软雅黑',sans-serif;">基于事件驱动，可以做到对事件处理的实时响应，也没有轮询造成的系统资源消耗，是非常自然的事件通知接口，也与自然世界事件机制相符合。</span>
</p>

inotify<span style="font-family: '微软雅黑',sans-serif;">的实现有几款软件：</span>

<div class="cnblogs_code">
  <pre>inotify-tools，sersync，lrsyncd</pre>
</div>

### <span id="132_inotifyrsync">1.3.2 inotify+rsync<span style="font-family: '微软雅黑',sans-serif;">使用方式</span></span>

inotify <span style="font-family: '微软雅黑',sans-serif;">对同步数据目录信息的监控</span>

rsync&nbsp; <span style="font-family: '微软雅黑',sans-serif;">完成对数据信息的实时同步</span>

<p style="text-indent: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">利用<span style="color: #00b0f0;">脚本</span>进行结合</span>
</p>

## <span id="14_inotify">1.4 <span style="font-family: '微软雅黑',sans-serif;">部署</span>inotify<span style="font-family: '微软雅黑',sans-serif;">软件的前提</span></span>

<p style="text-indent: 12.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">需要</span>2.6.13<span style="font-family: '微软雅黑',sans-serif;">以后内核版本才能支持</span>inotify<span style="font-family: '微软雅黑',sans-serif;">软件。</span>2.6.13<span style="font-family: '微软雅黑',sans-serif;">内核之后版本，在没有安装</span>inotify<span style="font-family: '微软雅黑',sans-serif;">软件之前，应该有这三个文件。</span>
</p>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /proc/sys/fs/inotify/</span>
<span style="color: #000000;">total 0
</span>-rw-r--r-- 1 root root 0 Oct 17 10:12<span style="color: #000000;"> max_queued_events
</span>-rw-r--r-- 1 root root 0 Oct 17 10:12<span style="color: #000000;"> max_user_instances
</span>-rw-r--r-- 1 root root 0 Oct 17 10:12 max_user_watches</pre>
</div>

### <span id="141">1.4.1 <span style="font-family: '微软雅黑',sans-serif;">三个重要文件的说明</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 140.45pt; border-width: 1pt; border-style: solid; border-color: windowtext; background: #a6a6a6; padding: 0cm 5.4pt;" width="187">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">文件</span></strong>
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #a6a6a6; padding: 0cm 5.4pt;" width="77">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">默认值</span></strong>
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #a6a6a6; padding: 0cm 5.4pt;" width="433">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">作用说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 140.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="187">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        max_user_watches
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="77">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        8192
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="433">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">设置</span>inotifywait<span style="font-family: '微软雅黑',sans-serif;">或</span>inotifywatch<span style="font-family: '微软雅黑',sans-serif;">命令可以监视的文件数量（单进程）</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 140.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="187">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        max_user_instances
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="77">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        128
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="433">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">设置每个用户可以运行的</span>inotifywait<span style="font-family: '微软雅黑',sans-serif;">或</span>inotifywatch<span style="font-family: '微软雅黑',sans-serif;">命令的进程数</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 140.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="187">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        max_queued_events
      </p>
    </td>
    
    <td style="width: 57.75pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="77">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        16384
      </p>
    </td>
    
    <td style="width: 324.6pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="433">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">设置</span>inotify<span style="font-family: '微软雅黑',sans-serif;">实例事件（</span>event<span style="font-family: '微软雅黑',sans-serif;">）队列可容纳的事件数量</span>
      </p>
    </td>
  </tr>
</table>

### <span id="142">1.4.2 <span style="font-family: '微软雅黑',sans-serif;">【服务优化】可以将三个文件的数值调大，监听更大的范围</span></span>

### <span id="143">1.4.3 <span style="font-family: '微软雅黑',sans-serif;">【官方说明】三个重要文件</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> man proc</span>
/proc/sys/fs/inotify (since Linux 2.6.13<span style="color: #000000;">)
       This   directory   contains    files    max_queued_events,
       max_user_instances, </span><span style="color: #0000ff;">and</span><span style="color: #000000;"> max_user_watches, that can be used
       to limit the amount of kernel memory consumed by the  inotify interface. 
</span><span style="color: #0000ff;">for</span> further details, see inotify(7).</pre>
</div>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">通过</span>man<span style="font-family: '微软雅黑',sans-serif;">手册的第</span>7<span style="font-family: '微软雅黑',sans-serif;">级别中查到</span> inotify<span style="font-family: '微软雅黑',sans-serif;">的默认文件的详细说明。</span>
  </p>
</div>

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> man 7 inotify</span>
/proc/sys/fs/inotify/<span style="color: #000000;">max_queued_events
       The  value  </span><span style="color: #0000ff;">in</span> this file <span style="color: #0000ff;">is</span><span style="color: #000000;"> used when an application calls
       inotify_init(</span>2<span style="color: #000000;">) to set an upper limit  on  the  number  of
       events  that  can  be  queued to the corresponding inotify
       instance.  Events </span><span style="color: #0000ff;">in</span><span style="color: #000000;"> excess of this limit are dropped, but
       an IN_Q_OVERFLOW event </span><span style="color: #0000ff;">is</span><span style="color: #000000;"> always generated.
 
</span>/proc/sys/fs/inotify/<span style="color: #000000;">max_user_instances
       This  specifies  an  upper  limit on the number of inotify
       instances that can be created per real user ID.
 
</span>/proc/sys/fs/inotify/<span style="color: #000000;">max_user_watches
       This specifies an upper limit on  the  number  of  watches
       that can be created per real user ID.</span></pre>
</div>

## <span id="15_inotify">1.5 inotify<span style="font-family: '微软雅黑',sans-serif;">软件介绍</span><span style="font-family: '微软雅黑',sans-serif;">及参数说明</span></span>

### <span id="151">1.5.1 <span style="font-family: '微软雅黑',sans-serif;">两种安装方式</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    1<span style="font-family: '微软雅黑',sans-serif;">）</span> <span style="background: lime;">yum install -y inotify-tools</span>
  </p>
  
  <p>
    2<span style="font-family: '微软雅黑',sans-serif;">）</span> <span style="font-family: '微软雅黑',sans-serif;">手工编译安装</span>
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">注：</span>&nbsp; &nbsp; YUM <span style="font-family: '微软雅黑',sans-serif;">安装需要有</span>epel<span style="font-family: '微软雅黑',sans-serif;">源</span>

<div class="cnblogs_code">
  <pre>http://mirrors.aliyun.com</pre>
</div>

<p style="text-indent: 24.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">手工编译安装方式需要到</span>github<span style="font-family: '微软雅黑',sans-serif;">上进行下载软件包</span>
</p>

&nbsp;&nbsp;&nbsp; inotify<span style="font-family: '微软雅黑',sans-serif;">软件的参考资料链接：</span>

<div class="cnblogs_code">
  <pre>https://github.com/rvoicilas/inotify-tools/wiki</pre>
</div>

### <span id="152_inotify">1.5.2 inotify<span style="font-family: '微软雅黑',sans-serif;">主要安装的两个软件</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <strong>inotifywait</strong><strong><span style="font-family: '微软雅黑',sans-serif;">：</span> </strong><strong><span style="font-family: '微软雅黑',sans-serif;">（主要）</span></strong>
  </p>
  
  <p>
    &nbsp;<span style="font-family: '微软雅黑',sans-serif;">在被监控的文件或目录上等待特定文件系统事件（</span>open close delete<span style="font-family: '微软雅黑',sans-serif;">等）发生，执行后处于阻塞状态，适合在</span>shell<span style="font-family: '微软雅黑',sans-serif;">脚本中使用</span>
  </p>
  
  <p>
    inotifywatch<span style="font-family: '微软雅黑',sans-serif;">：</span>
  </p>
  
  <p>
    <span style="font-family: '微软雅黑',sans-serif;">收集被监控的文件系统使用的统计数据，指文件系统事件发生的次数统计。</span>
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">说明：在实时实时同步的时候，主要是利用</span>inotifywait<span style="font-family: '微软雅黑',sans-serif;">对目录进行监控</span>

### <span id="153_inotifywait">1.5.3 inotifywait<span style="font-family: '微软雅黑',sans-serif;">命令参数说明</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 162.8pt; border-width: 1pt; border-style: solid; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">参数</span></strong>
      </p>
    </td>
    
    <td style="width: 360pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="480">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">含义</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        -m, --monitor
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Keep listening for events forever.&nbsp; Without this option, inotifywait will exit after one event is received.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">始终保持事件监听。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        -d, --daemon
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        111
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        -r, --recursive
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Watch all subdirectories of any directories passed as arguments.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">递归监控目录数据信息变化</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        -o, --outfile <file>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Print events to <file> rather than stdout.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">打印事件到文件中，相当于标准正确输出</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        -s, --syslog
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Output errors to syslog(3) system log module rather than stderr.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">发送错误到</span>syslog<span style="font-family: '微软雅黑',sans-serif;">相当于标准错误输出</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        -q, --quiet
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        If specified once, the program will be less verbose.&nbsp; Specifically, it will not state&nbsp; when&nbsp; it&nbsp; has&nbsp; completed establishing all inotify watches.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">输出信息少（只打印事件信息）</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        --exclude <pattern>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Exclude all events on files matching the extended regular expression <pattern>.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">排除文件或目录</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        --excludei <pattern>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Like --exclude but case insensitive.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">排除文件或目录时，不区分大小写</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        --timefmt <fmt>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Print using a specified printf-like format string; read the man page for more details.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">指定时间输出格式</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        --format <fmt>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Print using a specified printf-like formatstring; read the man page for more details.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">打印使用指定的输出类似格式字符串；即实际监控输出内容</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 162.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="217">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>-e</strong>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 360.0pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="480">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        Listen for specific event(s).&nbsp; If omitted, all events are listened for.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">指定监听指定的事件，如果省略，表示所有事件都进行监听</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" width="697">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">以上的信息可以通过</span> inotifywait --help&nbsp; <span style="font-family: '微软雅黑',sans-serif;">获得</span>
      </p>
    </td>
  </tr>
</table>

### <span id="154_-e_nbsp">1.5.4 -e[<span style="font-family: '微软雅黑',sans-serif;">参数</span>] &nbsp;<span style="font-family: '微软雅黑',sans-serif;">可以指定的事件</span><span style="font-family: '微软雅黑',sans-serif;">类型</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 106.1pt; border-width: 1pt; border-style: solid; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">事件名称</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="556">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">事件说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        access
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory contents were read
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录内容被读取</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        modify
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory contents were writterv
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录内容被写入</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        attrib
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory attributes changed
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录属性改变</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        close_write
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory closed, after being opened in writeable mode.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录关闭，在写入模式打开之后关闭的。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        close_nowrite
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory closed, after being opened in read-only mode.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录关闭，在只读模式打开之后关闭的</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        close
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory closed, regardless of read/write mode
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录关闭，不管读或是写模式</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        open
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory opened
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录被打开</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        moved_to
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">拉</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        file or directory moved to watched directory
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录被移动到监控的目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p>
        moved_from
      </p>
      
      <p>
        <span style="font-family: '微软雅黑',sans-serif;">推</span>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="556">
      <p>
        file or directory moved from watched directory
      </p>
      
      <p>
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录被移动从监控的目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p>
        move
      </p>
      
      <p>
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="556">
      <p>
        file or directory moved to or from watched directory
      </p>
      
      <p>
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录不管移动到或是移出监控目录都触发事件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p>
        create
      </p>
      
      <p>
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="556">
      <p>
        file or directory created within watched directory
      </p>
      
      <p>
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录创建在监控目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p>
        delete
      </p>
      
      <p>
        <strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="556">
      <p>
        file or directory deleted within watched directory
      </p>
      
      <p>
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录被删除在监控目录中</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p>
        delete_self
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="556">
      <p>
        file or directory was deleted
      </p>
      
      <p>
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录被删除，目录本身被删除</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" valign="top" width="141">
      <p>
        unmount
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="556">
      <p>
        file system containing file or directory unmounted
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" valign="top" width="697">
      <p>
        <span style="font-family: '微软雅黑',sans-serif;">以上的信息可以通过</span> inotifywait --help&nbsp; <span style="font-family: '微软雅黑',sans-serif;">获得</span>
      </p>
    </td>
  </tr>
</table>

#### <span id="1541nbspinotifywait"><span style="courier new"4courier new";background: olive;">1.5.4.1&nbsp;</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: olive;">【**<span style="color: #00b0f0;">实例</span>**】</span><span style="background: olive;">inotifywait</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: olive;">监控中的事件测试</span></span>

1<span style="font-family: '微软雅黑',sans-serif;">、创建事件</span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> touch test2.txt</span>
[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data --timefmt "%d-%m-%y %H:%M" --format "%T %w%f 事件信息: %e" -e create</span>
17-10-17 11:19 /data/test2.txt 事件信息: CREATE</pre>
</div>

2<span style="font-family: '微软雅黑',sans-serif;">、删除事件</span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> \rm -f test1.txt</span>
[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data --timefmt "%d-%m-%y %H:%M" --format "%T %w%f 事件信息: %e" -e delete</span>
17-10-17 11:28 /data/test1.txt 事件信息: DELETE</pre>
</div>

3<span style="font-family: '微软雅黑',sans-serif;">、修改事件</span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> echo "132" > test.txt</span>
[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data --timefmt "%d-%m-%y %H:%M" --format "%T %w%f 事件信息: %e" -e close_write</span>
17-10-17 11:30 /data/test.txt 事件信息: CLOSE_WRITE,CLOSE</pre>
</div>

4<span style="font-family: '微软雅黑',sans-serif;">、移动事件</span> moved_to

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> mv /etc/hosts .</span>
[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data --timefmt "%d-%m-%y %H:%M" --format "%T %w%f 事件信息: %e" -e moved_to</span>
17-10-17 11:33 /data/hosts 事件信息: MOVED_TO</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif;">移动事件</span> moved_from
</p>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> mv ./hosts  /tmp/</span>
[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data --timefmt "%d-%m-%y %H:%M" --format "%T %w%f 事件信息: %e" -e moved_from</span>
17-10-17 11:34 /data/hosts 事件信息: MOVED_FROM</pre>
</div>

### <span id="155_inotifywait_--format">1.5.5 inotifywait <span style="font-family: '微软雅黑',sans-serif;">参数</span> --format <fmt><span style="font-family: '微软雅黑',sans-serif;">格式定义参数</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 134.45pt; border-width: 1pt; border-style: solid; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="179">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">命令参数</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="518">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">参数说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %w<strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">事件出现时，监控文件或目录的名称信息</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %f<strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">事件出现时，将显示监控目录下触发事件的文件或目录信息，否则为空</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %e<strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">显示发生的事件信息，不同的事件信息用逗号进行分隔</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %Xe
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">显示发生的事件信息，不同的事件信息有</span>x<span style="font-family: '微软雅黑',sans-serif;">进行分隔，可以修改</span>X<span style="font-family: '微软雅黑',sans-serif;">为指定分隔符</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 134.45pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="179">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %T<strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 388.35pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="518">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">输出时间格式中定义的时间格式信息，通过</span> --timefmt option <span style="font-family: '微软雅黑',sans-serif;">语法格式指定时间信息</span>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">这个格式是通过</span>strftime<span style="font-family: '微软雅黑',sans-serif;">函数进行匹配时间格式信息的</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" valign="top" width="697">
      <p>
        <strong><span style="font-family: '微软雅黑',sans-serif;">以上的信息可以通过</span> inotifywait --help&nbsp; </strong><strong><span style="font-family: '微软雅黑',sans-serif;">获得</span></strong>
      </p>
    </td>
  </tr>
</table>

### <span id="156_inotifywait_--timefmt">1.5.6 inotifywait <span style="font-family: '微软雅黑',sans-serif;">参数</span>--timefmt <fmt><span style="font-family: '微软雅黑',sans-serif;">时间格式参数</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 106.1pt; border-width: 1pt; border-style: solid; border-color: windowtext; background: #a6a6a6; padding: 0cm 5.4pt;" valign="top" width="141">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">命令参数</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #a6a6a6; padding: 0cm 5.4pt;" valign="top" width="556">
      <p style="text-align: center;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif;">参数说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %d<strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        The day of the month as a decimal number(range 01 to 31)
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">每月的第几天，显示倍息为十进制数（范围是</span> 01-31 )
      </p>
    </td>
  </tr>
  
  <tr style="height: 39.15pt;">
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt; height: 39.15pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %m<strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt; height: 39.15pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        The month as a decimal number (range 01 to 12).
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">显示月份，显示信息为十进制（范围</span> 01-12 )
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %M
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        The minute as a decimal number (range 00 to 59).
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">显示分钟，显示信息为十进制（范围</span> 00-59 )
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %y<strong><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";color: red;">（重要参数）</span></strong>
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        The year as a decimal number without a century (range 00 to 99).
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">年份信息，显示信息为十进制，并且没有世纪信息</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %Y
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        The year as a decimal number including the century.
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">年份信息，显示信息为十进制，并且包含世纪信息</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 106.1pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="141">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        %H
      </p>
    </td>
    
    <td style="width: 416.7pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="556">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        The hour as a decimal number using a 24-hour clock (range 00 to 23).
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">小时信息，显示信息为十进制，使用</span> 24<span style="font-family: '微软雅黑',sans-serif;">小时制（范围</span> 00-23 )
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="2" valign="top" width="697">
      <p>
        <strong><span style="font-family: '微软雅黑',sans-serif;">说明：以上信息可以通过</span> man strftime</strong><strong><span style="font-family: '微软雅黑',sans-serif;">信息获取</span></strong>
      </p>
    </td>
  </tr>
</table>

#### <span id="1561nbsp">1.5.6.1&nbsp;<span style="font-family: '微软雅黑',sans-serif;">修改输出的日期格式</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data --timefmt "%d/%m/%y %H:%M" --format "%T %w%f"</span>
17/10/17 11:12 /data/test1.txt</pre>
</div>

### <span id="157_-e">1.5.7 -e[<span style="font-family: '微软雅黑',sans-serif;">参数</span>] <span style="font-family: '微软雅黑',sans-serif;">重要监控事件参数汇总表：</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 99pt; border-width: 1pt; border-style: solid; border-color: windowtext; background: #bfbfbf; padding: 0cm 5.4pt;" width="132">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">重要事件</span></strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="161">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">包含事件</span></strong>
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top: 1pt solid windowtext; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: none; background: #bfbfbf; padding: 0cm 5.4pt;" width="404">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong><span style="font-family: '微软雅黑',sans-serif;">备注说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="132">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>close</strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="161">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        close_write&nbsp;&nbsp;
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        close_nowrite
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="404">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录关闭，不管读或是写模式</span>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">即包含写关闭与读关闭</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="132">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>close_write</strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="161">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        create
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="404">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">包含文件创建事件，但不包含目录创建事件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; padding: 0cm 5.4pt;" width="132">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <strong>move</strong>
      </p>
    </td>
    
    <td style="width: 120.5pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="161">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        moved_to&nbsp;&nbsp;
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        moved_from
      </p>
    </td>
    
    <td style="width: 303.3pt; border-top: none; border-left: none; border-bottom: solid windowtext 1.0pt; border-right: solid windowtext 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="404">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">文件或目录不管移动到或是移动出监控目录都触发事件</span>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">即包含信息移入或移出监控目录事件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 522.8pt; border-right: 1pt solid windowtext; border-bottom: 1pt solid windowtext; border-left: 1pt solid windowtext; border-top: none; background: #92d050; padding: 0cm 5.4pt;" colspan="3" width="697">
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">重要参数汇总：根据以上说明，在实际使用时，只要监控以下事件即可</span>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        create <span style="font-family: '微软雅黑',sans-serif;">创建、</span> delete <span style="font-family: '微软雅黑',sans-serif;">删除、</span> movedjto <span style="font-family: '微软雅黑',sans-serif;">移入、</span> close_write <span style="font-family: '微软雅黑',sans-serif;">修</span> <span style="font-family: '微软雅黑',sans-serif;">改</span>
      </p>
      
      <p style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif;">inotifywait -mrq /data --format "%w%f"&nbsp; -e create,delete,moved_to,close_write</span>
      </p>
    </td>
  </tr>
</table>

## <span id="16_inotifywait">1.6 <span style="font-family: '微软雅黑',sans-serif;">对</span>inotifywait<span style="font-family: '微软雅黑',sans-serif;">命令的测试</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">对</span>inotifywait<span style="font-family: '微软雅黑',sans-serif;">命令测试的说明：</span>
</p>

<p style="margin-left: 21.0pt;">
  &nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">需要打开两个连接窗口</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <strong><span style="color: #00b0f0;">1</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; color: #00b0f0;">窗口</span></strong><span style="font-family: '微软雅黑',sans-serif;">运行</span>inotifywait
  </p>
  
  <p>
    <strong><span style="color: yellow;">2</span></strong><strong><span style="font-family: '微软雅黑',sans-serif; color: yellow;">窗口</span></strong><span style="font-family: '微软雅黑',sans-serif;">对文件夹进行操作，可在一窗口中查看出</span>inotifywait<span style="font-family: '微软雅黑',sans-serif;">的监控记录</span>
  </p>
</div>

### <span id="161_darr">1.6.1 <span style="font-family: '微软雅黑',sans-serif;">、创建文件的逻辑&darr;</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait /data</span>
<span style="color: #000000;">Setting up watches.
Watches established.
</span>/data/<span style="color: #000000;"> CREATE test1.txt
</span>/data/<span style="color: #000000;"> OPEN test1.txt
</span>/data/<span style="color: #000000;"> ATTRIB test1.txt
</span>/data/<span style="color: #000000;"> CLOSE_WRITE,CLOSE test1.txt
创建文件，inotifywait显示创建文件的过程&uarr;
[root@nfs01 data]</span><span style="color: #008000;">#</span><span style="color: #008000;"> touch test1.txt</span></pre>
</div>

### <span id="162_darr">1.6.2 <span style="font-family: '微软雅黑',sans-serif;">创建目录逻辑&darr;</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir testdir</span>
[root@nfs01 ~]<span style="color: #008000;">#
</span>/data/ CREATE,ISDIR testdir</pre>
</div>

### <span id="163_darr">1.6.3 <span style="font-family: '微软雅黑',sans-serif;">监控子目录下的文件&darr;</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> touch  testdir/test01.txt</span>
[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data</span>
/data/testdir/<span style="color: #000000;"> OPEN test01.txt
</span>/data/testdir/<span style="color: #000000;"> ATTRIB test01.txt
</span>/data/testdir/ CLOSE_WRITE,CLOSE test01.txt</pre>
</div>

### <span id="164_sed"><span style="courier new"4courier new";background: fuchsia;">1.6.4 </span><span style="background: fuchsia;">sed</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: fuchsia;">命令修改逻辑</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> sed 's#132#123#g' test.txt -i</span>
<span style="color: #000000;"> 
[root@nfs01 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq  /data --timefmt "%d-%m-%y %H:%M" --format "%T %w%f 事件信息: %e" -e moved_from</span>
 /data/<span style="color: #000000;">test.txt 事件信息: OPEN
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

**<span style="background: lime;">sed</span>****<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">命令替换逻辑</span>** **<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new";background: lime;">：</span>**

01. <span style="font-family: '微软雅黑',sans-serif;">创建临时文件</span>

02. <span style="font-family: '微软雅黑',sans-serif;">将原文件内容放置到临时文件中，修改替换临时文件中的内容，原有文件不做改动</span>

03. <span style="font-family: '微软雅黑',sans-serif;">重命名临时文件，覆盖原文件</span>

### <span id="165_inotifywait_-e">1.6.5 inotifywait<span style="font-family: '微软雅黑',sans-serif;">监控中</span> -e <span style="font-family: '微软雅黑',sans-serif;">的参数使用</span></span>

<div class="cnblogs_code">
  <pre>inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d/%m/%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span> -e create</pre>
</div>

&nbsp;&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">说明：表示只监听</span>create<span style="font-family: '微软雅黑',sans-serif;">事件</span>

<div class="cnblogs_code">
  <pre>inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%d/%m/%y %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %e</span><span style="color: #800000;">"</span></pre>
</div>

&nbsp;&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">说明：不指定</span>-e<span style="font-family: '微软雅黑',sans-serif;">参数，表示监听所有事件</span>

<p style="text-indent: 15.75pt;">
  02. <span style="font-family: '微软雅黑',sans-serif;">删除事件</span>delete
</p>

<div class="cnblogs_code">
  <pre>    <span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq /data --timefmt "%F %H:%M" --format "%T %w%f 事件信息: %@e" -e delete</span>
    2017-10-17 11:28 /data/02<span style="color: #000000;">.txt 事件信息: DELETE
    </span>2017-10-17 11:28 /data/03<span style="color: #000000;">.txt 事件信息: DELETE
    </span>2017-10-17 11:28 /data/04.txt 事件信息: DELETE</pre>
</div>

&nbsp;&nbsp;&nbsp; 03. <span style="font-family: '微软雅黑',sans-serif;">修改事件</span>close_write

<div class="cnblogs_code">
  <pre>  <span style="color: #008000;">#</span><span style="color: #008000;"> inotifywait -mrq /data --timefmt "%F %H:%M" --format "%T %w%f 事件信息: %@e" -e delete,close_write</span>
    2017-10-17 11:30 /data/<span style="color: #000000;">oldgirl.txt 事件信息: CLOSE_WRITE@CLOSE
    </span>2017-10-17 11:30 /data/<span style="color: #000000;">.oldgirl.txt.swx 事件信息: CLOSE_WRITE@CLOSE
    </span>2017-10-17 11:30 /data/<span style="color: #000000;">.oldgirl.txt.swx 事件信息: DELETE
    </span>2017-10-17 11:30 /data/<span style="color: #000000;">.oldgirl.txt.swp 事件信息: CLOSE_WRITE@CLOSE
    </span>2017-10-17 11:30 /data/<span style="color: #000000;">.oldgirl.txt.swp 事件信息: DELETE
    </span>2017-10-17 11:30 /data/<span style="color: #000000;">.oldgirl.txt.swp 事件信息: CLOSE_WRITE@CLOSE
    </span>2017-10-17 11:30 /data/.oldgirl.txt.swp 事件信息: DELETE</pre>
</div>

&nbsp;&nbsp;&nbsp; 04. <span style="font-family: '微软雅黑',sans-serif;">移动事件</span>moved_to

<div class="cnblogs_code">
  <pre>    inotifywait -mrq /data --timefmt <span style="color: #800000;">"</span><span style="color: #800000;">%F %H:%M</span><span style="color: #800000;">"</span> --format <span style="color: #800000;">"</span><span style="color: #800000;">%T %w%f 事件信息: %@e</span><span style="color: #800000;">"</span> -<span style="color: #000000;">e delete,close_write,moved_to
    </span>2017-10-17 11:34 /data/hosts 事件信息: MOVED_TO</pre>
</div>

## <span id="17">1.7 <span style="font-family: '微软雅黑',sans-serif;">实时同步命令参数示意图</span></span>

<p style="text-align: center;">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201710/1190037-20171022092343990-1382877860.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="inotify+rsync实现实时同步" alt="" />
</p>

# <span id="2_inotifyrsync">第2章 inotify+rsync<span style="font-family: '微软雅黑',sans-serif;">实时同步服务部署</span></span>

## <span id="21_rsync">2.1 <span style="font-family: '微软雅黑',sans-serif;">第一个里程碑：部署</span>rsync<span style="font-family: '微软雅黑',sans-serif;">服务</span></span>

### <span id="211_rsync">2.1.1 rsync<span style="font-family: '微软雅黑',sans-serif;">服务端部署</span></span>

1)<span style="font-family: '微软雅黑',sans-serif;">软件是否存在</span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -qa |grep rsync</span>
rsync-3.0.6-12.el6.x86_64</pre>
</div>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="font-family: '微软雅黑',sans-serif; background: yellow;">需求：查询到某个命令非常有用。但是不知道属于哪个软件包</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp;<span class="cnblogs_code"> yum provides rysnc</span>&nbsp;
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; provides&nbsp;&nbsp; Find what package provides the given value
  </p>
</div>

2)<span style="font-family: '微软雅黑',sans-serif;">进行软件服务配置</span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/rsyncd.conf</span>
uid =<span style="color: #000000;"> rsync
gid </span>=<span style="color: #000000;"> rsync
use chroot </span>=<span style="color: #000000;"> no
max connections </span>= 200<span style="color: #000000;">
timeout </span>= 300<span style="color: #000000;">
pid file </span>= /var/run/<span style="color: #000000;">rsyncd.pid
lock file </span>= /var/run/<span style="color: #000000;">rsync.lock
log file </span>= /var/log/<span style="color: #000000;">rsyncd.log
ignore errors
read only </span>=<span style="color: #000000;"> false
list </span>=<span style="color: #000000;"> false
hosts allow </span>= 172.16.1.0/24<span style="color: #000000;">
auth users </span>=<span style="color: #000000;"> rsync_backup
secrets file </span>= /etc/<span style="color: #000000;">rsync.password

[backup]
comment </span>= <span style="color: #800000;">"</span><span style="color: #800000;">backup dir by oldboy</span><span style="color: #800000;">"</span><span style="color: #000000;">
path </span>= /<span style="color: #000000;">backup

[nfsbackup]
comment </span>= <span style="color: #800000;">"</span><span style="color: #800000;">nfsbackup dir by hzs</span><span style="color: #800000;">"</span><span style="color: #000000;">
path </span>= /nfsbackup</pre>
</div>

3)<span style="font-family: '微软雅黑',sans-serif;">创建</span>rsync<span style="font-family: '微软雅黑',sans-serif;">管理用户</span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> useradd -s /sbin/nologin -M rsync</span></pre>
</div>

4)<span style="font-family: '微软雅黑',sans-serif;">创建数据备份储存目录</span>,<span style="font-family: '微软雅黑',sans-serif;">目录修改属主</span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mkdir /nfsbackup/</span>
[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chown -R rsync.rsync /nfsbackup/</span></pre>
</div>

5)<span style="font-family: '微软雅黑',sans-serif;">创建认证用户密码文件并进行授权</span>600

<div class="cnblogs_code">
  <pre>echo <span style="color: #800000;">"</span><span style="color: #800000;">rsync_backup:oldboy123</span><span style="color: #800000;">"</span> >>/etc/<span style="color: #000000;">rsync.password
chmod </span>600 /etc/rsync.password</pre>
</div>

6)<span style="font-family: '微软雅黑',sans-serif;">启动</span>rsync<span style="font-family: '微软雅黑',sans-serif;">服务</span>

<div class="cnblogs_code">
  <pre>rsync --daemon</pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">至此服务端配置完成</span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ps -ef |grep rsync</span>
root       2076      1  0 17:05 ?        00:00:00 rsync --<span style="color: #000000;">daemon
root       </span>2163   1817  0 17:38 pts/1    00:00:00 grep --color=auto rsync</pre>
</div>

### <span id="212_rsync">2.1.2 rsync<span style="font-family: '微软雅黑',sans-serif;">客户端配置</span></span>

1)<span style="font-family: '微软雅黑',sans-serif;">软件是否存在</span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -qa |grep rsync</span>
rsync-3.0.6-12.el6.x86_64</pre>
</div>

2)<span style="font-family: '微软雅黑',sans-serif;">创建安全认证文件，并进行修改权限</span>600

<div class="cnblogs_code">
  <pre>echo <span style="color: #800000;">"</span><span style="color: #800000;">oldboy123</span><span style="color: #800000;">"</span> >>/etc/<span style="color: #000000;">rsync.password
chmod </span>600 /etc/rsync.password</pre>
</div>

3) <span style="font-family: '微软雅黑',sans-serif;">测试数据传输</span>

<div class="cnblogs_code">
  <pre>[root@nfs01 sersync]<span style="color: #008000;">#</span><span style="color: #008000;"> rsync -avz /data  rsync_backup@172.16.1.41::nfsbackup  --password-file=/etc/rsync.password</span>
<span style="color: #000000;">sending incremental file list
data</span>/<span style="color: #000000;">
data</span>/<span style="color: #000000;">.hzs
data</span>/<span style="color: #000000;">.tar.gz
data</span>/.txt</pre>
</div>

## <span id="22_inotify">2.2 <span style="font-family: '微软雅黑',sans-serif;">第二个里程碑：部署</span>inotify<span style="font-family: '微软雅黑',sans-serif;">服务</span></span>

<span style="font-family: '微软雅黑',sans-serif;">首先先确认是否有</span>**epel****<span style="font-family: '微软雅黑',sans-serif;">源</span>**<span style="font-family: '微软雅黑',sans-serif;">用来安装</span>inotify-tools<span style="font-family: '微软雅黑',sans-serif;">软件</span>

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum repolist</span>
<span style="color: #000000;">Loaded plugins: fastestmirror, security
Loading mirror speeds </span><span style="color: #0000ff;">from</span><span style="color: #000000;"> cached hostfile
 </span>*<span style="color: #000000;"> base: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> epel: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> extras: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> updates: mirrors.aliyun.com
repo id  repo name                                       status
base     CentOS</span>-6 - Base - mirrors.aliyun.com             6,706<span style="color: #000000;">
epel     Extra Packages </span><span style="color: #0000ff;">for</span> Enterprise Linux 6 - x86_64  12,401<span style="color: #000000;">
extras   CentOS</span>-6 - Extras - mirrors.aliyun.com              46<span style="color: #000000;">
updates  CentOS</span>-6 - Updates - mirrors.aliyun.com            722<span style="color: #000000;">
repolist: </span>19,875</pre>
</div>

### <span id="221_inotify">2.2.1 <span style="font-family: '微软雅黑',sans-serif;">安装</span>inotify<span style="font-family: '微软雅黑',sans-serif;">软件</span></span>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">两种安装方式</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    1<span style="font-family: '微软雅黑',sans-serif;">）</span> <span style="background: lime;">yum install -y inotify-tools</span>
  </p>
  
  <p>
    2<span style="font-family: '微软雅黑',sans-serif;">）</span> <span style="font-family: '微软雅黑',sans-serif;">手工编译安装</span>
  </p>
</div>

<span style="font-family: '微软雅黑',sans-serif;">注：</span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">手工编译安装方式需要到</span>github<span style="font-family: '微软雅黑',sans-serif;">上进行下载软件包</span>
</p>

&nbsp;&nbsp;&nbsp; inotify<span style="font-family: '微软雅黑',sans-serif;">软件的参考资料链接：</span>

<p style="text-indent: 21.0pt;">
  https://github.com/rvoicilas/inotify-tools/wiki
</p>

### <span id="222_inotifyinotifywaitinotifywatch">2.2.2 <span style="font-family: '微软雅黑',sans-serif;">查看</span>inotify<span style="font-family: '微软雅黑',sans-serif;">安装上的两个命令</span>(inotifywait,inotifywatch)</span>

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -ql inotify-tools</span>
/usr/bin/inotifywait      <span style="color: #008000;">#</span><span style="color: #008000;">主要</span>
/usr/bin/inotifywatch</pre>
</div>

#### <span id="2221nbspinotifywaitinotifywatch">2.2.2.1&nbsp;inotifywait<span style="font-family: '微软雅黑',sans-serif;">和</span>inotifywatch<span style="font-family: '微软雅黑',sans-serif;">的作用：</span></span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">一共安装了2个工具（命令），即inotifywait和inotifywatch
inotifywait : 在被监控的文件或目录上等待特定文件系统事件（open close delete等）发生，
执行后处于阻塞状态，适合在shell脚本中使用
inotifywatch :收集被监控的文件系统使用的统计数据,指文件系统事件发生的次数统计。
说明：yum安装后可以直接使用，如果编译安装需要进入到相应软件目录的bin目录下使用
 
</span><span style="color: #008000;">#</span><span style="color: #008000;">命令 man手册说明</span><span style="color: #008000;">
#</span><span style="color: #008000;"> man inotifywait</span>
inotifywait - wait <span style="color: #0000ff;">for</span><span style="color: #000000;"> changes to files using inotify

使用inotify进行监控，等待产生变化的文件信息
</span><span style="color: #008000;">#</span><span style="color: #008000;"> man inotifywatch</span>
inotifywatch -<span style="color: #000000;"> gather filesystem access statistics using inotify
使用inotify进行监控，收集文件系统访问统计佶息</span></pre>
</div>

## <span id="23_rsyncinotify">2.3 <span style="font-family: '微软雅黑',sans-serif;">第三个里程碑：编写<span style="color: #00b0f0;">脚本</span>，实现</span>rsync+inotify<span style="font-family: '微软雅黑',sans-serif;">软件功能结合</span></span>

### <span id="231_rsync">2.3.1 rsync<span style="font-family: '微软雅黑',sans-serif;">服务命令：</span></span>

<div class="cnblogs_code">
  <pre>rsync -avz --delete /data/ rsync_backup@172.16.1.41::nfsbackup --password-file=/etc/rsync.password</pre>
</div>

### <span id="232_inotify">2.3.2 inotify<span style="font-family: '微软雅黑',sans-serif;">服务命令：</span></span>

<div class="cnblogs_code">
  <pre>inotifywait -mrq /data -format <span style="color: #800000;">"</span><span style="color: #800000;">%w%f</span><span style="color: #800000;">"</span>  -e create,delete,move_to,close_write</pre>
</div>

### <span id="233">2.3.3 <span style="font-family: '微软雅黑',sans-serif;">编写脚本：</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 sersync]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /server/scripts/inotify.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash</span>
inotifywait -mrq /data --format <span style="color: #800000;">"</span><span style="color: #800000;">%w%f</span><span style="color: #800000;">"</span> -e create,delete,moved_to,close_write|<span style="color: #000000;">\
</span><span style="color: #0000ff;">while</span><span style="color: #000000;"> read line
do
        rsync </span>-az --delete /data/ rsync_backup@172.16.1.41::nfsbackup --password-<span style="color: #000000;">
file</span>=/etc/<span style="color: #000000;">rsync.password
done</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">脚本说明：</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    for<span style="font-family: '微软雅黑',sans-serif;">循环会定义一个条件，当条件不满足时停止循环</span>
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    while<span style="font-family: '微软雅黑',sans-serif;">循环：只要条件满足就一直循环下去</span>
  </p>
</div>

### <span id="234">2.3.4 <span style="font-family: '微软雅黑',sans-serif;">对脚本进行优化</span></span>

<div class="cnblogs_code">
  <pre><span style="color: #008000;">#</span><span style="color: #008000;">!/bin/bash</span>
Path=/<span style="color: #000000;">data
backup_Server</span>=172.16.1.41
/usr/bin/inotifywait -mrq --format <span style="color: #800000;">'</span><span style="color: #800000;">%w%f</span><span style="color: #800000;">'</span> -e create,close_write,delete /data  | <span style="color: #0000ff;">while</span><span style="color: #000000;"> read line  
do
    </span><span style="color: #0000ff;">if</span> [ -<span style="color: #000000;">f $line ];then
        rsync </span>-az $line --delete rsync_backup@$backup_Server::nfsbackup --password-file=/etc/<span style="color: #000000;">rsync.password
    </span><span style="color: #0000ff;">else</span><span style="color: #000000;">
        cd $Path </span>&&<span style="color: #000000;">\
        rsync </span>-az ./ --delete rsync_backup@$backup_Server::nfsbackup --password-file=/etc/<span style="color: #000000;">rsync.password
    fi
done</span></pre>
</div>

## <span id="24">2.4 <span style="font-family: '微软雅黑',sans-serif;">第四个里程碑：测试编写的脚本</span></span>

### <span id="241">2.4.1 <span style="font-family: '微软雅黑',sans-serif;">让脚本在后台运行</span></span>

<p style="margin-left: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">在</span>/data <span style="font-family: '微软雅黑',sans-serif;">目录先创建</span>6<span style="font-family: '微软雅黑',sans-serif;">个文件</span>
</p>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> sh  /server/scripts/inotify.sh &</span>
[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> touch {1..6}.txt</span></pre>
</div>

<span style="font-family: '微软雅黑',sans-serif;">在</span>backup<span style="font-family: '微软雅黑',sans-serif;">服务器上，已经时候同步过去了</span>6<span style="font-family: '微软雅黑',sans-serif;">个文件。</span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /nfsbackup/</span>
total 8
-rw-r--r-- 1 rsync rsync 0 Oct 17 12:06 1<span style="color: #000000;">.txt
</span>-rw-r--r-- 1 rsync rsync 0 Oct 17 12:06 2<span style="color: #000000;">.txt
</span>-rw-r--r-- 1 rsync rsync 0 Oct 17 12:06 3<span style="color: #000000;">.txt
</span>-rw-r--r-- 1 rsync rsync 0 Oct 17 12:06 4<span style="color: #000000;">.txt
</span>-rw-r--r-- 1 rsync rsync 0 Oct 17 12:06 5<span style="color: #000000;">.txt
</span>-rw-r--r-- 1 rsync rsync 0 Oct 17 12:06 6.txt</pre>
</div>

## <span id="25_whilekill">2.5 <span style="font-family: '微软雅黑',sans-serif;">利用</span>while<span style="font-family: '微软雅黑',sans-serif;">循环语句编写的脚本停止方法（</span>kill<span style="font-family: '微软雅黑',sans-serif;">）</span></span>

<div class="cnblogs_code">
  <pre>01. ctrl+z暂停程序运行，kill -<span style="color: #000000;">9杀死
</span>02. 不要暂停程序，直接利用杀手三剑客进行杀进程</pre>
</div>

**&nbsp;****<span style="font-family: '微软雅黑',sans-serif;">说明：</span>**kill<span style="font-family: '微软雅黑',sans-serif;">三个杀手不是万能的，在进程暂停时，无法杀死；</span>**<span style="color: red;">kill -9 </span>****<span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; color: red;">（危险）</span>**

### <span id="251">2.5.1 <span style="font-family: '微软雅黑',sans-serif;">查看后台都要哪些程序在运行</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> jobs</span>
[1]+  Running                 sh /server/scripts/inotify.sh &</pre>
</div>

### <span id="252_fg">2.5.2 fg<span style="font-family: '微软雅黑',sans-serif;">将后台的程序调到前台来</span></span>

<div class="cnblogs_code">
  <pre>[root@nfs01 data]<span style="color: #008000;">#</span><span style="color: #008000;"> fg 1</span>
sh /server/scripts/inotify.sh</pre>
</div>

## <span id="26">2.6 <span style="font-family: '微软雅黑',sans-serif;">进程的前台和后台运行方法：</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    &nbsp;&nbsp;&nbsp; fg &nbsp;&nbsp;&nbsp;-- <span style="font-family: '微软雅黑',sans-serif;">前台</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; bg&nbsp;&nbsp;&nbsp; -- <span style="font-family: '微软雅黑',sans-serif;">后台</span>
  </p>
</div>

### <span id="261">2.6.1 <span style="font-family: '微软雅黑',sans-serif;">脚本后台运行方法</span></span>

<div class="cnblogs_code">
  <pre>    01. sh inotify.sh &
    02. nohup sh inotify.sh &
    03. screen实现脚本程序后台运行</pre>
</div>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="background: lime;">sh /server/scripts/inotify.sh &</span>
  </p>
</div>

nohup

<div class="cnblogs_code">
  <pre>nohup sh inotify.sh &</pre>
</div>

## <span id="27_screen">2.7 screen<span style="font-family: '微软雅黑',sans-serif;">实现脚本程序后台运行</span></span>

### <span id="271_yumscreenscreen">2.7.1 <span style="font-family: '微软雅黑',sans-serif;">经过</span>yum<span style="font-family: '微软雅黑',sans-serif;">查找发现</span>screen<span style="font-family: '微软雅黑',sans-serif;">命令属于</span>screen<span style="font-family: '微软雅黑',sans-serif;">包</span></span>

<div class="cnblogs_code">
  <pre>[root@test ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum provides screen</span>
<span style="color: #000000;">Loaded plugins: fastestmirror, security
Loading mirror speeds </span><span style="color: #0000ff;">from</span><span style="color: #000000;"> cached hostfile
 </span>*<span style="color: #000000;"> base: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> epel: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> extras: mirrors.aliyun.com
 </span>*<span style="color: #000000;"> updates: mirrors.aliyun.com
base                                                      </span>| 3.7 kB     00:00<span style="color: #000000;">    
epel                                                      </span>| 4.3 kB     00:00<span style="color: #000000;">    
extras                                                    </span>| 3.4 kB     00:00<span style="color: #000000;">    
updates                                                   </span>| 3.4 kB     00:00<span style="color: #000000;">    
screen</span>-4.0.3-19<span style="color: #000000;">.el6.x86_64 : A screen manager that supports multiple logins on
                           : one terminal
Repo        : base
Matched </span><span style="color: #0000ff;">from</span>:</pre>
</div>

### <span id="272_screen">2.7.2 <span style="font-family: '微软雅黑',sans-serif;">安装</span>screen<span style="font-family: '微软雅黑',sans-serif;">软件</span></span>

<div class="cnblogs_code">
  <pre>[root@test ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install -y  screen</span></pre>
</div>

### <span id="273_screen">2.7.3 screen<span style="font-family: '微软雅黑',sans-serif;">命令的参数</span></span>

<span style="font-family: '微软雅黑',sans-serif;">在</span>shell<span style="font-family: '微软雅黑',sans-serif;">中输入</span> screen<span style="font-family: '微软雅黑',sans-serif;">即可进入</span>screen <span style="font-family: '微软雅黑',sans-serif;">视图</span>

<div class="cnblogs_code">
  <pre>[root@test ~]<span style="color: #008000;">#</span><span style="color: #008000;"> screen</span></pre>
</div>

**Screen****<span style="font-family: '微软雅黑',sans-serif;">实现后台运行程序的简单步骤</span>:**

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    &nbsp; screen -ls <span style="font-family: '微软雅黑',sans-serif;">：可看</span>screen<span style="font-family: '微软雅黑',sans-serif;">会话</span>
  </p>
  
  <p>
    &nbsp; screen -r ID :<span style="font-family: '微软雅黑',sans-serif;">指定进入哪个</span>screen<span style="font-family: '微软雅黑',sans-serif;">会话</span>
  </p>
</div>

**Screen****<span style="font-family: '微软雅黑',sans-serif;">命令中用到的快捷键</span>**

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    &nbsp; Ctrl+a c <span style="font-family: '微软雅黑',sans-serif;">：创建窗口</span>
  </p>
  
  <p>
    &nbsp; Ctrl+a w <span style="font-family: '微软雅黑',sans-serif;">：窗口列表</span>
  </p>
  
  <p>
    &nbsp; Ctrl+a n <span style="font-family: '微软雅黑',sans-serif;">：下一个窗口</span>
  </p>
  
  <p>
    &nbsp; Ctrl+a p <span style="font-family: '微软雅黑',sans-serif;">：上一个窗口</span>
  </p>
  
  <p>
    &nbsp; Ctrl+a 0-9 <span style="font-family: '微软雅黑',sans-serif;">：在第</span><span style="font-family: '微软雅黑',sans-serif;">个窗口和第</span>9<span style="font-family: '微软雅黑',sans-serif;">个窗口之间切换</span>
  </p>
  
  <p>
    &nbsp; Ctrl+a K(<span style="font-family: '微软雅黑',sans-serif;">大写</span>) <span style="font-family: '微软雅黑',sans-serif;">：关闭当前窗口，并且切换到下一个窗口</span> <span style="font-family: '微软雅黑',sans-serif;">，</span>
  </p>
  
  <p style="text-indent: 115.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: '微软雅黑',sans-serif;">（当退出最后一个窗口时，该终端自动终止，并且退回到原始</span>shell<span style="font-family: '微软雅黑',sans-serif;">状态）</span>
  </p>
  
  <p>
    &nbsp; exit <span style="font-family: '微软雅黑',sans-serif;">：关闭当前窗口，并且切换到下一个窗口</span>
  </p>
  
  <p style="text-indent: 52.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: '微软雅黑',sans-serif;">（当退出最后一个窗口时，该终端自动终止，并且退回到原始</span>shell<span style="font-family: '微软雅黑',sans-serif;">状态）</span>
  </p>
  
  <p>
    &nbsp; Ctrl+a d <span style="font-family: '微软雅黑',sans-serif;">：退出当前终端，返回加载</span>screen<span style="font-family: '微软雅黑',sans-serif;">前的</span>shell<span style="font-family: '微软雅黑',sans-serif;">命令状态</span>
  </p>
  
  <p>
    &nbsp; Ctrl+a " : <span style="font-family: '微软雅黑',sans-serif;">窗口列表不同于w</span>
  </p>
</div>

## <span id="28_sersync">2.8 <span style="font-family: '微软雅黑',sans-serif;">sersync软件实现实时同步</span></span>

[http://www.cnblogs.com/znix/p/7707828.html][1]

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1">第1章 数据实时同步介绍</a><ul>
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
              <a href="#141">1.4.1 三个重要文件的说明</a>
            </li>
            <li>
              <a href="#142">1.4.2 【服务优化】可以将三个文件的数值调大，监听更大的范围</a>
            </li>
            <li>
              <a href="#143">1.4.3 【官方说明】三个重要文件</a>
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
              <a href="#153_inotifywait">1.5.3 inotifywait命令参数说明</a>
            </li>
            <li>
              <a href="#154_-e_nbsp">1.5.4 -e[参数] &nbsp;可以指定的事件类型</a><ul>
                <li>
                  <a href="#1541nbspinotifywait">1.5.4.1&nbsp;【实例】inotifywait监控中的事件测试</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#155_inotifywait_--format">1.5.5 inotifywait 参数 --format 格式定义参数</a>
            </li>
            <li>
              <a href="#156_inotifywait_--timefmt">1.5.6 inotifywait 参数--timefmt 时间格式参数</a><ul>
                <li>
                  <a href="#1561nbsp">1.5.6.1&nbsp;修改输出的日期格式</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#157_-e">1.5.7 -e[参数] 重要监控事件参数汇总表：</a>
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
      </ul>
    </li>
    
    <li>
      <a href="#2_inotifyrsync">第2章 inotify+rsync实时同步服务部署</a><ul>
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
                  <a href="#2221nbspinotifywaitinotifywatch">2.2.2.1&nbsp;inotifywait和inotifywatch的作用：</a>
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
        
        <li>
          <a href="#28_sersync">2.8 sersync软件实现实时同步</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

 [1]: /wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/znix/p/7707828.html