---
title: memcached 缓存数据库应用实践
author: 惨绿少年
type: post
date: 2017-12-07T00:03:00+00:00
url: /clsn/lx509.html
Baidusubmit:
  - 1
views:
  - 146
categories:
  - Linux运维
  - NoSQL
  - 数据库
  - 运维基本功

---
## <span id="11">1.1 数据库对比</span>

**缓存：** 将数据存储到内存中，只有当磁盘胜任不了的时候，才会启用缓存

&nbsp;&nbsp;　　&nbsp; 缺点：断电数据丢失(双电)，用缓存存储数据的目的只是为了应付大并发的业务。

**数据库：** mysql(关系型数据库，能够保证数据一致性，保证数据不丢失，当因为功能太多，导致性能不高) ===数据参考

**缓存数据库：** &nbsp;memcache redis(非关系型数据库，性能极高，但不保证数据完整性) === 业务的数据提供者

**&nbsp;&nbsp;&nbsp;&nbsp;　　　　&nbsp;&nbsp; &nbsp;&nbsp;**memcachedb 会将内存的数据写入到磁盘中

　　　　　&nbsp; 　redis 主要工作场所是内存中，但是定期备份内存数据到硬盘

### <span id="111">1.1.1 数据库的选择</span>

　　数据存储，数据仓库选择mysql这种磁盘的数据库

　　高并发，业务大的应用选择memcache这种内存数据库

### <span id="112">1.1.2 数据库分类</span>

　　关系型数据库&nbsp; mysql

　　非关系型数据库（NOSQL） memcached redis MongoDB

## <span id="12_memcached">1.2 memcached介绍</span>

&nbsp; &nbsp;&nbsp;&nbsp; Memcached是一款开源的、高性能的纯内存缓存服务软件。Mem是内存的意思，cache是缓存的意思，d是daemon的意思。

　　memcache 是项目名称，也是一款软件，其架构是<span style="background-color: #99cc00;"><strong>C/S</strong><strong>架构</strong>。</span>

&nbsp; &nbsp; &nbsp; &nbsp;memcached官网：http://memcached.org/

### <span id="121_memcache">1.2.1 memcache优点</span>

①&nbsp;&nbsp; 对于用户来讲，用户访问网站更快了，体验更好了。

②对网站来说，数据库压力降低了。只有当内存没有数据时才会去请求数据库。第一次写入的数据也会请求数据库。一般公司没有预热，只有当用户读取过数据库才会放到Memcached中。

②&nbsp;&nbsp; 提升了网站的并发访问，减少服务器数量。

## <span id="13_Memcached">1.3 Memcached在企业中使用场景</span>

### <span id="131">1.3.1 作为数据库的前端缓存应用</span>

　　　当数据库（mysql）承受不了大并发的请求时，可以将数据缓存到内存中（缓存数据库），然后就可以解决

&nbsp;&nbsp; &nbsp;　　作为数据库的前端缓存最大目的：减少数据库被大量访问的压力

### <span id="132_session">1.3.2 作为集群后端的session会话保持</span>

&nbsp; &nbsp; 　&nbsp; session存储在文件，数据库，memcache，或内存等的服务端上，

&nbsp;&nbsp;　　 cookie&nbsp; 存放在客户端浏览器上。

&nbsp;&nbsp;　　 session是一个存在服务器上的类似于一个散列表格的文件。里面存有我们需要的信息，在我们需要用的时候可以从里面取出来。

**　　　session****依赖cookie****存在**，请求客户端到达服务端后，服务端会随机生成一个字符串，作为该用户的标识，该字符串通过cookie返回给客户端，客户端浏览器会以该字符串为key放到session id里面，随机字符串的key里面可以先没有值。如果用户再次提交，请求信息中的用户名密码等用户信息保存在随机字符串的value中，请求到达服务端，用户名密码正确，随机字符串会被授权，提一个标记给到sessionid中的随机字符串的value中，证明该用户已经是登录状态，客户端再次带着该随机字符串访问服务端，服务端会知道该用户已经登录不需验证，直接返回请求的信息。

**session****和cookie****区别**

