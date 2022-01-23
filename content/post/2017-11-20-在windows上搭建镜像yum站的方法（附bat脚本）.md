---
title: 在windows上搭建镜像yum站的方法（附bat脚本）
author: 惨绿少年
type: post
date: 2017-11-19T23:48:00+00:00
url: /clsn/lx768.html
Baidusubmit:
  - 1
views:
  - 239
categories:
  - Linux运维
  - 运维基本功

---
## <span id="rsyncnbsp">方法一：支持rsync的网站&nbsp;</span>

对于常用的centos、Ubuntu、等使用官方yum源在&nbsp;http://mirrors.ustc.edu.cn 都存在镜像。

　　同时&nbsp;http://mirrors.ustc.edu.cn 网站又支持 rsync 协议， 可以通过rsync实现 镜像yum源。

<div class="cnblogs_code">
  <pre> <span style="color: #800080;">_______________________________________________________________</span>
|         University of Science <span style="color: #0000ff;">and</span> Technology of China         |
|           Open Source Mirror  (mirrors.ustc.edu.cn)           |
|===============================================================|
|                                                               |
| Debian primary mirror <span style="color: #0000ff;">in</span> China mainland (ftp.cn.debian.org),  |
|     also mirroring a great many OSS projects & Linux distros. |
|                                                               |
| Currently we don<span style="color: #800000;">'</span><span style="color: #800000;">t limit speed. To prevent overload, Each IP  |</span>
| <span style="color: #0000ff;">is</span> only allowed to start upto 2 concurrent rsync connections. |
|                                                               |
| This site also provides http/https/ftp access.                |
|                                                               |
| Supported by USTC Network Information Center                  |
|          <span style="color: #0000ff;">and</span> USTC Linux User Group (http://lug.ustc.edu.cn/). |
|                                                               |
|    Sync Status:  https://mirrors.ustc.edu.cn/status/          |
|           News:  https://servers.ustclug.org/                 |
|        Contact:  lug@ustc.edu.cn                              |
|                                                               |
|<span style="color: #800080;">_______________________________________________________________</span>|</pre>
</div>

在windows上使用rsync需要用到 cwRsync&nbsp; 下载地址是&nbsp;https://www.itefix.net/dl/cwRsync\_5.5.0\_x86_Free.zip；

　　其官网为&nbsp;https://www.itefix.net/cwrsync&nbsp;

使用rsync的时候需要编写脚本，设置要同步的内容

&nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171120152526149-766586914.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="在windows上搭建镜像yum站的方法（附bat脚本）" alt="" />

其中centos7_base.bat脚本内容 供大家参考：

<div class="cnblogs_code">
  <pre><span style="color: #000000;">@echo off
SETLOCAL

SET CWRSYNCHOME</span>=%~<span style="color: #000000;">dp0

IF NOT EXIST </span>%CWRSYNCHOME%home\%USERNAME%\.ssh MKDIR  %CWRSYNCHOME%home\%USERNAME%<span style="color: #000000;">\.ssh

SET CWOLDPATH</span>=%PATH%<span style="color: #000000;">

SET PATH</span>=%CWRSYNCHOME%bin;%PATH%<span style="color: #000000;">

rsync  </span>-av --delete rsync://mirrors.ustc.edu.cn/centos/7/os/x86_64/    /cygdrive/f/yum/nginx/html/centos/7/os/x86_64/</pre>
</div>

## <span id="wgetrsync">方法二：使用wget命令下载不支持rsync协议的源</span>

<div class="cnblogs_code">
  <pre>由于线上跑的系统还有CentOS5.4、6.4、6.5、6.5、6.6、6.8，而各镜像站维护的最早的版本已经是6.9<span style="color: #000000;">，所以需要爬archive站点的rpm包来自建yum仓库。

</span><span style="color: #008000;">#</span><span style="color: #008000;"> wget -r -p -np -k http://archives.fedoraproject.org/pub/archive/epel/5Server/x86_64/</span>

<span style="color: #008000;">#</span><span style="color: #008000;"> wget -r -p -np -k http://archives.fedoraproject.org/pub/epel/6Server/x86_64/</span>

-c, --<span style="color: #0000ff;">continue</span> resume getting a partially-<span style="color: #000000;">downloaded file. 断点续传
</span>-nd, --no-directories don<span style="color: #800000;">'</span><span style="color: #800000;">t create directories. 不创建层级目录，所有文件下载到当前目录</span>
-r, --<span style="color: #000000;">recursive specify recursive download. 递归下载
</span>-p, --page-<span style="color: #000000;">requisites get all images, etc. needed to display HTML page.
下载页面所有文件，使页面能在本地打开
</span>-k, --convert-links make links <span style="color: #0000ff;">in</span> downloaded HTML <span style="color: #0000ff;">or</span><span style="color: #000000;"> CSS point to local files.
转换链接指向本地文件
</span>-np, --no-parent don<span style="color: #800000;">'</span><span style="color: #800000;">t ascend to the parent directory. 不下载父级目录的文件</span>
-o, --output-file=<span style="color: #000000;">FILE log messages to FILE. 指定日志输出文件
</span>-O, --output-document=<span style="color: #000000;">FILE write documents to FILE. 指定文件下载位置
</span>-L, --relative follow relative links only. 只下载相对链接，如果页面嵌入其他站点不会被下载</pre>
</div>

windows上wget命令使用方法

　　下载：http://downloads.sourceforge.net/gnuwin32/wget-1.11.4-1-setup.exe

　　官网：http://gnuwin32.sourceforge.net/packages/wget.htm

**　　安装**

&nbsp; &nbsp; &nbsp; 　　双击即可安装，安装到目录：D:\GnuWin32

　　　　<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171120153247586-1565272663.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="在windows上搭建镜像yum站的方法（附bat脚本）" alt="" />

**　　修改环境变量** 

　　　　右键计算机 -> 属性 -> 高级系统设置 -> 环境变量 -> 系统变量&nbsp;&nbsp;GNU_HOME=D:\GnuWin32\bin

　　　　<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171120153421383-410967468.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="在windows上搭建镜像yum站的方法（附bat脚本）" alt="" />

　　　　添加path变量，在cmd中可以使用wget命令。&nbsp;&nbsp;PATH=;%GNU_HOME%

　　　　<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171120153527961-986325267.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="在windows上搭建镜像yum站的方法（附bat脚本）" alt="" />

<div class="cnblogs_code">
  <pre>c:\>wget -<span style="color: #000000;">V
SYSTEM_WGETRC </span>= c:/progra~1/wget/etc/<span style="color: #000000;">wgetrc
syswgetrc </span>= D:\GnuWin32/etc/<span style="color: #000000;">wgetrc
GNU Wget </span>1.11.4<span style="color: #000000;">

Copyright (C) </span>2008<span style="color: #000000;"> Free Software Foundation, Inc.
License GPLv3</span>+: GNU GPL version 3 <span style="color: #0000ff;">or</span><span style="color: #000000;"> later
</span>&lt;http://www.gnu.org/licenses/gpl.html><span style="color: #000000;">.
This </span><span style="color: #0000ff;">is</span> free software: you are free to change <span style="color: #0000ff;">and</span><span style="color: #000000;"> redistribute it.
There </span><span style="color: #0000ff;">is</span><span style="color: #000000;"> NO WARRANTY, to the extent permitted by law.

最初由 Hrvoje Niksic </span>&lt;hniksic@xemacs.org><span style="color: #000000;"> 编写。
Currently maintained by Micah Cowan </span>&lt;micah@cowan.name>.</pre>
</div>

**<span style="font-size: 1.5em;">提供web服务</span>**

&nbsp;同时提供web服务需要用到nginx&nbsp;

&nbsp;　　nginx 下载 页面 http://nginx.org/en/download.html

**<span style="color: #00ff00;">附：nginx 配置文件</span>**

<div class="cnblogs_code">
  <pre>worker_processes  1<span style="color: #000000;">;

events {
    worker_connections  </span>1024<span style="color: #000000;">;
}

http {
    include       mime.types;
    default_type  application</span>/octet-<span style="color: #000000;">stream;
    sendfile        on;
    keepalive_timeout  </span>65<span style="color: #000000;">;
    server {
        listen       </span>80<span style="color: #000000;">;
        server_name  localhost;
        location </span>/<span style="color: #000000;"> {
            autoindex  on;
            root   html;
            index  index.html index.htm;
        }
    }
    server {
        listen       </span>80<span style="color: #000000;">;
        server_name  repo.zabbix.com;
        location </span>/<span style="color: #000000;"> {
            autoindex  on;
            root   html</span>/<span style="color: #000000;">zabbix;
            index  index.html index.htm;
        }
    } 
    server {
        listen       </span>80<span style="color: #000000;">;
        server_name  sp.repo.webtatic.com mirror.webtatic.com;
        location </span>/<span style="color: #000000;"> {
            autoindex  on;
            root   html</span>/<span style="color: #000000;">webtatic;
            index  index.html index.htm;
        }
    }
}</span></pre>
</div>

## <span id="i">最后使用浏览器访问</span>

　　<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171120153940618-922950795.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="在windows上搭建镜像yum站的方法（附bat脚本）" alt="" />

在linux服务器中设置解析

<div class="cnblogs_code">
  <pre>[root@m01 ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/hosts</span>
127.0.0.1<span style="color: #000000;">    localhost localhost.localdomain localhost4 localhost4.localdomain4
::</span>1<span style="color: #000000;">          localhost localhost.localdomain localhost6 localhost6.localdomain6
</span>10.0.0.1     mirrors.aliyuncs.com mirrors.aliyun.com repo.zabbix.com</pre>
</div>

## <span id="i-2"><strong>附件：</strong></span>

附件1、&nbsp; cwrsync + 全部 bat 脚本&nbsp; <a href="https://files.cnblogs.com/files/clsn/cwRsync.rar" target="_blank">https://files.cnblogs.com/files/clsn/cwRsync.rar</a>

附件2、&nbsp; nginx + 配置文件&nbsp;&nbsp;<a href="https://files.cnblogs.com/files/clsn/nginx.zip" target="_blank">https://files.cnblogs.com/files/clsn/nginx.zip</a>

&nbsp;

## <span id="wget"><span style="color: #00ff00;">特别感谢:</span>国强哥提供wget命令使用</span>

<p style="text-align: right;">
  &nbsp;　　　　　　　　　　　　　　<strong>此文章出自惨绿少年，转载请注明&nbsp;　　</strong>　　
</p>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#rsyncnbsp">方法一：支持rsync的网站&nbsp;</a>
    </li>
    <li>
      <a href="#wgetrsync">方法二：使用wget命令下载不支持rsync协议的源</a>
    </li>
    <li>
      <a href="#i">最后使用浏览器访问</a>
    </li>
    <li>
      <a href="#i-2">附件：</a>
    </li>
    <li>
      <a href="#wget">特别感谢:国强哥提供wget命令使用</a>
    </li>
  </ul>
</div>