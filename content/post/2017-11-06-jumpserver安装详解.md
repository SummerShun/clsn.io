---
title: jumpserver安装详解
author: 惨绿少年
type: post
date: 2017-11-05T19:00:00+00:00
url: /clsn/lx857.html
Baidusubmit:
  - 1
views:
  - 207
categories:
  - Linux运维
  - 安全
  - 监控服务
  - 运维基本功

---
## <span id="2018119">2018年1月19日更新：</span>

**<span style="color: #ff0000;">最新安装方法&nbsp;</span>** &nbsp;<a href="https://github.com/jumpserver/jumpserver/wiki/v0.3.x-基于-RedHat" target="_blank">https://github.com/jumpserver/jumpserver/wiki/v0.3.x-基于-RedHat</a>

* * *

### <span id="nbsp">&nbsp;</span>

### <span id="i">环境说明</span>

　　主机为最小 安装的centos6.9 x86_64.

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> [root@m01 ~]# <span style="color: #0000ff;">cat</span> /etc/redhat-<span style="color: #000000;">release 
</span><span style="color: #008080;">2</span> CentOS release <span style="color: #800080;">6.9</span><span style="color: #000000;"> (Final)
</span><span style="color: #008080;">3</span> [root@m01 ~]# <span style="color: #0000ff;">uname</span> -<span style="color: #000000;">a
</span><span style="color: #008080;">4</span> Linux m01 <span style="color: #800080;">2.6</span>.<span style="color: #800080;">32</span>-<span style="color: #800080;">696</span>.el6.x86_64 #<span style="color: #800080;">1</span> SMP Tue Mar <span style="color: #800080;">21</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">29</span>:<span style="color: #800080;">05</span> UTC <span style="color: #800080;">2017</span> x86_64 x86_64 x86_64 GNU/Linux</pre>
</div>

### <span id="jumpserver"><span style="background-color: #00ffff;">jumpserver安装</span></span>

**第一个里程碑：安装git**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">1</span> [root@m01 ~]# <span style="color: #0000ff;">yum</span> -y <span style="color: #0000ff;">install</span> git</pre>
  </div>
</div>

**第二个里程碑：克隆jumpserver**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">1</span> [root@m01 ~]# cd /application/
<span style="color: #008080;">2</span> 
<span style="color: #008080;">3</span> [root@m01 application]# git clone https:<span style="color: #008000;">//</span><span style="color: #008000;">github.com/jumpserver/jumpserver.git</span>
<span style="color: #008080;">4</span> Initialized empty Git repository <span style="color: #0000ff;">in</span> /application/jumpserver/.git/
<span style="color: #008080;">5</span> remote: Counting objects: <span style="color: #800080;">20979</span>, <span style="color: #0000ff;">done</span><span style="color: #000000;">.
</span><span style="color: #008080;">6</span> remote: Compressing objects: <span style="color: #800080;">100</span>% (<span style="color: #800080;">27</span>/<span style="color: #800080;">27</span>), <span style="color: #0000ff;">done</span><span style="color: #000000;">.
</span><span style="color: #008080;">7</span> remote: Total <span style="color: #800080;">20979</span> (delta <span style="color: #800080;"></span>), reused <span style="color: #800080;">8</span> (delta <span style="color: #800080;"></span>), pack-reused <span style="color: #800080;">20951</span>
<span style="color: #008080;">8</span> Receiving objects: <span style="color: #800080;">100</span>% (<span style="color: #800080;">20979</span>/<span style="color: #800080;">20979</span>), <span style="color: #800080;">26.20</span> MiB | <span style="color: #800080;">141</span> KiB/s, <span style="color: #0000ff;">done</span><span style="color: #000000;">.
</span><span style="color: #008080;">9</span> Resolving deltas: <span style="color: #800080;">100</span>% (<span style="color: #800080;">14550</span>/<span style="color: #800080;">14550</span>), <span style="color: #0000ff;">done</span>.</pre>
  </div>
</div>

**第三个里程碑：将jumpserver****切换到主线版本**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">1</span> [root@m01 application]# cd /application/jumpserver/
<span style="color: #008080;">2</span> 
<span style="color: #008080;">3</span> <span style="color: #000000;">[root@m01 jumpserver]# git checkout master
</span><span style="color: #008080;">4</span> <span style="color: #000000;">Branch master set up to track remote branch master from origin.
</span><span style="color: #008080;">5</span> Switched to a new branch <span style="color: #800000;">'</span><span style="color: #800000;">master</span><span style="color: #800000;">'</span></pre>
  </div>
</div>

