---
title: 企业级memcached部署(session共享)
author: 惨绿少年
type: post
date: 2017-11-07T00:29:00+00:00
url: /clsn/lx850.html
Baidusubmit:
  - 1
views:
  - 147
zm_favorites:
  - 1
categories:
  - Linux运维
  - NoSQL
  - 数据库

---
# <span id="i"><strong>服务端部署</strong></span>

&nbsp;&nbsp; **第一个里程碑：安装依赖关系**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Memcache用到了libevent这个库用于Socket的处理。

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@nfs01 ~]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> libevent libevent-devel nc -y</pre>
</div>

**&nbsp;&nbsp;** **第二个里程碑：安装memcache**

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@nfs01 ~]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> memcached -<span style="color: #000000;">y
</span><span style="color: #008080;">2</span> [root@nfs01 ~]# <span style="color: #0000ff;">which</span><span style="color: #000000;"> memcached
</span><span style="color: #008080;">3</span> /usr/bin/memcached</pre>
</div>

**&nbsp; &nbsp;****第三个里程碑：启动memcached****服务**

<div class="cnblogs_code">
  <pre><span style="color: #008080;"> 1</span> [root@nfs01 ~]# memcached -m 16m -p <span style="color: #800080;">11211</span> -d -u root -c <span style="color: #800080;">8192</span>
<span style="color: #008080;"> 2</span> [root@nfs01 ~]# lsof -i :<span style="color: #800080;">11211</span>
<span style="color: #008080;"> 3</span> COMMAND     PID USER   FD   TYPE DEVICE SIZE/<span style="color: #000000;">OFF NODE NAME
</span><span style="color: #008080;"> 4</span> memcached <span style="color: #800080;">10796</span> root   <span style="color: #800080;">26u</span>  IPv4  <span style="color: #800080;">85717</span>      0t0  TCP *<span style="color: #000000;">:memcache (LISTEN)
</span><span style="color: #008080;"> 5</span> memcached <span style="color: #800080;">10796</span> root   <span style="color: #800080;">27u</span>  IPv6  <span style="color: #800080;">85718</span>      0t0  TCP *<span style="color: #000000;">:memcache (LISTEN)
</span><span style="color: #008080;"> 6</span> memcached <span style="color: #800080;">10796</span> root   <span style="color: #800080;">28u</span>  IPv4  <span style="color: #800080;">85721</span>      0t0  UDP *<span style="color: #000000;">:memcache
</span><span style="color: #008080;"> 7</span> memcached <span style="color: #800080;">10796</span> root   <span style="color: #800080;">29u</span>  IPv6  <span style="color: #800080;">85722</span>      0t0  UDP *<span style="color: #000000;">:memcache
</span><span style="color: #008080;"> 8</span> [root@nfs01 ~]# netstat -lntup |<span style="color: #0000ff;">grep</span><span style="color: #000000;"> memca
</span><span style="color: #008080;"> 9</span> tcp        <span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">11211</span>        <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*      LISTEN      <span style="color: #800080;">10796</span>/<span style="color: #000000;">memcached    
</span><span style="color: #008080;">10</span> tcp        <span style="color: #800080;"></span>      <span style="color: #800080;"></span> :::<span style="color: #800080;">11211</span>               :::*           LISTEN      <span style="color: #800080;">10796</span>/<span style="color: #000000;">memcached    
</span><span style="color: #008080;">11</span> udp        <span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">11211</span>      <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*                      <span style="color: #800080;">10796</span>/<span style="color: #000000;">memcached    
</span><span style="color: #008080;">12</span> udp        <span style="color: #800080;"></span>      <span style="color: #800080;"></span> :::<span style="color: #800080;">11211</span>                    :::*                    <span style="color: #800080;">10796</span>/memcached  </pre>
</div>

**注：memcached****可以同时启动多个实例，端口不一致即可。**

<div class="cnblogs_code">
  <pre>[root@nfs01 ~]# memcached -m 16m -p <span style="color: #800080;">11212</span> -d -u root -c <span style="color: #800080;">8192</span></pre>
</div>

