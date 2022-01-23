---
title: Confluence 平台部署记录
author: 惨绿少年
type: post
date: 2017-12-27T17:08:00+00:00
url: /clsn/lx355.html
Baidusubmit:
  - 1
views:
  - 186
categories:
  - Linux运维
  - 敏捷开发
  - 自动化

---
## <span id="11_Confluence"><span style="font-size: 1.5em;">1.1 Confluence简介</span></span>

　　Confluence是一个专业的企业知识管理与协同软件，也可以用于构建企业wiki。使用简单，但它强大的编辑和站点管理特征能够帮助团队成员之间共享信息、文档协作、集体讨论，信息推送。

&nbsp;&nbsp;　&nbsp;Confluence为团队提供一个协作环境。在这里，团队成员齐心协力，各擅其能，协同地编写文档和管理项目。从此打破不同团队、不同部门以及个人之间信息孤岛的僵局，Confluence真正实现了组织资源共享。

### <span id="111">1.1.1 使用情况</span>

　　Confluence 已经在超过100个国家，13500个组织中成功地应用于企业内网平台、知识管理及文档管理，涉及财富1000企业、政府机构、教育机构、财务金融机构及技术研究领域。

　　包括IBM、Sun MicroSystems、SAP等众多知名企业使用Confluence来构建企业Wiki并面向公众开放。

## <span id="12">1.2 环境准备</span>

　　confluence的运行是依赖java环境的，也就是说需要安装jdk并且要是1.7以上版本，

### <span id="121">1.2.1 系统环境说明</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@conflunce ~]# <span style="color: #0000ff;">cat</span> /etc/redhat-<span style="color: #000000;">release 
CentOS Linux release </span><span style="color: #800080;">7.4</span>.<span style="color: #800080;">1708</span><span style="color: #000000;"> (Core) 
[root@conflunce </span>~]# <span style="color: #0000ff;">uname</span> -<span style="color: #000000;">a
Linux conflunce </span><span style="color: #800080;">3.10</span>.<span style="color: #800080;"></span>-<span style="color: #800080;">693</span>.el7.x86_64 #<span style="color: #800080;">1</span> SMP Tue Aug <span style="color: #800080;">22</span> <span style="color: #800080;">21</span>:<span style="color: #800080;">09</span>:<span style="color: #800080;">27</span> UTC <span style="color: #800080;">2017</span> x86_64 x86_64 x86_64 GNU/<span style="color: #000000;">Linux
[root@conflunce </span>~<span style="color: #000000;">]# getenforce 
Disabled
[root@conflunce </span>~<span style="color: #000000;">]# systemctl status firewalld.service 
● firewalld.service </span>- firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: </span><span style="color: #0000ff;">man</span>:firewalld(<span style="color: #800080;">1</span>)</pre>
  </div>
</div>

### <span id="122">1.2.2 软件环境说明</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@conflunce tools]# java -<span style="color: #000000;">version
java version </span><span style="color: #800000;">"</span><span style="color: #800000;">1.8.0_60</span><span style="color: #800000;">"</span><span style="color: #000000;">
Java(TM) SE Runtime Environment (build </span><span style="color: #800080;">1.8</span>.0_60-<span style="color: #000000;">b27)
Java HotSpot(TM) </span><span style="color: #800080;">64</span>-Bit Server VM (build <span style="color: #800080;">25.60</span>-b23, mixed mode)</pre>
  </div>
</div>

<div>
  <p class="a">
    # 安装 jdk
  </p>
  
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">wget</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">10.0.0.1/apache/tomcat/jdk-8u60-linux-x64.tar.gz</span>
<span style="color: #0000ff;">tar</span> xf jdk-8u60-linux-x64.<span style="color: #0000ff;">tar</span>.gz -C /application/
<span style="color: #0000ff;">ln</span> -s /application/jdk1.<span style="color: #800080;">8</span>.0_60 /application/<span style="color: #000000;">jdk
</span><span style="color: #0000ff;">sed</span> -i.ori <span style="color: #800000;">'</span><span style="color: #800000;">$a export JAVA_HOME=/application/jdk\nexport PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH\nexport CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar</span><span style="color: #800000;">'</span> /etc/<span style="color: #000000;">profile
source </span>/etc/profile</pre>
  </div>