****第四个里程碑：在数据库服务上为jumpserver创建数据库****

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> <span style="color: #000000;">###设置jumpserver库
</span><span style="color: #008080;">2</span> <span style="color: #000000;">CREATE DATABASE jumpserver CHARACTER SET utf8 COLLATE utf8_general_ci;
</span><span style="color: #008080;">3</span> ###---<span style="color: #000000;">授权root给数据库
</span><span style="color: #008080;">4</span> grant all on jumpserver.* to jumpserver@<span style="color: #800000;">'</span><span style="color: #800000;">%</span><span style="color: #800000;">'</span> identified by <span style="color: #800000;">"</span><span style="color: #800000;">123456</span><span style="color: #800000;">"</span><span style="color: #000000;">;
</span><span style="color: #008080;">5</span> <span style="color: #000000;">###
</span><span style="color: #008080;">6</span> grant all on jumpserver.* to jumpserver@<span style="color: #800000;">'</span><span style="color: #800000;">localhost</span><span style="color: #800000;">'</span> identified by <span style="color: #800000;">"</span><span style="color: #800000;">123456</span><span style="color: #800000;">"</span><span style="color: #000000;">;
</span><span style="color: #008080;">7</span> <span style="color: #000000;">## 更新库
</span><span style="color: #008080;">8</span> flush privileges;----
<span style="color: #008080;">9</span> ##</pre>
</div>

**第五个里程碑：执行安装脚本**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">1</span> [root@m01 ~]# cd /application/jumpserver/<span style="color: #0000ff;">install</span>/
<span style="color: #008080;">2</span> [root@m01 <span style="color: #0000ff;">install</span>]# python <span style="color: #0000ff;">install</span>.py</pre>
  </div>
</div>

_<span style="text-decoration: underline;">安装过程中输入相关信息</span>_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008080;"> 1</span> <span style="color: #000000;">开始关闭防火墙和selinux
</span><span style="color: #008080;"> 2</span> <span style="color: #000000;">setenforce: SELinux is disabled
</span><span style="color: #008080;"> 3</span> 
<span style="color: #008080;"> 4</span> 请输入您服务器的IP地址，用户浏览器可以访问 [<span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.61</span><span style="color: #000000;">]:
</span><span style="color: #008080;"> 5</span> 是否安装新的MySQL服务器? (y/<span style="color: #000000;">n) [y]: n
</span><span style="color: #008080;"> 6</span> 请输入数据库服务器IP [<span style="color: #800080;">127.0</span>.<span style="color: #800080;">0.1</span>]: <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.51</span>
<span style="color: #008080;"> 7</span> 请输入数据库服务器端口 [<span style="color: #800080;">3306</span><span style="color: #000000;">]:
</span><span style="color: #008080;"> 8</span> <span style="color: #000000;">请输入数据库服务器用户 [jumpserver]:
</span><span style="color: #008080;"> 9</span> <span style="color: #000000;">请输入数据库服务器密码: 123456
</span><span style="color: #008080;">10</span> <span style="color: #000000;">请输入使用的数据库 [jumpserver]:
</span><span style="color: #008080;">11</span> <span style="color: #000000;">连接数据库成功
</span><span style="color: #008080;">12</span> 
<span style="color: #008080;">13</span> 请输入SMTP地址: smtp.<span style="color: #800080;">163</span><span style="color: #000000;">.com
</span><span style="color: #008080;">14</span> 请输入SMTP端口 [<span style="color: #800080;">25</span><span style="color: #000000;">]:
</span><span style="color: #008080;">15</span> 请输入账户: ****@<span style="color: #800080;">163</span><span style="color: #000000;">.com
</span><span style="color: #008080;">16</span> 请输入密码: ****
<span style="color: #008080;">17</span> 
<span style="color: #008080;">18</span> <span style="color: #000000;">   请登陆邮箱查收邮件, 然后确认是否继续安装
</span><span style="color: #008080;">19</span> 
<span style="color: #008080;">20</span> 是否继续? (y/<span style="color: #000000;">n) [y]: y
</span><span style="color: #008080;">21</span> <span style="color: #000000;">开始写入配置文件
</span><span style="color: #008080;">22</span> <span style="color: #000000;">开始安装Jumpserver ...
</span><span style="color: #008080;">23</span> <span style="color: #000000;">开始更新jumpserver
</span><span style="color: #008080;">24</span> <span style="color: #000000;">Creating tables ...
</span><span style="color: #008080;">25</span> <span style="color: #000000;">Installing custom SQL ...
</span><span style="color: #008080;">26</span> <span style="color: #000000;">Installing indexes ...
</span><span style="color: #008080;">27</span> Installed <span style="color: #800080;"></span> <span style="color: #0000ff;">object</span>(s) from <span style="color: #800080;"></span><span style="color: #000000;"> fixture(s)
</span><span style="color: #008080;">28</span> 
<span style="color: #008080;">29</span> <span style="color: #000000;">请输入管理员用户名 [admin]:
</span><span style="color: #008080;">30</span> <span style="color: #000000;">请输入管理员密码: [5Lov@wife]: admin
</span><span style="color: #008080;">31</span> <span style="color: #000000;">请再次输入管理员密码: [5Lov@wife]: admin
</span><span style="color: #008080;">32</span> <span style="color: #000000;">Starting jumpserver service:                               [  OK  ]
</span><span style="color: #008080;">33</span> 
<span style="color: #008080;">34</span> 安装成功，Web登录请访问http:<span style="color: #008000;">//</span><span style="color: #008000;">ip:8000, 祝你使用愉快。</span>
<span style="color: #008080;">35</span> 
<span style="color: #008080;">36</span> 请访问 https:<span style="color: #008000;">//</span><span style="color: #008000;">github.com/jumpserver/jumpserver/wiki 查看文档</span></pre>
  </div>