<div align="center">
  <p style="text-align: left;">
    　　　　参数说明：
  </p>
  
  <table class="MsoTable15Grid4Accent2" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
    <tr>
      <td style="width: 120.5pt; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #c0504d; border-bottom-color: #c0504d; border-left-color: #c0504d; border-right: none; background: #c0504d; padding: 0cm 5.4pt;" valign="top" width="161">
        <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 5;" align="center">
          <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1; mso-bidi-font-weight: bold;">参数</span>
        </p>
      </td>
      
      <td style="width: 290.55pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #c0504d; border-right-color: #c0504d; border-bottom-color: #c0504d; border-left: none; background: #c0504d; padding: 0cm 5.4pt;" valign="top" width="387">
        <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 1;" align="center">
          <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1; mso-bidi-font-weight: bold;">参数说明</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 120.5pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #d99594; border-bottom-color: #d99594; border-left-color: #d99594; border-top: none; background: #f2dbdb; padding: 0cm 5.4pt;" valign="top" width="161">
        <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 68;" align="center">
          <strong><span lang="EN-US">-m</span></strong>
        </p>
      </td>
      
      <td style="width: 290.55pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #d99594; border-right-width: 1pt; border-right-color: #d99594; background: #f2dbdb; padding: 0cm 5.4pt;" valign="top" width="387">
        <p class="MsoNormal">
          <span lang="EN-US">max memory to use for items in megabytes (default: 64 MB)</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 120.5pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #d99594; border-bottom-color: #d99594; border-left-color: #d99594; border-top: none; padding: 0cm 5.4pt;" valign="top" width="161">
        <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 4;" align="center">
          <strong><span lang="EN-US">-p</span></strong>
        </p>
      </td>
      
      <td style="width: 290.55pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #d99594; border-right-width: 1pt; border-right-color: #d99594; padding: 0cm 5.4pt;" valign="top" width="387">
        <p class="MsoNormal">
          <span lang="EN-US">TCP port number to listen on (default: 11211)</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 120.5pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #d99594; border-bottom-color: #d99594; border-left-color: #d99594; border-top: none; background: #f2dbdb; padding: 0cm 5.4pt;" valign="top" width="161">
        <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 68;" align="center">
          <strong><span lang="EN-US">-d</span></strong>
        </p>
      </td>
      
      <td style="width: 290.55pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #d99594; border-right-width: 1pt; border-right-color: #d99594; background: #f2dbdb; padding: 0cm 5.4pt;" valign="top" width="387">
        <p class="MsoNormal">
          <span lang="EN-US">run as a daemon</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 120.5pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #d99594; border-bottom-color: #d99594; border-left-color: #d99594; border-top: none; padding: 0cm 5.4pt;" valign="top" width="161">
        <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 4;" align="center">
          <strong><span lang="EN-US">-u</span></strong>
        </p>
      </td>
      
      <td style="width: 290.55pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #d99594; border-right-width: 1pt; border-right-color: #d99594; padding: 0cm 5.4pt;" valign="top" width="387">
        <p class="MsoNormal">
          <span lang="EN-US">assume identity of <username> (only when run as root)</span>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 120.5pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #d99594; border-bottom-color: #d99594; border-left-color: #d99594; border-top: none; background: #f2dbdb; padding: 0cm 5.4pt;" valign="top" width="161">
        <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 68;" align="center">
          <strong><span lang="EN-US">-c</span></strong>
        </p>
      </td>
      
      <td style="width: 290.55pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #d99594; border-right-width: 1pt; border-right-color: #d99594; background: #f2dbdb; padding: 0cm 5.4pt;" valign="top" width="387">
        <p class="MsoNormal">
          <span lang="EN-US">max simultaneous connections (default: 1024)</span>
        </p>
      </td>
    </tr>
  </table>
</div>

**&nbsp;&nbsp;** **第四个里程碑：写入开机自启动**

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> <span style="color: #0000ff;">echo</span> <span style="color: #800000;">'</span><span style="color: #800000;">memcached -m 16m -p 11211 -d -u root -c 8192</span><span style="color: #800000;">'</span> >>/etc/rc.local</pre>
</div>

# <span id="web"><strong>客户端部署</strong>（web服务器）</span>

&nbsp;&nbsp; **第一个里程碑：安装PHP memcache** **扩展插件**

_<span style="text-decoration: underline;">命令集如下：</span>_

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> cd /server/<span style="color: #000000;">tools
</span><span style="color: #008080;">2</span> <span style="color: #0000ff;">wget</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">pecl.php.net/get/memcache-2.2.7.tgz</span>
<span style="color: #008080;">3</span> <span style="color: #0000ff;">tar</span> xf memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span><span style="color: #000000;">.tgz
</span><span style="color: #008080;">4</span> cd memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span>
<span style="color: #008080;">5</span> /application/php/bin/<span style="color: #000000;">phpize
</span><span style="color: #008080;">6</span> ./configure -enable-memcache --with-php-config=/application/php/bin/php-<span style="color: #000000;">config
</span><span style="color: #008080;">7</span> <span style="color: #0000ff;">make</span> && <span style="color: #0000ff;">make</span> <span style="color: #0000ff;">install</span></pre>
</div>

