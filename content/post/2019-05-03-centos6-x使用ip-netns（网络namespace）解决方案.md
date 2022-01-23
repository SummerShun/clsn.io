---
title: CentOS6.x使用ip netns（网络namespace）解决方案
author: 惨绿少年
type: post
date: 2019-05-03T14:21:29+00:00
url: /clsn/lx1510.html
Baidusubmit:
  - 1
categories:
  - 运维基本功

---
<a name="c6018755"></a>

## <span id="1">1、前言</span>

CentOS6.5以前，内核不支持网络namespace，需要升级内核和iproute。  
CentOS6.5以后，内核已支持网络namespace，只需要升级iproute即可。  
<a name="19b6a59d"></a>

## <span id="21_-_iproute">2、升级方法1 - 升级iproute</span>

<pre><code class="language-bash line-numbers">[root@clsn.io ~]# cat /etc/redhat-release
CentOS release 6.8 (Final)

[root@clsn.io ~]# uname -r
2.6.32-573.el6.x86_64
</code></pre>

升级iproute方法

<pre><code class="line-numbers"># 安装yum源文件
yum install -y http://rdo.fedorapeople.org/rdo-release.rpm
# 修改文件内容
cat /etc/yum.repos.d/rdo-release.repo
[openstack-kilo]
name=OpenStack Kilo Repository
baseurl=https://repos.fedorapeople.org/repos/openstack/EOL/openstack-icehouse/epel-6/
skip_if_unavailable=0
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-RDO-kilo
安装iproute
[root@clsn.io ~]# yum install -y iproute
</code></pre>

<a name="38558b90"></a>

### <span id="32">3、升级方法2 - 升级内核</span>

升级方法参考：<https://www.yuque.com/zs.hou/blog/kernel-update>  
<a name="7baec7e9"></a>

## <span id="4">4、 参考文献</span>

> <https://www.cnblogs.com/puremans/p/6415505.html>  
> [http://www.jixuege.com/?id=42][1] 

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1">1、前言</a>
    </li>
    <li>
      <a href="#21_-_iproute">2、升级方法1 - 升级iproute</a><ul>
        <li>
          <a href="#32">3、升级方法2 - 升级内核</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#4">4、 参考文献</a>
    </li>
  </ul>
</div>

 [1]: /wp-content/themes/clsn-003/inc/go.php?url=http://www.jixuege.com/?id=42&utm_source=tuicool&utm_medium=referral