---
title: 企业级LNMP架构搭建实例(基于Centos6.x)
author: 惨绿少年
type: post
date: 2017-12-11T06:19:00+00:00
url: /clsn/lx480.html
Baidusubmit:
  - 1
views:
  - 277
specs_zan:
  - 3
categories:
  - Linux运维
  - Shell编程
  - Web应用
  - 数据库
  - 玩转Linux
  - 运维基本功

---
<div id="log-box">
  <div id="catalog">
    <ul id="catalog-ul">
      <li>
        <i class="be be-arrowright"></i> <a href="#title-0" title="2.3.3.1&nbsp; 查看数据库">2.3.3.1&nbsp; 查看数据库</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-1" title="2.3.3.2&nbsp; 查看数据表信息">2.3.3.2&nbsp; 查看数据表信息</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-2" title="2.3.3.3&nbsp; 退出数据库">2.3.3.3&nbsp; 退出数据库</a>
      </li>
    </ul>
    
    <span class="log-zd"><span class="log-close"><a title="隐藏目录"><i class="be be-cross"></i><strong>目录</strong></a></span></span>
  </div>
</div>

## <span id="11_LNMP">1.1 部署LNMP架构说明</span>

### <span id="111_LNMP">1.1.1 LNMP架构内容</span>

　　01.部署linux系统

　　02.部署nginx网站服务

　　03.部署mysql数据库服务

　　04.部署php动态解析服务

### <span id="112_LNMP">1.1.2 配置LNMP架构步骤</span>

　　01.配置Nginx配置文件

　　02.配置mysql数据库信息（SQL语句）

　　03.配置wordpress博客网站

### <span id="113">1.1.3 架构服务器串联</span>

　　01.数据库数据信息迁移（web服务器上的mysql数据 迁移到10.0.0.51 数据库服务器上）

　　02.将本地储存数据挂载到NFS共享储存服务器里（共享储存用户上传的数据信息）

### <span id="114_LNMP_FastCGI">1.1.4 LNMP FastCGI知识说明</span>

&nbsp;&nbsp;&nbsp; <span style="background-color: #ff9900;">工作原理讲解说明:</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ①. 用户请求的静态文件，由nginx服务自行处理，根据静态的location配置进行处理

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; 用户请求的动态文件，由php服务进行处理，根据动态的location配置进行处理

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ②. nginx服务接收到动态请求，会将请求抛送给fastcgi，类似于nginx服务接收动态请求的秘书，秘书会将动态请求送给PHP程序

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ③. PHP如果可以处理，会将处理结果直接通过fastcgi返回给nginx程序；如果不可以处理，还会请求后端数据库，最终再把处理结果返回给nginx

## <span id="2_LNMP"><span style="background-color: #00ff00;">第2章 LNMP环境搭建步骤</span></span>

## <span id="21_linux">2.1 部署linux系统</span>

　　基本优化（ip地址 yum更新 字符集）

　　安全优化完成（iptables关闭&nbsp; selinux关闭&nbsp; tmp目录权限777）

<div>
  <p class="a4">
    &nbsp;&nbsp;&nbsp; 　　　　说明：详细配置参见 <a href="https://www.cnblogs.com/znix/p/7736899.html" target="_blank">https://www.cnblogs.com/znix/p/7736899.html</a>
  </p>
</div>

## <span id="22_nginx">2.2 部署nginx网站服务</span>

### <span id="221">2.2.1 检查软件安装的系统环境</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# <span style="color: #0000ff;">cat</span> /etc/redhat-<span style="color: #000000;">release 
CentOS release </span><span style="color: #800080;">6.9</span><span style="color: #000000;"> (Final)
[root@web01 </span>~]# <span style="color: #0000ff;">uname</span> -<span style="color: #000000;">r
</span><span style="color: #800080;">2.6</span>.<span style="color: #800080;">32</span>-<span style="color: #800080;">696</span>.el6.x86_64</pre>
  </div>
</div>

### <span id="222_nginxpcre-devel_openssl-devel">2.2.2 安装nginx的依赖包（pcre-devel openssl-devel）</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> -y pcre-devel openssl-devel</pre>
  </div>
  
  <p>
    　　pcre：兼容perl语言正则表达式，perl compatible regular expressions
  </p>
</div>

<div>
  <p class="a4">
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 　　rewirte模块 参数信息（perl方式定义正则表达式）
  </p>
  
  <p class="a4">
    　　　　　　openssl：ssh---openssh/openssl---https
  </p>
  
  <p class="a4">
    <span style="color: #00ff00;"><strong>总结：所有安装依赖软件，后面都要加上-devel</strong></span>
  </p>
</div>

### <span id="223_nginx">2.2.3 下载nginx软件</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">wget</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">nginx.org/download/nginx-1.10.2.tar.gz</span></pre>
  </div>
  
  <p>
    　　　说明：软件很小，用心查看一下
  </p>
</div>

　**　解压软件**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">tar</span> xf nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>.<span style="color: #0000ff;">tar</span>.gz</pre>
  </div>
</div>

### <span id="224_www">2.2.4 创建管理用户 www</span>

<div>
  <div class="cnblogs_code">
    <pre>useradd -M -s /sbin/nologin www</pre>
  </div>
</div>

### <span id="225_nbspnginx">2.2.5 &nbsp;nginx软件编译安装过程</span>

#### <span id="2251nbsp"><strong>2.2.5.1&nbsp; </strong><strong>注意</strong></span>

<div>
  <p class="a4">
    　　软件编译安装步骤
  </p>
  
  <p class="a4">
    　　&nbsp; a>软件解压配置（将软件程序安装到哪个目录中 开启nginx软件的哪些功能）
  </p>
  
  <p class="a4">
    &nbsp;　　 b>软件编译过程
  </p>
  
  <p class="a4">
    &nbsp; 　　c>软件编译安装过程
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;　　&nbsp;&nbsp;&nbsp; 注意顺序，顺序不对软件安装会出错

#### <span id="2252nbsp"><strong>2.2.5.2&nbsp; </strong><strong>编译安装软件</strong></span>

　　1、配置软件，在软件的解压目录中

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>]# ./configure --prefix=/application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span> --user=www --group=www --with-http_stub_status_module --with-http_ssl_module</pre>
  </div>
</div>

编译参数说明：

<div>
  <div style="mso-element: para-border-div; border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; mso-border-shadow: yes; background: #F2F2F2; mso-shading: windowtext; mso-pattern: gray-5 auto; margin-left: 38.95pt; margin-right: 37.85pt;">
    <p class="a" style="margin: 1pt 0cm; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">--prefix&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">表示指定软件安装到哪个目录中，指定目录不存在会自动创建</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">--user/--group &nbsp; &nbsp; &nbsp; &nbsp;nginx</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">工作进程由哪个用户运行管理</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">--with-http_stub_status_module&nbsp; &nbsp;&nbsp;</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">启动</span><span lang="EN-US">nginx</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">状态模块功能（用户访问</span><span lang="EN-US">nginx</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">的网络信息）</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">--with-http_ssl_module &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">启动</span><span lang="EN-US">https</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">功能模块</span>
    </p>
  </div>
</div>

通过软件编译过程中的返回值是否正确，确认配置是否正确

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>]# <span style="color: #0000ff;">echo</span> $?
<span style="color: #800080;"></span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2、编译软件

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>]# <span style="color: #0000ff;">make</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3、编译安装

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>]# <span style="color: #0000ff;">make</span> <span style="color: #0000ff;">install</span></pre>
  </div>
</div>

### <span id="226">2.2.6 创建软连接</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# <span style="color: #0000ff;">ln</span> -s /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>/ /application/nginx</pre>
  </div>
</div>

### <span id="227_nginxconf_nginx">2.2.7 精简化nginx.conf 主配置文件内容, 编写nginx配置文件</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 conf]# <span style="color: #0000ff;">egrep</span> -v <span style="color: #800000;">"</span><span style="color: #800000;">#|^$</span><span style="color: #800000;">"</span> nginx.conf.default >nginx.conf</pre>
  </div>
</div>

### <span id="228">2.2.8 启动程序</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# /application/nginx/sbin/<span style="color: #000000;">nginx
[root@web01 application]#</span></pre>
  </div>
  
  <p class="ad">
    检查是否启动
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# <span style="color: #0000ff;">ps</span> -ef |<span style="color: #0000ff;">grep</span><span style="color: #000000;"> nginx
root      </span><span style="color: #800080;">26548</span>      <span style="color: #800080;">1</span>  <span style="color: #800080;"></span> <span style="color: #800080;">20</span>:<span style="color: #800080;">13</span> ?        <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span> nginx: master process /application/nginx/sbin/<span style="color: #000000;">nginx
www       </span><span style="color: #800080;">26549</span>  <span style="color: #800080;">26548</span>  <span style="color: #800080;"></span> <span style="color: #800080;">20</span>:<span style="color: #800080;">13</span> ?        <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span><span style="color: #000000;"> nginx: worker process        
root      </span><span style="color: #800080;">26551</span>  <span style="color: #800080;">23431</span>  <span style="color: #800080;">3</span> <span style="color: #800080;">20</span>:<span style="color: #800080;">13</span> pts/<span style="color: #800080;"></span>    <span style="color: #800080;">00</span>:<span style="color: #800080;">00</span>:<span style="color: #800080;">00</span> <span style="color: #0000ff;">grep</span> --color=auto nginx</pre>
  </div>
</div>

检查端口信息

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 application]# netstat -lntup |<span style="color: #0000ff;">grep</span> <span style="color: #800080;">80</span><span style="color: #000000;">
tcp        </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">80</span>                  <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*                   LISTEN      <span style="color: #800080;">26548</span>/nginx  </pre>
  </div>
</div>

服务部署完成, 修改hosts解析文件，进行浏览器访问测试

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211214724321-1303615457.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" />
</p>

**至此软件安装完毕！**

## <span id="23_mysql">2.3 部署mysql数据库服务</span>

### <span id="231_mysql">2.3.1 下载mysql软件</span>

这里使用的是5.6.34版本；在下载mysql的时候一定要注意与系统匹配的版本。

<div>
  <div class="cnblogs_code">
    <pre>mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>-linux-glibc2.<span style="color: #800080;">5</span>-x86_64.<span style="color: #0000ff;">tar</span>.gz</pre>
  </div>
</div>

方法一：mysql官网下载地址

<div>
  <p class="a4">
    　　　　<a href="https://dev.mysql.com/downloads/mirrors/" target="_blank">https://dev.mysql.com/downloads/mirrors/</a>
  </p>
</div>

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211214845821-742864751.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" />
</p>

尽量使用ftp下载，http的下载方式较为繁琐。下载的时候选择与自己近的服务进行下载即可。

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211214854555-1174512644.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" />&nbsp;
</p>

<div>
  <p class="a4">
    方法二： 使用搜狐的镜像站也可以进行下载，注意使用的软件版本。
  </p>
  
  <p class="a4">
    　　<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://mirrors.sohu.com/mysql/"  target="_blank">http://mirrors.sohu.com/mysql/</a>
  </p>
</div>

### <span id="232_mysql">2.3.2 【二进制包方式】安装mysql数据库软件</span>

#### <span id="2321nbsp"><strong>2.3.2.1&nbsp; </strong><strong>解压二进制包软件</strong><strong>?</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>cd /server/tools/<span style="color: #000000;">
[root@web01 tools]# </span><span style="color: #0000ff;">tar</span> xf mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>-linux-glibc2.<span style="color: #800080;">5</span>-x86_64.<span style="color: #0000ff;">tar</span>.gz</pre>
  </div>
</div>

