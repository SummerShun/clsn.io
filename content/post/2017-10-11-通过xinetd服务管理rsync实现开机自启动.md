---
title: 通过xinetd服务管理 rsync 实现开机自启动
author: 惨绿少年
type: post
date: 2017-10-10T18:44:00+00:00
url: /clsn/lx929.html
Baidusubmit:
  - 1
views:
  - 92
categories:
  - Linux运维
  - 玩转Linux
  - 运维基本功

---
## <span id="11_xinetd">1.1 xinetd<span style="font-family: '微软雅黑',sans-serif;">服务配置</span></span>

### <span id="111_xinetd">1.1.1 <span style="font-family: '微软雅黑',sans-serif;">检查</span>xinetd<span style="font-family: '微软雅黑',sans-serif;">服务是否安装</span></span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -qa xinetd</span>
[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -ql xinetd</span>
package xinetd <span style="color: #0000ff;">is</span> <span style="color: #0000ff;">not</span> installed</pre>
</div>

### <span id="112_xinetd">1.1.2 <span style="font-family: '微软雅黑',sans-serif;">安装</span>xinetd<span style="font-family: '微软雅黑',sans-serif;">服务</span></span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> yum install xinetd -y</span>
[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> rpm -qa xinetd</span>
xinetd-2.3.14-40.el6.x86_64</pre>
</div>

## <span id="12_etcxinetddrsyncxinetd">1.2 <span style="font-family: '微软雅黑',sans-serif;">修改</span>/etc/xinetd.d/rsync<span style="font-family: '微软雅黑',sans-serif;">文件，使其随</span>xinetd<span style="font-family: '微软雅黑',sans-serif;">启动而启动</span></span>

<div class="cnblogs_code">
  <pre>vim /etc/xinetd.d/<span style="color: #000000;">rsync
......将disable </span>= yes 修改为 disable =<span style="color: #000000;"> no
disable </span>=<span style="color: #000000;"> no
使用命令修改（centos6.x）
sed </span>-i  <span style="color: #800000;">'</span><span style="color: #800000;">s#yes#no#g</span><span style="color: #800000;">'</span> /etc/xinetd.d/rsync</pre>
</div>

## <span id="13_873xinetd">1.3 <span style="font-family: '微软雅黑',sans-serif;">重启系统发现</span>873<span style="font-family: '微软雅黑',sans-serif;">端口交由</span>xinetd<span style="font-family: '微软雅黑',sans-serif;">管理</span></span>

<div class="cnblogs_code">
  <pre>[root@backup ~]<span style="color: #008000;">#</span><span style="color: #008000;"> netstat -lntup |grep 873</span>
tcp        0      0 :::873                      :::*                        LISTEN      1229/xinetd   </pre>
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11_xinetd">1.1 xinetd服务配置</a><ul>
        <li>
          <a href="#111_xinetd">1.1.1 检查xinetd服务是否安装</a>
        </li>
        <li>
          <a href="#112_xinetd">1.1.2 安装xinetd服务</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12_etcxinetddrsyncxinetd">1.2 修改/etc/xinetd.d/rsync文件，使其随xinetd启动而启动</a>
    </li>
    <li>
      <a href="#13_873xinetd">1.3 重启系统发现873端口交由xinetd管理</a>
    </li>
  </ul>
</div>