</div>

为confluence创建对应的数据库

<div>
  <p class="a">
    # 安装数据库
  </p>
  
  <div class="cnblogs_code">
    <pre>[root@conflunce ~]# <span style="color: #0000ff;">yum</span> <span style="color: #0000ff;">install</span> -y mariadb-<span style="color: #000000;">server 
[root@conflunce </span>~]# systemctl start   mariadb.service</pre>
  </div>
</div>

<div>
  <p class="a1">
    mysql配置
  </p>
  
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">database</span> confluence <span style="color: #0000ff;">default</span> <span style="color: #0000ff;">character</span> <span style="color: #0000ff;">set</span><span style="color: #000000;"> utf8 collate utf8_bin；
</span><span style="color: #0000ff;">grant</span> <span style="color: #808080;">all</span> <span style="color: #0000ff;">on</span> confluence.<span style="color: #808080;">*</span> <span style="color: #0000ff;">to</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">confluence</span><span style="color: #ff0000;">'</span>@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">localhost</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">confluence</span><span style="color: #ff0000;">'</span>;</pre>
  </div>
</div>

## <span id="13_confluence">1.3 下载confluence</span>

<div>
  <div class="cnblogs_code">
    <pre>cd <span style="color: #808080;">/</span>server<span style="color: #808080;">/</span><span style="color: #000000;">tools
wget https:</span><span style="color: #808080;">//</span>www.atlassian.com<span style="color: #808080;">/</span>software<span style="color: #808080;">/</span>confluence<span style="color: #808080;">/</span>downloads<span style="color: #808080;">/</span><span style="color: #0000ff;">binary</span><span style="color: #808080;">/</span>atlassian<span style="color: #808080;">-</span>confluence<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">5.6</span>.<span style="color: #800000; font-weight: bold;">6</span><span style="color: #808080;">-</span>x64.bin</pre>
  </div>
</div>

## <span id="14_confluence">1.4 安装confluence</span>

### <span id="141">1.4.1 安装</span>

修改权限

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@conflunce tools</span><span style="color: #ff0000;">]</span># chmod <span style="color: #800000; font-weight: bold;">755</span> atlassian<span style="color: #808080;">-</span>confluence<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">5.6</span>.<span style="color: #800000; font-weight: bold;">6</span><span style="color: #808080;">-</span><span style="color: #000000;">x64.bin
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@conflunce tools</span><span style="color: #ff0000;">]</span># .<span style="color: #808080;">/</span>atlassian<span style="color: #808080;">-</span>confluence<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">5.6</span>.<span style="color: #800000; font-weight: bold;">6</span><span style="color: #808080;">-</span>x64.bin</pre>
  </div>
</div>

安装confluence

<div>
  <div class="cnblogs_code">
    <pre>[root@conflunce tools]# ./atlassian-confluence-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">6</span>-<span style="color: #000000;">x64.bin 
Unpacking JRE ...
Starting Installer ...
十一月 </span><span style="color: #800080;">24</span>, <span style="color: #800080;">2017</span> <span style="color: #800080;">4</span>:<span style="color: #800080;">56</span>:<span style="color: #800080;">41</span><span style="color: #000000;"> 下午 java.util.prefs.FileSystemPreferences$
INFO: Created user preferences directory.
十一月 </span><span style="color: #800080;">24</span>, <span style="color: #800080;">2017</span> <span style="color: #800080;">4</span>:<span style="color: #800080;">56</span>:<span style="color: #800080;">41</span><span style="color: #000000;"> 下午 java.util.prefs.FileSystemPreferences$
INFO: Created system preferences directory </span><span style="color: #0000ff;">in</span><span style="color: #000000;"> java.home.