#### <span id="2322nbsp_mysql"><strong>2.3.2.2&nbsp; </strong><strong>创建储存目录管理用户mysql</strong><strong>?</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 tools]# useradd -s /sbin/nologin -M mysql</pre>
  </div>
</div>

#### <span id="2323nbsp"><strong>2.3.2.3&nbsp; </strong><strong>将解压后的二进制包放置到程序目录中</strong><strong>?</strong></span>

将mysql解压后的程序包搬家到程序目录下，并创建软连接。

<div>
  <div class="cnblogs_code">
    <pre>cd /server/tools/
<span style="color: #0000ff;">mv</span> mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>-linux-glibc2.<span style="color: #800080;">5</span>-x86_64 /application/mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>
<span style="color: #0000ff;">ln</span> -s /application/mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>  /application/mysql</pre>
  </div>
</div>

#### <span id="2324nbsp_mysql"><strong>2.3.2.4&nbsp; </strong><strong>对mysql</strong><strong>数据储存目录进行授权</strong><strong>?</strong></span>

让mysql用户管理 /application/mysql/data

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# <span style="color: #0000ff;">chown</span> -R mysql.mysql /application/mysql/data/<span style="color: #000000;">
[root@web01 </span>~]# ll /application/mysql/data/ -<span style="color: #000000;">d
drwxr</span>-xr-x <span style="color: #800080;">3</span> mysql mysql <span style="color: #800080;">4096</span> Oct <span style="color: #800080;">26</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">26</span> /application/mysql/data/</pre>
  </div>
</div>

#### <span id="2325nbsp"><strong>2.3.2.5&nbsp; </strong><strong>初始化数据库服务</strong><strong>?</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>/application/mysql/scripts/mysql_install_db --basedir=/application/mysql --datadir=/application/mysql/data --user=mysql</pre>
  </div>
</div>

**①始化参数说明：**

<div>
  <div style="mso-element: para-border-div; border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; mso-border-shadow: yes; background: #F2F2F2; mso-shading: windowtext; mso-pattern: gray-5 auto; margin-left: 38.95pt; margin-right: 89.85pt;">
    <p class="a" style="margin: 1pt 0cm; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">--basedir&nbsp; </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">数据库软件命令，软件安装在哪里</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">--datadir&nbsp; </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">数据存放目录，数据存放在哪里</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">--user&nbsp;&nbsp;&nbsp;&nbsp; </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">管理</span><span lang="EN-US">mysql</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">的用户，</span><span lang="EN-US">MySQL</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">使用的用户谁</span>
    </p>
  </div>
</div>

②【*****】**判定初始化命令执行成功的方法**

　　1）确认返回值，看是否为0

　　　　&nbsp;<span class="cnblogs_code">[root@web01 ~]# <span style="color: #0000ff;">echo</span> $?</span>&nbsp;

　　2）确认输出的内容中有两个ok

　　3）通过数据库初始化操作，在data目录中创建出默认的数据库信息和相关表信息

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# <span style="color: #0000ff;">ls</span> -l /application/mysql/data/<span style="color: #000000;">
total </span><span style="color: #800080;">110604</span>
-rw-rw---- <span style="color: #800080;">1</span> mysql mysql <span style="color: #800080;">12582912</span> Oct <span style="color: #800080;">26</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">56</span><span style="color: #000000;"> ibdata1
</span>-rw-rw---- <span style="color: #800080;">1</span> mysql mysql <span style="color: #800080;">50331648</span> Oct <span style="color: #800080;">26</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">56</span><span style="color: #000000;"> ib_logfile0
</span>-rw-rw---- <span style="color: #800080;">1</span> mysql mysql <span style="color: #800080;">50331648</span> Oct <span style="color: #800080;">26</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">56</span><span style="color: #000000;"> ib_logfile1
drwx</span>------ <span style="color: #800080;">2</span> mysql mysql     <span style="color: #800080;">4096</span> Oct <span style="color: #800080;">26</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">56</span><span style="color: #000000;"> mysql
drwx</span>------ <span style="color: #800080;">2</span> mysql mysql     <span style="color: #800080;">4096</span> Oct <span style="color: #800080;">26</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">56</span><span style="color: #000000;"> performance_schema
drwxr</span>-xr-x <span style="color: #800080;">2</span> mysql mysql     <span style="color: #800080;">4096</span> Oct <span style="color: #800080;">26</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">26</span> test</pre>
  </div>
</div>

**③初始化输出的内容信息**

<div>
  <div class="cnblogs_code">
    <pre>To start mysqld at boot <span style="color: #0000ff;">time</span><span style="color: #000000;"> you have to copy
support</span>-files/mysql.server to the right place <span style="color: #0000ff;">for</span> your system</pre>
  </div>
  
  <p>
    　　启动mysql服务，可以复制support-files/mysql.server到系统的启动目录中
  </p>
</div>

　　mysql.server程序自带的启动脚本文件

<div>
  <div class="cnblogs_code">
    <pre>PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER !<span style="color: #000000;">
To </span><span style="color: #0000ff;">do</span> so, start the server, <span style="color: #0000ff;">then</span><span style="color: #000000;"> issue the following commands:
</span>/application/mysql/bin/mysqladmin -u root password <span style="color: #800000;">'</span><span style="color: #800000;">new-password</span><span style="color: #800000;">'</span>
/application/mysql/bin/mysqladmin -u root -h web01 password <span style="color: #800000;">'</span><span style="color: #800000;">new-password</span><span style="color: #800000;">'</span></pre>
  </div>
  
  <p>
    　　说明： 表示对mysql服务管理源root用户设置密码
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">You can start the MySQL daemon with:
cd . ; </span>/application/mysql/bin/mysqld_safe &</pre>
  </div>
  
  <p>
    　　可以以后台方式运行 mysqld_safe 脚本命令，也可以运行mysql服务
  </p>
</div>

#### <span id="2326nbsp"><strong>2.3.2.6&nbsp; </strong><strong>将启动脚本文件复制到启动目录中</strong><strong>?</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# <span style="color: #0000ff;">cp</span> -a  /application/mysql/support-files/mysql.server  /etc/init.d/mysqld</pre>
  </div>
  
  <p>
    　　修改启动服务脚本相关文件内容--<span style="background-color: #00ff00;">更改软件的存放目录</span>
  </p>
</div>

&nbsp; 　　　　<span style="color: #ff0000;"><strong>注意：</strong></span> 修改的是两个位置

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">sed</span> -i <span style="color: #800000;">'</span><span style="color: #800000;">s#/usr/local/mysql#/application/mysql#g</span><span style="color: #800000;">'</span> /application/mysql/bin/mysqld_safe /etc/init.d/mysqld</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　添加到开机自启动，让chkconfig 管理，能够开机自启动

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# chkconfig --<span style="color: #000000;">add mysqld
[root@web01 </span>~]# chkconfig mysqld on</pre>
  </div>
</div>

#### <span id="2327nbsp_mysql"><strong>2.3.2.7&nbsp; </strong><strong>设置mysql</strong><strong>服务配置文件</strong><strong>?</strong></span>

mysql默认配置文件保存位置

<div>
  <div class="cnblogs_code">
    <pre>/etc/my.cnf</pre>
  </div>
</div>

从软件中复制出来配置文件，使用软件中自带的配置文件即可

<div>
  <div class="cnblogs_code">
    <pre> \<span style="color: #0000ff;">cp</span> /application/mysql/support-files/my-default.cnf /etc/my.cnf</pre>
  </div>
</div>

#### <span id="2328nbsp_mysql"><strong>2.3.2.8&nbsp; </strong><strong>启动mysql</strong><strong>服务</strong><strong>?</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# /etc/init.d/<span style="color: #000000;">mysqld start
Starting MySQL...... SUCCESS</span>!</pre>
  </div>
</div>

#### <span id="2329nbsp"><strong>2.3.2.9&nbsp; </strong><strong>检查端口信息，确认服务是否启动</strong><strong>?</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# netstat -lntup |<span style="color: #0000ff;">grep</span> <span style="color: #800080;">3306</span><span style="color: #000000;">
tcp    </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> :::<span style="color: #800080;">3306</span>               :::*             LISTEN      <span style="color: #800080;">54042</span>/mysqld    </pre>
  </div>
</div>

#### <span id="23210nbspnbspnbspnbsp_root"><strong>2.3.2.10&nbsp;&nbsp;&nbsp;&nbsp; </strong><strong>设置root</strong><strong>用户密码信息</strong><strong>?</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# /application/mysql/bin/mysqladmin -u root password <span style="color: #800000;">'</span><span style="color: #800000;">clsn123</span><span style="color: #800000;">'</span><span style="color: #000000;">
Warning: Using a password on the command line interface can be insecure.</span></pre>
  </div>
</div>

#### <span id="23211nbspnbspnbspnbsp"><strong>2.3.2.11&nbsp;&nbsp;&nbsp;&nbsp; </strong><strong>测试</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@web01 ~]# /application/mysql/bin/mysql -uroot -<span style="color: #000000;">pclsn123
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection </span><span style="color: #0000ff;">id</span> is <span style="color: #800080;">14</span><span style="color: #000000;">
Server version: </span><span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span><span style="color: #000000;"> MySQL Community Server (GPL)

Copyright (c) </span><span style="color: #800080;">2000</span>, <span style="color: #800080;">2016</span>, Oracle and/<span style="color: #000000;">or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and</span>/<span style="color: #000000;">or its
affiliates. Other names may be trademarks of their respective
owners.

Type </span><span style="color: #800000;">'</span><span style="color: #800000;">help;</span><span style="color: #800000;">'</span> or <span style="color: #800000;">'</span><span style="color: #800000;">\h</span><span style="color: #800000;">'</span> <span style="color: #0000ff;">for</span> help. Type <span style="color: #800000;">'</span><span style="color: #800000;">\c</span><span style="color: #800000;">'</span> to <span style="color: #0000ff;">clear</span><span style="color: #000000;"> the current input statement.

mysql</span>></pre>
  </div>
</div>

**登录数据库命令简化方法**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">echo</span> <span style="color: #800000;">'</span><span style="color: #800000;">export PATH=/application/mysql/bin:$PATH</span><span style="color: #800000;">'</span> >>/etc/<span style="color: #000000;">profile
source </span>/etc/<span style="color: #000000;">profile
</span><span style="color: #0000ff;">which</span> mysql</pre>
  </div>
</div>

### <span id="233_mysql">2.3.3 管理mysql数据库</span>

<span class="directory"></span>

#### <span id="2331nbsp">2.3.3.1&nbsp; 查看数据库</span> {#title-0}

<div>
  <div class="cnblogs_code">
    <pre>mysql><span style="color: #000000;"> show databases;
</span>+--------------------+
| Database             |
+--------------------+
| information_schema |
| mysql                 |
| performance_schema |
| test                  |
+--------------------+
<span style="color: #800080;">4</span> rows <span style="color: #0000ff;">in</span> set (<span style="color: #800080;">0.26</span> sec)</pre>
  </div>
</div>

<span class="directory"></span>

#### <span id="2332nbsp">2.3.3.2&nbsp; 查看数据表信息</span> {#title-1}

<div>
  <div class="cnblogs_code">
    <pre>mysql><span style="color: #000000;"> use  mysql;show tables;
Reading table information </span><span style="color: #0000ff;">for</span><span style="color: #000000;"> completion of table and column names
You can turn off this feature to get a quicker startup with </span>-<span style="color: #000000;">A

Database changed
</span>+---------------------------+
| Tables_in_mysql             |
+---------------------------+
| columns_priv                 |
| db                             |
| event                         |
| func                          |
| general_log                  |
| help_category                |
| help_keyword                 |
| help_relation                |
| help_topic                   |
| innodb_index_stats        |
| innodb_table_stats        |
| ndb_binlog_index          |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| servers                   |
| slave_master_info         |
| slave_relay_log_info      |
| slave_worker_info         |
| slow_log                  |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
<span style="color: #800080;">28</span> rows <span style="color: #0000ff;">in</span> set (<span style="color: #800080;">0.00</span> sec)</pre>
  </div>
