---
title: windows 上rsync客户端使用方法
author: 惨绿少年
type: post
date: 2017-10-11T22:40:00+00:00
url: /clsn/lx922.html
Baidusubmit:
  - 1
views:
  - 136
categories:
  - Linux运维
  - 运维基本功

---
## <span id="11_windowsrsynccwRsync">1.1 <span style="font-family: '微软雅黑',sans-serif;">获取</span><span style="font-size: 1.5em;"> windows</span><span style="font-family: '微软雅黑',sans-serif;">上实现</span><span style="font-size: 1.5em;">rsync</span><span style="font-family: '微软雅黑',sans-serif;">的软件（</span><span style="font-size: 1.5em;">cwRsync</span><span style="font-family: '微软雅黑',sans-serif;">）</span></span>

<p style="margin-left: 7.1pt; text-indent: 13.9pt;">
  cwRsync<span style="font-family: '微软雅黑',sans-serif;">是</span>Windows <span style="font-family: '微软雅黑',sans-serif;">客户端</span>GUI<span style="font-family: '微软雅黑',sans-serif;">的一个包含</span>Rsync<span style="font-family: '微软雅黑',sans-serif;">的包装。您可以使用</span>cwRsync<span style="font-family: '微软雅黑',sans-serif;">快速远程文件备份和同步。</span>
</p>

### <span id="111">1.1.1 <span style="font-family: '微软雅黑',sans-serif;">官网下载地址</span></span>