This will </span><span style="color: #0000ff;">install</span> Confluence <span style="color: #800080;">5.6</span>.<span style="color: #800080;">6</span><span style="color: #000000;"> on your computer.
OK [o, Enter], Cancel [c]
o
Choose the appropriate installation or upgrade option.
Please choose one of the following:
Express Install (uses default settings) [</span><span style="color: #800080;">1</span>], Custom Install (recommd users) [<span style="color: #800080;">2</span>, Enter], Upgrade an existing Confluence installation [<span style="color: #800080;">3</span>
<span style="color: #800080;">1</span><span style="color: #000000;">
See where Confluence will be installed and the settings that will b
Installation Directory: </span>/opt/atlassian/<span style="color: #000000;">confluence 
Home Directory: </span>/var/atlassian/application-data/<span style="color: #000000;">confluence 
HTTP Port: </span><span style="color: #800080;">8090</span><span style="color: #000000;"> 
RMI Port: </span><span style="color: #800080;">8000</span><span style="color: #000000;"> 
Install as service: Yes 
Install [i, Enter], Exit [e]
i

Extracting files ...

&hellip;&hellip;

Please </span><span style="color: #0000ff;">wait</span> a few moments <span style="color: #0000ff;">while</span><span style="color: #000000;"> Confluence starts up.
Launching Confluence ...
Installation of Confluence </span><span style="color: #800080;">5.6</span>.<span style="color: #800080;">6</span><span style="color: #000000;"> is complete
Your installation of Confluence </span><span style="color: #800080;">5.6</span>.<span style="color: #800080;">6</span><span style="color: #000000;"> is now ready and can be accessed via
your browser.
Confluence </span><span style="color: #800080;">5.6</span>.<span style="color: #800080;">6</span> can be accessed at http:<span style="color: #008000;">//</span><span style="color: #008000;">localhost:8090</span>
Finishing installation ...</pre>
  </div>
</div>

使用浏览器访问

_<span style="text-decoration: underline;">http://10.0.0.211:8090/setup/</span>_

　　注意：这个访问地址根据自己的世纪服务器地址进行调整。

<p style="text-align: center;">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223093616396-178689559.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />&nbsp;
</p>

### <span id="142">1.4.2 修改程序</span>

<p style="text-align: center;">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223093823818-1831762087.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />
</p>

　　　　通过上图，我们可以看到现在confluence要我们输入license，下面我们进行破解。

<div>
  <p class="a">
    # 首先下载修改包
  </p>
  
  <div class="cnblogs_code">
    <pre>http:<span style="color: #008000;">//</span><span style="color: #008000;">down.51cto.com/data/2236416</span>
https:<span style="color: #008000;">//</span><span style="color: #008000;">page00.ctfile.com/fs/15323800-217465309</span></pre>
  </div>
</div>

<div>
  <p class="a1">
    # 先停止 conflunce服务
  </p>
  
  <div class="cnblogs_code">
    <pre>[root@conflunce tools]# /etc/init.d/<span style="color: #000000;">confluence stop
executing using dedicated user
If you encounter issues starting up Confluence, please see the Installation guide at http:</span><span style="color: #008000;">//</span><span style="color: #008000;">confluence.atlassian.com/display/DOC/Confluence+Installation+Guide</span>
<span style="color: #000000;">
Server startup logs are located </span><span style="color: #0000ff;">in</span> /opt/atlassian/confluence/logs/<span style="color: #000000;">catalina.out
Using CATALINA_BASE:   </span>/opt/atlassian/<span style="color: #000000;">confluence
Using CATALINA_HOME:   </span>/opt/atlassian/<span style="color: #000000;">confluence
Using CATALINA_TMPDIR: </span>/opt/atlassian/confluence/<span style="color: #000000;">temp
Using JRE_HOME:        </span>/opt/atlassian/confluence/jre/<span style="color: #000000;">
Using CLASSPATH:       </span>/opt/atlassian/confluence/bin/bootstrap.jar:/opt/atlassian/confluence/bin/tomcat-<span style="color: #000000;">juli.jar
Using CATALINA_PID:    </span>/opt/atlassian/confluence/work/<span style="color: #000000;">catalina.pid
Tomcat stopped.</span></pre>
  </div>
</div>

\# 删除原来的包文件

<div>
  <div class="cnblogs_code">
    <pre>[root@conflunce ~]# cd /opt/atlassian/confluence/confluence/WEB-INF/<span style="color: #000000;">lib
[root@conflunce lib]# ll </span>|<span style="color: #0000ff;">grep</span> atlassian-extra |<span style="color: #0000ff;">wc</span> -<span style="color: #000000;">l
</span><span style="color: #800080;">6</span><span style="color: #000000;">
[root@conflunce lib]# ll </span>|<span style="color: #0000ff;">grep</span> atlassian-<span style="color: #000000;">extra 
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root   <span style="color: #800080;">14935</span> 12月  <span style="color: #800080;">1</span> <span style="color: #800080;">2014</span> atlassian-extras-api-<span style="color: #800080;">3.2</span><span style="color: #000000;">.jar
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root   <span style="color: #800080;">21788</span> 12月  <span style="color: #800080;">1</span> <span style="color: #800080;">2014</span> atlassian-extras-common-<span style="color: #800080;">3.2</span><span style="color: #000000;">.jar
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root   <span style="color: #800080;">38244</span> 12月  <span style="color: #800080;">1</span> <span style="color: #800080;">2014</span> atlassian-extras-core-<span style="color: #800080;">3.2</span><span style="color: #000000;">.jar
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root    <span style="color: #800080;">5171</span> 12月  <span style="color: #800080;">1</span> <span style="color: #800080;">2014</span> atlassian-extras-decoder-api-<span style="color: #800080;">3.2</span><span style="color: #000000;">.jar
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root    <span style="color: #800080;">6668</span> 12月  <span style="color: #800080;">1</span> <span style="color: #800080;">2014</span> atlassian-extras-decoder-v2-<span style="color: #800080;">3.2</span><span style="color: #000000;">.jar
</span>-rw-r--r-- <span style="color: #800080;">1</span> root root   <span style="color: #800080;">68438</span> 12月  <span style="color: #800080;">1</span> <span style="color: #800080;">2014</span> atlassian-extras-legacy-<span style="color: #800080;">3.2</span><span style="color: #000000;">.jar
[root@conflunce lib]# </span><span style="color: #0000ff;">rm</span> -fr atlassian-extra*</pre>
  </div>
</div>

解压修改包，然后把里面的&nbsp;<span class="cnblogs_code">atlassian-extras-<span style="color: #800080;">3.2</span>.jar、Confluence-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">6</span>-language-pack-zh_CN.jar、mysql-connector-java-<span style="color: #800080;">5.1</span>.<span style="color: #800080;">39</span>-bin.jar</span>&nbsp; 将三个jar文件复制到/opt/atlassian/confluence/confluence/WEB-INF/lib目录下

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">wget</span> http:<span style="color: #008000;">//</span><span style="color: #008000;">15323800.144.unicom.data.tv002.com:443/down/36077cbf0624ef69db7b6416be45dbcf-1924995/confluence5.6.6%20crack.zip?cts=ot-f-D116A117A134A73Fc448e&ctp=116A117A134A73&ctt=1511507163&limit=1&spd=100000&ctk=0c1f445e181194c024eaeaa2a268a3c2&chk=36077cbf0624ef69db7b6416be45dbcf-1924995</span>
<span style="color: #0000ff;">unzip</span> confluence5.<span style="color: #800080;">6.6</span>\ crack.<span style="color: #0000ff;">zip</span><span style="color: #000000;">
cd  confluence5.</span><span style="color: #800080;">6.6</span>-crack/<span style="color: #000000;">jar
</span><span style="color: #0000ff;">cp</span>  .<span style="color: #008000;">/*</span><span style="color: #008000;"> /opt/atlassian/confluence/confluence/WEB-INF/lib/</span></pre>
  </div>
  
  <p class="a1">
    　　其中atlassian-extras-3.2.jar文件是和license相关的，&nbsp;<span class="cnblogs_code">Confluence-<span style="color: #800080;">5.6</span>.<span style="color: #800080;">6</span>-language-pack-zh_CN.jar</span>&nbsp;是confluence中文语言包，而&nbsp;<span class="cnblogs_code">mysql-connector-java-<span style="color: #800080;">5.1</span>.<span style="color: #800080;">39</span>-bin.jar</span>&nbsp;是confluence连接mysql数据库相关的jar包。
  </p>
</div>

**再次说明下：**

　　atlassian所有产品的中文语言包，我们都可以通过以下地址下载到：

<div>
  <div class="cnblogs_code">
    <pre>https:<span style="color: #008000;">//</span><span style="color: #008000;">translations.atlassian.com/dashboard/download?lang=zh_CN#/Confluence/5.6.6</span></pre>
  </div>
  
  <p>
    　　而mysql-connector-java-5.1.39-bin.jar文件可以连接mysql5.7及其以下的mysql版本，可以参考如下连接：
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>http:<span style="color: #008000;">//</span><span style="color: #008000;">www.w3resource.com/mysql/mysql-java-connection.php</span></pre>
  </div>
</div>

<p style="text-align: center;" align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133555662-21501114.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />
</p>

　　最后要启动confluence

<div>
  <div class="cnblogs_code">
    <pre>[root@conflunce ~]# /etc/init.d/confluence start</pre>
  </div>
</div>

### <span id="143_windowsconfluence_keygenjar">1.4.3 在windows上运行confluence_keygen.jar</span>

注意windows上需要安装jdk运行环境。

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133625225-2007420799.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />
</p>

&nbsp;&nbsp; serverID 要填写web界面上的

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133634350-1810051463.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />
</p>

&nbsp;&nbsp; 将生成的key复制带web界面即可

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133639334-419402100.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />&nbsp;
</p>

## <span id="15">1.5 配置数据库</span>

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133650537-1002578091.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />&nbsp;
</p>

选择direct JDBC

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133658475-474305535.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />
</p>

输入数据库用户密码

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133704693-1631623087.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />&nbsp;
</p>

数据库初始化完毕后，会跳转到如下界面

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133711943-465030399.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />&nbsp;
</p>

配置confluence的管理员账号和密码

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133718615-1887355473.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />&nbsp;
</p>

输入管理员信息

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133724803-1355588166.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />&nbsp;
</p>

安装完成

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133731896-1136071482.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />
</p>

安装完成后的界面

<p style="text-align: center;">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171223133742006-767354610.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Confluence 平台部署记录" alt="" />
</p>

　　<span style="background-color: #ffff00;">到此Confluence就安装完成了。</span>

## <span id="16">1.6 参考文档</span>

<div class="cnblogs_code">
  <pre>https:<span style="color: #008000;">//</span><span style="color: #008000;">www.ilanni.com/?p=11989#</span>
https:<span style="color: #008000;">//</span><span style="color: #008000;">baike.baidu.com/item/confluence/452961?fr=aladdin</span></pre>
</div>

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11_Confluence">1.1 Confluence简介</a><ul>
        <li>
          <a href="#111">1.1.1 使用情况</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12">1.2 环境准备</a><ul>
        <li>
          <a href="#121">1.2.1 系统环境说明</a>
        </li>
        <li>
          <a href="#122">1.2.2 软件环境说明</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_confluence">1.3 下载confluence</a>
    </li>
    <li>
      <a href="#14_confluence">1.4 安装confluence</a><ul>
        <li>
          <a href="#141">1.4.1 安装</a>
        </li>
        <li>
          <a href="#142">1.4.2 修改程序</a>
        </li>
        <li>
          <a href="#143_windowsconfluence_keygenjar">1.4.3 在windows上运行confluence_keygen.jar</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15">1.5 配置数据库</a>
    </li>
    <li>
      <a href="#16">1.6 参考文档</a>
    </li>
  </ul>
</div>