_<span style="text-decoration: underline;">完整操作过程：</span>_

<div class="cnblogs_code">
  <pre><span style="color: #008080;"> 1</span> [root@web01 ~]# cd /server/tools/
<span style="color: #008080;"> 2</span> [root@web01 tools]# <span style="color: #0000ff;">wget</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">pecl.php.net/get/memcache-2.2.7.tgz</span>
<span style="color: #008080;"> 3</span> [root@web01 tools]# <span style="color: #0000ff;">tar</span> xf memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span><span style="color: #000000;">.tgz
</span><span style="color: #008080;"> 4</span> [root@web01 tools]# cd memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span>
<span style="color: #008080;"> 5</span> 
<span style="color: #008080;"> 6</span> [root@web01 memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span>]# /application/php/bin/<span style="color: #000000;">phpize
</span><span style="color: #008080;"> 7</span> Configuring <span style="color: #0000ff;">for</span><span style="color: #000000;">:
</span><span style="color: #008080;"> 8</span> PHP Api Version:         <span style="color: #800080;">20121113</span>
<span style="color: #008080;"> 9</span> Zend Module Api No:      <span style="color: #800080;">20121212</span>
<span style="color: #008080;">10</span> Zend Extension Api No:   <span style="color: #800080;">220121212</span>
<span style="color: #008080;">11</span> [root@web01 memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span>]# ./configure -enable-memcache --with-php-config=/application/php/bin/php-<span style="color: #000000;">config
</span><span style="color: #008080;">12</span> [root@web01 memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span>]# <span style="color: #0000ff;">make</span> && <span style="color: #0000ff;">make</span> <span style="color: #0000ff;">install</span></pre>
</div>

**_<span style="text-decoration: underline;">查看是否安装成功</span>_**

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@web01 memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span>]# <span style="color: #0000ff;">ls</span> -l /application/php/lib/php/extensions/no-debug-non-zts-<span style="color: #800080;">20121212</span>/
<span style="color: #008080;">2</span> total <span style="color: #800080;">252</span>
<span style="color: #008080;">3</span> 
<span style="color: #008080;">4</span> -rwxr-xr-x <span style="color: #800080;">1</span> root root <span style="color: #800080;">258048</span> Nov  <span style="color: #800080;">7</span> <span style="color: #800080;">10</span>:<span style="color: #800080;">03</span> memcache.so</pre>
</div>

_&nbsp;&nbsp; memcache.so__表示插件安装成功。_

&nbsp;&nbsp; **第二个里程碑：配置memcache****客户端使其生效**

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@web01 memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">7</span>]# cd /application/php/lib/
<span style="color: #008080;">2</span> <span style="color: #000000;">[root@web01 lib]# vim php.ini
</span><span style="color: #008080;">3</span> <span style="color: #000000;">&hellip;&hellip;
</span><span style="color: #008080;">4</span> [root@web01 lib]# <span style="color: #0000ff;">tail</span> -<span style="color: #800080;">2</span><span style="color: #000000;"> php.ini
</span><span style="color: #008080;">5</span> extension_dir = <span style="color: #800000;">"</span><span style="color: #800000;">/application/php/lib/php/extensions/no-debug-non-zts-20121212/</span><span style="color: #800000;">"</span>
<span style="color: #008080;">6</span> extension = memcache.so</pre>
</div>

&nbsp;&nbsp; **第三个里程碑：检测语法，重启服务**

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@web01 lib]# /application/php/sbin/php-fpm  -<span style="color: #000000;">t
</span><span style="color: #008080;">2</span> [<span style="color: #800080;">07</span>-Nov-<span style="color: #800080;">2017</span> <span style="color: #800080;">10</span>:<span style="color: #800080;">20</span>:<span style="color: #800080;">44</span>] NOTICE: configuration <span style="color: #0000ff;">file</span> /application/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span>/etc/php-fpm.conf test is successful</pre>
</div>

<div>
  <p class="a1">
    <em>#</em><em>重启服务&nbsp;</em>
  </p>
  
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">1</span> <span style="color: #0000ff;">killall</span> php-<span style="color: #000000;">fpm
</span><span style="color: #008080;">2</span> <span style="color: #0000ff;">killall</span> php-<span style="color: #000000;">fpm
</span><span style="color: #008080;">3</span> /application/php/sbin/php-fpm</pre>
  </div>