</div>

<span class="directory"></span>

#### <span id="2333nbsp">2.3.3.3&nbsp; 退出数据库</span> {#title-2}

<div>
  <p class="a4">
    　　&nbsp;<span class="cnblogs_code">quit | exit</span>&nbsp;
  </p>
  
  <p class="a4">
    退出数据库时，尽量不要用ctrl+c进行退出mysql 用ctrl+d进行退出
  </p>
</div>

数据库基础操作（数据库框架）

<div>
  <div class="cnblogs_code">
    <pre>show databases;                &lt;---<span style="color: #000000;"> 查询默认的数据库信息
create database clsn;        </span>&lt;---<span style="color: #000000;">创建新的数据库
drop database clsn;          </span>&lt;---<span style="color: #000000;">删除存在的数据库
use mysql;                     </span>&lt;---<span style="color: #000000;"> 表示选择使用一个数据库，相当于cd进入一个数据库
show tables；                  </span>&lt;---<span style="color: #000000;">查看数据库中表信息
</span><span style="color: #0000ff;">select</span> database();             &lt;---<span style="color: #000000;"> 表示查看当前所在数据库，类似于pwd命令的功能
</span><span style="color: #0000ff;">select</span> user();                 &lt;---<span style="color: #000000;"> 查看当前登录数据库的用户，类似于whoami命令
                                    并且mysql还可以限制指定用户可以从哪里进行连接登录数据库
</span><span style="color: #0000ff;">select</span> * from user\G;          &lt;---<span style="color: #000000;">查看user表中所有信息，并且纵行显示

</span><span style="color: #0000ff;">select</span> user,host from user;         ---<span style="color: #000000;">查看user表中指定信息，并且横行显示
</span><span style="color: #0000ff;">select</span> user,host from mysql.user;   ---<span style="color: #000000;">查看可以登录mysql数据库的目录，以及都可以从哪里进行管理mysql数据库
grant all on </span>*.* to user@<span style="color: #800000;">'</span><span style="color: #800000;">host</span><span style="color: #800000;">'</span> identified by <span style="color: #800000;">'</span><span style="color: #800000;">clsn123</span><span style="color: #800000;">'</span>;           ---<span style="color: #000000;">创建用户 
grant all on </span>*.* to Old_Boy@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> identified by <span style="color: #800000;">'</span><span style="color: #800000;">clsn123</span><span style="color: #800000;">'</span>;   ---<span style="color: #000000;">创建用户（大写用户）
drop user </span><span style="color: #800000;">'</span><span style="color: #800000;">user</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">host</span><span style="color: #800000;">'</span><span style="color: #000000;">;
flush privileges；                  </span>--- 刷新权限</pre>
  </div>
</div>

## <span id="24_php">2.4 部署php服务</span>

### <span id="241_PHP14">2.4.1 解决PHP软件的依赖关系（14个依赖包）</span>

#### <span id="2411nbsp_base"><strong>2.4.1.1&nbsp; </strong><strong>基于base</strong><strong>源的个依赖包</strong></span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> zlib-devel libxml2-devel libjpeg-devel libjpeg-turbo-devel libiconv-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel libxslt-devel -y</pre>
  </div>
</div>

**检查的方法一：rpm**

<div>
  <div class="cnblogs_code">
    <pre>rpm -qa zlib-devel libxml2-devel libjpeg-devel libjpeg-turbo-devel libiconv-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel</pre>
  </div>
</div>

<span style="background-color: #ffff00;"><strong>检查的方法二：</strong>再安装一遍即可确认是否都安装上</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> -y zlib-devel libxml2-devel libjpeg-devel libjpeg-turbo-devel libiconv-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel</pre>
  </div>
</div>

#### <span id="2412nbsp_libiconv"><strong>2.4.1.2&nbsp; </strong><strong>libiconv</strong><strong>软件 </strong><strong>和字符集转换相关软件</strong></span>

　　由于该软件yum安装不上，需要单独安装一下。

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">mkdir</span> -p /server/<span style="color: #000000;">tools
cd </span>/server/<span style="color: #000000;">tools
#</span><span style="color: #0000ff;">wget</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz</span>
<span style="color: #0000ff;">tar</span> zxf libiconv-<span style="color: #800080;">1.14</span>.<span style="color: #0000ff;">tar</span><span style="color: #000000;">.gz
cd libiconv</span>-<span style="color: #800080;">1.14</span><span style="color: #000000;">
.</span>/configure --prefix=/usr/local/<span style="color: #000000;">libiconv
</span><span style="color: #0000ff;">make</span>
<span style="color: #0000ff;">make</span> <span style="color: #0000ff;">install</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="background-color: #00ff00;">&nbsp; 说明:此软件在centos6.8之后已经自带此软件功能，可以不进行安装&nbsp;</span>

**<span style="font-family: 等线; mso-ascii-font-family: Calibri; mso-ascii-theme-font: minor-latin; mso-fareast-font-family: 等线; mso-fareast-theme-font: minor-fareast; mso-hansi-font-family: Calibri; mso-hansi-theme-font: minor-latin; color: red; background: aqua; mso-highlight: aqua;">编译好的软件如何删除</span>**

<div>
  <p class="a4">
    &nbsp; &nbsp; 　　<span style="font-family: 等线; font-size: 11pt;">删除安装后的程序目录即可</span>
  </p>
</div>

<p class="MsoNormal">
  <strong><span style="color: red; background: aqua; mso-highlight: aqua;" lang="EN-US">fpm </span></strong><strong><span style="font-family: 等线; mso-ascii-font-family: Calibri; mso-ascii-theme-font: minor-latin; mso-fareast-font-family: 等线; mso-fareast-theme-font: minor-fareast; mso-hansi-font-family: Calibri; mso-hansi-theme-font: minor-latin; color: red; background: aqua; mso-highlight: aqua;">定制</span><span style="color: red; background: aqua; mso-highlight: aqua;" lang="EN-US">rpm</span></strong><strong><span style="font-family: 等线; mso-ascii-font-family: Calibri; mso-ascii-theme-font: minor-latin; mso-fareast-font-family: 等线; mso-fareast-theme-font: minor-fareast; mso-hansi-font-family: Calibri; mso-hansi-theme-font: minor-latin; color: red; background: aqua; mso-highlight: aqua;">包</span></strong>
</p>

<div>
  <p class="a4">
    　　　rpm包制作软件---把编译后的程序目录进行打包，通过fpm相关参数指定rpm解压之前要先安装哪些依赖
  </p>
</div>

#### <span id="2413nbsp_3"><strong>2.4.1.3&nbsp; </strong><strong>安装加密相关的依赖软件（3</strong><strong>个）</strong></span>

　　　　这三个软件依赖与epel源

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">yum</span> -y <span style="color: #0000ff;">install</span> libmcrypt-<span style="color: #000000;">devel mhash mcrypt
rpm </span>-qa libmcrypt-devel mhash mcrypt</pre>
  </div>
</div>

### <span id="242_php">2.4.2 编译安装php过程</span>

**解压安装包**

<div>
  <div class="cnblogs_code">
    <pre>cd /server/tools/<span style="color: #000000;">
[root@web01 lnmp]# </span><span style="color: #0000ff;">tar</span> xf php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span>.<span style="color: #0000ff;">tar</span>.gz </pre>
  </div>
</div>

**配置php** **（配置的参数较多）**

　　mysqlnd本地没有mysql

<div>
  <div class="cnblogs_code">
    <pre>./<span style="color: #000000;">configure \
</span>--prefix=/application/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span><span style="color: #000000;"> \
</span>--with-mysql=<span style="color: #000000;">mysqlnd \
</span>--with-pdo-mysql=<span style="color: #000000;">mysqlnd \
</span>--with-iconv-<span style="color: #0000ff;">dir</span>=/usr/local/<span style="color: #000000;">libiconv \
</span>--with-freetype-<span style="color: #0000ff;">dir</span><span style="color: #000000;"> \
</span>--with-jpeg-<span style="color: #0000ff;">dir</span><span style="color: #000000;"> \
</span>--with-png-<span style="color: #0000ff;">dir</span><span style="color: #000000;"> \
</span>--with-<span style="color: #000000;">zlib \
</span>--with-libxml-<span style="color: #0000ff;">dir</span>=/<span style="color: #000000;">usr \
</span>--enable-<span style="color: #000000;">xml \
</span>--disable-<span style="color: #000000;">rpath \
</span>--enable-<span style="color: #000000;">bcmath \
</span>--enable-<span style="color: #000000;">shmop \
</span>--enable-<span style="color: #000000;">sysvsem \
</span>--enable-inline-<span style="color: #000000;">optimization \
</span>--with-<span style="color: #000000;">curl \
</span>--enable-<span style="color: #000000;">mbregex \
</span>--enable-<span style="color: #000000;">fpm \
</span>--enable-<span style="color: #000000;">mbstring \
</span>--with-<span style="color: #000000;">mcrypt \
</span>--with-<span style="color: #000000;">gd \
</span>--enable-gd-native-<span style="color: #000000;">ttf \
</span>--with-<span style="color: #000000;">openssl \
</span>--with-<span style="color: #000000;">mhash \
</span>--enable-<span style="color: #000000;">pcntl \
</span>--enable-<span style="color: #000000;">sockets \
</span>--with-<span style="color: #000000;">xmlrpc \
</span>--enable-<span style="color: #000000;">soap \
</span>--enable-<span style="color: #0000ff;">short</span>-<span style="color: #000000;">tags \
</span>--enable-<span style="color: #000000;">static \
</span>--with-<span style="color: #000000;">xsl \
</span>--with-fpm-user=<span style="color: #000000;">www \
</span>--with-fpm-group=<span style="color: #000000;">www \
</span>--enable-<span style="color: #0000ff;">ftp</span><span style="color: #000000;"> \
</span>--enable-opcache=no</pre>
  </div>
</div>

**PHP编译参数详解**

<div class="cnblogs_code" onclick="cnblogs_code_show('a4acb392-e71b-448d-89b2-1168c1e66e5c')">
  <img id="code_img_closed_a4acb392-e71b-448d-89b2-1168c1e66e5c" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-10.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" /><img id="code_img_opened_a4acb392-e71b-448d-89b2-1168c1e66e5c" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('a4acb392-e71b-448d-89b2-1168c1e66e5c',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-10.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" /></p> 
  
  <div id="cnblogs_code_open_a4acb392-e71b-448d-89b2-1168c1e66e5c" class="cnblogs_code_hide">
    <pre><span style="color: #008080;"> 1</span> ./<span style="color: #000000;">configure 编译参数
