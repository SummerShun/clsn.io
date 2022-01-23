---
title: Oracle Database 12c Release 2安装详解
author: 惨绿少年
type: post
date: 2017-11-11T01:45:00+00:00
url: /clsn/lx809.html
Baidusubmit:
  - 1
views:
  - 129
zm_favorites:
  - 1
categories:
  - Linux运维
  - 数据库

---
<div id="log-box">
  <div id="catalog">
    <ul id="catalog-ul">
      <li>
        <i class="be be-arrowright"></i> <a href="#title-0" title="1.1 下载方法">1.1 下载方法</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-1" title="1.2 安装过程详解">1.2 安装过程详解</a>
      </li>
    </ul>
    
    <span class="log-zd"><span class="log-close"><a title="隐藏目录"><i class="be be-cross"></i><strong>目录</strong></a></span></span>
  </div>
</div>

**第1章 Oracle Database 12c Release 2安装详解**

<span class="directory"></span>

#### <span id="11">1.1 下载方法</span> {#title-0}

oracle官网https://www.oracle.com

1)打开官方网站，找到下载连接

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174526481-1228264518.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

2)选择更多下载。

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174538388-544870068.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

3)选择数据库版本，这里选择的是目前的最新版本

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174548263-505569097.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

4)接收许可协议，选在linux版本进行下载

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174558138-84364061.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

5) 接收许可协议，点击linuxx64\_12201\_database.zip

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174607356-1960299399.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

6)登陆oracle账户，没有的可以自己创建一个

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174626294-1364106926.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

7）然后就能够进行下载

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174705622-1926240508.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

_https://docs.oracle.com/database/122/LADBI/toc.htm_

<span class="directory"></span>

#### <span id="12">1.2 安装过程详解</span> {#title-1}

注意oracle的安装需要在图形化界面中进行安装。本次使用的是centos6.9 Desktop版本

##### <span id="121">1.2.1 系统版本说明</span>

<div class="cnblogs_code">
  <pre>[root@Oracle ~]# <span style="color: #0000ff;">cat</span> /etc/redhat-<span style="color: #000000;">release
CentOS release </span><span style="color: #800080;">6.9</span><span style="color: #000000;"> (Final)

[root@Oracle </span>~]# <span style="color: #0000ff;">uname</span> -<span style="color: #000000;">a
Linux Oracle </span><span style="color: #800080;">2.6</span>.<span style="color: #800080;">32</span>-<span style="color: #800080;">696</span>.el6.x86_64 #<span style="color: #800080;">1</span> SMP Tue Mar <span style="color: #800080;">21</span> <span style="color: #800080;">19</span>:<span style="color: #800080;">29</span>:<span style="color: #800080;">05</span> UTC <span style="color: #800080;">2017</span> x86_64 x86_64 x86_64 GNU/Linux</pre>
</div>

##### <span id="122">1.2.2 安装依赖包</span>

**安装依赖包，并出现检查**

<div class="cnblogs_code">
  <pre>[root@oracle ~]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> <span style="color: #0000ff;">gcc</span> <span style="color: #0000ff;">gcc</span>-c++ ksh libaio-devel libstdc++-devel compat-libstdc++-<span style="color: #800080;">33</span> compat-libcap1 -y</pre>
</div>

已安装:

<div class="cnblogs_code">
  <pre>compat-libcap1.x86_64 <span style="color: #800080;"></span>:<span style="color: #800080;">1.10</span>-<span style="color: #800080;">1</span><span style="color: #000000;">