</div>

&nbsp;&nbsp; 浏览器_访问__phpinfo__页面出现memcache__信息表示配置成功_。

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171107162209591-1628473255.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级memcached部署(session共享)" alt="" />&nbsp;
</p>

&nbsp;&nbsp; **第四个里程碑：编写测试memcache****文件**

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@web01 blog]# <span style="color: #0000ff;">cat</span><span style="color: #000000;">  test_memcache.php
</span><span style="color: #008080;">2</span> <?<span style="color: #000000;">php
&lt;/span>

<span style="color: #008080;">3</span>      $memcache =<span style="color: #000000;"> new Memcache;
</span><span style="color: #008080;">4</span>      $memcache->connect(<span style="color: #800000;">'</span><span style="color: #800000;">172.16.1.31</span><span style="color: #800000;">'</span>, <span style="color: #800080;">11211</span>) or die (<span style="color: #800000;">"</span><span style="color: #800000;">Could not connect NFS server</span><span style="color: #800000;">"</span><span style="color: #000000;">);
</span><span style="color: #008080;">5</span>      $memcache->set(<span style="color: #800000;">'</span><span style="color: #800000;">key</span><span style="color: #800000;">'</span>, <span style="color: #800000;">'</span><span style="color: #800000;">Memcache connect OK</span><span style="color: #800000;">'</span><span style="color: #000000;">);
</span><span style="color: #008080;">6</span>      $get = $memcache->get(<span style="color: #800000;">'</span><span style="color: #800000;">key</span><span style="color: #800000;">'</span><span style="color: #000000;">);
</span><span style="color: #008080;">7</span>      <span style="color: #0000ff;">echo</span><span style="color: #000000;"> $get;
</span><span style="color: #008080;">8</span> ?></pre>
</div>

&nbsp;&nbsp; _测试出现Memcache connect OK_ _表示连接成功_

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@web01 blog]# /application/php/bin/<span style="color: #000000;">php   test_memcache.php 
</span><span style="color: #008080;">2</span> Memcache connect OK</pre>
</div>

&nbsp;&nbsp; **第五个里程碑：修改php****配置**_(__设置session__共享)_

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@web01 ~]# vim /application/php/lib/php.ini</pre>
</div>

_<span style="text-decoration: underline;">原配置</span>_

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> session.save_handler =<span style="color: #000000;"> files
</span><span style="color: #008080;">2</span> session.save_path = <span style="color: #800000;">"</span><span style="color: #800000;">/tmp</span><span style="color: #800000;">"</span></pre>
</div>

**_<span style="text-decoration: underline;">修改为：</span>_**

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> session.save_handler =<span style="color: #000000;"> memcache
</span><span style="color: #008080;">2</span> session.save_path = <span style="color: #800000;">"</span><span style="color: #800000;">tcp://172.16.1.31:11211</span><span style="color: #800000;">"</span></pre>
</div>

<div>
  <p class="a">
    ⚠修改完成之后要重启php服务
  </p>
  
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">1</span> <span style="color: #0000ff;">killall</span> php-<span style="color: #000000;">fpm
</span><span style="color: #008080;">2</span> <span style="color: #0000ff;">killall</span> php-<span style="color: #000000;">fpm
</span><span style="color: #008080;">3</span> /application/php/sbin/php-fpm</pre>
  </div>
</div>

_<span style="text-decoration: underline;">修改之前phpinfo</span>__<span style="text-decoration: underline;">信息</span>_

<p style="text-align: center;">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171107162644763-1657697992.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级memcached部署(session共享)" alt="" />
</p>

_<span style="text-decoration: underline;">修改之后phpinfo</span>__<span style="text-decoration: underline;">信息</span>_

<p style="text-align: center;">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171107162816919-1984760094.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级memcached部署(session共享)" alt="" />
</p>

到此企业级memcache(session共享)部署完毕

## <span id="111_Memcachedsession">1.1.1 Memcached在集群中session共享存储的优缺点</span>

**优点：**

&nbsp;　　 1）读写速度上会比普通文件files速度快很多。

&nbsp;&nbsp; 　　2）可以解决多个服务器共用session的难题。

**缺点：**

&nbsp;　　&nbsp; 1）session数据都保存在memory中，持久化方面有所欠缺，但对session数据来说不是问题。

