---
title: NFS启动时报错Linux NFS:could not open connection for tcp6
author: 惨绿少年
type: post
date: 2017-10-10T19:30:00+00:00
url: /clsn/lx928.html
Baidusubmit:
  - 1
views:
  - 101
categories:
  - Linux运维

---
# <span id="11">1.1 <span style="font-size: 14px; font-family: 微软雅黑, sans-serif;">启动时出现的错误</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;">/etc/init.d/nfs start</span>
<span style="color: #000000;">Shutting down NFS daemon:                                 [  OK  ]
Shutting down NFS mountd:                                  [  OK  ]
Shutting down NFS services:                                 [  OK  ]
Shutting down RPC idmapd:                                  [  OK  ]
Starting NFS services:                                           [  OK  ]
Starting NFS mountd: rpc.mountd: svc_tli_create: could </span><span style="color: #0000ff;">not</span> open connection <span style="color: #0000ff;">for</span><span style="color: #000000;"> udp6
rpc.mountd: svc_tli_create: could </span><span style="color: #0000ff;">not</span> open connection <span style="color: #0000ff;">for</span><span style="color: #000000;"> tcp6
rpc.mountd: svc_tli_create: could </span><span style="color: #0000ff;">not</span> open connection <span style="color: #0000ff;">for</span><span style="color: #000000;"> udp6
rpc.mountd: svc_tli_create: could </span><span style="color: #0000ff;">not</span> open connection <span style="color: #0000ff;">for</span><span style="color: #000000;"> tcp6
rpc.mountd: svc_tli_create: could </span><span style="color: #0000ff;">not</span> open connection <span style="color: #0000ff;">for</span><span style="color: #000000;"> udp6
rpc.mountd: svc_tli_create: could </span><span style="color: #0000ff;">not</span> open connection <span style="color: #0000ff;">for</span><span style="color: #000000;"> tcp6
                                                                          [  OK  ]
Starting NFS daemon: rpc.nfsd: address family inet6 </span><span style="color: #0000ff;">not</span><span style="color: #000000;"> supported by protocol TCP
                                                                 [  OK  ]
Starting RPC idmapd:                                         [  OK  ]<br /></span></pre>
</div>

> 根据启动提示可以获知，inet6地址族不被支持，原因是当前主机没有加载ipv6的模块，可以重新加载一遍ipv6模块解决这个问题。
> 
> 由于我的系统不需要ipv6的支持，所以还可以通过下面的操作，取消NFS的ipv6调用。

## <span id="12_netconfigTCPUDP6">1.2 <span style="font-family: '微软雅黑',sans-serif;">【<span style="background: yellow;">解决办法</span>】编辑</span>netconfig<span style="font-family: '微软雅黑',sans-serif;">配置文件，注释相关</span>TCP/UDP6<span style="font-family: '微软雅黑',sans-serif;">的信息条目</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;">vim /etc/netconfig</span>
 
<span style="color: #008000;">#
#</span><span style="color: #008000;"> Entries consist of:</span><span style="color: #008000;">
#
#</span><span style="color: #008000;">       &lt;network_id> &lt;semantics> &lt;flags> &lt;protofamily> &lt;protoname> \</span><span style="color: #008000;">
#</span><span style="color: #008000;">               &lt;device> &lt;nametoaddr_libs></span><span style="color: #008000;">
#
#</span><span style="color: #008000;"> The &lt;device> and &lt;nametoaddr_libs> fields are always empty in this</span><span style="color: #008000;">
#</span><span style="color: #008000;"> implementation.</span><span style="color: #008000;">
#
</span>udp        tpi_clts      v     inet     udp     -       -<span style="color: #000000;">
tcp        tpi_cots_ord  v     inet     tcp     </span>-       -
<span style="color: #008000;">#</span><span style="color: #008000;">udp6       tpi_clts      v     inet6    udp     -       -</span><span style="color: #008000;">
#</span><span style="color: #008000;">tcp6       tpi_cots_ord  v     inet6    tcp     -       -</span>
rawip      tpi_raw       -     inet      -      -       -
<span style="color: #800000;">"</span><span style="color: #800000;">/etc/netconfig</span><span style="color: #800000;">"</span> 19L, 769C written</pre>
</div>

## <span id="13">1.3 <span style="font-family: '微软雅黑',sans-serif;">重启服务验证</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;">/etc/init.d/nfs restart </span>
<span style="color: #000000;">Shutting down NFS daemon:                                  [  OK  ]
Shutting down NFS mountd:                                  [  OK  ]
Shutting down NFS services:                                 [  OK  ]
Shutting down RPC idmapd:                                   [  OK  ]
Starting NFS services:                                           [  OK  ]
Starting NFS mountd:                                           [  OK  ]
Starting NFS daemon:                                          [  OK  ]
Starting RPC idmapd:                                           [  OK  ]</span></pre>
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#11">1.1 启动时出现的错误</a><ul>
        <li>
          <a href="#12_netconfigTCPUDP6">1.2 【解决办法】编辑netconfig配置文件，注释相关TCP/UDP6的信息条目</a>
        </li>
        <li>
          <a href="#13">1.3 重启服务验证</a>
        </li>
      </ul>
    </li>
  </ul>
</div>