compat</span>-libstdc++-<span style="color: #800080;">33</span>.x86_64 <span style="color: #800080;"></span>:<span style="color: #800080;">3.2</span>.<span style="color: #800080;">3</span>-<span style="color: #800080;">69</span><span style="color: #000000;">.el6
</span><span style="color: #0000ff;">gcc</span>.x86_64 <span style="color: #800080;"></span>:<span style="color: #800080;">4.4</span>.<span style="color: #800080;">7</span>-<span style="color: #800080;">18</span><span style="color: #000000;">.el6
</span><span style="color: #0000ff;">gcc</span>-c++.x86_64 <span style="color: #800080;"></span>:<span style="color: #800080;">4.4</span>.<span style="color: #800080;">7</span>-<span style="color: #800080;">18</span><span style="color: #000000;">.el6
ksh.x86_64 </span><span style="color: #800080;"></span>:<span style="color: #800080;">20120801</span>-<span style="color: #800080;">35</span><span style="color: #000000;">.el6_9
libaio</span>-devel.x86_64 <span style="color: #800080;"></span>:<span style="color: #800080;">0.3</span>.<span style="color: #800080;">107</span>-<span style="color: #800080;">10</span><span style="color: #000000;">.el6
libstdc</span>++-devel.x86_64 <span style="color: #800080;"></span>:<span style="color: #800080;">4.4</span>.<span style="color: #800080;">7</span>-<span style="color: #800080;">18</span>.el6</pre>
</div>

##### <span id="123">1.2.3 安装过程</span>

**第一个里程碑:****对文件进行解压**

<div class="cnblogs_code">
  <pre>cd /server/tools/
<span style="color: #0000ff;">unzip</span> linuxx64_12201_database.<span style="color: #0000ff;">zip</span></pre>
</div>

**第二个里程碑：创建oracle****用户，并切换到oracle****用户**

<div class="cnblogs_code">
  <pre><span style="color: #000000;">useradd oracle
</span><span style="color: #0000ff;">passwd</span><span style="color: #000000;"> oracle

</span><span style="color: #0000ff;">chown</span> -R oracle.oracle /server/tools/database/</pre>
</div>

**第三个里程碑：切换到oracle****用户，执行安装脚本**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174839184-1669441578.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

<div class="cnblogs_code">
  <pre>cd /server/tools/<span style="color: #000000;">database
.</span>/runInstaller</pre>
</div>

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174857497-1914028509.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174905559-227118882.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**第五个里程碑:****进行数据库配置**

输入自己的邮箱.

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174921684-129732769.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**第六个里程碑：选择创建新的数据库**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174930763-464088814.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**第七个里程碑：安装选择服务器类型**

桌面类型少好得多的功能

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174942638-1412570706.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**第八个里程碑：选择数据库的安装类型**

这里选择单实例即可

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111174950575-1505201103.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**第九个里程碑：进行安装**

选在高级安装，进行定制化的安装

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175000294-93123834.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**第十个里程碑：选在数据库版本**

这里选择企业版

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175009872-2010586263.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**第十一个里程碑：指定安装目录**

注意安装的目录要有足够的空间，oracle所需空间较大

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175021278-1146075842.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**第十二个里程碑：指定产品清单目录**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175031356-1856331285.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**第十三个里程碑：选择创建的数据库类型**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175039434-1854581197.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**选择数据库名称，默认即可**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175052794-481745682.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**注意内存设置**

由于我是虚拟机所以内存给成最小

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175103028-233040001.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**在字符集选择utf8**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175110341-88599343.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**选在安装上示例**

因为我是做学习用途，所以安装示例

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175118919-1511061070.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**选在数据的存储方式**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175127591-1834180077.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**云管理，有oracle****的可以添加**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175141544-566417911.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**数据恢复，开启**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175204716-57524078.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**设置用户口令**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175213434-2138249107.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

我这里统一密码，生产环境中建议设置高强度密码

设置的密码为oracle 比较简单，所有系统会提示不符合安全规范，选择是即可

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175224372-937410809.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

**使用的数据库操作类型（默认即可）**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175233481-1356437615.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

**开始进行安装。**

**<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175247122-2044614618.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />**

检查是否环境正确，错误会有修复脚本。

<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175257059-307469629.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /> 

_使用修复脚本进行修复，注意使用root__用户_

_<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175307325-232849373.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />_

_以root__用户运行这个脚本_