　　1、cookie数据存放在用户的浏览器上，session数据存储在服务器上

　　2、cookie在本地的浏览器中，可以被提取分析，安全性差。为了安全，登录账户等信息可以缓存在session中。

　　3、session会在一定时间内保存在服务器上，访问量增大会给服务器带来压力，可以使用缓存工具，如memcache等

### <span id="133">1.3.3 网站开发如何判断用户信息</span>

　　最开始的技术方法：服务器在你的浏览器中写一个cookies，这个cookies就包含了你的用户名及登录信息。因为cookies是存储在本地浏览器中，所以第三方工具很容易盗取cookies信息。

<span style="background-color: #99cc00;"><strong>最开始：</strong></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; cookies&nbsp;&nbsp; cookies名字：内容（用户名，登录信息）

<span style="background-color: #99cc00;"><strong>改进后：</strong></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 本地浏览器存放：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; cookies&nbsp;&nbsp; cookies名字：内容（session id 编号）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 服务器存放：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; session&nbsp;&nbsp; session id：内容（用户名，登录信息）

<span style="background-color: #99cc00;"><strong>主流使用场景：cookies + session</strong></span>

### <span id="134_session">1.3.4 session共享的不同解决方案</span>

&nbsp;&nbsp;　　 1、session文件提供NFS共享

&nbsp;　　&nbsp; 2、session文件提供rsync&nbsp; scp共享

&nbsp;&nbsp;　　 3、将session的内容存放在数据库（mysql）中，所有的机器都可以通过ip：port读取

　&nbsp; &nbsp;　4、将session的内容存放在缓存数据库中，所有的机器都可以通过ip：port读取

&nbsp;&nbsp;　　 好处：利用断电、重启丢失数据的特性。定时清理数据；提高并发

### <span id="135_memcache">1.3.5 memcache原理优点</span>

　　启动Memcached吋，根据指定的内存大小参数，会被分配一个内存个间。当我们读取数据库的各类业务数据后，数据会同吋放入Memcached缓存中，，下一次用户请求同样的数据，程序直接去Memcached取数据返回给用户。

&nbsp;优点：

　　①&nbsp;&nbsp; &nbsp;对于用户来讲，用户访问网站更快了，体验更好了。#

　　②&nbsp;&nbsp; 对网站来说，数据库压力降低了。只有当内存没有数据时才会去请求数据库。第一次写入的数据 也会请求数据库。一般公司没有预热，只有，用户读取过数据库才会放到Memcached中。

　　③&nbsp;&nbsp; 提升了网站的并发访问，减少服务器数最。

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207155429206-1734759516.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />
</p>

<p align="center">
  <strong>原理图</strong>
</p>

## <span id="14_Memcached">1.4 Memcached分布式缓存集群</span>

　　memcached天生不支持分布式集群,需要通过程序支持分布式存储

### <span id="141_Memcached">1.4.1 Memcached分布式缓存集群的特点</span>

&nbsp;&nbsp;　　 1. 所有MC服务器内存的内容都是不一样的。这些服务器内容加起来接近数据库的容量。比如1T的数据库，一台缓存数据库的内存没有那么大，因此分成10台缓存服务器。

&nbsp;　　&nbsp; 2. 通过在客户端(Web)程序或者MC的负载均衡器上用HASH算法，让同一内容都分配到一个MC服务器。

　　&nbsp;&nbsp; 3. 普通的HASH算法对于节点宕机会带来大量的数据流动(失效)，可能会引起雪崩效应。

&nbsp;　　&nbsp; 4. 一致性HASH可以让节点宕机对节点的数据流动(失效)降到最低。

**_普通的hash_****_算法_**

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207155447738-175574885.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />
</p>

　　首先将key处理为一个32位字符串，取前8位，在经过hash计算处理成整数并返回，然后映射到其中一台服务器这样得到其中一台服务器的配置，利用这个配置完成分布式部署。在服务器数量不发生变化的情况下，普通hash分布可以很好的运作，当服务器的数量发生变化，问题就来了。试想，增加一台服务器，同一个key经过hash之后，与服务器取模的结果和没增加之前的结果肯定不一样，这就导致了，之前保存的数据丢失。