</span><span style="color: #008080;"> 2</span> 
<span style="color: #008080;"> 3</span> &ndash;prefix=/application/php5.<span style="color: #800080;">3.27</span> 指定php的安装路径为/application/php5.<span style="color: #800080;">3.27</span>
<span style="color: #008080;"> 4</span> 
<span style="color: #008080;"> 5</span> &ndash;with-mysql=/application/mysql/ 
<span style="color: #008080;"> 6</span> 需要指定mysql的安装路径,安装PHP需要的MySQL相关内容。当然如果没有MySQL软件包，也可以不单独安装，这样的情况可使用&ndash;with-mysql=mysqlnd替代&ndash;with-mysql=/application/<span style="color: #000000;">mysql,因为PHP软件里面已经自带连接MySQL的客户端工具。    
</span><span style="color: #008080;"> 7</span> 
<span style="color: #008080;"> 8</span> &ndash;with-iconv-<span style="color: #0000ff;">dir</span>=/usr/local/<span style="color: #000000;">libiconv    libiconv库,各种字符集间的转换
</span><span style="color: #008080;"> 9</span> 
<span style="color: #008080;">10</span> &ndash;with-freetype-<span style="color: #0000ff;">dir</span><span style="color: #000000;">    打开对freetype字体库支持
</span><span style="color: #008080;">11</span> 
<span style="color: #008080;">12</span> &ndash;with-jpeg-<span style="color: #0000ff;">dir</span><span style="color: #000000;"> 打开对jpeg图片的支持
</span><span style="color: #008080;">13</span> 
<span style="color: #008080;">14</span> &ndash;with-png-<span style="color: #0000ff;">dir</span><span style="color: #000000;"> 打开对png图片的支持
</span><span style="color: #008080;">15</span> 
<span style="color: #008080;">16</span> &ndash;with-<span style="color: #000000;">zlib 打开zlib库的支持,用于http压缩传输
</span><span style="color: #008080;">17</span> 
<span style="color: #008080;">18</span> &ndash;with-libxml-<span style="color: #0000ff;">dir</span>=/<span style="color: #000000;">usr 打开libxml2库的支持
</span><span style="color: #008080;">19</span> 
<span style="color: #008080;">20</span> &ndash;enable-<span style="color: #000000;">xml    
</span><span style="color: #008080;">21</span> 
<span style="color: #008080;">22</span> &ndash;disable-<span style="color: #000000;">rpath 关闭额外的运行库文件
</span><span style="color: #008080;">23</span> 
<span style="color: #008080;">24</span> &ndash;enable-safe-<span style="color: #000000;">mode 打开安全模式
</span><span style="color: #008080;">25</span> 
<span style="color: #008080;">26</span> &ndash;enable-<span style="color: #000000;">bcmath 打开图片大小调整,用zabbix监控时会用到该模块
</span><span style="color: #008080;">27</span> 
<span style="color: #008080;">28</span> &ndash;enable-<span style="color: #000000;">shmop 
</span><span style="color: #008080;">29</span> 
<span style="color: #008080;">30</span> &ndash;enable-<span style="color: #000000;">sysvsem 使用sysv信号机制,则打开此选项
</span><span style="color: #008080;">31</span> 
<span style="color: #008080;">32</span> &ndash;enable-inline-<span style="color: #000000;">optimization 优化线程
</span><span style="color: #008080;">33</span> 
<span style="color: #008080;">34</span> &ndash;with-<span style="color: #000000;">curl 打开curl浏览工具的支持
</span><span style="color: #008080;">35</span> 
<span style="color: #008080;">36</span> &ndash;with-<span style="color: #000000;">curlwrappers 运维curl工具打开url流
</span><span style="color: #008080;">37</span> 
<span style="color: #008080;">38</span> &ndash;enable-<span style="color: #000000;">mbregex     
</span><span style="color: #008080;">39</span> 
<span style="color: #008080;">40</span> &ndash;enable-<span style="color: #000000;">mbstring 支持mbstring
</span><span style="color: #008080;">41</span> 
<span style="color: #008080;">42</span> &ndash;with-<span style="color: #000000;">mcrypt 编码函数库
</span><span style="color: #008080;">43</span> 
<span style="color: #008080;">44</span> &ndash;with-<span style="color: #000000;">gd 打开gd库的支持
</span><span style="color: #008080;">45</span> 
<span style="color: #008080;">46</span> &ndash;enable-gd-native-<span style="color: #000000;">ttf 支持TrueType字符串函数库
</span><span style="color: #008080;">47</span> 
<span style="color: #008080;">48</span> &ndash;with-<span style="color: #000000;">openl openl的支持,加密传输时用到
</span><span style="color: #008080;">49</span> 
<span style="color: #008080;">50</span> &ndash;with-<span style="color: #000000;">mhash mhash算法的扩展
</span><span style="color: #008080;">51</span> 
<span style="color: #008080;">52</span> &ndash;enable-<span style="color: #000000;">pcntl freeTDS需要用到,可能是链接mql
</span><span style="color: #008080;">53</span> 
<span style="color: #008080;">54</span> &ndash;enable-<span style="color: #000000;">sockets 打开sockets支持
</span><span style="color: #008080;">55</span> 
<span style="color: #008080;">56</span> &ndash;with-xmlrpc 打开xml-<span style="color: #000000;">rpc的c语言
</span><span style="color: #008080;">57</span> 
<span style="color: #008080;">58</span> &ndash;enable-<span style="color: #0000ff;">zip</span><span style="color: #000000;"> 打开对zip的支持
</span><span style="color: #008080;">59</span> 
<span style="color: #008080;">60</span> &ndash;enable-<span style="color: #000000;">soap soap模块的扩展
</span><span style="color: #008080;">61</span> 
<span style="color: #008080;">62</span> &ndash;enable-<span style="color: #0000ff;">short</span>-<span style="color: #000000;">tags 开始和标记函数
</span><span style="color: #008080;">63</span> 
<span style="color: #008080;">64</span> &ndash;enable-zend-<span style="color: #000000;">multibyte 支持zend的多字节
</span><span style="color: #008080;">65</span> 
<span style="color: #008080;">66</span> &ndash;enable-<span style="color: #000000;">static 生成静态链接库
</span><span style="color: #008080;">67</span> 
<span style="color: #008080;">68</span> &ndash;with-<span style="color: #000000;">xsl 打开XSLT文件支持,扩展libXML2库,需要libxslt软件
</span><span style="color: #008080;">69</span> 
<span style="color: #008080;">70</span> &ndash;enable-<span style="color: #0000ff;">ftp</span><span style="color: #000000;">    打开ftp的支持
</span><span style="color: #008080;">71</span> 
<span style="color: #008080;">72</span> &ndash;enable-fpm    表示激活PHP-<span style="color: #000000;">FPM方式服务,即FactCGI方式运行PHP服务。
</span><span style="color: #008080;">73</span> 
<span style="color: #008080;">74</span> &ndash;with-fpm-user=www    指定PHP-<span style="color: #000000;">FPM进程管理的用户为www,此处最好和Nginx服务用户统一。
</span><span style="color: #008080;">75</span> 
<span style="color: #008080;">76</span> &ndash;with-fpm-group=www    指定PHP-FPM进程管理用户组为www,此处最好和Nginx服务用户组统一。</pre>
  </div>
  
  <p>
    <span class="cnblogs_code_collapse">View Code PHP编译参数详解</span></div> 
    
    <p>
      <strong>&nbsp; &nbsp; &nbsp;&nbsp;</strong><strong>输出的信息</strong>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #000000;">Generating files
configure: creating .</span>/<span style="color: #000000;">config.status
creating main</span>/<span style="color: #000000;">internal_functions.c
creating main</span>/<span style="color: #000000;">internal_functions_cli.c
</span>+--------------------------------------------------------------------+
| License:                                                           |
| This software is subject to the PHP License, available <span style="color: #0000ff;">in</span> this     |
| distribution <span style="color: #0000ff;">in</span> the <span style="color: #0000ff;">file</span> LICENSE.  By continuing this installation |
| process, you are bound by the terms of this license agreement.     |
| If you <span style="color: #0000ff;">do</span> not agree with the terms of this license, you must abort |
| the installation process at this point.                            |
+--------------------------------------------------------------------+<span style="color: #000000;">

Thank you </span><span style="color: #0000ff;">for</span> using PHP.</pre>
      </div>
    </div>
    
    <p>
      <strong>防错&nbsp; </strong>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #0000ff;">ln</span> -s /application/mysql/lib/libmysqlclient.so.<span style="color: #800080;">18</span>  /usr/lib64/
<span style="color: #0000ff;">touch</span> ext/phar/phar.phar</pre>
      </div>
    </div>
    
    <p>
      <strong>编译 && </strong><strong>编译安装</strong>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #0000ff;">make</span> && <span style="color: #0000ff;">make</span> <span style="color: #0000ff;">install</span></pre>
      </div>
    </div>
    
    <h3>
      <span id="243_PHP">2.4.3 PHP软件程序创建软链接</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #0000ff;">ln</span> -s /application/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span>/ /application/php</pre>
      </div>
    </div>
    
    <h3>
      <span id="244_phpphp-fpm">2.4.4 配置php解析文件/配置php-fpm配置文件</span>
    </h3>
    
    <p>
      两个默认的配置文件区别
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>cd /server/tools/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span><span style="color: #000000;">
ll php.ini</span>*
-rw-r--r--. <span style="color: #800080;">1</span> <span style="color: #800080;">1001</span> <span style="color: #800080;">1001</span> <span style="color: #800080;">69236</span> <span style="color: #800080;">2016</span>-<span style="color: #800080;">02</span>-<span style="color: #800080;">02</span> <span style="color: #800080;">21</span>:<span style="color: #800080;">33</span> php.ini-<span style="color: #000000;">development
</span>-rw-r--r--. <span style="color: #800080;">1</span> <span style="color: #800080;">1001</span> <span style="color: #800080;">1001</span> <span style="color: #800080;">69266</span> <span style="color: #800080;">2016</span>-<span style="color: #800080;">02</span>-<span style="color: #800080;">02</span> <span style="color: #800080;">21</span>:<span style="color: #800080;">33</span> php.ini-production</pre>
      </div>
    </div>
    
    <p>
      <strong>配置文件说明：</strong>
    </p>
    
    <p>
      　　　　php.ini-developments是开发人员调试用配置文件
    </p>
    
    <p>
      　　　　php.ini-production是生产常见所有配置文件
    </p>
    
    <div>
      <p class="a4">
        <span style="background-color: #00ff00;">文件区别对比：</span>
      </p>
      
      <p class="a4">
        &nbsp;&nbsp;&nbsp; 生产的文件不会输出过多的日志信息，而开发文件会输出大量程序测试日志信息。
      </p>
    </div>
    
    <p>
      对比俩个文件不同的命令
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #0000ff;">diff</span> / vimdiff</pre>
      </div>
      
      <p class="ad">
        <strong>复制配置文件(2</strong><strong>个)</strong>
      </p>
    </div>
    
    <div>
      <div class="cnblogs_code">
        <pre># 创建软连接 ： <span style="color: #0000ff;">ln</span> -sf /application/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span> /application/<span style="color: #000000;">php
[root@web01 </span>~]#cd /server/tools/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span><span style="color: #000000;">
[root@web01 php</span>-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span>]# <span style="color: #0000ff;">cp</span> php.ini-production /application/php/lib/<span style="color: #000000;">php.ini
[root@web01 etc]# cd </span>/application/php/etc/<span style="color: #000000;">
[root@web01 etc]# </span><span style="color: #0000ff;">cp</span> php-fpm.conf.default php-fpm.conf</pre>
      </div>
    </div>
    
    <h3>
      <span id="245_php-fpm">2.4.5 启动php-fpm程序</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 ~]# /application/php/sbin/php-fpm </pre>
      </div>
      
      <p>
        　　确认php 9000端口是否正确启动（检查服务是否启动）
      </p>
    </div>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 ~]# netstat -lntup |<span style="color: #0000ff;">grep</span> <span style="color: #800080;">9000</span><span style="color: #000000;">
tcp        </span><span style="color: #800080;"></span>      <span style="color: #800080;"></span> <span style="color: #800080;">127.0</span>.<span style="color: #800080;">0.1</span>:<span style="color: #800080;">9000</span>              <span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:*                   LISTEN  </pre>
      </div>
    </div>
    
    <h2>
      <span id="25_nginx_php">2.5 nginx 与 php 建立连接关系</span>
    </h2>
    
    <h3>
      <span id="251_nginxnginxphp">2.5.1 修改nginx配置文件，使nginx程序与php程序建立联系</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>vim extra/<span style="color: #000000;">blog.conf