</div>

**第六个里程碑：安装jinjia****模块**

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]#  cd /server/<span style="color: #000000;">tools
[root@m01 tools]#  </span><span style="color: #0000ff;">wget</span> https:<span style="color: #008000;">//</span><span style="color: #008000;">pypi.python.org/packages/47/83/679b5592feb54e948d6599edf5dac61d2991778c3ecbef6b8041663f4740/Jinja2-2.7.1.tar.gz</span>
[root@m01 tools]#  <span style="color: #0000ff;">tar</span> xf Jinja2-<span style="color: #800080;">2.7</span>.<span style="color: #800080;">1</span>.<span style="color: #0000ff;">tar</span><span style="color: #000000;">.gz
[root@m01 tools]# cd Jinja2</span>-<span style="color: #800080;">2.7</span>.<span style="color: #800080;">1</span><span style="color: #000000;">
[root@m01 Jinja2</span>-<span style="color: #800080;">2.7</span>.<span style="color: #800080;">1</span>]# python setup.py  <span style="color: #0000ff;">install</span></pre>
  </div>
</div>

**第七个里程碑：重启jumpserver****服务**

<div>
  <div class="cnblogs_code">
    <pre>[root@m01 ~]# cd /application/jumpserver/<span style="color: #000000;">
[root@m01 jumpserver]# python manage.py runserver </span><span style="color: #800080;">0.0</span>.<span style="color: #800080;">0.0</span>:<span style="color: #800080;">8000</span><span style="color: #000000;">
或者执行下面的命令
[root@m01 </span>~]# cd /application/jumpserver/<span style="color: #000000;">
[root@m01 jumpserver]# .</span>/service.<span style="color: #0000ff;">sh</span> restart</pre>
  </div>
</div>

<span style="background-color: #00ffff;"><em><span style="text-decoration: underline;">到此jumpserver</span></em><em><span style="text-decoration: underline;">安装就完成了</span></em></span>

### <span id="112_jumpserver">1.1.2 jumpserver操作指南</span>

1）浏览器访问服务器 _<span style="text-decoration: underline;">http://ip</span>__<span style="text-decoration: underline;">：端口</span>_<span style="text-decoration: underline;">,</span> &nbsp;使用之前设置的用户名和密码登陆。

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171106105246716-1002784991.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="jumpserver安装详解" alt="" />
</p>

&nbsp;&nbsp; 2)登陆上以后就可以进行管理，在管理之前想要添加主机.

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171106105259856-1907859151.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="jumpserver安装详解" alt="" />
</p>

&nbsp;&nbsp; 3)先添加用户组。

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171106105328481-1435680792.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="jumpserver安装详解" alt="" />
</p>

&nbsp;&nbsp; 4)然后进行资产管理，添加主机，可以批量添加主机

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171106105344669-2049626084.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="jumpserver安装详解" alt="" />&nbsp;
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _<span style="text-decoration: underline;">批量添加示意： </span>_

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171106105456106-282604829.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="jumpserver安装详解" alt="" />&nbsp;
</p>

&nbsp;&nbsp; 5)更多帮助信息请参照官方文档。(使用0.3.2)

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span>  https:<span style="color: #008000;">//</span><span style="color: #008000;">github.com/jumpserver/jumpserver/wiki/v0.3.2-应用图解</span></pre>
</div>

### <span id="113">1.1.3 安装常见问题</span>

请参照官方文档。

<div class="cnblogs_code">
  <pre><span style="color: #008080;">1</span> https:<span style="color: #008000;">//</span><span style="color: #008000;">github.com/jumpserver/jumpserver/wiki/v0.3.2-常见问题-FAQ</span></pre>
</div>

&nbsp;

> # <span id="i-2"><span style="font-size: 13px;">特别感谢:国强哥挖的坑</span></span>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <ul>
      <li>
        <a href="#2018119">2018年1月19日更新：</a><ul>
          <li>
            <a href="#nbsp">&nbsp;</a>
          </li>
          <li>
            <a href="#i">环境说明</a>
          </li>
          <li>
            <a href="#jumpserver">jumpserver安装</a>
          </li>
          <li>
            <a href="#112_jumpserver">1.1.2 jumpserver操作指南</a>
          </li>
          <li>
            <a href="#113">1.1.3 安装常见问题</a>
          </li>
        </ul>
      </li>
    </ul></li>
    
    <li>
      <a href="#i-2">特别感谢:国强哥挖的坑</a>
    </li>
  </ul>
</div>