<div class="cnblogs_code" onclick="cnblogs_code_show('60746ead-d842-49bc-ad78-be75b1a92cbf')">
  <img id="code_img_closed_60746ead-d842-49bc-ad78-be75b1a92cbf" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-16.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /><img id="code_img_opened_60746ead-d842-49bc-ad78-be75b1a92cbf" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('60746ead-d842-49bc-ad78-be75b1a92cbf',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-16.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /></p> 
  
  <div id="cnblogs_code_open_60746ead-d842-49bc-ad78-be75b1a92cbf" class="cnblogs_code_hide">
    <pre><span style="color: #008080;">  1</span> [root@oracle ~]# <span style="color: #0000ff;">sh</span>  /tmp/CVU_12.<span style="color: #800080;">2.0</span>.<span style="color: #800080;">1</span>.0_oracle/runfixup.<span style="color: #0000ff;">sh</span> 
<span style="color: #008080;">  2</span> All Fix-<span style="color: #000000;">up operations were completed successfully.
</span><span style="color: #008080;">  3</span> [root@oracle ~]# <span style="color: #0000ff;">cat</span> /tmp/CVU_12.<span style="color: #800080;">2.0</span>.<span style="color: #800080;">1</span>.0_oracle/runfixup.<span style="color: #0000ff;">sh</span>
<span style="color: #008080;">  4</span> #!/bin/<span style="color: #0000ff;">sh</span>
<span style="color: #008080;">  5</span> <span style="color: #000000;">#
</span><span style="color: #008080;">  6</span> # $Header: opsm/cvutl/runfixup.<span style="color: #0000ff;">sh</span> /main/<span style="color: #800080;">16</span> <span style="color: #800080;">2012</span>/<span style="color: #800080;">11</span>/<span style="color: #800080;">13</span> <span style="color: #800080;">21</span>:<span style="color: #800080;">44</span>:<span style="color: #800080;">52</span><span style="color: #000000;"> ptar
</span><span style="color: #008080;">  7</span> <span style="color: #000000;">#
</span><span style="color: #008080;">  8</span> # runfixup.<span style="color: #0000ff;">sh</span>
<span style="color: #008080;">  9</span> <span style="color: #000000;">#
</span><span style="color: #008080;"> 10</span> # Copyright (c) <span style="color: #800080;">2007</span>, <span style="color: #800080;">2012</span>, Oracle and/<span style="color: #000000;">or its affiliates. All right
</span><span style="color: #008080;"> 11</span> <span style="color: #000000;">#
</span><span style="color: #008080;"> 12</span> <span style="color: #000000;">#    NAME
</span><span style="color: #008080;"> 13</span> #      runfixup.<span style="color: #0000ff;">sh</span> -<span style="color: #000000;"> This script is used to run fixups on a node
</span><span style="color: #008080;"> 14</span> <span style="color: #000000;">#
</span><span style="color: #008080;"> 15</span> <span style="color: #000000;">#    DESCRIPTION
</span><span style="color: #008080;"> 16</span> #      &lt;<span style="color: #0000ff;">short</span> description of component this <span style="color: #0000ff;">file</span> declares/defines>
<span style="color: #008080;"> 17</span> <span style="color: #000000;">#
</span><span style="color: #008080;"> 18</span> <span style="color: #000000;">#    NOTES
</span><span style="color: #008080;"> 19</span> #      &lt;other useful comments, qualifications, etc.>
<span style="color: #008080;"> 20</span> <span style="color: #000000;">#
</span><span style="color: #008080;"> 21</span> #    MODIFIED   (MM/DD/<span style="color: #000000;">YY)
</span><span style="color: #008080;"> 22</span> #    ptare       <span style="color: #800080;">11</span>/<span style="color: #800080;">09</span>/<span style="color: #800080;">12</span> -<span style="color: #000000;"> retrieve fixup information from fixup i
</span><span style="color: #008080;"> 23</span> #    dsaggi      <span style="color: #800080;">09</span>/<span style="color: #800080;">11</span>/<span style="color: #800080;">12</span> - Fix <span style="color: #800080;">14612018</span> -- Qualify path <span style="color: #0000ff;">for</span><span style="color: #000000;"> dirnam
</span><span style="color: #008080;"> 24</span> #    ptare       <span style="color: #800080;">03</span>/<span style="color: #800080;">13</span>/<span style="color: #800080;">12</span> -<span style="color: #000000;"> enhance the output of the script to makiendly instead of displaying exectask tags
</span><span style="color: #008080;"> 25</span> #    ptare       <span style="color: #800080;">05</span>/<span style="color: #800080;">19</span>/<span style="color: #800080;">11</span> - Make changes <span style="color: #0000ff;">for</span><span style="color: #000000;"> fixup project
</span><span style="color: #008080;"> 26</span> #    agorla      <span style="color: #800080;">08</span>/<span style="color: #800080;">18</span>/<span style="color: #800080;">10</span> - bug#<span style="color: #800080;">10023742</span> - donot <span style="color: #0000ff;">echo</span> <span style="color: #0000ff;">id</span><span style="color: #000000;"> cmd
</span><span style="color: #008080;"> 27</span> #    nvira       <span style="color: #800080;">05</span>/<span style="color: #800080;">04</span>/<span style="color: #800080;">10</span> - fix the <span style="color: #0000ff;">id</span><span style="color: #000000;"> command
</span><span style="color: #008080;"> 28</span> #    dsaggi      <span style="color: #800080;">01</span>/<span style="color: #800080;">27</span>/<span style="color: #800080;">10</span> - Fix <span style="color: #800080;">8729861</span>
<span style="color: #008080;"> 29</span> #    nvira       <span style="color: #800080;">06</span>/<span style="color: #800080;">24</span>/<span style="color: #800080;">08</span> - remove <span style="color: #0000ff;">sudo</span>
<span style="color: #008080;"> 30</span> #    dsaggi      <span style="color: #800080;">05</span>/<span style="color: #800080;">29</span>/<span style="color: #800080;">08</span> -<span style="color: #000000;"> remove orarun.log before invocation
</span><span style="color: #008080;"> 31</span> #    dsaggi      <span style="color: #800080;">10</span>/<span style="color: #800080;">24</span>/<span style="color: #800080;">07</span> -<span style="color: #000000;"> Creation
</span><span style="color: #008080;"> 32</span> <span style="color: #000000;">#
</span><span style="color: #008080;"> 33</span> AWK=/bin/<span style="color: #0000ff;">awk</span>
<span style="color: #008080;"> 34</span> SED=/bin/<span style="color: #0000ff;">sed</span>
<span style="color: #008080;"> 35</span> ECHO=/bin/<span style="color: #0000ff;">echo</span>
<span style="color: #008080;"> 36</span> ID=/usr/bin/<span style="color: #0000ff;">id</span>
<span style="color: #008080;"> 37</span> GREP=/bin/<span style="color: #0000ff;">grep</span>
<span style="color: #008080;"> 38</span> DIRNAME=/usr/bin/<span style="color: #0000ff;">dirname</span>
<span style="color: #008080;"> 39</span> FIXUP_INPUT_FILE=<span style="color: #000000;">fixup.conf
</span><span style="color: #008080;"> 40</span> FIXUP_INPUT_FILE_PATH=`$DIRNAME $<span style="color: #800080;"></span>`/fixup/<span style="color: #000000;">$FIXUP_INPUT_FILE
</span><span style="color: #008080;"> 41</span> 
<span style="color: #008080;"> 42</span> <span style="color: #000000;">#internal method to initialize the fixup instructions from the inpu
</span><span style="color: #008080;"> 43</span> <span style="color: #000000;">initializeFixupInstructions()
</span><span style="color: #008080;"> 44</span> <span style="color: #000000;">{ 
</span><span style="color: #008080;"> 45</span>   <span style="color: #0000ff;">if</span> [ -<span style="color: #000000;">f $FIXUP_INPUT_FILE_PATH ]
</span><span style="color: #008080;"> 46</span>   <span style="color: #0000ff;">then</span>
<span style="color: #008080;"> 47</span>      FIXUP_DATA_FILE=<span style="color: #000000;">`$GREP FIXUP_DATA_FILE $FIXUP_INPUT_FILE_PATH `
</span><span style="color: #008080;"> 48</span>      FIXUP_TRACE_LEVEL=`$GREP FIXUP_TRACE_LEVEL $FIXUP_INPUT_FILE_P-f <span style="color: #800080;">2</span><span style="color: #000000;">`
</span><span style="color: #008080;"> 49</span>   <span style="color: #0000ff;">else</span>
<span style="color: #008080;"> 50</span>      $ECHO <span style="color: #800000;">"</span> <span style="color: #800000;">"</span>
<span style="color: #008080;"> 51</span>      $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">ERROR: </span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 52</span>      $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">Fixup instructions are not yet generated for this node.</span>
<span style="color: #008080;"> 53</span>      exit <span style="color: #800080;">1</span>
<span style="color: #008080;"> 54</span>   <span style="color: #0000ff;">fi</span>
<span style="color: #008080;"> 55</span> <span style="color: #000000;">} 
</span><span style="color: #008080;"> 56</span> 
<span style="color: #008080;"> 57</span> #initialize the fixup instructions from the fixup input <span style="color: #0000ff;">file</span>
<span style="color: #008080;"> 58</span> <span style="color: #000000;">initializeFixupInstructions
</span><span style="color: #008080;"> 59</span> 
<span style="color: #008080;"> 60</span> RUID=`$ID -u <span style="color: #800080;">1</span>> /dev/<span style="color: #0000ff;">null</span> <span style="color: #800080;">2</span>>&<span style="color: #800080;">1</span><span style="color: #000000;">`
</span><span style="color: #008080;"> 61</span> status=$?
<span style="color: #008080;"> 62</span> 
<span style="color: #008080;"> 63</span> <span style="color: #0000ff;">if</span> [ <span style="color: #800000;">"</span><span style="color: #800000;">$status</span><span style="color: #800000;">"</span> != <span style="color: #800000;">"</span><span style="color: #800000;"></span><span style="color: #800000;">"</span><span style="color: #000000;"> ];
</span><span style="color: #008080;"> 64</span> <span style="color: #0000ff;">then</span>
<span style="color: #008080;"> 65</span>   RUID=`$ID | $AWK -F\( <span style="color: #800000;">'</span><span style="color: #800000;">{print $1}</span><span style="color: #800000;">'</span> | $AWK -F= <span style="color: #800000;">'</span><span style="color: #800000;">{ print $2}</span><span style="color: #800000;">'</span><span style="color: #000000;">`
</span><span style="color: #008080;"> 66</span> <span style="color: #0000ff;">else</span>
<span style="color: #008080;"> 67</span> RUID=`$ID -<span style="color: #000000;">u`
</span><span style="color: #008080;"> 68</span> <span style="color: #0000ff;">fi</span>
<span style="color: #008080;"> 69</span> 
<span style="color: #008080;"> 70</span> <span style="color: #0000ff;">if</span> [ -z <span style="color: #800000;">"</span><span style="color: #800000;">$RUID</span><span style="color: #800000;">"</span><span style="color: #000000;"> ];
</span><span style="color: #008080;"> 71</span> <span style="color: #0000ff;">then</span>
<span style="color: #008080;"> 72</span>   $ECHO <span style="color: #800000;">"</span> <span style="color: #800000;">"</span>
<span style="color: #008080;"> 73</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">ERROR: </span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 74</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">Failed to get effective user id.</span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 75</span>   exit <span style="color: #800080;">1</span>
<span style="color: #008080;"> 76</span> <span style="color: #0000ff;">fi</span> 
<span style="color: #008080;"> 77</span> 
<span style="color: #008080;"> 78</span> <span style="color: #0000ff;">if</span> [ <span style="color: #800000;">"</span><span style="color: #800000;">${RUID}</span><span style="color: #800000;">"</span> != <span style="color: #800000;">"</span><span style="color: #800000;"></span><span style="color: #800000;">"</span> ];<span style="color: #0000ff;">then</span>
<span style="color: #008080;"> 79</span>   $ECHO <span style="color: #800000;">"</span> <span style="color: #800000;">"</span>
<span style="color: #008080;"> 80</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">ERROR: </span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 81</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">You must be logged in as root (uid=0) when running $0.</span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 82</span>   exit <span style="color: #800080;">1</span>
<span style="color: #008080;"> 83</span> <span style="color: #0000ff;">fi</span>
<span style="color: #008080;"> 84</span> 
<span style="color: #008080;"> 85</span> EXEC_DIR=`$DIRNAME $<span style="color: #800080;"></span><span style="color: #000000;">`
</span><span style="color: #008080;"> 86</span> RMF=<span style="color: #800000;">"</span><span style="color: #800000;">/bin/rm -f</span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 87</span> 
<span style="color: #008080;"> 88</span> <span style="color: #0000ff;">if</span> [ <span style="color: #800000;">"</span><span style="color: #800000;">X$FIXUP_DATA_FILE</span><span style="color: #800000;">"</span> = <span style="color: #800000;">"</span><span style="color: #800000;">X</span><span style="color: #800000;">"</span><span style="color: #000000;"> ]
</span><span style="color: #008080;"> 89</span> <span style="color: #0000ff;">then</span>
<span style="color: #008080;"> 90</span>   $ECHO <span style="color: #800000;">"</span> <span style="color: #800000;">"</span>
<span style="color: #008080;"> 91</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">ERROR: </span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 92</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">fixup instructions are not yet generated for this node.</span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 93</span>   exit <span style="color: #800080;">1</span>
<span style="color: #008080;"> 94</span> <span style="color: #0000ff;">else</span>
<span style="color: #008080;"> 95</span> 
<span style="color: #008080;"> 96</span> $RMF ${EXEC_DIR}/cvu_fixup_trace_*<span style="color: #000000;">.log
</span><span style="color: #008080;"> 97</span> 
<span style="color: #008080;"> 98</span> <span style="color: #0000ff;">if</span> [ <span style="color: #800000;">"</span><span style="color: #800000;">X$FIXUP_TRACE_LEVEL</span><span style="color: #800000;">"</span> = <span style="color: #800000;">"</span><span style="color: #800000;">X</span><span style="color: #800000;">"</span><span style="color: #000000;"> ]
</span><span style="color: #008080;"> 99</span> <span style="color: #0000ff;">then</span>
<span style="color: #008080;">100</span> FIXUP_TRACE_OPTION=
<span style="color: #008080;">101</span> <span style="color: #0000ff;">else</span>
<span style="color: #008080;">102</span> FIXUP_TRACE_OPTION=<span style="color: #800000;">"</span><span style="color: #800000;">-tracelevel $FIXUP_TRACE_LEVEL</span><span style="color: #800000;">"</span>
<span style="color: #008080;">103</span> <span style="color: #0000ff;">fi</span>
<span style="color: #008080;">104</span> 
<span style="color: #008080;">105</span> <span style="color: #000000;"># Execute the exectask 
</span><span style="color: #008080;">106</span> EXECTASK_OUTPUT=`${EXEC_DIR}/exectask.<span style="color: #0000ff;">sh</span> -runfixup $FIXUP_DATA_FILEION <span style="color: #800080;">2</span>>&<span style="color: #800080;">1</span><span style="color: #000000;">`
</span><span style="color: #008080;">107</span> status=$?
<span style="color: #008080;">108</span> 
<span style="color: #008080;">109</span> <span style="color: #0000ff;">if</span> [ <span style="color: #800000;">"</span><span style="color: #800000;">$status</span><span style="color: #800000;">"</span> != <span style="color: #800000;">"</span><span style="color: #800000;"></span><span style="color: #800000;">"</span><span style="color: #000000;"> ];
</span><span style="color: #008080;">110</span> <span style="color: #0000ff;">then</span>
<span style="color: #008080;">111</span>   $ECHO <span style="color: #800000;">"</span> <span style="color: #800000;">"</span>
<span style="color: #008080;">112</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">FAILED: Fix-up operations could not be completed on this n</span>
<span style="color: #008080;">113</span> <span style="color: #000000;">#Extract the exectask error details from the CV_ERR TAGS
</span><span style="color: #008080;">114</span>   EXECTASK_ERROR=`$ECHO $EXECTASK_OUTPUT | $SED <span style="color: #800000;">"</span><span style="color: #800000;">s/&lt;CV_ERR>//;s/&lt;\/</span>
<span style="color: #008080;">115</span> #Check <span style="color: #0000ff;">if</span> we have the exectask error, <span style="color: #0000ff;">if</span> yes <span style="color: #0000ff;">then</span><span style="color: #000000;"> print it 
</span><span style="color: #008080;">116</span> <span style="color: #0000ff;">if</span> [ <span style="color: #800000;">"</span><span style="color: #800000;">X$EXECTASK_ERROR</span><span style="color: #800000;">"</span> != <span style="color: #800000;">"</span><span style="color: #800000;">X</span><span style="color: #800000;">"</span><span style="color: #000000;"> ]
</span><span style="color: #008080;">117</span> <span style="color: #0000ff;">then</span>
<span style="color: #008080;">118</span>   $ECHO <span style="color: #800000;">"</span> <span style="color: #800000;">"</span>
<span style="color: #008080;">119</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">ERROR: </span><span style="color: #800000;">"</span>
<span style="color: #008080;">120</span> <span style="color: #000000;">  $ECHO $EXECTASK_ERROR
</span><span style="color: #008080;">121</span>   $ECHO <span style="color: #800000;">"</span> <span style="color: #800000;">"</span>
<span style="color: #008080;">122</span> <span style="color: #0000ff;">fi</span>
<span style="color: #008080;">123</span> <span style="color: #0000ff;">else</span>
<span style="color: #008080;">124</span>   $ECHO <span style="color: #800000;">"</span><span style="color: #800000;">All Fix-up operations were completed successfully.</span><span style="color: #800000;">"</span>
<span style="color: #008080;">125</span> <span style="color: #0000ff;">fi</span>
<span style="color: #008080;">126</span> <span style="color: #0000ff;">fi</span></pre>
  </div>
  
  <p>
    <span class="cnblogs_code_collapse">代码详情</span></div> 
    
    <p>
      <strong>修复完成后可以继续后面的操作</strong>
    </p>
    
    <p>
      <strong><img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175410200-467610588.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /></strong>
    </p>
    
    <p>
      <strong>点击下一步进行安装即可,</strong><strong>安装速度较慢，耐心等待</strong>
    </p>
    
    <p>
      <strong><img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175419731-1977997941.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" /></strong>
    </p>
    
    <p>
      安装的过程中执行脚本
    </p>
    
    <p>
      <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175432216-1977236920.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />
    </p>
    
    <div class="cnblogs_code">
      <pre>[root@oracle ~]# <span style="color: #0000ff;">sh</span> /oracle/app/oraInventory/orainstRoot.<span style="color: #0000ff;">sh</span><span style="color: #000000;">

更改权限</span>/oracle/app/<span style="color: #000000;">oraInventory.

添加组的读取和写入权限。

删除全局的读取, 写入和执行权限。

更改组名</span>/oracle/app/<span style="color: #000000;">oraInventory 到 oracle.

脚本的执行已完成。

[root@oracle </span>~]# <span style="color: #0000ff;">sh</span> /oracle/app/oraclea/product/<span style="color: #800080;">12.2</span>.<span style="color: #800080;"></span>/dbhome_1/root.<span style="color: #0000ff;">sh</span></pre>
    </div>
    
    <p>
      <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111175458184-1250137667.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />
    </p>
    
    <p>
      &nbsp;安装完成，根据提示用浏览器访问
    </p>
    
    <p>
      <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111191032247-1991758576.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />
    </p>
    
    <div class="cnblogs_code">
      <pre><span style="color: #000000;">用户名为 system
密码为 oracle</span></pre>
    </div>
    
    <p>
      <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171111191521809-55534252.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Oracle Database 12c Release 2安装详解" alt="" />
    </p>
    
    <p>
      中间出现错误Configuration Assistant 失败 的原因
    </p>
    
    <p>
      1.看一下c:\windows\System32\drivers\etc\hosts 文件 127.0.0.1是否被屏蔽掉了。<br />2.还有IP地址不要使用DHCP 自动获取IP的方式，需要指定IP地址。出现这个问题错误只要你找到原因，然后让监听正常启动就可以解决问题了。
    </p>
    
    <p>
      3.是防火墙没有关闭引起的。
    </p>
    
    <p style="text-align: right;">
      <span style="font-size: 18pt;"><strong>&nbsp;</strong></span>
    </p>
    
    <div id="toc_container" class="toc_white have_bullets">
      <ul class="toc_list">
        <li>
          <a href="#11">1.1 下载方法</a>
        </li>
        <li>
          <a href="#12">1.2 安装过程详解</a><ul>
            <li>
              <a href="#121">1.2.1 系统版本说明</a>
            </li>
            <li>
              <a href="#122">1.2.2 安装依赖包</a>
            </li>
            <li>
              <a href="#123">1.2.3 安装过程</a>
            </li>
          </ul>
        </li>
      </ul>
    </div>