server {
        listen       </span><span style="color: #800080;">80</span><span style="color: #000000;">;
        server_name  blog.etiantian.org;
        location </span>/<span style="color: #000000;"> {
                    root   html</span>/<span style="color: #000000;">blog;
                    index  index.php index.html index.htm;       
        }
        location </span>~* .*\.(php|php5)?<span style="color: #000000;">$ {
                    root html</span>/<span style="color: #000000;">blog;
                    fastcgi_pass  </span><span style="color: #800080;">127.0</span>.<span style="color: #800080;">0.1</span>:<span style="color: #800080;">9000</span><span style="color: #000000;">;
                    fastcgi_index index.php;
                    include fastcgi.conf;
        }
}</span></pre>
      </div>
    </div>
    
    <div>
      <p class="a4">
        说明：利用nginx的location区块实现动态请求与静态请求的分别处理
      </p>
      
      <p class="a4">
        &nbsp;　　 &nbsp;&nbsp; <-- 需要注意编辑修改默认首页文件&nbsp; index&nbsp; index.php index.html index.htm;
      </p>
      
      <p class="a4">
        　　　让nginx服务具有动态请求解析功能。
      </p>
    </div>
    
    <h3>
      <span id="252">2.5.2 重启服务</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 ~]# /application/nginx/sbin/nginx -<span style="color: #000000;">t
nginx: the configuration </span><span style="color: #0000ff;">file</span> /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>/conf/<span style="color: #000000;">nginx.conf syntax is ok
nginx: configuration </span><span style="color: #0000ff;">file</span> /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>/conf/<span style="color: #000000;">nginx.conf test is successful
[root@web01 </span>~]# /application/nginx/sbin/nginx -s reload</pre>
      </div>
    </div>
    
    <h3>
      <span id="253_nginxphp">2.5.3 编辑nginx与php连通性测试文件,并进行测试</span>
    </h3>
    
    <p>
      测试动态请求是否可以处理：
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #0000ff;">echo</span> <span style="color: #800000;">'</span><span style="color: #800000;"><?php phpinfo(); ?></span>

<span style="color: #800000;">'</span>                    >/application/nginx/html/blog/test_info.php</pre>
      </div>
    </div>
    
    <p>
      测试站点
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>        curl  http:<span style="color: #008000;">//</span><span style="color: #008000;">blog.etiantian.org/index.html            &lt;-- 静态请求站点文件信息测试    </span>
<span style="color: #000000;">
        curl  http:</span><span style="color: #008000;">//</span><span style="color: #008000;">blog.etiantian.org/test_info.php         &lt;-- 动态请求站点文件信息测试</span></pre>
      </div>
    </div>
    
    <p>
      说明：当php服务停止时，9000端口信息消失，即停止PHP错误报502错误
    </p>
    
    <p>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; linux系统测试完毕后，建议利用浏览器进行最终测试，测试效果更明显些
    </p>
    
    <h3>
      <span id="254">2.5.4 浏览器测试</span>
    </h3>
    
    <p>
      浏览器访问
    </p>
    
    <p>
      http://blog.znix.top/test_info.php
    </p>
    
    <p style="text-align: center;">
      <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211220249399-1514471914.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" />
    </p>
    
    <h2>
      <span id="26_phpmysql">2.6 编辑php与mysql连通性测试文件,并进行测试</span>
    </h2>
    
    <h3>
      <span id="261">2.6.1 创建数据库</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>mysql -uroot -<span style="color: #000000;">pclsn123;
show databases;                      </span>&lt;---<span style="color: #000000;"> 查看当前数据库信息
create database wordpress;            </span>&lt;---创建博客储存数据库    </pre>
      </div>
    </div>
    
    <h3>
      <span id="262_mysql">2.6.2 在mysql中添加用户信息</span>
    </h3>
    
    <p>
      创建数据库授权用户
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>grant all on wordpress.* to <span style="color: #800000;">'</span><span style="color: #800000;">wordpress</span><span style="color: #800000;">'</span>@<span style="color: #800000;">'</span><span style="color: #800000;">10.0.0.%</span><span style="color: #800000;">'</span> identified by <span style="color: #800000;">'</span><span style="color: #800000;">clsn123</span><span style="color: #800000;">'</span><span style="color: #000000;">;
flush privileges;</span></pre>
      </div>
      
      <p>
        　　授权 所有权限 为 wordpress库的所有表&nbsp; &nbsp;用户@地址 设置密码 &nbsp;；&nbsp; &nbsp;
      </p>
    </div>
    
    <div>
      <p class="a4">
        　　刷新数据库
      </p>
    </div>
    
    <p>
      添加上用于blog使用的mysql用户
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>drop user wordpress@<span style="color: #800000;">'</span><span style="color: #800000;">172.16.1.8</span><span style="color: #800000;">'</span>;    &lt;---<span style="color: #000000;"> 删除用户信息
</span><span style="color: #0000ff;">select</span> user,host from mysql.user;    &lt;---<span style="color: #000000;"> 查看用户信息
mysql </span>-uwordpress -p123456           &lt;---<span style="color: #000000;"> 测试创建的用户连接
show databases;                      </span>&lt;--- 查看当前数据库信息</pre>
      </div>
    </div>
    
    <h2>
      <span id="27_php">2.7 测试php与数据库连通性</span>
    </h2>
    
    <div>
      <div class="cnblogs_code">
        <pre>vim test_mysql.<span style="color: #000000;">php
</span><?<span style="color: #000000;">php
&lt;/span>

<span style="color: #008000;">//</span><span style="color: #008000;">$link_id=mysql_connect('主机名','用户','密码');
//mysql -u用户 -p密码 -h 主机</span>
<span style="color: #800080;">$link_id</span>=<span style="color: #008080;">mysql_connect</span>('localhost','wordpress','clsn123') or <span style="color: #008080;">mysql_error</span><span style="color: #000000;">();
</span><span style="color: #0000ff;">if</span>(<span style="color: #800080;">$link_id</span><span style="color: #000000;">){
             </span><span style="color: #0000ff;">echo</span> "mysql successful by clsn !\n"<span style="color: #000000;">;
            }</span><span style="color: #0000ff;">else</span><span style="color: #000000;">{
             </span><span style="color: #0000ff;">echo</span> <span style="color: #008080;">mysql_error</span><span style="color: #000000;">();
            }
</span>?></pre>
      </div>
    </div>
    
    <h3>
      <span id="271">2.7.1 网站访问测试</span>
    </h3>
    
    <p>
      测试动态请求访问nginx服务是否可以到达数据库
    </p>
    
    <p style="text-align: center;" align="center">
      <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211220409884-1879003437.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" />&nbsp;
    </p>
    
    <h2>
      <span id="28_wordpress">2.8 下载部署wordpress博客程序</span>
    </h2>
    
    <p>
      　　下载地址：https://cn.wordpress.org
    </p>
    
    <h3>
      <span id="281">2.8.1 解压出来</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>tar xf wordpress-4.7.3-zh_CN.tar.gz  </pre>
      </div>
    </div>
    
    <h3>
      <span id="282">2.8.2 代码上线</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 wordpress]<span style="color: #008000;">#</span><span style="color: #008000;"> pwd</span>
/server/tools/lnmp/<span style="color: #000000;">wordpress
[root@web01 wordpress]</span><span style="color: #008000;">#</span><span style="color: #008000;"> mv ./* /application/nginx/html/blog/</span></pre>
      </div>
    </div>
    
    <h3>
      <span id="283">2.8.3 统一代码属主.属组</span>
    </h3>
    
    <p>
      对站点目录进行 授权
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 wordpress]<span style="color: #008000;">#</span><span style="color: #008000;"> cd /application/nginx/html/blog/</span>
[root@web01 blog]<span style="color: #008000;">#</span><span style="color: #008000;"> chown www.www -R /application/nginx/html/blog/</span>
<span style="color: #000000;">
[root@web01 blog]</span><span style="color: #008000;">#</span><span style="color: #008000;"> ll</span>
total 200
-rw-r--r--  1 www www    11 Oct 25 09:20 index.<span style="color: #000000;">html
</span>-rw-r--r--  1 www www   418 Sep 25  2013 index.<span style="color: #000000;">php
&hellip;&hellip;</span></pre>
      </div>
    </div>
    
    <p>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 说明：wp-config.php文件创建需要能够有权限对目录操作。
    </p>
    
    <p>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 此文件定义数据库连接信息
    </p>
    
    <h3>
      <span id="284">2.8.4 创建数据库</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #008080;">mysql</span> -uroot -<span style="color: #000000;">pclsn123;
show databases;              
create database wordpress;    </span></pre>
      </div>
    </div>
    
    <h3>
      <span id="285_wordpress">2.8.5 添加wordpress数据库用户</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">grant</span> <span style="color: #808080;">all</span> <span style="color: #0000ff;">on</span> wordpress.<span style="color: #808080;">*</span> <span style="color: #0000ff;">to</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">wordpress</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">10.0.0.%</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">clsn123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">; 
Query OK, </span><span style="color: #800000; font-weight: bold;"></span> rows affected (<span style="color: #800000; font-weight: bold;">0.16</span><span style="color: #000000;"> sec)

mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #ff00ff;">user</span>,host <span style="color: #0000ff;">from</span> mysql.<span style="color: #ff00ff;">user</span><span style="color: #000000;">;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------+-----------+</span>
<span style="color: #808080;">|</span> <span style="color: #ff00ff;">user</span>      <span style="color: #808080;">|</span> host      <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------+-----------+</span>
<span style="color: #808080;">|</span> wordpress <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;">10.0</span>.<span style="color: #800000; font-weight: bold;"></span>.<span style="color: #808080;">%</span>  <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> root      <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;">127.0</span>.<span style="color: #800000; font-weight: bold;">0.1</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> root      <span style="color: #808080;">|</span> ::<span style="color: #800000; font-weight: bold;">1</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>           <span style="color: #808080;">|</span> localhost <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> root      <span style="color: #808080;">|</span> localhost <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>           <span style="color: #808080;">|</span> web01     <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> root      <span style="color: #808080;">|</span> web01     <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------+-----------+</span>
<span style="color: #800000; font-weight: bold;">7</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
      </div>
    </div>
    
    <h3>
      <span id="286_wordpress">2.8.6 安装wordpress</span>
    </h3>
    
    <p>
      <strong>访问网站进行初始化操作</strong>
    </p>
    
    <p>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;填写的数据为
    </p>
    
    <p align="center">
      &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211220643571-299539412.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" />
    </p>
    
    <p>
      <strong>连接数据库配置说明</strong>
    </p>
    
    <div>
      <p class="a4">
        数据库名：指定数据存储到哪一个数据库当中，例如：存储到wordpress数据库中
      </p>
      
      <p class="a4">
        用户名：以什么用户身份管理wordpress数据库
      </p>
      
      <p class="a4">
        密码： 用户的密码
      </p>
      
      <p class="a4">
        数据库主机： 指定连接的数据库服务器地址信息
      </p>
      
      <p class="a4">
        表前缀：标识相应表属于哪一个数据库
      </p>
      
      <p class="a4">
        说明：配置完数据连接信息后，会自动创建wp-config.php文件，此文件定义数据库连接配置信息
      </p>
    </div>
    
    <p>
      <strong>安装完成效果</strong>
    </p>
    
    <p align="center">
      <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171211220711384-942725444.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="企业级LNMP架构搭建实例(基于Centos6.x)" alt="" />&nbsp;
    </p>
    
    <h1>
      <span id="3_mysql">第3章 mysql数据/储存数据迁移</span>
    </h1>
    
    <h2>
      <span id="31_mysql">3.1 mysql数据库迁移</span>
    </h2>
    
    <p>
      <strong>说明：</strong>
    </p>
    
    <div>
      <p class="a4">
        　　　　以上的mysql配置都是在web01 上进行 ，现在需要将web01上的mysql数据进行迁移到db01（数据库服务器）上去。
      </p>
    </div>
    
    <h3>
      <span id="311">3.1.1 备份数据库中的数据</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@db01 ~]# mysqldump -uroot -pclsn123 --all-databases >/tmp/bak.sql</pre>
      </div>
    </div>
    
    <p>
      使用mysqldump命令将数据库中的全部数据进行备份 备份到 /tmp/bak.sql 。
    </p>
    
    <p>
      <strong>mysqldump </strong><strong>命令参数说明：</strong>
    </p>
    
    <table border="1" cellspacing="0" cellpadding="0">
      <tr>
        <td width="245">
          <p align="center">
            <strong>参数</strong>
          </p>
        </td>
        
        <td width="452">
          <p align="center">
            <strong>参数说明</strong>
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--add-drop-table </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            在每个创建数据库表语句前添加删除数据库表的语句；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--add-locks </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            备份数据库表时锁定数据库表；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--all-databases</strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            备份MySQL服务器上的所有数据库；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--comments</strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            添加注释信息；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--compact </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            压缩模式，产生更少的输出；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--complete-insert </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            输出完成的插入语句；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--databases </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            指定要备份的数据库；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--default-character-set</strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            指定默认字符集；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--force </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            当出现错误时仍然继续备份操作；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--host </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            指定要备份数据库的服务器；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--lock-tables&nbsp; </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            备份前，锁定所有数据库表；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--no-create-db </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            禁止生成创建数据库语句；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--no-create-info</strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            禁止生成创建数据库库表语句；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--password&nbsp;&nbsp; </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            连接MySQL服务器的密码；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--port&nbsp;&nbsp;&nbsp;&nbsp; </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            MySQL服务器的端口号；
          </p>
        </td>
      </tr>
      
      <tr>
        <td width="245">
          <p>
            <strong>--user&nbsp;&nbsp;&nbsp; </strong>
          </p>
        </td>
        
        <td width="452">
          <p>
            连接MySQL服务器的用户名。
          </p>
        </td>
      </tr>
    </table>
    
    <h3 style="text-align: left;">
      <span id="312_mysqldb01">3.1.2 将备份数据传输到mysql服务器（db01）</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 tools]# rsync  -avz /tmp/bak.sql  <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.51</span>:/tmp/<span style="color: #000000;">
The authenticity of host </span><span style="color: #800000;">'</span><span style="color: #800000;">172.16.1.51 (172.16.1.51)</span><span style="color: #800000;">'</span> can<span style="color: #800000;">'</span><span style="color: #800000;">t be established.</span>
RSA key fingerprint is d3:<span style="color: #800080;">41</span>:bb:0d:<span style="color: #800080;">43</span>:<span style="color: #800080;">88</span>:da:a3:2c:e8:<span style="color: #800080;">36</span>:<span style="color: #800080;">91</span>:<span style="color: #800080;">11</span><span style="color: #000000;">:c9:e4:9c.
Are you sure you want to continue connecting (yes</span>/no)?<span style="color: #000000;"> yes
Warning: Permanently added </span><span style="color: #800000;">'</span><span style="color: #800000;">172.16.1.51</span><span style="color: #800000;">'</span><span style="color: #000000;"> (RSA) to the list of known hosts.
root@</span><span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.51</span><span style="color: #800000;">'</span><span style="color: #800000;">s password: </span>
sending incremental <span style="color: #0000ff;">file</span><span style="color: #000000;"> list
bak.sql

sent </span><span style="color: #800080;">377261</span> bytes  received <span style="color: #800080;">31</span> bytes  <span style="color: #800080;">83842.67</span> bytes/<span style="color: #000000;">sec
total size is </span><span style="color: #800080;">1483738</span>  speedup is <span style="color: #800080;">3.93</span></pre>
      </div>
    </div>
    
    <p>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 使用rsync将数据推送到MySQL服务器的/tmp 目录下面。
    </p>
    
    <h3>
      <span id="313_mysql">3.1.3 数据库服务器部署mysql服务(快速部署命令集)</span>
    </h3>
    
    <p>
      mysql服务快速部署过程脚本。详情参见<a href="#_部署mysql数据库服务">mysql数据库部署安装。</a>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>cd /server/<span style="color: #000000;">tools
</span><span style="color: #0000ff;">tar</span> xf mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>-linux-glibc2.<span style="color: #800080;">5</span>-x86_64.<span style="color: #0000ff;">tar</span><span style="color: #000000;">.gz
useradd </span>-s /sbin/nologin  -<span style="color: #000000;">M mysql
</span><span style="color: #0000ff;">mkdir</span> -p /application/
<span style="color: #0000ff;">mv</span> /server/tools/mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>-linux-glibc2.<span style="color: #800080;">5</span>-x86_64 /application/mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>
<span style="color: #0000ff;">ln</span> -s /application/mysql-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">34</span>/ /application/<span style="color: #000000;">mysql
</span><span style="color: #0000ff;">chown</span> -R mysql.mysql /application/<span style="color: #000000;">mysql
</span>/application/mysql/scripts/mysql_install_db --basedir=/application/mysql --datadir=/application/mysql/data --user=<span style="color: #000000;">mysql
</span><span style="color: #0000ff;">cp</span> /application/mysql/support-files/mysql.server  /etc/init.d/<span style="color: #000000;">mysqld
</span><span style="color: #0000ff;">chmod</span> +x /etc/init.d/<span style="color: #000000;">mysqld 
</span><span style="color: #0000ff;">sed</span> -i <span style="color: #800000;">'</span><span style="color: #800000;">s#/usr/local/mysql#/application/mysql#g</span><span style="color: #800000;">'</span> /application/mysql/bin/mysqld_safe /etc/init.d/<span style="color: #000000;">mysqld
\</span><span style="color: #0000ff;">cp</span> /application/mysql/support-files/my-default.cnf /etc/<span style="color: #000000;">my.cnf 
</span>/etc/init.d/<span style="color: #000000;">mysqld start
</span>/application/mysql/bin/mysqladmin -u root password <span style="color: #800000;">'</span><span style="color: #800000;">clsn123</span><span style="color: #800000;">'</span></pre>
      </div>
    </div>
    
    <h3>
      <span id="314">3.1.4 将备份的数据恢复到数据库服务器上</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@db01 ~]# /application/mysql/bin/mysql -uroot -pclsn123 &lt;/tmp/<span style="color: #000000;">bak.sql 
Warning: Using a password on the command line interface can be insecure.</span></pre>
      </div>
    </div>
    
    <p>
      <strong>注意，</strong>数据库导入之后要刷新数据库，让导入的数据被识别（重要）
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>mysql><span style="color: #000000;"> flush privileges;
Query OK, </span><span style="color: #800080;"></span> rows affected (<span style="color: #800080;">0.00</span> sec)</pre>
      </div>
    </div>
    
    <h3>
      <span id="315_web01">3.1.5 在web01服务器上进行远程登陆数据库测试</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@web01 ~</span><span style="color: #ff0000;">]</span># <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>bin<span style="color: #808080;">/</span>mysql <span style="color: #808080;">-</span>u wordpress <span style="color: #808080;">-</span>pclsn123  <span style="color: #808080;">-</span>h <span style="color: #800000; font-weight: bold;">10.0</span>.<span style="color: #800000; font-weight: bold;">0.51</span><span style="color: #000000;">
Warning: Using a password </span><span style="color: #0000ff;">on</span><span style="color: #000000;"> the command line interface can be insecure.
Welcome </span><span style="color: #0000ff;">to</span> the MySQL monitor.  Commands <span style="color: #0000ff;">end</span> <span style="color: #0000ff;">with</span> ; <span style="color: #808080;">or</span><span style="color: #000000;"> \g.
Your MySQL connection id </span><span style="color: #0000ff;">is</span> <span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">
Server version: </span><span style="color: #800000; font-weight: bold;">5.6</span>.<span style="color: #800000; font-weight: bold;">34</span><span style="color: #000000;"> MySQL Community Server (GPL)

Copyright (c) </span><span style="color: #800000; font-weight: bold;">2000</span>, <span style="color: #800000; font-weight: bold;">2016</span>, Oracle <span style="color: #808080;">and/or</span> its affiliates. <span style="color: #808080;">All</span><span style="color: #000000;"> rights reserved.

Oracle </span><span style="color: #0000ff;">is</span> a registered trademark <span style="color: #0000ff;">of</span> Oracle Corporation <span style="color: #808080;">and/or</span><span style="color: #000000;"> its
affiliates. Other names may be trademarks </span><span style="color: #0000ff;">of</span><span style="color: #000000;"> their respective
owners.

Type </span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">help;</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">or</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">\h</span><span style="color: #ff0000;">'</span> <span style="color: #0000ff;">for</span> help. Type <span style="color: #ff0000;">'</span><span style="color: #ff0000;">\c</span><span style="color: #ff0000;">'</span> <span style="color: #0000ff;">to</span> clear the <span style="color: #0000ff;">current</span><span style="color: #000000;"> input statement.

mysql</span><span style="color: #808080;">></span><span style="color: #000000;"> 
mysql</span><span style="color: #808080;">></span><span style="color: #000000;"> show databases;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> <span style="color: #0000ff;">Database</span>             <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> information_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> test                  <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> wordpress            <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #800000; font-weight: bold;">3</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
      </div>
    </div>
    
    <h3>
      <span id="316_webphp">3.1.6 修改web服务器php连接数据库主机的配置文件</span>
    </h3>
    
    <p>
      修改wordpress软件的配置，将连接的主机地址改为数据库服务器的地址
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 ~]# vim /application/nginx/html/blog/wp-<span style="color: #000000;">config.php

&hellip;&hellip;

</span><span style="color: #008000;">/*</span><span style="color: #008000;">* MySQL主机 </span><span style="color: #008000;">*/</span><span style="color: #000000;">

define(</span><span style="color: #800000;">'</span><span style="color: #800000;">DB_HOST</span><span style="color: #800000;">'</span>, <span style="color: #800000;">'</span><span style="color: #800000;">10.0.0.51</span><span style="color: #800000;">'</span><span style="color: #000000;">);

