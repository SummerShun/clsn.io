---
title: 使用cobbler批量安装操作系统(基于Centos7.x )
author: 惨绿少年
type: post
date: 2017-11-15T01:19:00+00:00
url: /clsn/lx775.html
Baidusubmit:
  - 1
views:
  - 209
specs_zan:
  - 1
categories:
  - Linux运维
  - 玩转Linux
  - 自动化
  - 运维基本功

---
## <span id="11_cobbler">1.1 cobbler简介</span>

　　Cobbler是一个Linux服务器安装的服务，可以通过网络启动(PXE)的方式来快速安装、重装物理服务器和虚拟机，同时还可以管理DHCP，DNS等。

　　Cobbler可以使用命令行方式管理，也提供了基于Web的界面管理工具(cobbler-web)，还提供了API接口，可以方便二次开发使用。

　　Cobbler是较早前的kickstart的升级版，优点是比较容易配置，还自带web界面比较易于管理。

　　Cobbler内置了一个轻量级配置管理系统，但它也支持和其它配置管理系统集成，如Puppet，暂时不支持SaltStack。

　　Cobbler官网<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://cobbler.github.io"  target="_blank">http://cobbler.github.io</a>

　　在使用cobbler之前需要了解kickstart的使用：&nbsp;<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/7833333.html"  target="_blank">http://www.cnblogs.com/clsn/p/7833333.html</a>

### <span id="111_cobbler">1.1.1 cobbler集成的服务</span>

&nbsp;　　 PXE服务支持

&nbsp;　　 DHCP服务管理

&nbsp;　　 DNS服务管理(可选bind,dnsmasq)

&nbsp; 　　电源管理

&nbsp; 　　Kickstart服务支持

&nbsp; 　　YUM仓库管理

&nbsp; 　　TFTP(PXE启动时需要)

&nbsp; 　　Apache**(****提供kickstart****的安装源，并提供定制化的kickstart****配置)**

## <span id="12_cobbler">1.2 安装cobbler</span>

### <span id="121">1.2.1 环境说明</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat /etc/redhat-release</span>
CentOS Linux release 7.4.1708<span style="color: #000000;"> (Core)