<span style="background-color: #99cc00;"><strong><em>一致性hash</em></strong><strong><em>算法</em></strong></span>

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207155459363-1160784505.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />&nbsp;
</p>

一致性哈希算法

　　优点：在分布式的cache缓存中，其中一台宕机，迁移key效率最高

　　将服务器列表进行排序，根据mHash($key) 匹配相邻服务器

**一致性hash****算法&nbsp;** **将数据流动降到最低**

<div>
  <p class="a">
    参考资料
  </p>
  
  <div class="cnblogs_code">
    <pre>http:<span style="color: #008000;">//</span><span style="color: #008000;">blog.csdn.net/cywosp/article/details/23397179</span>
http:<span style="color: #008000;">//</span><span style="color: #008000;">blog.csdn.net/zhangskd/article/details/50256111</span></pre>
  </div>
</div>

# <span id="2_memcached">第2章 memcached使用</span>

## <span id="21_memcached">2.1 安装memcached</span>

### <span id="211">2.1.1 环境说明</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# cat /etc/redhat-<span style="color: #000000;">release
CentOS Linux release </span><span style="color: #800080;">7.4</span>.<span style="color: #800080;">1708</span><span style="color: #000000;"> (Core) 
[root@cache01 </span>~]# uname -<span style="color: #000000;">r
</span><span style="color: #800080;">3.10</span>.<span style="color: #800080;"></span>-<span style="color: #800080;">693</span><span style="color: #000000;">.el7.x86_64
[root@cache01 </span>~<span style="color: #000000;">]# getenforce
Disabled
[root@cache01 </span>~<span style="color: #000000;">]# systemctl status firewalld.service 
● firewalld.service </span>- firewalld - <span style="color: #0000ff;">dynamic</span><span style="color: #000000;"> firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(</span><span style="color: #800080;">1</span><span style="color: #000000;">)
[root@cache01 </span>~]# hostname -<span style="color: #000000;">I
</span><span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">172.16</span>.<span style="color: #800080;">1.21</span></pre>
  </div>
</div>

### <span id="212_memcached">2.1.2 安装memcached</span>

<div class="cnblogs_code">
  <pre>[root@cache01 ~]# yum -y install memcached</pre>
</div>

### <span id="213">2.1.3 查看配置</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# cat /etc/sysconfig/<span style="color: #000000;">memcached 
PORT</span>=<span style="color: #800000;">"</span><span style="color: #800000;">11211</span><span style="color: #800000;">"</span><span style="color: #000000;">
USER</span>=<span style="color: #800000;">"</span><span style="color: #800000;">memcached</span><span style="color: #800000;">"</span><span style="color: #000000;">
MAXCONN</span>=<span style="color: #800000;">"</span><span style="color: #800000;">1024</span><span style="color: #800000;">"</span><span style="color: #000000;">
CACHESIZE</span>=<span style="color: #800000;">"</span><span style="color: #800000;">64</span><span style="color: #800000;">"</span><span style="color: #000000;">
OPTIONS</span>=<span style="color: #800000;">""</span></pre>
  </div>
</div>

### <span id="214">2.1.4 查看启动脚本</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# cat /usr/lib/systemd/system/<span style="color: #000000;">memcached.service 
[Unit]
Description</span>=<span style="color: #000000;">Memcached 
Before</span>=<span style="color: #000000;">httpd.service
After</span>=<span style="color: #000000;">network.target

[Service]
Type</span>=<span style="color: #000000;">simple
EnvironmentFile</span>=-/etc/sysconfig/<span style="color: #000000;">memcached
ExecStart</span>=/usr/bin/memcached -u $USER -p $PORT -m $CACHESIZE -<span style="color: #000000;">c $MAXCONN $OPTIONS

[Install]
WantedBy</span>=multi-user.target</pre>
  </div>
</div>

### <span id="215">2.1.5 启动服务</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# systemctl start memcached.service</pre>
  </div>
</div>

## <span id="22_memcached">2.2 管理memcached</span>