&hellip;&hellip;</span></pre>
      </div>
    </div>
    
    <h2>
      <span id="32_nfs">3.2 本地数据挂载到nfs共享储存</span>
    </h2>
    
    <h3>
      <span id="321">3.2.1 确认本地数据的储存位置（三种方法）</span>
    </h3>
    
    <p>
      01.通过网页图片属性信息进行确认路径
    </p>
    
    <div>
      <p class="a4">
        　　http://blog.clsn.top/wp-content/uploads/2017/10/cropped-Frog-2.png
      </p>
    </div>
    
    <p>
      02.通过find查看数据储存路径信息，上传个图片，查找相同中1分钟以内的文件
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #0000ff;">find</span> -type f -mmin -<span style="color: #800080;">1</span></pre>
      </div>
    </div>
    
    <p>
      03.通过inotify软件进行监控
    </p>
    
    <div>
      <p class="ad">
        　　确认文件的储存目录
      </p>
      
      <div class="cnblogs_code">
        <pre>/application/nginx/html/blog/wp-content/uploads</pre>
      </div>
    </div>
    
    <h3>
      <span id="322">3.2.2 将已有数据进行迁移备份</span>
    </h3>
    
    <p>
      备份数据是因为挂载的时候会将当前的数据全部'覆盖'掉，只显示nfs共享目录的信息。
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 uploads]# <span style="color: #0000ff;">pwd</span>
/application/nginx/html/blog/wp-content/<span style="color: #000000;">uploads
[root@web01 uploads]# </span><span style="color: #0000ff;">mkdir</span> /tmp/<span style="color: #000000;">wordpress_bak
[root@web01 uploads]# </span><span style="color: #0000ff;">mv</span> .<span style="color: #008000;">/*</span><span style="color: #008000;">  /tmp/wordpress_bak/</span></pre>
      </div>
    </div>
    
    <h3>
      <span id="323_nfs">3.2.3 nfs储存服务配置</span>
    </h3>
    
    <p>
      配置nfs服务的时候注意权限的设置
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@nfs01 data]# <span style="color: #0000ff;">cat</span> /etc/<span style="color: #000000;">exports 
#share user:hzs
</span>/data <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.0</span>/<span style="color: #800080;">24</span>(rw,<span style="color: #0000ff;">sync</span>,root_squash,no_all_squash,anonuid=<span style="color: #800080;">501</span>,anongid=<span style="color: #800080;">501</span>)</pre>
      </div>
    </div>
    
    <p>
      <strong>注意：</strong>
    </p>
    
    <p>
      　　anonuid 与 anongid 要和web服务器上的www用户的相同<strong>(</strong>UID与GID相同）
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@nfs01 /]# <span style="color: #0000ff;">id</span><span style="color: #000000;"> www
uid</span>=<span style="color: #800080;">501</span>(www) gid=<span style="color: #800080;">501</span>(www) <span style="color: #0000ff;">groups</span>=<span style="color: #800080;">501</span>(www)</pre>
      </div>
    </div>
    
    <p>
      目录的属组要是与nfs配置的anonuid，anongid相同的用户。
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@nfs01 /]# ll /data/ -<span style="color: #000000;">d
drwxr</span>-xr-x <span style="color: #800080;">3</span> www www <span style="color: #800080;">4096</span> Oct <span style="color: #800080;">27</span> <span style="color: #800080;">12</span>:<span style="color: #800080;">11</span> /data/</pre>
      </div>
    </div>
    
    <div>
      <p class="a4">
        NFS的配置详情参见：<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/znix/p/7694456.html" > NFS存储服务部署</a>一篇。
      </p>
    </div>
    
    <h3>
      <span id="324_nfs">3.2.4 将储存目录挂载到nfs共享目录上</span>
    </h3>
    
    <p>
      注：作为nfs客户端需要安装nfs-utils 和 rpcbind
    </p>
    
    <p>
      ①先检查是否能挂载，显示可以挂载的目录
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 uploads]# showmount -e <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.31</span><span style="color: #000000;">
Export list </span><span style="color: #0000ff;">for</span> <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.31</span><span style="color: #000000;">:
</span>/data <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.0</span>/<span style="color: #800080;">24</span></pre>
      </div>
    </div>
    
    <p>
      ②将磁盘进行挂载
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 uploads]# <span style="color: #0000ff;">mount</span> -t nfs <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.31</span>:/data  /application/nginx/html/blog/wp-content/uploads/</pre>
      </div>
    </div>
    
    <p>
      ③显示磁盘信息
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 uploads]# <span style="color: #0000ff;">df</span> -<span style="color: #000000;">h
Filesystem         Size  Used Avail Use</span>%<span style="color: #000000;"> Mounted on
</span>/dev/sda3           19G  <span style="color: #800080;">3</span>.7G   15G  <span style="color: #800080;">21</span>% /<span style="color: #000000;">
tmpfs              238M     </span><span style="color: #800080;"></span>  238M   <span style="color: #800080;"></span>% /dev/<span style="color: #000000;">shm
</span>/dev/sda1          190M   40M  141M  <span style="color: #800080;">22</span>% /<span style="color: #000000;">boot
</span><span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.31</span>:/data   19G  <span style="color: #800080;">1</span>.5G   17G   <span style="color: #800080;">9</span>% /application/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>/html/blog/wp-content/uploads</pre>
      </div>
    </div>
    
    <h3>
      <span id="325">3.2.5 恢复数据（将之前备份的数据还原回来）</span>
    </h3>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@web01 uploads]# <span style="color: #0000ff;">pwd</span><span style="color: #000000;">
application</span>/nginx-<span style="color: #800080;">1.10</span>.<span style="color: #800080;">2</span>/html/blog/wp-content/<span style="color: #000000;">uploads
[root@web01 uploads]# </span><span style="color: #0000ff;">mv</span> /tmp/wordpress_bak<span style="color: #008000;">/*</span><span style="color: #008000;"> ./</span></pre>
      </div>
      
      <h3>
        <span id="326">3.2.6 命令补全功能</span>
      </h3>
      
      <div class="cnblogs_code">
        <pre>yum install bash-completion -y</pre>
      </div>
      
      <h2>
        <span id="nbsp33">&nbsp;3.3各服务的启动脚本</span>
      </h2>
      
      <h3>
        <span id="nbsp331php">&nbsp;3.3.1php启动脚本</span>
      </h3>
      
      <div class="cnblogs_code">
        <pre><span style="color: #008000;">#</span><span style="color: #008000;"> 复制php启动脚本</span>