[<span style="color: black; background: yellow;">https://www.itefix.net/cwrsync</span>][1]

<span style="font-family: 微软雅黑, sans-serif;">下载方法：</span>

<p style="text-indent: 21.0pt;">
  1<span style="font-family: 微软雅黑, sans-serif;">．点击面页中的</span>get<span style="font-family: 微软雅黑, sans-serif;">，获取</span>Free<span style="font-family: 微软雅黑, sans-serif;">（免费版本）</span>
</p>

<p style="text-indent: 21.0pt;">
  2.<span style="font-family: 微软雅黑, sans-serif;">转跳后点击</span> <strong>&nbsp;</strong><a title="Download cwRsync Free Edition!" href="https://www.itefix.net/dl/cwRsync_5.5.0_x86_Free.zip"><span style="color: black; background: #FF6600;">Download cwRsync Free Edition!</span></a><tt> <span style="background: #ffffff;">进行下载</span></tt>
</p>

<p style="text-indent: 21pt; text-align: center;">
  <span style="background: #ffffff;"><img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171210173444911-1086187508.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="windows 上rsync客户端使用方法" alt="" /></span>
</p>

<p style="text-align: center;">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171210173458396-1029544448.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="windows 上rsync客户端使用方法" alt="" />
</p>

### <span id="112_cwRsync_550_x86_Freezip">1.1.2 <span style="font-family: '微软雅黑',sans-serif;">下载完成后得到一个</span>cwRsync_5.5.0_x86_Free.zip<span style="font-family: '微软雅黑',sans-serif;">的压缩包</span></span>

<span style="font-family: '微软雅黑',sans-serif;">包内容如下：</span>

<div class="cnblogs_code">
  <pre>[root@backup backup]<span style="color: #008000;">#</span><span style="color: #008000;"> tree cwRsync_5.5.0_x86_Free</span>
cwRsync_5.5<span style="color: #000000;">.0_x86_Free
├── bin
│   ├── cygcrypto</span>-1.0<span style="color: #000000;">.0.dll
│   ├── cyggcc_s</span>-1<span style="color: #000000;">.dll
│   ├── cygiconv</span>-2<span style="color: #000000;">.dll
│   ├── cygintl</span>-8<span style="color: #000000;">.dll
│   ├── cygpopt</span>-<span style="color: #000000;">0.dll
│   ├── cygssp</span>-<span style="color: #000000;">0.dll
│   ├── cygwin1.dll
│   ├── cygz.dll
│   ├── rsync.exe
│   ├── ssh.exe
│   └── ssh</span>-<span style="color: #000000;">keygen.exe
├── cwrsync.cmd
├── README.cwrsync.txt
└── README.rsync.txt</span></pre>
</div>

## <span id="12_cwrsync"><span style="background-color: #99ccff;">1.2 cwrsync</span><span style="font-family: 微软雅黑, sans-serif; background-color: #99ccff;">的使用方法</span></span>

### <span id="121">1.2.1 <span style="font-family: '微软雅黑',sans-serif;">将压缩包解压出来</span></span>

<p style="text-indent: 7.1pt;">
  <span style="font-family: '微软雅黑',sans-serif;">双击</span> cwrsync[.cmd]<span style="font-family: '微软雅黑',sans-serif;">进行安装</span>
</p>

<p style="text-align: center;">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171210173542177-70326892.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="windows 上rsync客户端使用方法" alt="" />
</p>

### <span id="122_home">1.2.2 <span style="font-family: '微软雅黑',sans-serif;">安装完成会有多一个</span>home<span style="font-family: '微软雅黑',sans-serif;">目录</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">该目录下存放的是</span>ssh<span style="font-family: '微软雅黑',sans-serif;">认证信息</span>
</p>

<p style="text-indent: 21pt; text-align: center;">
  <span style="font-family: '微软雅黑',sans-serif;"><img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171210173605630-912576382.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="windows 上rsync客户端使用方法" alt="" /></span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">到此安装完成。</span>
</p>

## <span id="13_cwrsync">1.3 cwrsync<span style="font-family: '微软雅黑',sans-serif;">的使用</span></span>

### <span id="131_windowscmd">1.3.1 <span style="font-family: '微软雅黑',sans-serif;">在</span>windows<span style="font-family: '微软雅黑',sans-serif;">上打开</span>cmd<span style="font-family: '微软雅黑',sans-serif;">（命令提示符）</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: '微软雅黑',sans-serif;">打开后，将</span>cmd<span style="font-family: '微软雅黑',sans-serif;">的路径切换到</span>cwrysnc<span style="font-family: '微软雅黑',sans-serif;">的安装目录的</span>bin<span style="font-family: '微软雅黑',sans-serif;">目录下，作为工作目录。</span>
</p>

<p style="margin-left: 21pt; text-align: center;">
  <span style="font-family: '微软雅黑',sans-serif;"><img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171210173619771-1670547701.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="windows 上rsync客户端使用方法" alt="" /></span>
</p>

### <span id="132_window">1.3.2 window<span style="font-family: '微软雅黑',sans-serif;">上的推送测试</span></span>

<div class="cnblogs_code">
  <pre>C:\Users\Administrator\Desktop\cwRsync_5.5.0_x86_Free\bin>rsync.exe -avzP ./<span style="color: #000000;">cwRs
ync_5.</span>5.0_x86_Free.zip rsync_backup@172.16.1.41::backup --password-file=./<span style="color: #000000;">rsync.
password
sending incremental file list
cwRsync_5.</span>5<span style="color: #000000;">.0_x86_Free.zip
      </span>3,486,341 100%   21.11MB/s    0:00:00 (xfr<span style="color: #008000;">#</span><span style="color: #008000;">1, to-chk=0/1)</span>
<span style="color: #000000;"> 
sent </span>3,475,491 bytes  received 34 bytes  2,317,016.67 bytes/<span style="color: #000000;">sec
total size </span><span style="color: #0000ff;">is</span> 3,486,341  speedup <span style="color: #0000ff;">is</span> 1.00</pre>
</div>

&nbsp;&nbsp; <span style="font-family: '微软雅黑',sans-serif;">在</span>windows<span style="font-family: '微软雅黑',sans-serif;">上</span>rsync<span style="font-family: '微软雅黑',sans-serif;">的命令与在</span>linux<span style="font-family: '微软雅黑',sans-serif;">上基本类似。</span>

### <span id="133">1.3.3 <span style="font-family: '微软雅黑',sans-serif;">服务端上检查</span></span>

<div class="cnblogs_code">
  <pre>[root@backup backup]<span style="color: #008000;">#</span><span style="color: #008000;"> ll cwRsync_5.5.0_x86_Free.zip</span>
-rwxrwx--- 1 rsync rsync 3486341 Oct 12 13:25 cwRsync_5.5.0_x86_Free.zip</pre>
</div>

<p style="text-indent: 15.75pt;">
  <span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">至此</span><span style="background: lime;">windows</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">上的</span><span style="background: lime;">rsync</span><span style="font-family: '微软雅黑',sans-serif; courier new"4courier new"; background: lime;">的客户端可以正常使用。</span>
</p>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11_windowsrsynccwRsync">1.1 获取 windows上实现rsync的软件（cwRsync）</a><ul>
        <li>
          <a href="#111">1.1.1 官网下载地址</a>
        </li>
        <li>
          <a href="#112_cwRsync_550_x86_Freezip">1.1.2 下载完成后得到一个cwRsync_5.5.0_x86_Free.zip的压缩包</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_cwrsync">1.2 cwrsync的使用方法</a><ul>
        <li>
          <a href="#121">1.2.1 将压缩包解压出来</a>
        </li>
        <li>
          <a href="#122_home">1.2.2 安装完成会有多一个home目录</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_cwrsync">1.3 cwrsync的使用</a><ul>
        <li>
          <a href="#131_windowscmd">1.3.1 在windows上打开cmd（命令提示符）</a>
        </li>
        <li>
          <a href="#132_window">1.3.2 window上的推送测试</a>
        </li>
        <li>
          <a href="#133">1.3.3 服务端上检查</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

 [1]: https://www.itefix.net/cwrsync