　　&nbsp;&nbsp; 2）一般是单台，如果部署多台，多台之间数据无法同步。通过hash算法分配依然有session丢失的问题。

**替代方案：**

　　&nbsp;&nbsp; 1）可以用其他的持久化系统存储session，例如redis，ttserver来替代memcached.

&nbsp;&nbsp;　　 2)高性能并发场景,cookies效率比session要好很多,因此,大网站都会用cookies解决会话共享的问题.

　　&nbsp;&nbsp; 3)一些不好的方法:lvs-p,nginx&nbsp; ip_hash,不推荐使用.

## <span id="nbspDedeCMSmemcache"><span style="background-color: #00ff00;">&nbsp;DedeCMS使用memcache问题</span></span>

<div>
  <div class="cnblogs_Highlighter">
    <pre class="brush:bash;gutter:true;">问题:
    上述文件进行修改后,DedeCMS发现无法访问后台 http://www.etiantia.org/dede</pre>
  </div>
</div>

**解决办法:**

&nbsp;&nbsp; 修改文件一:

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@web01 include]# <span style="color: #0000ff;">pwd</span>
<span style="color: #008080;">2</span> /application/nginx/html/www/<span style="color: #000000;">include
</span><span style="color: #008080;">3</span> 
<span style="color: #008080;">4</span> <span style="color: #000000;">[root@web01 include]# vim common.inc.php
</span><span style="color: #008080;">5</span> <span style="color: #800080;">135</span> <span style="color: #008000;">//</span><span style="color: #008000;">Session保存路径</span>
<span style="color: #008080;">6</span> <span style="color: #800080;">136</span> $enkey = substr(md5(substr($cfg_cookie_encode,<span style="color: #800080;"></span>,<span style="color: #800080;">5</span>)),<span style="color: #800080;"></span>,<span style="color: #800080;">10</span><span style="color: #000000;">);
</span><span style="color: #008080;">7</span> <span style="color: #800080;">137</span> <span style="color: #008000;">//</span><span style="color: #008000;">$sessSavePath = DEDEDATA."/sessions_{$enkey}";</span>
<span style="color: #008080;">8</span> <span style="color: #800080;">138</span> $sessSavePath = <span style="color: #800000;">"</span><span style="color: #800000;">tcp://172.16.1.31:11211</span><span style="color: #800000;">"</span><span style="color: #000000;">;
</span><span style="color: #008080;">9</span> <span style="color: #800080;">139</span> <span style="color: #0000ff;">if</span> ( !is_dir($sessSavePath) ) <span style="color: #0000ff;">mkdir</span>($sessSavePath);</pre>
</div>

&nbsp;&nbsp; 修改文件二:

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> <span style="color: #000000;">[root@web01 include]# vim vdimgck.php
</span><span style="color: #008080;">2</span> <span style="color: #800080;">24</span> $enkey = substr(md5(substr($cfg_cookie_encode,<span style="color: #800080;"></span>,<span style="color: #800080;">5</span>)),<span style="color: #800080;"></span>,<span style="color: #800080;">10</span><span style="color: #000000;">);
</span><span style="color: #008080;">3</span> <span style="color: #800080;">25</span> <span style="color: #008000;">//</span><span style="color: #008000;">$sessSavePath = DEDEDATA."/sessions_{$enkey}";</span>
<span style="color: #008080;">4</span> <span style="color: #800080;">26</span> $sessSavePath = <span style="color: #800000;">"</span><span style="color: #800000;">tcp://172.16.1.31:11211</span><span style="color: #800000;">"</span><span style="color: #000000;">;
</span><span style="color: #008080;">5</span> <span style="color: #800080;">27</span> <span style="color: #0000ff;">if</span> ( !is_dir($sessSavePath) ) <span style="color: #0000ff;">mkdir</span>($sessSavePath);</pre>
</div>

&nbsp;&nbsp; 让DedeCMS直接使用memcache的共享.解决问题.

**<span style="color: #ff0000;">特别感谢:</span>元芳**

<div style="text-align: right;" align="center">
  此文章出自惨绿少年，转载请注明&nbsp;
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#i">服务端部署</a>
    </li>
    <li>
      <a href="#web">客户端部署（web服务器）</a><ul>
        <li>
          <a href="#111_Memcachedsession">1.1.1 Memcached在集群中session共享存储的优缺点</a>
        </li>
        <li>
          <a href="#nbspDedeCMSmemcache">&nbsp;DedeCMS使用memcache问题</a>
        </li>
      </ul>
    </li>
  </ul>
</div>