[root@Cobbler </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> uname -r</span>
3.10.0-693<span style="color: #000000;">.el7.x86_64

[root@Cobbler </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> getenforce</span>
<span style="color: #000000;">Disabled

[root@Cobbler </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl status firewalld.service</span>
● firewalld.service - firewalld -<span style="color: #000000;"> dynamic firewall daemon
   Loaded: loaded (</span>/usr/lib/systemd/system/<span style="color: #000000;">firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(</span>1<span style="color: #000000;">)

[root@Cobbler </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> hostname -I</span>
10.0.0.202 172.16.1.202</pre>
  </div>
</div>

_yum__源说明：_

<div>
  <div class="cnblogs_code">
    <pre>curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7<span style="color: #000000;">.repo
curl </span>-o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo</pre>
  </div>
</div>

### <span id="122_yumcobbler">1.2.2 使用yum安装cobbler</span>

<div>
  <div class="cnblogs_code">
    <pre>yum -y install cobbler cobbler-web dhcp tftp-server pykickstart httpd</pre>
  </div>
</div>

&nbsp;&nbsp; _说明：cobbler__是依赖与epel__源下载_

### <span id="123_cobblerhttpcobbler">1.2.3 cobbler语法检查前先启动http与cobbler</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #000000;">systemctl start httpd.service
systemctl start cobblerd.service
cobbler check</span></pre>
  </div>
</div>

### <span id="124">1.2.4 进行语法检查</span>

<div>
  <div class="cnblogs_code">
    <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cobbler check</span>
<span style="color: #000000;">The following are potential configuration items that you may want to fix:

</span>1 : The <span style="color: #800000;">'</span><span style="color: #800000;">server</span><span style="color: #800000;">'</span> field <span style="color: #0000ff;">in</span> /etc/cobbler/settings must be set to something other than localhost, <span style="color: #0000ff;">or</span> kickstarting features will <span style="color: #0000ff;">not</span> work.  This should be a resolvable hostname <span style="color: #0000ff;">or</span> IP <span style="color: #0000ff;">for</span><span style="color: #000000;"> the boot server as reachable by all machines that will use it.

</span>2 : For PXE to be functional, the <span style="color: #800000;">'</span><span style="color: #800000;">next_server</span><span style="color: #800000;">'</span> field <span style="color: #0000ff;">in</span> /etc/cobbler/settings must be set to something other than 127.0.0.1, <span style="color: #0000ff;">and</span><span style="color: #000000;"> should match the IP of the boot server on the PXE network.

</span>3 : change <span style="color: #800000;">'</span><span style="color: #800000;">disable</span><span style="color: #800000;">'</span> to <span style="color: #800000;">'</span><span style="color: #800000;">no</span><span style="color: #800000;">'</span> <span style="color: #0000ff;">in</span> /etc/xinetd.d/<span style="color: #000000;">tftp

</span>4 : Some network boot-loaders are missing <span style="color: #0000ff;">from</span> /var/lib/cobbler/loaders, you may run <span style="color: #800000;">'</span><span style="color: #800000;">cobbler get-loaders</span><span style="color: #800000;">'</span> to download them, <span style="color: #0000ff;">or</span>, <span style="color: #0000ff;">if</span> you only want to handle x86/x86_64 netbooting, you may ensure that you have installed a *recent* version of the syslinux package installed <span style="color: #0000ff;">and</span> can ignore this message entirely.  Files <span style="color: #0000ff;">in</span> this directory, should you want to support all architectures, should include pxelinux.0, menu.c32, elilo.efi, <span style="color: #0000ff;">and</span> yaboot. The <span style="color: #800000;">'</span><span style="color: #800000;">cobbler get-loaders</span><span style="color: #800000;">'</span> command <span style="color: #0000ff;">is</span><span style="color: #000000;"> the easiest way to resolve these requirements.

</span>5 : enable <span style="color: #0000ff;">and</span><span style="color: #000000;"> start rsyncd.service with systemctl

</span>6 : debmirror package <span style="color: #0000ff;">is</span> <span style="color: #0000ff;">not</span> installed, it will be required to manage debian deployments <span style="color: #0000ff;">and</span><span style="color: #000000;"> repositories

</span>7 : The default password used by the sample templates <span style="color: #0000ff;">for</span> newly installed machines (default_password_crypted <span style="color: #0000ff;">in</span> /etc/cobbler/settings) <span style="color: #0000ff;">is</span> still set to <span style="color: #800000;">'</span><span style="color: #800000;">cobbler</span><span style="color: #800000;">'</span> <span style="color: #0000ff;">and</span> should be changed, <span style="color: #0000ff;">try</span>: <span style="color: #800000;">"</span><span style="color: #800000;">openssl passwd -1 -salt 'random-phrase-here' 'your-password-here'</span><span style="color: #800000;">"</span><span style="color: #000000;"> to generate new one

</span>8 : fencing tools were <span style="color: #0000ff;">not</span> found, <span style="color: #0000ff;">and</span> are required to use the (optional) power management features. install cman <span style="color: #0000ff;">or</span> fence-<span style="color: #000000;">agents to use them

Restart cobblerd </span><span style="color: #0000ff;">and</span> then run <span style="color: #800000;">'</span><span style="color: #800000;">cobbler sync</span><span style="color: #800000;">'</span> to apply changes.</pre>
  </div>
</div>

### <span id="125">1.2.5 解决当中的报错</span>

_<span style="text-decoration: underline;">命令集</span>_

<div class="cnblogs_code" onclick="cnblogs_code_show('4fe6c664-64d6-4fbf-af7d-f9f6cee803ba')">
  <img id="code_img_closed_4fe6c664-64d6-4fbf-af7d-f9f6cee803ba" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-16.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" /><img id="code_img_opened_4fe6c664-64d6-4fbf-af7d-f9f6cee803ba" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('4fe6c664-64d6-4fbf-af7d-f9f6cee803ba',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-16.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" /></p> 
  
  <div id="cnblogs_code_open_4fe6c664-64d6-4fbf-af7d-f9f6cee803ba" class="cnblogs_code_hide">
    <pre>sed -i <span style="color: #800000;">'</span><span style="color: #800000;">s/server: 127.0.0.1/server: 172.16.1.202/</span><span style="color: #800000;">'</span> /etc/cobbler/<span style="color: #000000;">settings
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s/next_server: 127.0.0.1/next_server: 172.16.1.202/</span><span style="color: #800000;">'</span> /etc/cobbler/<span style="color: #000000;">settings
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s/manage_dhcp: 0/manage_dhcp: 1/</span><span style="color: #800000;">'</span> /etc/cobbler/<span style="color: #000000;">settings
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s/pxe_just_once: 0/pxe_just_once: 1/</span><span style="color: #800000;">'</span> /etc/cobbler/<span style="color: #000000;">settings
sed </span>-ri <span style="color: #800000;">"</span><span style="color: #800000;">/default_password_crypted/s#(.*: ).*#\1\"`openssl passwd -1 -salt 'oldboy' '123456'`\"#</span><span style="color: #800000;">"</span> /etc/cobbler/<span style="color: #000000;">settings
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s#yes#no#</span><span style="color: #800000;">'</span> /etc/xinetd.d/<span style="color: #000000;">tftp

systemctl start rsyncd
systemctl enable rsyncd
systemctl enable tftp.socket
systemctl start tftp.socket
systemctl restart cobblerd.service

sed </span>-i.ori <span style="color: #800000;">'</span><span style="color: #800000;">s#192.168.1#172.16.1#g;22d;23d</span><span style="color: #800000;">'</span> /etc/cobbler/<span style="color: #000000;">dhcp.template

cobbler sync</span></pre>
  </div>
  
  <p>
    <span class="cnblogs_code_collapse">View&nbsp;<em>命令集&nbsp; 单击+打开</em></span></div> 
    
    <p>
      <em><span style="text-decoration: underline;">详解</span></em>
    </p>
    
    <p>
      <em>解决1</em><em>、2</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>cp /etc/cobbler/<span style="color: #000000;">settings{,.ori}
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s/server: 127.0.0.1/server: 172.16.1.202/</span><span style="color: #800000;">'</span> /etc/cobbler/<span style="color: #000000;">settings
sed </span>-i <span style="color: #800000;">'</span><span style="color: #800000;">s/next_server: 127.0.0.1/next_server: 172.16.1.202/</span><span style="color: #800000;">'</span> /etc/cobbler/settings</pre>
      </div>
    </div>
    
    <p>
      <em>问题3</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>sed <span style="color: #800000;">'</span><span style="color: #800000;">s#yes#no#g</span><span style="color: #800000;">'</span> /etc/xinetd.d/tftp -i</pre>
      </div>
    </div>
    
    <p>
      <em>4</em><em>下载包所需的软件包</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cobbler get-loaders</span>
[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> ls  /var/lib/cobbler/loaders</span>
COPYING.elilo     elilo-<span style="color: #000000;">ia64.efi   menu.c32    yaboot
COPYING.syslinux  grub</span>-<span style="color: #000000;">x86_64.efi  pxelinux.0
COPYING.yaboot    grub</span>-x86.efi     README</pre>
      </div>
    </div>
    
    <p>
      <em>5</em><em>启动rsync</em><em>服务</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl start rsyncd.service</span>
[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> systemctl enable rsyncd.service</span></pre>
      </div>
    </div>
    
    <p>
      <em>6 debian</em><em>相关无需修改</em>
    </p>
    
    <p>
      <em>7</em><em>、修改安装完成后的root</em><em>密码</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>openssl passwd -1 -salt <span style="color: #800000;">'</span><span style="color: #800000;">random-phrase-here</span><span style="color: #800000;">'</span> <span style="color: #800000;">'</span><span style="color: #800000;">your-password-here</span><span style="color: #800000;">'</span><span style="color: #000000;">
random</span>-phrase-<span style="color: #000000;">here  随机字符串
your</span>-password-here 密码</pre>
      </div>
    </div>
    
    <p>
      <em><span style="text-decoration: underline;">示例</span></em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> openssl passwd -1 -salt 'CLSN' '123456'</span>
$1$CLSN$LpJk4x1cplibx3q/O4O/K/</pre>
      </div>
    </div>
    
    <p>
      <em>管理dhcp</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>sed -i <span style="color: #800000;">'</span><span style="color: #800000;">s/manage_dhcp: 0/manage_dhcp: 1/</span><span style="color: #800000;">'</span> /etc/cobbler/settings</pre>
      </div>
    </div>
    
    <p>
      <em><span style="text-decoration: underline;">防止重装</span></em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>sed -i <span style="color: #800000;">'</span><span style="color: #800000;">s/pxe_just_once: 0/pxe_just_once: 1/</span><span style="color: #800000;">'</span> /etc/cobbler/settings</pre>
      </div>
    </div>
    
    <p>
      <em><span style="text-decoration: underline;">修改dhcp</span></em><em><span style="text-decoration: underline;">模板</span></em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>sed -i.ori <span style="color: #800000;">'</span><span style="color: #800000;">s#192.168.1#172.16.1#g;22d;23d</span><span style="color: #800000;">'</span> /etc/cobbler/dhcp.template</pre>
      </div>
    </div>
    
    <p>
      cobbler组配置文件位置
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>/etc/cobbler/settings</pre>
      </div>
    </div>
    
    <p>
      <strong>注意：修改完成之后要使用cobbler sync </strong><strong>进行同步，否则不生效。</strong>
    </p>
    
    <h3>
      <span id="126">1.2.6 修改之后</span>
    </h3>
    
    <p>
      <em>再次检查语法：</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cobbler check</span>
<span style="color: #000000;">The following are potential configuration items that you may want to fix:

</span>1 : debmirror package <span style="color: #0000ff;">is</span> <span style="color: #0000ff;">not</span> installed, it will be required to manage debian deployments <span style="color: #0000ff;">and</span><span style="color: #000000;"> repositories
</span>2 : fencing tools were <span style="color: #0000ff;">not</span> found, <span style="color: #0000ff;">and</span> are required to use the (optional) power management features. install cman <span style="color: #0000ff;">or</span> fence-<span style="color: #000000;">agents to use them

Restart cobblerd </span><span style="color: #0000ff;">and</span> then run <span style="color: #800000;">'</span><span style="color: #800000;">cobbler sync</span><span style="color: #800000;">'</span> to apply changes.</pre>
      </div>
    </div>
    
    <p>
      重启所有服务
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre><span style="color: #000000;">systemctl restart httpd.service
systemctl restart cobblerd.service
systemctl restart dhcpd.service
systemctl restart rsyncd.service
systemctl restart tftp.socket</span></pre>
      </div>
    </div>
    
    <p>
      <strong>到此cobbler</strong><strong>就安装完成，下面进行web</strong><strong>界面的操作。</strong>
    </p>
    
    <h2>
      <span id="13_cobblerweb">1.3 cobbler的web及界面操作</span>
    </h2>
    
    <p>
      浏览器访问<strong><em>https://10.0.0.202/cobbler_web</em></strong>
    </p>
    
    <p>
      &nbsp;&nbsp; <em>注意CentOS7</em><em>中cobbler</em><em>只支持https</em><em>访问。</em>
    </p>
    
    <p>
      <em>&nbsp;&nbsp; </em><em>账号密码默认均为</em><strong>cobbler</strong>
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171117046-397590980.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
    </p>
    
    <h3>
      <span id="131">1.3.1 操作说明--导入镜像</span>
    </h3>
    
    <p>
      1）在虚拟机上添加上镜像
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171127546-900178910.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
    </p>
    
    <p>
      2)挂载上镜像
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> mount /dev/cdrom  /mnt/</span>
mount: /dev/sr0 <span style="color: #0000ff;">is</span> write-protected, mounting read-<span style="color: #000000;">only

[root@Cobbler </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> df -h |grep mnt</span>
/dev/sr0        4.3G  4.3G     0 100% /mnt</pre>
      </div>
    </div>
    
    <p>
      &nbsp;&nbsp; 3)进行导入镜像
    </p>
    
    <p>
      &nbsp;&nbsp; 选择<em>Import DVD</em> &nbsp;输入Prefix(文件前缀)，Arch（版本），Breed（品牌），Path(要从什么地方导入)
    </p>
    
    <p>
      &nbsp;&nbsp; 在导入镜像的时候要注意路径，防止循环导入。
    </p>
    
    <p>
      &nbsp;&nbsp; 信息配置好后，点击run，即可进行导入。
    </p>
    
    <p align="center">
      &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171154031-1223678363.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
    </p>
    
    <p>
      <em>导入过程使用rsync</em><em>进行导入，三个进程消失表示导入完毕</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@Cobbler mnt]<span style="color: #008000;">#</span><span style="color: #008000;"> ps -ef |grep rsync</span>
root   12026      1  0 19:04 ?   00:00:00 /usr/bin/rsync --daemon --no-<span style="color: #000000;">detach
root  </span>13554  11778 12 19:51 ?    00:00:06 rsync -a /mnt/ /var/www/cobbler/ks_mirror/CentOS7.4-x86_64 --<span style="color: #000000;">progress
root   </span>13555  13554  0 19:51 ?     00:00:00 rsync -a /mnt/ /var/www/cobbler/ks_mirror/CentOS7.4-x86_64 --<span style="color: #000000;">progress
root   </span>13556  13555 33 19:51 ?        00:00:17 rsync -a /mnt/ /var/www/cobbler/ks_mirror/CentOS7.4-x86_64 --<span style="color: #000000;">progress
root   </span>13590  10759  0 19:52 pts/1    00:00:00 grep --color=auto rsync</pre>
      </div>
    </div>
    
    <p>
      查看日志可以发现右running进程
    </p>
    
    <p>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 日志位于 <strong>Events</strong>
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171219140-820246395.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
    </p>
    
    <p>
      <em>导入完成后生成的文件夹</em>
    </p>
    
    <div>
      <div class="cnblogs_code">
        <pre>[root@Cobbler ks_mirror]<span style="color: #008000;">#</span><span style="color: #008000;"> pwd</span>
/var/www/cobbler/<span style="color: #000000;">ks_mirror

[root@Cobbler ks_mirror]</span><span style="color: #008000;">#</span><span style="color: #008000;"> ls</span>
CentOS7.4-x86_64  config</pre>
      </div>
    </div>
    
    <h3>
      <span id="132">1.3.2 创建一台空白虚拟机，进行测试网路安装</span>
    </h3>
    
    <p>
      注意：虚拟机的内存不能小于2G,网卡的配置要保证网络互通
    </p>
    
    <p>
      <strong>启动虚拟机</strong>
    </p>
    
    <p>
      <strong>&nbsp;&nbsp; </strong>启动虚拟机即可发现会有cobbler的选择界面
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171242656-398272156.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
    </p>
    
    <p>
      选择CentOS7.4即可进行安装，安装过程与光盘安装一致，这里就不在复述。
    </p>
    
    <h2>
      <span id="14">1.4 定制化安装操作系统</span>
    </h2>
    
    <h3>
      <span id="141">1.4.1 添加内核参数</span>
    </h3>
    
    <p>
      1）查看导入的镜像，点击edit
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171258015-1494072420.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
    </p>
    
    <p>
      2）在内核参数中添加net.ifnames=0 biosdevname=0
    </p>
    
    <p>
      &nbsp;&nbsp; 能够让显示的网卡变为eth0 ，而不是CentOS7中的ens33
    </p>
    
    <p>
      &nbsp;&nbsp; 修改完成后点击保存
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171309874-60725189.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
    </p>
    
    <h3>
      <span id="142">1.4.2 查看镜像属性</span>
    </h3>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171319609-343797442.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
    </p>
    
    <h3>
      <span id="143_ks">1.4.3 编写ks文件</span>
    </h3>
    
    <p>
      1)创建新的ks文件
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171338593-1336390307.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
    </p>
    
    <p>
      2）添加ks文件，并配置文件名
    </p>
    
    <p>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 创建完成后点击Save进行保存
    </p>
    
    <p align="center">
      <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171346874-1039121760.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
    </p>
    
    <p>
      <em><span style="text-decoration: underline;">CentOS7 &nbsp;ks</span></em><em><span style="text-decoration: underline;">配置文件参考</span></em>
    </p>
    
    <div>
      <div class="cnblogs_code" onclick="cnblogs_code_show('7b3e062f-ee05-45e1-ab6e-16bbbd318202')">
        <img id="code_img_closed_7b3e062f-ee05-45e1-ab6e-16bbbd318202" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-16.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" /><img id="code_img_opened_7b3e062f-ee05-45e1-ab6e-16bbbd318202" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('7b3e062f-ee05-45e1-ab6e-16bbbd318202',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-16.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" /></p> 
        
        <div id="cnblogs_code_open_7b3e062f-ee05-45e1-ab6e-16bbbd318202" class="cnblogs_code_hide">
          <pre><span style="color: #008080;"> 1</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Cobbler for Kickstart Configurator for CentOS 7 by clsn</span>
<span style="color: #008080;"> 2</span> <span style="color: #000000;">install
</span><span style="color: #008080;"> 3</span> url --url=<span style="color: #000000;">$tree
</span><span style="color: #008080;"> 4</span> <span style="color: #000000;">text
</span><span style="color: #008080;"> 5</span> lang en_US.UTF-8
<span style="color: #008080;"> 6</span> <span style="color: #000000;">keyboard us
</span><span style="color: #008080;"> 7</span> <span style="color: #000000;">zerombr
</span><span style="color: #008080;"> 8</span> bootloader --location=mbr --driveorder=sda --append=<span style="color: #800000;">"</span><span style="color: #800000;">crashkernel=auto rhgb quiet</span><span style="color: #800000;">"</span>
<span style="color: #008080;"> 9</span> <span style="color: #008000;">#</span><span style="color: #008000;">Network information</span>
<span style="color: #008080;">10</span> $SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">network_config</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008080;">11</span> <span style="color: #008000;">#</span><span style="color: #008000;">network --bootproto=dhcp --device=eth0 --onboot=yes --noipv6 --hostname=CentOS7</span>
<span style="color: #008080;">12</span> timezone --utc Asia/<span style="color: #000000;">Shanghai
</span><span style="color: #008080;">13</span> authconfig --enableshadow --passalgo=<span style="color: #000000;">sha512
</span><span style="color: #008080;">14</span> rootpw  --<span style="color: #000000;">iscrypted $default_password_crypted
</span><span style="color: #008080;">15</span> clearpart --all --<span style="color: #000000;">initlabel
</span><span style="color: #008080;">16</span> part /boot --fstype xfs --size 1024
<span style="color: #008080;">17</span> part swap --size 1024
<span style="color: #008080;">18</span> part / --fstype xfs --size 1 --<span style="color: #000000;">grow
</span><span style="color: #008080;">19</span> firstboot --<span style="color: #000000;">disable
</span><span style="color: #008080;">20</span> selinux --<span style="color: #000000;">disabled
</span><span style="color: #008080;">21</span> firewall --<span style="color: #000000;">disabled
</span><span style="color: #008080;">22</span> logging --level=<span style="color: #000000;">info
</span><span style="color: #008080;">23</span> <span style="color: #000000;">reboot
</span><span style="color: #008080;">24</span> 
<span style="color: #008080;">25</span> %<span style="color: #000000;">pre
</span><span style="color: #008080;">26</span> $SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">log_ks_pre</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008080;">27</span> $SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">kickstart_start</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008080;">28</span> $SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">pre_install_network_config</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008080;">29</span> <span style="color: #008000;">#</span><span style="color: #008000;"> Enable installation monitoring</span>
<span style="color: #008080;">30</span> $SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">pre_anamon</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008080;">31</span> %<span style="color: #000000;">end
</span><span style="color: #008080;">32</span> 
<span style="color: #008080;">33</span> %<span style="color: #000000;">packages
</span><span style="color: #008080;">34</span> @^<span style="color: #000000;">minimal
</span><span style="color: #008080;">35</span> @compat-<span style="color: #000000;">libraries
</span><span style="color: #008080;">36</span> <span style="color: #000000;">@core
</span><span style="color: #008080;">37</span> <span style="color: #000000;">@debugging
</span><span style="color: #008080;">38</span> <span style="color: #000000;">@development
</span><span style="color: #008080;">39</span> bash-<span style="color: #000000;">completion
</span><span style="color: #008080;">40</span> <span style="color: #000000;">chrony
</span><span style="color: #008080;">41</span> <span style="color: #000000;">dos2unix
</span><span style="color: #008080;">42</span> kexec-<span style="color: #000000;">tools
</span><span style="color: #008080;">43</span> <span style="color: #000000;">lrzsz
</span><span style="color: #008080;">44</span> <span style="color: #000000;">nmap
</span><span style="color: #008080;">45</span> <span style="color: #000000;">sysstat
</span><span style="color: #008080;">46</span> <span style="color: #000000;">telnet
</span><span style="color: #008080;">47</span> <span style="color: #000000;">tree
</span><span style="color: #008080;">48</span> <span style="color: #000000;">vim
</span><span style="color: #008080;">49</span> <span style="color: #000000;">wget
</span><span style="color: #008080;">50</span> %<span style="color: #000000;">end
</span><span style="color: #008080;">51</span> 
<span style="color: #008080;">52</span> %<span style="color: #000000;">post
</span><span style="color: #008080;">53</span> <span style="color: #000000;">systemctl disable postfix.service
</span><span style="color: #008080;">54</span> %end</pre>
        </div>
        
        <p>
          <span class="cnblogs_code_collapse">View Code&nbsp; ks文件内容(centos7.x)</span></div> </div> 
          
          <h3>
            <span id="144">1.4.4 自定义安装系统</span>
          </h3>
          
          <p>
            1)选择systems 创建一个新的系统
          </p>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171448749-413659574.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
          </p>
          
          <p>
            2)定义系统信息
          </p>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171500702-1774899020.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
          </p>
          
          <p>
            3)配置全局网络信息
          </p>
          
          <p>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 主机名、网关、DNS
          </p>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171510515-1866830973.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
          </p>
          
          <p>
            4)配置网卡信息，eth0，eth1
          </p>
          
          <p>
            &nbsp;&nbsp; 需要注意，选择static静态，
          </p>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171519781-484237309.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
          </p>
          
          <p>
            &nbsp;&nbsp; <strong>以上的所有配置完成后，点击Save</strong><strong>进行保存</strong>
          </p>
          
          <p>
            <span style="background-color: #ffffff; color: #0000ff;"><strong><em>附录：</em></strong></span>
          </p>
          
          <p>
            &nbsp;&nbsp; VMware workstation中查看虚拟机mac地址的方法。在虚拟机设置中。
          </p>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171558437-2147316862.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
          </p>
          
          <p>
            <strong>&nbsp;cobbler web 界面</strong><strong>说明</strong>
          </p>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171610484-679767567.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
          </p>
          
          <h2>
            <span id="15">1.5 安装虚拟机</span>
          </h2>
          
          <h3>
            <span id="151">1.5.1 开启虚拟机</span>
          </h3>
          
          <p>
            如果之前的设置就显示安装进度
          </p>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171637515-1915678553.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
          </p>
          
          <h3>
            <span id="152">1.5.2 安装完成进行检查</span>
          </h3>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171645890-120797278.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />
          </p>
          
          <h2>
            <span id="16_cobbler">1.6 cobbler使用常见错误</span>
          </h2>
          
          <h3>
            <span id="161_cobbler_check">1.6.1 cobbler check报错</span>
          </h3>
          
          <div>
            <div class="cnblogs_code">
              <pre>[root@Cobbler ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cobbler sync</span>
<span style="color: #000000;">Traceback (most recent call last):
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/bin/cobbler</span><span style="color: #800000;">"</span>, line 36, <span style="color: #0000ff;">in</span> &lt;module><span style="color: #000000;">
    sys.exit(app.main())
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib/python2.7/site-packages/cobbler/cli.py</span><span style="color: #800000;">"</span>, line 662, <span style="color: #0000ff;">in</span><span style="color: #000000;"> main
    rc </span>=<span style="color: #000000;"> cli.run(sys.argv)
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib/python2.7/site-packages/cobbler/cli.py</span><span style="color: #800000;">"</span>, line 269, <span style="color: #0000ff;">in</span><span style="color: #000000;"> run
    self.token         </span>= self.remote.login(<span style="color: #800000;">""</span><span style="color: #000000;">, self.shared_secret)
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib64/python2.7/xmlrpclib.py</span><span style="color: #800000;">"</span>, line 1233, <span style="color: #0000ff;">in</span> <span style="color: #800080;">__call__</span>
    <span style="color: #0000ff;">return</span> self.<span style="color: #800080;">__send</span>(self.<span style="color: #800080;">__name</span><span style="color: #000000;">, args)
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib64/python2.7/xmlrpclib.py</span><span style="color: #800000;">"</span>, line 1587, <span style="color: #0000ff;">in</span> <span style="color: #800080;">__request</span><span style="color: #000000;">
    verbose</span>=self.<span style="color: #800080;">__verbose</span><span style="color: #000000;">
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib64/python2.7/xmlrpclib.py</span><span style="color: #800000;">"</span>, line 1273, <span style="color: #0000ff;">in</span><span style="color: #000000;"> request
    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> self.single_request(host, handler, request_body, verbose)
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib64/python2.7/xmlrpclib.py</span><span style="color: #800000;">"</span>, line 1306, <span style="color: #0000ff;">in</span><span style="color: #000000;"> single_request
    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> self.parse_response(response)
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib64/python2.7/xmlrpclib.py</span><span style="color: #800000;">"</span>, line 1482, <span style="color: #0000ff;">in</span><span style="color: #000000;"> parse_response
    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> u.close()
  File </span><span style="color: #800000;">"</span><span style="color: #800000;">/usr/lib64/python2.7/xmlrpclib.py</span><span style="color: #800000;">"</span>, line 794, <span style="color: #0000ff;">in</span><span style="color: #000000;"> close
    </span><span style="color: #0000ff;">raise</span> Fault(**<span style="color: #000000;">self._stack[0])
xmlrpclib.Fault: </span>&lt;Fault 1: <span style="color: #800000;">"</span><span style="color: #800000;">&lt;class 'cobbler.cexceptions.CX'>:'login failed'</span><span style="color: #800000;">"</span>></pre>
            </div>
          </div>
          
          <p>
            解决办法
          </p>
          
          <div>
            <div class="cnblogs_code">
              <pre><span style="color: #000000;">systemctl restart httpd.service
systemctl restart cobblerd.service
cobbler check</span></pre>
            </div>
          </div>
          
          <h3>
            <span id="162_No_space_left_on_device">1.6.2 No space left on device</span>
          </h3>
          
          <p align="center">
            <img data-original="https://images2017.cnblogs.com/blog/1190037/201711/1190037-20171115171739296-1920171628.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" />&nbsp;
          </p>
          
          <p>
            &nbsp;&nbsp; 出现这个错误的原因是虚拟机的内存不足2G，
          </p>
          
          <p>
            &nbsp;&nbsp; 将内存调为2G即可（这个错误只会出现在CentOS7.3之上）
          </p>
          
          <h2>
            <span id="17_cobbler_CentOS6x_ks"><span style="color: #99cc00;">1.7 附录cobbler_CentOS6.x_ks配置文件</span></span>
          </h2>
          
          <div>
            <div class="cnblogs_code" onclick="cnblogs_code_show('ef9ba030-45ea-46b1-b5c0-4642b2fbd94e')">
              <img id="code_img_closed_ef9ba030-45ea-46b1-b5c0-4642b2fbd94e" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-16.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" /><img id="code_img_opened_ef9ba030-45ea-46b1-b5c0-4642b2fbd94e" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('ef9ba030-45ea-46b1-b5c0-4642b2fbd94e',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-16.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="使用cobbler批量安装操作系统(基于Centos7.x )" alt="" /></p> 
              
              <div id="cnblogs_code_open_ef9ba030-45ea-46b1-b5c0-4642b2fbd94e" class="cnblogs_code_hide">
                <pre><span style="color: #008000;">#</span><span style="color: #008000;"> Cobbler for Kickstart Configurator for CentOS 6  by clsn</span>
<span style="color: #000000;">install
url </span>--url=<span style="color: #000000;">$tree
text
lang en_US.UTF</span>-8<span style="color: #000000;">
keyboard us
zerombr
bootloader </span>--location=mbr --driveorder=sda --append=<span style="color: #800000;">"</span><span style="color: #800000;">crashkernel=auto rhgb quiet</span><span style="color: #800000;">"</span><span style="color: #000000;">
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">network_config</span><span style="color: #800000;">'</span><span style="color: #000000;">)
timezone </span>--utc Asia/<span style="color: #000000;">Shanghai
authconfig </span>--enableshadow --passalgo=<span style="color: #000000;">sha512
rootpw  </span>--<span style="color: #000000;">iscrypted $default_password_crypted
clearpart </span>--all --<span style="color: #000000;">initlabel
part </span>/boot --fstype=ext4 --asprimary --size=200<span style="color: #000000;">
part swap </span>--size=1024<span style="color: #000000;">
part </span>/ --fstype=ext4 --grow --asprimary --size=200<span style="color: #000000;">
firstboot </span>--<span style="color: #000000;">disable
selinux </span>--<span style="color: #000000;">disabled
firewall </span>--<span style="color: #000000;">disabled
logging </span>--level=<span style="color: #000000;">info
reboot

</span>%<span style="color: #000000;">pre
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">log_ks_pre</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">kickstart_start</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">pre_install_network_config</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008000;">#</span><span style="color: #008000;"> Enable installation monitoring</span>
$SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">pre_anamon</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span>%<span style="color: #000000;">end

</span>%<span style="color: #000000;">packages
@base
@compat</span>-<span style="color: #000000;">libraries
@debugging
@development
tree
nmap
sysstat
lrzsz
dos2unix
telnet
</span>%<span style="color: #000000;">end

</span>%post --<span style="color: #000000;">nochroot
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">log_ks_post_nochroot</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span>%<span style="color: #000000;">end

</span>%<span style="color: #000000;">post
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">log_ks_post</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008000;">#</span><span style="color: #008000;"> Start yum configuration</span>
<span style="color: #000000;">$yum_config_stanza
</span><span style="color: #008000;">#</span><span style="color: #008000;"> End yum configuration</span>
$SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">post_install_kernel_options</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">post_install_network_config</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">func_register_if_enabled</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">download_config_files</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">koan_environment</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">redhat_register</span><span style="color: #800000;">'</span><span style="color: #000000;">)
$SNIPPET(</span><span style="color: #800000;">'</span><span style="color: #800000;">cobbler_register</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008000;">#</span><span style="color: #008000;"> Enable post-install boot notification</span>
$SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">post_anamon</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008000;">#</span><span style="color: #008000;"> Start final steps</span>
$SNIPPET(<span style="color: #800000;">'</span><span style="color: #800000;">kickstart_done</span><span style="color: #800000;">'</span><span style="color: #000000;">)
</span><span style="color: #008000;">#</span><span style="color: #008000;"> End final steps</span>
%end</pre>
              </div>
              
              <p>
                <span class="cnblogs_code_collapse">View ks文件参考&nbsp; centos6.x</span></div> </div> 
                
                <h2>
                  <span id="18">1.8 参考文档</span>
                </h2>
                
                <p>
                  　　http://blog.oldboyedu.com/autoinstall-cobbler/
                </p>
                
                <p>
                  　　http://www.zyops.com/autoinstall-cobbler
                </p>
                
                <div id="toc_container" class="toc_white have_bullets">
                  <ul class="toc_list">
                    <li>
                      <a href="#11_cobbler">1.1 cobbler简介</a><ul>
                        <li>
                          <a href="#111_cobbler">1.1.1 cobbler集成的服务</a>
                        </li>
                      </ul>
                    </li>
                    
                    <li>
                      <a href="#12_cobbler">1.2 安装cobbler</a><ul>
                        <li>
                          <a href="#121">1.2.1 环境说明</a>
                        </li>
                        <li>
                          <a href="#122_yumcobbler">1.2.2 使用yum安装cobbler</a>
                        </li>
                        <li>
                          <a href="#123_cobblerhttpcobbler">1.2.3 cobbler语法检查前先启动http与cobbler</a>
                        </li>
                        <li>
                          <a href="#124">1.2.4 进行语法检查</a>
                        </li>
                        <li>
                          <a href="#125">1.2.5 解决当中的报错</a>
                        </li>
                        <li>
                          <a href="#126">1.2.6 修改之后</a>
                        </li>
                      </ul>
                    </li>
                    
                    <li>
                      <a href="#13_cobblerweb">1.3 cobbler的web及界面操作</a><ul>
                        <li>
                          <a href="#131">1.3.1 操作说明--导入镜像</a>
                        </li>
                        <li>
                          <a href="#132">1.3.2 创建一台空白虚拟机，进行测试网路安装</a>
                        </li>
                      </ul>
                    </li>
                    
                    <li>
                      <a href="#14">1.4 定制化安装操作系统</a><ul>
                        <li>
                          <a href="#141">1.4.1 添加内核参数</a>
                        </li>
                        <li>
                          <a href="#142">1.4.2 查看镜像属性</a>
                        </li>
                        <li>
                          <a href="#143_ks">1.4.3 编写ks文件</a>
                        </li>
                        <li>
                          <a href="#144">1.4.4 自定义安装系统</a>
                        </li>
                      </ul>
                    </li>
                    
                    <li>
                      <a href="#15">1.5 安装虚拟机</a><ul>
                        <li>
                          <a href="#151">1.5.1 开启虚拟机</a>
                        </li>
                        <li>
                          <a href="#152">1.5.2 安装完成进行检查</a>
                        </li>
                      </ul>
                    </li>
                    
                    <li>
                      <a href="#16_cobbler">1.6 cobbler使用常见错误</a><ul>
                        <li>
                          <a href="#161_cobbler_check">1.6.1 cobbler check报错</a>
                        </li>
                        <li>
                          <a href="#162_No_space_left_on_device">1.6.2 No space left on device</a>
                        </li>
                      </ul>
                    </li>
                    
                    <li>
                      <a href="#17_cobbler_CentOS6x_ks">1.7 附录cobbler_CentOS6.x_ks配置文件</a>
                    </li>
                    <li>
                      <a href="#18">1.8 参考文档</a>
                    </li>
                  </ul>
                </div>