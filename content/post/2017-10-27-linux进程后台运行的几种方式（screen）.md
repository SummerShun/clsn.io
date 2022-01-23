---
title: Linux 进程后台运行的几种方式（screen）
author: 惨绿少年
type: post
date: 2017-10-27T03:15:00+00:00
url: /clsn/lx894.html
Baidusubmit:
  - 1
views:
  - 120
categories:
  - Linux运维
  - 玩转Linux
  - 运维基本功

---
# <span id="Ctrlzbgnohupsetsid">Ctrl+z/bg/nohup/setsid/&</span> {.post-title}

<div class="post-body">
  <p>
    在Linux中，如果要让进程在后台运行，一般情况下，我们在命令后面加上&即可，实际上，这样是将命令放入到一个作业队列中了：
  </p>
  
  <div class="cnblogs_Highlighter">
    <pre class="brush:bash;gutter:true;">./rsync.sh &
# jobs</pre>
  </div>
  
  <p>
    但是如上方到后台执行的进程，其父进程还是当前终端shell的进程，而一旦父进程退出，则会发送hangup信号给所有子进程，子进程收到hangup以后也会退出。如果我们要在退出shell的时候继续运行进程，则需要使用<code>nohup</code>忽略hangup信号，或者<code>setsid</code>将将父进程设为init进程(进程号为1)：对于已经在前台执行的命令，也可以重新放到后台执行，首先按ctrl+z暂停已经运行的进程，然后使用bg命令将停止的作业放到后台运行：<code>bg %1</code>，放回前台运行：<code>%1</code>。
  </p>
  
  <div class="cnblogs_code">
    <pre># nohup ./rsync.<span style="color: #0000ff;">sh</span> &<span style="color: #000000;">

# setsid .</span>/rsync.<span style="color: #0000ff;">sh</span> &<span style="color: #000000;">
或
# (.</span>/rsync.<span style="color: #0000ff;">sh</span> &) <span style="color: #808080;">///</span><span style="color: #008000;">/在一个subshell中执行</span>
# <span style="color: #0000ff;">ps</span> -ef|<span style="color: #0000ff;">grep</span> rsync</pre>
  </div>
  
  <p>
    nohup 的用途就是让提交的命令忽略 hangup 信号，标准输出和标准错误缺省会被重定向到 nohup.out 文件中。。一般我们可在结尾加上&rdquo;&&rdquo;来将命令同时放入后台运行，也可用&rdquo; > log.out 2>&1&rdquo;来更改缺省的重定向文件名。
  </p>
  
  <p>
    上面的试验演示了使用nohup/setsid加上&使进程在后台运行，同时不受当前shell退出的影响。那么对于已经在后台运行的进程，该怎么办呢？可以使用<code>disown</code>命令：
  </p>
  
  <div class="cnblogs_code">
    <pre><span style="color: #000000;"># jobs
# disown </span>-h %<span style="color: #800080;">1</span><span style="color: #000000;">
# </span><span style="color: #0000ff;">ps</span> -ef|<span style="color: #0000ff;">grep</span> rsync</pre>
  </div>
  
  <p>
    效果与setid相同，但是disown后无法通过<code>jobs</code>命令查看了。
  </p>
  
  <h2 id="screen">
    <span id="screen">screen</span>
  </h2>
  
  <p>
    还有一种更加强大的方式是使用screen，首先创建一个断开模式的虚拟终端，然后用<code>-r</code>选项重新连接这个虚拟终端，在其中执行的任何命令，都能达到nohup的效果，这在有多个命令需要在后台连续执行的时候比较方便。
  </p>
  
  <p>
    <a id="more"></a>
  </p>
  
  <p>
    GNU Screen是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换，可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。
  </p>
  
  <p>
  </p>
  
  <div class="cnblogs_code">
    <pre># <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> screen -y</pre>
  </div>
  
  <p>
    常用screen参数：
  </p>
  
  <div class="cnblogs_code">
    <pre># screen -S docker-d 新建一个名叫docker-<span style="color: #000000;">d的session，并马上进入
# screen </span>-dmS docker-d 新建一个名叫docker-<span style="color: #000000;">d的session，但暂不进入，可用于系统启动脚本里
# screen </span>-<span style="color: #0000ff;">ls</span><span style="color: #000000;"> 列出当前所有session
# screen </span>-r docker-d 恢复到zhouxiao这个session，前提是已经是断开状态（-<span style="color: #000000;">d可以远程断开会话）
# screen </span>-x docker-<span style="color: #000000;">d 连接到离线模式的会话（多窗口同步演示）
# screen .</span>/rsync.<span style="color: #0000ff;">sh</span><span style="color: #000000;"> screen创建一个执行脚本的单窗口会话，可以attach进程ID
# screen </span>-wipe 检查目前所有的screen作业，并删除已经无法使用的screen作业</pre>
  </div>
  
  <p>
    正常情况下，当你退出一个窗口中最后一个程序（通常是bash）后，这个窗口就关闭了。另一个关闭窗口的方法是使用C-a k，这个快捷键杀死当前的窗口，同时也将杀死这个窗口中正在运行的进程。
  </p>
  
  <p>
    在每个screen session 下，所有命令都以 ctrl+a(C-a) 开始。
  </p>
  
  <div class="cnblogs_code">
    <pre>C-a <span style="color: #0000ff;">w</span><span style="color: #000000;"> 显示所有窗口列表
C</span>-<span style="color: #000000;">a k 这个快捷键杀死当前的窗口，同时也将杀死这个窗口中正在运行的进程。 
C</span>-a d detach，暂时离开当前session</pre>
  </div>
  
  <p>
    上面只是基本也是最常用的用法，更多请参考<code>man screen</code>或<a href="https://www.cnblogs.com/znix/">linux screen 命令详解</a>。需要了解的是，一个用户创建的screen，其他用户（甚至root）通过<code>screen -ls</code>是看不见的。另外，<code>Ctrl+a</code>在bash下是用来回到行开头，不幸与上面的组合快捷键冲突。
  </p>
  
  <div class="cnblogs_code" style="text-align: center;">
    <pre>原文连接： http:<span style="color: #008000;">//</span><span style="color: #008000;">seanlook.com/2014/02/20/linux-process-running-background-screen/</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#Ctrlzbgnohupsetsid">Ctrl+z/bg/nohup/setsid/&</a><ul>
        <li>
          <a href="#screen">screen</a>
        </li>
      </ul>
    </li>
  </ul>
</div>