### <span id="221_memcached">2.2.1 memcached数据库语法格式</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">set</span>              key   <span style="color: #800080;"></span>        <span style="color: #800080;"></span>           <span style="color: #800080;">10</span>

&lt;command name>  &lt;key> &lt;flags> &lt;exptime> &lt;bytes>\r\n</pre>
  </div>
</div>

&nbsp; \n 换行且光标移至行首

&nbsp; \r 光标移至行首，但不换行

<div align="center">
  <div align="center">
    <table class="MsoTable15Grid4Accent5" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
      <tr>
        <td style="width: 127.35pt; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #4bacc6; border-bottom-color: #4bacc6; border-left-color: #4bacc6; border-right: none; background: #4bacc6; padding: 0cm 5.4pt;" width="170">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 5;">
            <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">参数</span></strong>
          </p>
        </td>
        
        <td style="width: 395.45pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #4bacc6; border-right-color: #4bacc6; border-bottom-color: #4bacc6; border-left: none; background: #4bacc6; padding: 0cm 5.4pt;" width="527">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 1;">
            <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">说明</span></strong>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 127.35pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #92cddc; border-bottom-color: #92cddc; border-left-color: #92cddc; border-top: none; background: #daeef3; padding: 0cm 5.4pt;" width="170">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
            <strong><span lang="EN-US"><flags></span></strong>
          </p>
        </td>
        
        <td style="width: 395.45pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #92cddc; border-right-width: 1pt; border-right-color: #92cddc; background: #daeef3; padding: 0cm 5.4pt;" width="527">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
            <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">是在取回内容时，与数据和发送块一同保存服务器上的任意</span><span lang="EN-US">16</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">位无符号整形（用十进制来书写）。客户端可以用它作为&ldquo;位域&rdquo;来存储一些特定的信息；它对服务器是不透明的。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 127.35pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #92cddc; border-bottom-color: #92cddc; border-left-color: #92cddc; border-top: none; padding: 0cm 5.4pt;" width="170">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
            <strong><span lang="EN-US"><exptime></span></strong>
          </p>
        </td>
        
        <td style="width: 395.45pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #92cddc; border-right-width: 1pt; border-right-color: #92cddc; padding: 0cm 5.4pt;" width="527">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
            <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">是终止时间。如果为</span><span lang="EN-US"></span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">，该项永不过期</span><span lang="EN-US">(</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">虽然它可能被删除，以便为其他缓存项目腾出位置</span><span lang="EN-US">)</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">。如果非</span><span lang="EN-US"></span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">（</span><span lang="EN-US">Unix</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">时间戳或当前时刻的秒偏移），到达终止时间后，客户端无法再获得这项内容</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 127.35pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #92cddc; border-bottom-color: #92cddc; border-left-color: #92cddc; border-top: none; background: #daeef3; padding: 0cm 5.4pt;" width="170">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 68;">
            <strong><span lang="EN-US"><bytes></span></strong>
          </p>
        </td>
        
        <td style="width: 395.45pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #92cddc; border-right-width: 1pt; border-right-color: #92cddc; background: #daeef3; padding: 0cm 5.4pt;" width="527">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 64;">
            <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">是随后的数据区块的字节长度，不包括用于分页的&ldquo;</span><span lang="EN-US">\r\n</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">&rdquo;。它可以是</span><span lang="EN-US"></span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">（这时后面跟随一个空的数据区块）。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 127.35pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #92cddc; border-bottom-color: #92cddc; border-left-color: #92cddc; border-top: none; padding: 0cm 5.4pt;" width="170">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph; mso-yfti-cnfc: 4;">
            <strong><span lang="EN-US"><data block>\r\n</span></strong>
          </p>
        </td>
        
        <td style="width: 395.45pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #92cddc; border-right-width: 1pt; border-right-color: #92cddc; padding: 0cm 5.4pt;" width="527">
          <p class="MsoNormal" style="text-align: justify; text-justify: inter-ideograph;">
            <span lang="EN-US"><data block> </span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">是大段的</span><span lang="EN-US">8</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">位数据，其长度由前面的命令行中的</span><span lang="EN-US"><bytes></span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">指定。</span>
          </p>
        </td>
      </tr>
    </table>
  </div>
</div>

### <span id="222">2.2.2 数据库使用</span>

写入读取数据

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">set key008 0 0 10\r\noldboy0987\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
STORED
[root@cache01 </span>~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">get key008\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
VALUE key008 </span><span style="color: #800080;"></span> <span style="color: #800080;">10</span><span style="color: #000000;">
oldboy0987
END</span></pre>
  </div>
</div>

写入数据长度不符合，定义过大

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">set key009 0 0 11\r\noldboy0987\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
[root@cache01 </span>~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">get key009\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
END</span></pre>
  </div>
</div>

写入数据长度不符合，定义过小

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">set key010 0 0 9\r\noldboy0987\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
CLIENT_ERROR bad data chunk
ERROR
[root@cache01 </span>~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">get key010\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
END</span></pre>
  </div>
</div>

时效性

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">set key011 0 10 10\r\noldboy0987\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
STORED
[root@cache01 </span>~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">get key011\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
VALUE key011 </span><span style="color: #800080;"></span> <span style="color: #800080;">10</span><span style="color: #000000;">
oldboy0987
END
[root@cache01 </span>~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">get key011\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
END</span></pre>
  </div>
</div>

删除数据

<div>
  <div class="cnblogs_code">
    <pre>[root@cache01 ~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">delete key008\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
DELETED
[root@cache01 </span>~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">get key008\r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;">
END</span></pre>
  </div>
</div>

## <span id="23_memcache_php">2.3 memcache php版本客户端安装使用</span>

_<span style="text-decoration: underline;">命令集</span>_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">#编译进去php_mem
tar zxvf memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span><span style="color: #000000;">.tgz
cd memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span>
/application/php/bin/<span style="color: #000000;">phpize
.</span>/configure --enable-memcache --with-php-config=/application/php/bin/php-config --with-zlib-<span style="color: #000000;">dir
make
make install
# 激活php_memcached
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">$a extension=memcache.so</span><span style="color: #800000;">'</span> /application/php/lib/<span style="color: #000000;">php.ini
pkill php
</span>/application/php/sbin/php-fpm -<span style="color: #000000;">t
</span>/application/php/sbin/php-<span style="color: #000000;">fpm
</span>/application/php/bin/php -m|grep memcache</pre>
  </div>
</div>

检查当前环境

<div>
  <p class="a1">
    查看php的模块
  </p>
  
  <div class="cnblogs_code" onclick="cnblogs_code_show('333e383e-76e0-4e18-8635-8bcec6402e58')">
    <img id="code_img_closed_333e383e-76e0-4e18-8635-8bcec6402e58" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-12.gif" alt="memcached 缓存数据库应用实践" alt="" /><img id="code_img_opened_333e383e-76e0-4e18-8635-8bcec6402e58" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('333e383e-76e0-4e18-8635-8bcec6402e58',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-12.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" /></p> 
    
    <div id="cnblogs_code_open_333e383e-76e0-4e18-8635-8bcec6402e58" class="cnblogs_code_hide">
      <pre><span style="color: #008080;"> 1</span> <span style="color: #000000;">查看php的模块
</span><span style="color: #008080;"> 2</span> [root@web06 ~]# /application/php/bin/php -<span style="color: #000000;">m
</span><span style="color: #008080;"> 3</span> <span style="color: #000000;">[PHP Modules]
</span><span style="color: #008080;"> 4</span> <span style="color: #000000;">bcmath
</span><span style="color: #008080;"> 5</span> <span style="color: #000000;">Core
</span><span style="color: #008080;"> 6</span> <span style="color: #000000;">ctype
</span><span style="color: #008080;"> 7</span> <span style="color: #000000;">curl
</span><span style="color: #008080;"> 8</span> <span style="color: #000000;">date
</span><span style="color: #008080;"> 9</span> <span style="color: #000000;">dom
</span><span style="color: #008080;">10</span> <span style="color: #000000;">ereg
</span><span style="color: #008080;">11</span> <span style="color: #000000;">fileinfo
</span><span style="color: #008080;">12</span> <span style="color: #000000;">filter
</span><span style="color: #008080;">13</span> <span style="color: #000000;">ftp
</span><span style="color: #008080;">14</span> <span style="color: #000000;">gd
</span><span style="color: #008080;">15</span> <span style="color: #000000;">hash
</span><span style="color: #008080;">16</span> <span style="color: #000000;">iconv
</span><span style="color: #008080;">17</span> <span style="color: #000000;">json
</span><span style="color: #008080;">18</span> <span style="color: #000000;">libxml
</span><span style="color: #008080;">19</span> <span style="color: #000000;">mbstring
</span><span style="color: #008080;">20</span> <span style="color: #000000;">mcrypt
</span><span style="color: #008080;">21</span> <span style="color: #000000;">mhash
</span><span style="color: #008080;">22</span> <span style="color: #000000;">mysql
</span><span style="color: #008080;">23</span> <span style="color: #000000;">mysqlnd
</span><span style="color: #008080;">24</span> <span style="color: #000000;">openssl
</span><span style="color: #008080;">25</span> <span style="color: #000000;">pcntl
</span><span style="color: #008080;">26</span> <span style="color: #000000;">pcre
</span><span style="color: #008080;">27</span> <span style="color: #000000;">PDO
</span><span style="color: #008080;">28</span> <span style="color: #000000;">pdo_mysql
</span><span style="color: #008080;">29</span> <span style="color: #000000;">pdo_sqlite
</span><span style="color: #008080;">30</span> <span style="color: #000000;">Phar
</span><span style="color: #008080;">31</span> <span style="color: #000000;">posix
</span><span style="color: #008080;">32</span> <span style="color: #000000;">Reflection
</span><span style="color: #008080;">33</span> <span style="color: #000000;">session
</span><span style="color: #008080;">34</span> <span style="color: #000000;">shmop
</span><span style="color: #008080;">35</span> <span style="color: #000000;">SimpleXML
</span><span style="color: #008080;">36</span> <span style="color: #000000;">soap
</span><span style="color: #008080;">37</span> <span style="color: #000000;">sockets
</span><span style="color: #008080;">38</span> <span style="color: #000000;">SPL
</span><span style="color: #008080;">39</span> <span style="color: #000000;">sqlite3
</span><span style="color: #008080;">40</span> <span style="color: #000000;">standard
</span><span style="color: #008080;">41</span> <span style="color: #000000;">sysvsem
</span><span style="color: #008080;">42</span> <span style="color: #000000;">tokenizer
</span><span style="color: #008080;">43</span> <span style="color: #000000;">xml
</span><span style="color: #008080;">44</span> <span style="color: #000000;">xmlreader
</span><span style="color: #008080;">45</span> <span style="color: #000000;">xmlrpc
</span><span style="color: #008080;">46</span> <span style="color: #000000;">xmlwriter
</span><span style="color: #008080;">47</span> <span style="color: #000000;">xsl
</span><span style="color: #008080;">48</span> <span style="color: #000000;">zlib
</span><span style="color: #008080;">49</span> 
<span style="color: #008080;">50</span> [Zend Modules]</pre>
    </div>
    
    <p>
      <span class="cnblogs_code_collapse">View Code 查看php的模块</span></div> </div> 
      
      <p>
        <strong><em><span style="text-decoration: underline;">执行过程</span></em></strong>
      </p>
      
      <p>
        编译安装
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>[root@web06 memcache-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span><span style="color: #000000;">]# make install
Installing shared extensions:     </span>/application/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span>/lib/php/extensions/no-debug-non-zts-<span style="color: #800080;">20121212</span>/<span style="color: #000000;">
[root@web06 memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span>]# ls /application/php/lib/php/extensions/no-debug-non-zts-<span style="color: #800080;">20121212</span>/<span style="color: #000000;">
memcache.so
[root@web06 memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span>]# sed -i <span style="color: #800000;">'</span><span style="color: #800000;">$a extension=memcache.so</span><span style="color: #800000;">'</span> /application/php/lib/<span style="color: #000000;">php.ini
[root@web06 memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span><span style="color: #000000;">]# pkill php
[root@web06 memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span>]# /application/php/sbin/php-fpm -<span style="color: #000000;">t
[</span><span style="color: #800080;">17</span>-Nov-<span style="color: #800080;">2017</span> <span style="color: #800080;">11</span>:<span style="color: #800080;">39</span>:<span style="color: #800080;">13</span>] NOTICE: configuration file /application/php-<span style="color: #800080;">5.5</span>.<span style="color: #800080;">32</span>/etc/php-fpm.conf test <span style="color: #0000ff;">is</span><span style="color: #000000;"> successful

[root@web06 memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span>]# /application/php/sbin/php-<span style="color: #000000;">fpm
[root@web06 memcache</span>-<span style="color: #800080;">2.2</span>.<span style="color: #800080;">5</span>]# /application/php/bin/php -m|<span style="color: #000000;">grep memcache
memcache</span></pre>
        </div>
      </div>
      
      <h3>
        <span id="231">2.3.1 编写测试文件</span>
      </h3>
      
      <div>
        <div class="cnblogs_code">
          <pre>[root@web01 blog]# cat /application/nginx/html/blog/<span style="color: #000000;">mc.php
</span><?<span style="color: #000000;">php
    $memcache &lt;/span>= 

<span style="color: #0000ff;">new</span><span style="color: #000000;"> Memcache;
    $memcache</span>->connect(<span style="color: #800000;">'</span><span style="color: #800000;">10.0.0.21</span><span style="color: #800000;">'</span>, <span style="color: #800080;">11211</span>) or die (<span style="color: #800000;">"</span><span style="color: #800000;">Could not connect</span><span style="color: #800000;">"</span><span style="color: #000000;">);
    $memcache</span>-><span style="color: #0000ff;">set</span>(<span style="color: #800000;">'</span><span style="color: #800000;">key20171117</span><span style="color: #800000;">'</span>, <span style="color: #800000;">'</span><span style="color: #800000;">hello,world</span><span style="color: #800000;">'</span><span style="color: #000000;">);
    $get_value </span>= $memcache-><span style="color: #0000ff;">get</span>(<span style="color: #800000;">'</span><span style="color: #800000;">key20171117</span><span style="color: #800000;">'</span><span style="color: #000000;">);
    echo $get_value;
</span>?></pre>
        </div>
      </div>
      
      <p>
        浏览器访问
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207155911878-447894378.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />&nbsp;
      </p>
      
      <p>
        数据库读取测试
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>[root@cache01 ~]# printf <span style="color: #800000;">"</span><span style="color: #800000;">get key20171117 \r\n</span><span style="color: #800000;">"</span>|nc <span style="color: #800080;">10.0</span>.<span style="color: #800080;">0.21</span> <span style="color: #800080;">11211</span><span style="color: #000000;"> 
VALUE key20171117 </span><span style="color: #800080;"></span> <span style="color: #800080;">11</span><span style="color: #000000;">
hello,world
END</span></pre>
        </div>
      </div>
      
      <h2>
        <span id="24_webmemcached">2.4 web管理memcached</span>
      </h2>
      
      <p>
        　　使用的软件memadmin
      </p>
      
      <p>
        　　官网：http://www.junopen.com/memadmin/
      </p>
      
      <p>
        将程序包放如站点目录，浏览器进行访问即可
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>[root@web06 tools]# tar xf memadmin-<span style="color: #800080;">1.0</span>.<span style="color: #800080;">12</span>.tar.gz -C /application/nginx/html/blog/</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp; 默认用户名密码为admin
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207160010097-70003361.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />&nbsp;
      </p>
      
      <p>
        &nbsp;&nbsp; 添加一个新的memcached服务器
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207160024222-645669662.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />&nbsp;
      </p>
      
      <p>
        &nbsp;&nbsp; web界面管理全中文，较为简单
      </p>
      
      <p align="center">
        &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207160031722-1722233913.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />
      </p>
      
      <h2>
        <span id="25_memcached">2.5 memcached数据缓存</span>
      </h2>
      
      <p>
        通过程序实现
      </p>
      
      <h3>
        <span id="251_blogmemcached">2.5.1 blog站点实现memcached存储</span>
      </h3>
      
      <div>
        <div class="cnblogs_code">
          <pre>[root@web06 ~]# cat /application/nginx/html/blog/wp-content/<span style="color: #0000ff;">object</span>-cache.php</pre>
        </div>
      </div>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171207160048659-177631846.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="memcached 缓存数据库应用实践" alt="" />&nbsp;
      </p>
      
      <h2>
        <span id="26_memcached_session">2.6 memcached session共享</span>
      </h2>
      
      <p>
        方法1：
      </p>
      
      <p>
        　　通过程序实现，web01只需要往memcahce写session，web02从memcahce读session<strong>（更具有通用性）</strong>
      </p>
      
      <p>
        方法2：
      </p>
      
      <p>
        　　通过php的配置文件，让php默认将session存储在文件中，修改为存储在memcached中
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>sed -i <span style="color: #800000;">'</span><span style="color: #800000;">s#session.save_handler = files#session.save_handler = memcache#;$a session.save_path = "tcp://10.0.0.21:11211"</span><span style="color: #800000;">'</span> /application/php/lib/php.ini</pre>
        </div>
        
        <p>
          　　使用这个功能，需要使用php的session函数
        </p>
        
        <p style="text-align: right;">
          &nbsp;&nbsp;<strong>&nbsp;本文出自&ldquo;惨绿少年&rdquo;，欢迎转载，转载请注明出处！http://blog.znix.top</strong>
        </p>
      </div>
      
      <div id="toc_container" class="toc_white have_bullets">
        <ul class="toc_list">
          <ul>
            <li>
              <a href="#11">1.1 数据库对比</a><ul>
                <li>
                  <a href="#111">1.1.1 数据库的选择</a>
                </li>
                <li>
                  <a href="#112">1.1.2 数据库分类</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#12_memcached">1.2 memcached介绍</a><ul>
                <li>
                  <a href="#121_memcache">1.2.1 memcache优点</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#13_Memcached">1.3 Memcached在企业中使用场景</a><ul>
                <li>
                  <a href="#131">1.3.1 作为数据库的前端缓存应用</a>
                </li>
                <li>
                  <a href="#132_session">1.3.2 作为集群后端的session会话保持</a>
                </li>
                <li>
                  <a href="#133">1.3.3 网站开发如何判断用户信息</a>
                </li>
                <li>
                  <a href="#134_session">1.3.4 session共享的不同解决方案</a>
                </li>
                <li>
                  <a href="#135_memcache">1.3.5 memcache原理优点</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#14_Memcached">1.4 Memcached分布式缓存集群</a><ul>
                <li>
                  <a href="#141_Memcached">1.4.1 Memcached分布式缓存集群的特点</a>
                </li>
              </ul>
            </li>
          </ul></li>
          
          <li>
            <a href="#2_memcached">第2章 memcached使用</a><ul>
              <li>
                <a href="#21_memcached">2.1 安装memcached</a><ul>
                  <li>
                    <a href="#211">2.1.1 环境说明</a>
                  </li>
                  <li>
                    <a href="#212_memcached">2.1.2 安装memcached</a>
                  </li>
                  <li>
                    <a href="#213">2.1.3 查看配置</a>
                  </li>
                  <li>
                    <a href="#214">2.1.4 查看启动脚本</a>
                  </li>
                  <li>
                    <a href="#215">2.1.5 启动服务</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#22_memcached">2.2 管理memcached</a><ul>
                  <li>
                    <a href="#221_memcached">2.2.1 memcached数据库语法格式</a>
                  </li>
                  <li>
                    <a href="#222">2.2.2 数据库使用</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#23_memcache_php">2.3 memcache php版本客户端安装使用</a><ul>
                  <li>
                    <a href="#231">2.3.1 编写测试文件</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#24_webmemcached">2.4 web管理memcached</a>
              </li>
              <li>
                <a href="#25_memcached">2.5 memcached数据缓存</a><ul>
                  <li>
                    <a href="#251_blogmemcached">2.5.1 blog站点实现memcached存储</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#26_memcached_session">2.6 memcached session共享</a>
              </li>
            </ul>
          </li>
        </ul>
      </div>