[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cp /server/tools/php-5.5.32/sapi/fpm/init.d.php-fpm  /etc/init.d/php-fpm </span>
[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chmod +x  /etc/init.d/php-fpm</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 找到pid文件，开启它</span>
[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> vim /application/php/etc/php-fpm.conf</span><span style="color: #008000;">
#</span><span style="color: #008000;"> &middot;&middot;&middot;</span>
[<span style="color: #0000ff;">global</span><span style="color: #000000;">]
; Pid file
; Note: the default prefix </span><span style="color: #0000ff;">is</span> /application/php-5.5.32/<span style="color: #000000;">var
; Default Value: none
pid </span>= run/php-<span style="color: #000000;">fpm.pid
</span><span style="color: #008000;">#</span><span style="color: #008000;"> &middot;&middot;&middot;</span><span style="color: #008000;">
#</span><span style="color: #008000;"> 启动php</span>
[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> /etc/init.d/php-fpm status</span>
php-fpm (pid 27931) <span style="color: #0000ff;">is</span> running...</pre>
      </div>
      
      <h3>
        <span id="332NGINX">3.3.2NGINX管理脚本</span>
      </h3>
      
      <div class="cnblogs_code">
        <pre>[root@clsn ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/init.d/nginx </span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/sh</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> nginx - this script starts and stops the nginx daemon</span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> chkconfig:   - 85 15</span><span style="color: #008000;">
#</span><span style="color: #008000;"> description:  NGINX is an HTTP(S) server, HTTP(S) reverse \</span><span style="color: #008000;">
#</span><span style="color: #008000;">               proxy and IMAP/POP3 proxy server</span><span style="color: #008000;">
#</span><span style="color: #008000;"> processname: nginx</span><span style="color: #008000;">
#</span><span style="color: #008000;"> config:         /application/nginx/conf/nginx.conf</span><span style="color: #008000;">
#</span><span style="color: #008000;"> config:      /application/nginx/sbin/nginx </span><span style="color: #008000;">
#</span><span style="color: #008000;"> pidfile:     </span><span style="color: #008000;">
#</span><span style="color: #008000;"> by:  http://www.nmtui.com</span>

<span style="color: #008000;">#</span><span style="color: #008000;"> Source function library.</span>
. /etc/rc.d/init.d/<span style="color: #000000;">functions

</span><span style="color: #008000;">#</span><span style="color: #008000;"> Source networking configuration.</span>
. /etc/sysconfig/<span style="color: #000000;">network

</span><span style="color: #008000;">#</span><span style="color: #008000;"> Check that networking is up.</span>
[ <span style="color: #800000;">"</span><span style="color: #800000;">$NETWORKING</span><span style="color: #800000;">"</span> = <span style="color: #800000;">"</span><span style="color: #800000;">no</span><span style="color: #800000;">"</span> ] &&<span style="color: #000000;"> exit 0

nginx</span>=<span style="color: #800000;">"</span><span style="color: #800000;">/application/nginx/sbin/nginx</span><span style="color: #800000;">"</span><span style="color: #000000;">
prog</span>=<span style="color: #000000;">$(basename $nginx)

NGINX_CONF_FILE</span>=<span style="color: #800000;">"</span><span style="color: #800000;">/application/nginx/conf/nginx.conf</span><span style="color: #800000;">"</span>

<span style="color: #008000;">#</span><span style="color: #008000;">[ -f /application/nginx/sbin/nginx ] && . /application/nginx/sbin/nginx</span>
<span style="color: #000000;">
lockfile</span>=/var/lock/subsys/<span style="color: #000000;">nginx

make_dirs() {
   </span><span style="color: #008000;">#</span><span style="color: #008000;"> make required directories</span>
   user=`$nginx -V 2>&1 | grep <span style="color: #800000;">"</span><span style="color: #800000;">configure arguments:.*--user=</span><span style="color: #800000;">"</span> | sed <span style="color: #800000;">'</span><span style="color: #800000;">s/[^*]*--user=\([^ ]*\).*/\1/g</span><span style="color: #800000;">'</span> -<span style="color: #000000;">`
   </span><span style="color: #0000ff;">if</span> [ -n <span style="color: #800000;">"</span><span style="color: #800000;">$user</span><span style="color: #800000;">"</span><span style="color: #000000;"> ]; then
      </span><span style="color: #0000ff;">if</span> [ -z <span style="color: #800000;">"</span><span style="color: #800000;">`grep $user /etc/passwd`</span><span style="color: #800000;">"</span><span style="color: #000000;"> ]; then
         useradd </span>-M -s /bin/<span style="color: #000000;">nologin $user
      fi
      options</span>=`$nginx -V 2>&1 | grep <span style="color: #800000;">'</span><span style="color: #800000;">configure arguments:</span><span style="color: #800000;">'</span><span style="color: #000000;">`
      </span><span style="color: #0000ff;">for</span> opt <span style="color: #0000ff;">in</span><span style="color: #000000;"> $options; do
          </span><span style="color: #0000ff;">if</span> [ `echo $opt | grep <span style="color: #800000;">'</span><span style="color: #800000;">.*-temp-path</span><span style="color: #800000;">'</span><span style="color: #000000;">` ]; then
              value</span>=`echo $opt | cut -d <span style="color: #800000;">"</span><span style="color: #800000;">=</span><span style="color: #800000;">"</span> -f 2<span style="color: #000000;">`
              </span><span style="color: #0000ff;">if</span> [ ! -d <span style="color: #800000;">"</span><span style="color: #800000;">$value</span><span style="color: #800000;">"</span><span style="color: #000000;"> ]; then
                  </span><span style="color: #008000;">#</span><span style="color: #008000;"> echo "creating" $value</span>
                  mkdir -p $value && chown -<span style="color: #000000;">R $user $value
              fi
          fi
       done
    fi
}

start() {
    [ </span>-x $nginx ] || exit 5<span style="color: #000000;">
    [ </span>-f $NGINX_CONF_FILE ] || exit 6<span style="color: #000000;">
    make_dirs
    echo </span>-n $<span style="color: #800000;">"</span><span style="color: #800000;">Starting $prog: </span><span style="color: #800000;">"</span><span style="color: #000000;">
    daemon $nginx </span>-<span style="color: #000000;">c $NGINX_CONF_FILE
    retval</span>=<span style="color: #000000;">$?
    echo
    [ $retval </span>-eq 0 ] &&<span style="color: #000000;"> touch $lockfile
    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> $retval
}

stop() {
    echo </span>-n $<span style="color: #800000;">"</span><span style="color: #800000;">Stopping $prog: </span><span style="color: #800000;">"</span><span style="color: #000000;">
    killproc $prog </span>-<span style="color: #000000;">QUIT
    retval</span>=<span style="color: #000000;">$?
    echo
    [ $retval </span>-eq 0 ] && rm -<span style="color: #000000;">f $lockfile
    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> $retval
}

restart() {
    configtest </span>|| <span style="color: #0000ff;">return</span><span style="color: #000000;"> $?
    stop
    sleep </span>1<span style="color: #000000;">
    start
}

reload() {
    configtest </span>|| <span style="color: #0000ff;">return</span><span style="color: #000000;"> $?
    echo </span>-n $<span style="color: #800000;">"</span><span style="color: #800000;">Reloading $prog: </span><span style="color: #800000;">"</span><span style="color: #000000;">
    killproc $nginx </span>-<span style="color: #000000;">HUP
    RETVAL</span>=<span style="color: #000000;">$?
    echo
}

force_reload() {
    restart
}

configtest() {
  $nginx </span>-t -<span style="color: #000000;">c $NGINX_CONF_FILE
}

rh_status() {
    status $prog
}

rh_status_q() {
    rh_status </span>>/dev/null 2>&1<span style="color: #000000;">
}

case </span><span style="color: #800000;">"</span><span style="color: #800000;">$1</span><span style="color: #800000;">"</span> <span style="color: #0000ff;">in</span><span style="color: #000000;">
    start)
        rh_status_q </span>&&<span style="color: #000000;"> exit 0
        $</span>1<span style="color: #000000;">
        ;;
    stop)
        rh_status_q </span>||<span style="color: #000000;"> exit 0
        $</span>1<span style="color: #000000;">
        ;;
    restart</span>|<span style="color: #000000;">configtest)
        $</span>1<span style="color: #000000;">
        ;;
    reload)
        rh_status_q </span>|| exit 7<span style="color: #000000;">
        $</span>1<span style="color: #000000;">
        ;;
    force</span>-<span style="color: #000000;">reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart</span>|<span style="color: #0000ff;">try</span>-<span style="color: #000000;">restart)
        rh_status_q </span>||<span style="color: #000000;"> exit 0
            ;;
    </span>*<span style="color: #000000;">)
        echo $</span><span style="color: #800000;">"</span><span style="color: #800000;">Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}</span><span style="color: #800000;">"</span><span style="color: #000000;">
        exit </span>2<span style="color: #000000;">
esac</span></pre>
      </div>
      
      <p>
        &nbsp;
      </p>
      
      <p>
        &nbsp;
      </p>
      
      <p>
        &nbsp;
      </p>
      
      <p>
        &nbsp;
      </p>
      
      <p>
        &nbsp;
      </p>
    </div>
    
    <p>
      &nbsp;
    </p>
    
    <div id="toc_container" class="toc_white have_bullets">
      <ul class="toc_list">
        <ul>
          <li>
            <a href="#11_LNMP">1.1 部署LNMP架构说明</a><ul>
              <li>
                <a href="#111_LNMP">1.1.1 LNMP架构内容</a>
              </li>
              <li>
                <a href="#112_LNMP">1.1.2 配置LNMP架构步骤</a>
              </li>
              <li>
                <a href="#113">1.1.3 架构服务器串联</a>
              </li>
              <li>
                <a href="#114_LNMP_FastCGI">1.1.4 LNMP FastCGI知识说明</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#2_LNMP">第2章 LNMP环境搭建步骤</a>
          </li>
          <li>
            <a href="#21_linux">2.1 部署linux系统</a>
          </li>
          <li>
            <a href="#22_nginx">2.2 部署nginx网站服务</a><ul>
              <li>
                <a href="#221">2.2.1 检查软件安装的系统环境</a>
              </li>
              <li>
                <a href="#222_nginxpcre-devel_openssl-devel">2.2.2 安装nginx的依赖包（pcre-devel openssl-devel）</a>
              </li>
              <li>
                <a href="#223_nginx">2.2.3 下载nginx软件</a>
              </li>
              <li>
                <a href="#224_www">2.2.4 创建管理用户 www</a>
              </li>
              <li>
                <a href="#225_nbspnginx">2.2.5 &nbsp;nginx软件编译安装过程</a><ul>
                  <li>
                    <a href="#2251nbsp">2.2.5.1&nbsp; 注意</a>
                  </li>
                  <li>
                    <a href="#2252nbsp">2.2.5.2&nbsp; 编译安装软件</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#226">2.2.6 创建软连接</a>
              </li>
              <li>
                <a href="#227_nginxconf_nginx">2.2.7 精简化nginx.conf 主配置文件内容, 编写nginx配置文件</a>
              </li>
              <li>
                <a href="#228">2.2.8 启动程序</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#23_mysql">2.3 部署mysql数据库服务</a><ul>
              <li>
                <a href="#231_mysql">2.3.1 下载mysql软件</a>
              </li>
              <li>
                <a href="#232_mysql">2.3.2 【二进制包方式】安装mysql数据库软件</a><ul>
                  <li>
                    <a href="#2321nbsp">2.3.2.1&nbsp; 解压二进制包软件?</a>
                  </li>
                  <li>
                    <a href="#2322nbsp_mysql">2.3.2.2&nbsp; 创建储存目录管理用户mysql?</a>
                  </li>
                  <li>
                    <a href="#2323nbsp">2.3.2.3&nbsp; 将解压后的二进制包放置到程序目录中?</a>
                  </li>
                  <li>
                    <a href="#2324nbsp_mysql">2.3.2.4&nbsp; 对mysql数据储存目录进行授权?</a>
                  </li>
                  <li>
                    <a href="#2325nbsp">2.3.2.5&nbsp; 初始化数据库服务?</a>
                  </li>
                  <li>
                    <a href="#2326nbsp">2.3.2.6&nbsp; 将启动脚本文件复制到启动目录中?</a>
                  </li>
                  <li>
                    <a href="#2327nbsp_mysql">2.3.2.7&nbsp; 设置mysql服务配置文件?</a>
                  </li>
                  <li>
                    <a href="#2328nbsp_mysql">2.3.2.8&nbsp; 启动mysql服务?</a>
                  </li>
                  <li>
                    <a href="#2329nbsp">2.3.2.9&nbsp; 检查端口信息，确认服务是否启动?</a>
                  </li>
                  <li>
                    <a href="#23210nbspnbspnbspnbsp_root">2.3.2.10&nbsp;&nbsp;&nbsp;&nbsp; 设置root用户密码信息?</a>
                  </li>
                  <li>
                    <a href="#23211nbspnbspnbspnbsp">2.3.2.11&nbsp;&nbsp;&nbsp;&nbsp; 测试</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#233_mysql">2.3.3 管理mysql数据库</a><ul>
                  <li>
                    <a href="#2331nbsp">2.3.3.1&nbsp; 查看数据库</a>
                  </li>
                  <li>
                    <a href="#2332nbsp">2.3.3.2&nbsp; 查看数据表信息</a>
                  </li>
                  <li>
                    <a href="#2333nbsp">2.3.3.3&nbsp; 退出数据库</a>
                  </li>
                </ul>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#24_php">2.4 部署php服务</a><ul>
              <li>
                <a href="#241_PHP14">2.4.1 解决PHP软件的依赖关系（14个依赖包）</a><ul>
                  <li>
                    <a href="#2411nbsp_base">2.4.1.1&nbsp; 基于base源的个依赖包</a>
                  </li>
                  <li>
                    <a href="#2412nbsp_libiconv">2.4.1.2&nbsp; libiconv软件 和字符集转换相关软件</a>
                  </li>
                  <li>
                    <a href="#2413nbsp_3">2.4.1.3&nbsp; 安装加密相关的依赖软件（3个）</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#242_php">2.4.2 编译安装php过程</a>
              </li>
              <li>
                <a href="#243_PHP">2.4.3 PHP软件程序创建软链接</a>
              </li>
              <li>
                <a href="#244_phpphp-fpm">2.4.4 配置php解析文件/配置php-fpm配置文件</a>
              </li>
              <li>
                <a href="#245_php-fpm">2.4.5 启动php-fpm程序</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#25_nginx_php">2.5 nginx 与 php 建立连接关系</a><ul>
              <li>
                <a href="#251_nginxnginxphp">2.5.1 修改nginx配置文件，使nginx程序与php程序建立联系</a>
              </li>
              <li>
                <a href="#252">2.5.2 重启服务</a>
              </li>
              <li>
                <a href="#253_nginxphp">2.5.3 编辑nginx与php连通性测试文件,并进行测试</a>
              </li>
              <li>
                <a href="#254">2.5.4 浏览器测试</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#26_phpmysql">2.6 编辑php与mysql连通性测试文件,并进行测试</a><ul>
              <li>
                <a href="#261">2.6.1 创建数据库</a>
              </li>
              <li>
                <a href="#262_mysql">2.6.2 在mysql中添加用户信息</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#27_php">2.7 测试php与数据库连通性</a><ul>
              <li>
                <a href="#271">2.7.1 网站访问测试</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#28_wordpress">2.8 下载部署wordpress博客程序</a><ul>
              <li>
                <a href="#281">2.8.1 解压出来</a>
              </li>
              <li>
                <a href="#282">2.8.2 代码上线</a>
              </li>
              <li>
                <a href="#283">2.8.3 统一代码属主.属组</a>
              </li>
              <li>
                <a href="#284">2.8.4 创建数据库</a>
              </li>
              <li>
                <a href="#285_wordpress">2.8.5 添加wordpress数据库用户</a>
              </li>
              <li>
                <a href="#286_wordpress">2.8.6 安装wordpress</a>
              </li>
            </ul>
          </li>
        </ul></li>
        
        <li>
          <a href="#3_mysql">第3章 mysql数据/储存数据迁移</a><ul>
            <li>
              <a href="#31_mysql">3.1 mysql数据库迁移</a><ul>
                <li>
                  <a href="#311">3.1.1 备份数据库中的数据</a>
                </li>
                <li>
                  <a href="#312_mysqldb01">3.1.2 将备份数据传输到mysql服务器（db01）</a>
                </li>
                <li>
                  <a href="#313_mysql">3.1.3 数据库服务器部署mysql服务(快速部署命令集)</a>
                </li>
                <li>
                  <a href="#314">3.1.4 将备份的数据恢复到数据库服务器上</a>
                </li>
                <li>
                  <a href="#315_web01">3.1.5 在web01服务器上进行远程登陆数据库测试</a>
                </li>
                <li>
                  <a href="#316_webphp">3.1.6 修改web服务器php连接数据库主机的配置文件</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#32_nfs">3.2 本地数据挂载到nfs共享储存</a><ul>
                <li>
                  <a href="#321">3.2.1 确认本地数据的储存位置（三种方法）</a>
                </li>
                <li>
                  <a href="#322">3.2.2 将已有数据进行迁移备份</a>
                </li>
                <li>
                  <a href="#323_nfs">3.2.3 nfs储存服务配置</a>
                </li>
                <li>
                  <a href="#324_nfs">3.2.4 将储存目录挂载到nfs共享目录上</a>
                </li>
                <li>
                  <a href="#325">3.2.5 恢复数据（将之前备份的数据还原回来）</a>
                </li>
                <li>
                  <a href="#326">3.2.6 命令补全功能</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#nbsp33">&nbsp;3.3各服务的启动脚本</a><ul>
                <li>
                  <a href="#nbsp331php">&nbsp;3.3.1php启动脚本</a>
                </li>
                <li>
                  <a href="#332NGINX">3.3.2NGINX管理脚本</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </div>