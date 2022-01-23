---
title: yum中$releasever、 $basearch等变量含义
author: 惨绿少年
type: post
date: 2017-11-13T05:59:00+00:00
url: /clsn/lx808.html
Baidusubmit:
  - 1
views:
  - 106
zm_favorites:
  - 1
categories:
  - Linux运维

---
<div class="cnblogs_code">
  <pre>[root@kickstart ~]# rpm -qf /etc/redhat-<span style="color: #000000;">release 
centos</span>-release-<span style="color: #800080;">7</span>-<span style="color: #800080;">4.1708</span>.el7.centos.x86_64</pre>
</div>

<div>
  yum中的$releasever变量是取redhat-release-server rpm包的属性值(&nbsp;%{version})。
</div>

<div>
  <div>
    [root@ldap01 ~]# rpm -q --qf %{version} redhat-release-server;echo
  </div>
  
  <div>
    6Server
  </div>
</div>

<div class="cnblogs_code">
  <pre>[root@kickstart ~<span style="color: #000000;">]# arch
x86_64</span></pre>
</div>

参考&nbsp;

> http://blog.sina.com.cn/s/blog_69b59e3b0101lu12.html
> 
> http://blog.csdn.net/whatday/article/details/51097456