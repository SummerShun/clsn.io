---
title: 日志切割之Logrotate
author: 惨绿少年
type: post
date: 2018-02-07T12:05:00+00:00
url: /clsn/lx109.html
Baidusubmit:
  - 1
views:
  - 438
categories:
  - Linux运维
  - 运维基本功

---
## <span id="1">1、关于日志切割</span>

　　日志文件包含了关于系统中发生的事件的有用信息，在排障过程中或者系统性能分析时经常被用到。对于忙碌的服务器，日志文件大小会增长极快，服务器会很快消耗磁盘空间，这成了个问题。除此之外，处理一个单个的庞大日志文件也常常是件十分棘手的事。  
　　logrotate是个十分有用的工具，它可以自动对日志进行截断（或轮循）、压缩以及删除旧的日志文件。例如，你可以设置logrotate，让/var/log/foo日志文件每30天轮循，并删除超过6个月的日志。配置完后，logrotate的运作完全自动化，不必进行任何进一步的人为干预。

## <span id="2logrotate">2、安装logrotate</span>

系统版本说明

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/redhat-release </span>
CentOS release 6.9<span style="color: #000000;"> (Final)
[root@clsn6 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r </span>
2.6.32-696.el6.x86_64</pre>
</div>

　　默认centos系统安装自带logrotate，安装方法如下

<div class="cnblogs_code">
  <pre>yum -y install logrotate crontabs </pre>
</div>

软件包信息说明

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -ql  logrotate</span>
/etc/cron.daily/<span style="color: #000000;">logrotate
</span>/etc/logrotate.conf  <span style="color: #008000;">#</span><span style="color: #008000;"> 主配置文件</span>
/etc/logrotate.d   <span style="color: #008000;">#</span><span style="color: #008000;"> 配置目录</span></pre>
</div>

　　logrotate的配置文件是/etc/logrotate.conf，通常不需要对它进行修改。日志文件的轮循设置在独立的配置文件中，它（们）放在/etc/logrotate.d/目录下。

## <span id="3logrotate">3、实践配置logrotate</span>

### <span id="31_logrotate">3.1 测试logrotate如何管理日志</span>

　　这里我们将创建一个10MB的日志文件/var/log/log-file。我们将展示怎样使用logrotate来管理该日志文件。

我们从创建一个日志文件开始吧，然后在其中填入一个10MB的随机比特流数据文件。

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> touch /var/log/log-file</span>
[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> head -c 10M &lt; /dev/urandom > /var/log/log-file </span></pre>
</div>

　　由于现在日志文件已经准备好，我们将配置logrotate来轮循该日志文件。让我们为该文件创建一个配置文件。

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /etc/logrotate.d/log-file </span>
/var/log/log-<span style="color: #000000;">file {
    monthly
    rotate </span>5<span style="color: #000000;">
    compress
    delaycompress
    missingok
    notifempty
    create </span>644<span style="color: #000000;"> root root
    postrotate
        </span>/usr/bin/killall -<span style="color: #000000;">HUP rsyslogd
    endscript
}</span></pre>
</div>

<address>
  <span style="font-style: normal;">　　上面的模板是通用的，而配置参数则根据你的需求进行调整，不是所有的参数都是必要的。也可以通过man手册中的例子进行配置。</span>
</address>

### <span id="32">3.2配置文件说明</span>

<table class="MsoTable15Grid4Accent3" style="width: 100%; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 29.62%; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #9bbb59; border-bottom-color: #9bbb59; border-left-color: #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 5;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">配置参数</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #9bbb59; border-right-color: #9bbb59; border-bottom-color: #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 1;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">monthly</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">日志文件将按月轮循。其它可用值为</span><span lang="EN-US">'daily'</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">，</span><span lang="EN-US">'weekly'</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">或者</span><span lang="EN-US">'yearly'</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">rotate 5</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">一次将存储</span><span lang="EN-US">5</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">个归档日志。对于第六个归档，时间最久的归档将被删除。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">compress</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">在轮循任务完成后，已轮循的归档将使用</span><span lang="EN-US">gzip</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">进行压缩。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">delaycompress</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">总是与</span><span lang="EN-US">compress</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">选项一起用，</span><span lang="EN-US">delaycompress</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">选项指示</span><span lang="EN-US">logrotate</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">不要将最近的归档压缩，压缩将在下一次轮循周期进行。这在你或任何软件仍然需要读取最新归档时很有用。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">missingok</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">在日志轮循期间，任何错误将被忽略，例如&ldquo;文件无法找到&rdquo;之类的错误。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">notifempty</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">如果日志文件为空，轮循不会进行。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">create 644 root root</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">以指定的权限创建全新的日志文件，同时</span><span lang="EN-US">logrotate</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">也会重命名原始日志文件。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.62%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">postrotate/endscript</span></strong>
      </p>
    </td>
    
    <td style="width: 70.38%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">在所有其它指令完成后，</span><span lang="EN-US">postrotate</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">和</span><span lang="EN-US">endscript</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">里面指定的命令将被执行。在这种情况下，</span><span lang="EN-US">rsyslogd </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">进程将立即再次读取其配置并继续运行。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 100%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #daeef3; padding: 0cm 5.4pt;" colspan="2" width="100%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; mso-bidi-font-weight: bold;">以上信息来源</span> <span lang="EN-US">"man logrotate"</span>
      </p>
    </td>
  </tr>
</table>

### <span id="nbsp33logrotate">&nbsp;3.3手动运行logrotate</span>

　　logrotate可以在任何时候从命令行手动调用。要调用为/etc/lograte.d/下配置的所有日志调用logrotate：

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> logrotate /etc/logrotate.conf</span></pre>
</div>

要为某个特定的配置调用logrotate,执行一次切割任务测试

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /var/log/log-file </span>
-rw-r--r-- 1 root root 10485760 Feb  7 18:50 /var/log/log-<span style="color: #000000;">file
[root@clsn6 </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> logrotate -vf /etc/logrotate.d/log-file </span>
[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ll /var/log/log-file* </span>
-rw-r--r-- 1 root root        0 Feb  7 19:17 /var/log/log-<span style="color: #000000;">file
</span>-rw-r--r-- 1 root root 10485760 Feb  7 18:50 /var/log/log-file.1</pre>
</div>

　　即使轮循条件没有满足，我们也可以通过使用&lsquo;-f&rsquo;选项来强制logrotate轮循日志文件，&lsquo;-v&rsquo;参数提供了详细的输出。

### <span id="34Logrotate">3.4Logrotate的记录日志</span>

　　logrotate自身的日志通常存放于/var/lib/logrotate/status目录。如果处于排障目的，我们想要logrotate记录到任何指定的文件，我们可以指定像下面这样从命令行指定。

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> logrotate -vf -s /var/log/logrotate-status /etc/logrotate.d/log-file</span>
reading config file /etc/logrotate.d/log-<span style="color: #000000;">file
reading config info </span><span style="color: #0000ff;">for</span> /var/log/log-<span style="color: #000000;">file 

Handling </span>1<span style="color: #000000;"> logs

rotating pattern: </span>/var/log/log-file  forced <span style="color: #0000ff;">from</span> command line (5<span style="color: #000000;"> rotations)
empty log files are </span><span style="color: #0000ff;">not</span><span style="color: #000000;"> rotated, old logs are removed
considering log </span>/var/log/log-<span style="color: #000000;">file
  log does </span><span style="color: #0000ff;">not</span><span style="color: #000000;"> need rotating
</span><span style="color: #0000ff;">not</span> running postrotate script, since no logs were rotated</pre>
</div>

### <span id="35_Logrotate">3.5 Logrotate定时任务</span>

　　logrotate需要的cron任务应该在安装时就自动创建了，我把cron文件的内容贴出来，以供大家参考。

<div class="cnblogs_code">
  <pre>[root@clsn6 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/cron.daily/logrotate </span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/sh</span>

/usr/sbin/logrotate /etc/<span style="color: #000000;">logrotate.conf
EXITVALUE</span>=<span style="color: #000000;">$?
</span><span style="color: #0000ff;">if</span> [ $EXITVALUE !=<span style="color: #000000;"> 0 ]; then
    </span>/usr/bin/logger -t logrotate <span style="color: #800000;">"</span><span style="color: #800000;">ALERT exited abnormally with [$EXITVALUE]</span><span style="color: #800000;">"</span><span style="color: #000000;">
fi
exit 0</span></pre>
</div>

## <span id="4logrotate">4、logrotate生产应用</span>

### <span id="41nginx">4.1为nginx设置日志切割</span>

　　防止访问日志文件过大

<div class="cnblogs_code">
  <pre>[root@clsn nginx]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/logrotate.d/nginx </span>
/var/log/nginx/*<span style="color: #000000;">.log {
    daily
    rotate </span>5<span style="color: #000000;">
    missingok
    notifempty
    create </span>644<span style="color: #000000;"> www www
    postrotate
      </span><span style="color: #0000ff;">if</span> [ -f /application/nginx/logs/<span style="color: #000000;">nginx.pid ]; then
          kill </span>-USR1 `cat /application/nginx/logs/<span style="color: #000000;">nginx.pid`
      fi
endscript
}</span></pre>
</div>

　　logrotate工具对于防止因庞大的日志文件而耗尽存储空间是十分有用的。配置完毕后，进程是全自动的，可以长时间在不需要人为干预下运行。本教程重点关注几个使用logrotate的几个基本样例，你也可以定制它以满足你的需求。

　　<span style="background-color: #ffff00;"><strong>对于其他服务日志切割后续补充</strong></span>

## <span id="5">5、附录</span>

### <span id="51USR1">5.1关于USR1信号解释</span>

　　摘自： http://www.xuebuyuan.com/323422.html

> USR1亦通常被用来告知应用程序重载配置文件；例如，向Apache HTTP服务器发送一个USR1信号将导致以下步骤的发生：停止接受新的连接，等待当前连接停止，重新载入配置文件，重新打开日志文件，重启服务器，从而实现相对平滑的不关机的更改。内容摘自wiki：http://zh.wikipedia.org/wiki/SIGUSR1%E5%92%8CSIGUSR2　　

　　对于USR1和2都可以用户自定义的，在POSIX兼容的平台上，SIGUSR1和SIGUSR2是发送给一个进程的信号，它表示了用户定义的情况。它们的符号常量在头文件signal.h中定义。在不同的平台上，信号的编号可能发生变化，因此需要使用符号名称。

<div class="cnblogs_code">
  <pre>kill -HUP pid 或者 killall -HUP pName：</pre>
</div>

　　其中pid是进程标识，pName是进程的名称。  
　　如果想要<span style="background-color: #ffff00;">更改配置而不需停止并重新启动服务</span>，可以使用上面两个命令。在对配置文件作必要的更改后，发出该命令以动态更新服务配置。根据约定，当你发送一个挂起信号(信号1或HUP)时，大多数服务器进程(所有常用的进程)都会进行复位操作并<span style="background-color: #ffff00;">重新加载它们的配置文件。</span>

### <span id="52">5.2常见配置参数小结</span>

<table class="MsoTable15Grid4Accent3" style="width: 99%; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 29.32%; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #9bbb59; border-bottom-color: #9bbb59; border-left-color: #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 5;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">配置参数</span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #9bbb59; border-right-color: #9bbb59; border-bottom-color: #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 1;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">compress&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">通过</span><span lang="EN-US">gzip</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">压缩转储以后的日志</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">nocompress&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">不压缩</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">copytruncate&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">用于还在打开中的日志文件，把当前日志备份并截断</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">nocopytruncate&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">备份日志文件但是不截断</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">create<em> mode owner group</em>&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">转储文件，使用指定的文件模式创建新的日志文件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">nocreate&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">不建立新的日志文件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">delaycompress&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">和</span><span lang="EN-US"><br /> compress </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">一起使用时，转储的日志文件到下一次转储时才压缩</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">nodelaycompress&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">覆盖</span><span lang="EN-US"> delaycompress<br /> </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">选项，转储同时压缩。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">errors address&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">专储时的错误信息发送到指定的</span><span lang="EN-US">Email </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">地址</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">ifempty&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">即使是空文件也转储，这个是</span><span lang="EN-US"><br /> logrotate </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">的缺省选项。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">notifempty&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">如果是空文件的话，不转储</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">mail <em>address&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </em>&nbsp;&nbsp;&nbsp;&nbsp;</span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">把转储的日志文件发送到指定的</span><span lang="EN-US">E-mail<br /> </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">地址</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">nomail&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">转储时不发送日志文件</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">olddir<em> directory&nbsp;&nbsp;&nbsp;&nbsp; </em>&nbsp;&nbsp;&nbsp;&nbsp;</span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">noolddir&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">转储后的日志文件和当前日志文件放在同一个目录下</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">prerotate/endscript&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">在转储以前需要执行的命令可以放入这个对，这两个关键字必须单独成行</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">daily&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指定转储周期为每天</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">weekly&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指定转储周期为每周</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">monthly&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指定转储周期为每月</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">rotate count&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指定日志文件删除之前转储的次数，</span><span lang="EN-US"><br /> </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指没有备份，</span><span lang="EN-US">5 </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指保留</span><span lang="EN-US">5 </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">个备份</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">tabooext [+] list </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">让</span><span lang="EN-US">logrotate</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">不转储指定扩展名的文件，缺省的扩展名是：</span><span lang="EN-US">.rpm-orig, .rpmsave, v, </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">和</span><span lang="EN-US"> ~ </span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
        <strong><span lang="EN-US">size size&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">当日志文件到达指定的大小时才转储，</span><span lang="EN-US">bytes(</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">缺省</span><span lang="EN-US">)</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">及</span><span lang="EN-US">KB(sizek)</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">或</span><span lang="EN-US">MB(sizem)</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 29.32%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #c2d69b; border-bottom-color: #c2d69b; border-left-color: #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="29%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
        <strong><span lang="EN-US">missingok</span></strong>
      </p>
    </td>
    
    <td style="width: 70.68%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #c2d69b; border-right-width: 1pt; border-right-color: #c2d69b; background: #eaf1dd; padding: 0cm 5.4pt;" width="70%">
      <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">在日志轮循期间，任何错误将被忽略，例如&ldquo;文件无法找到&rdquo;之类的错误。</span>
      </p>
    </td>
  </tr>
</table>

## <span id="6">6、参考文献</span>

> [1]https://linux.cn/article-4126-1.html  
> [2]http://xmodulo.com/2014/09/logrotate-manage-log-files-linux.html  
> [3]http://blog.csdn.net/fuming0210sc/article/details/50906372  
> [4]http://blog.csdn.net/forthemyth/article/details/44062529

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1">1、关于日志切割</a>
    </li>
    <li>
      <a href="#2logrotate">2、安装logrotate</a>
    </li>
    <li>
      <a href="#3logrotate">3、实践配置logrotate</a><ul>
        <li>
          <a href="#31_logrotate">3.1 测试logrotate如何管理日志</a>
        </li>
        <li>
          <a href="#32">3.2配置文件说明</a>
        </li>
        <li>
          <a href="#nbsp33logrotate">&nbsp;3.3手动运行logrotate</a>
        </li>
        <li>
          <a href="#34Logrotate">3.4Logrotate的记录日志</a>
        </li>
        <li>
          <a href="#35_Logrotate">3.5 Logrotate定时任务</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#4logrotate">4、logrotate生产应用</a><ul>
        <li>
          <a href="#41nginx">4.1为nginx设置日志切割</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#5">5、附录</a><ul>
        <li>
          <a href="#51USR1">5.1关于USR1信号解释</a>
        </li>
        <li>
          <a href="#52">5.2常见配置参数小结</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#6">6、参考文献</a>
    </li>
  </ul>
</div>