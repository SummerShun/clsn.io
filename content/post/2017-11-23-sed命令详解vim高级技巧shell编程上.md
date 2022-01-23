---
title: 'sed命令详解 vim高级技巧  shell编程上'
author: 惨绿少年
type: post
date: 2017-11-23T06:35:00+00:00
url: /clsn/lx657.html
Baidusubmit:
  - 1
views:
  - 162
specs_zan:
  - 1
categories:
  - Linux运维
  - 玩转Linux
  - 运维基本功

---
# <span id="1_sed">第1章 sed<span style="font-family: 新宋体;">命令详解</span></span>

## <span id="11">1.1 <span style="font-family: 新宋体;">查找固定的某一行</span></span>

### <span id="111_awk">1.1.1 awk<span style="font-family: 新宋体;">命令方法</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> awk '!/clsn/' person.txt</span>
102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

### <span id="112_grep">1.1.2 grep<span style="font-family: 新宋体;">方法</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> grep -v "clsn" person.txt</span>
102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

### <span id="113_sed">1.1.3 sed<span style="font-family: 新宋体;">方法</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed -n '/clsn/!p' person.txt</span>
102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105<span style="color: #000000;">,hehe,CIO

[root@znix </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed '/clsn/d' person.txt</span>
102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

## <span id="12_sed">1.2 sed<span style="font-family: 新宋体;">的替换</span></span>

<p style="text-indent: 18.0pt;">
  <span style="background: yellow;">s</span><span style="font-family: 新宋体;">为</span> sub<span style="font-family: 新宋体;">（</span>substitute<span style="font-family: 新宋体;">）替换</span>
</p>

<p style="text-indent: 18.0pt;">
  <span style="background: yellow;">g</span>&nbsp; global&nbsp; <span style="font-family: 新宋体;">表示全局替换</span>
</p>

### <span id="121_clsnclsnedu">1.2.1 <span style="font-family: 新宋体;">将</span>clsn<span style="font-family: 新宋体;">替换程</span>clsnedu</span>

<p style="margin-left: 21.0pt;">
  &<span style="font-family: 新宋体;">表示前面找到的东西。</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed 's#clsn#&666#g' person.txt</span>
101<span style="color: #000000;">,clsn666,CEO
</span>102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

### <span id="122">1.2.2 <span style="font-family: 新宋体;">把文件中的数字都替换成</span><num><span style="font-family: 新宋体;">样式。</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed 's#[0-9]#&lt;&>#g' person.txt</span>
&lt;1>&lt;0>&lt;1><span style="color: #000000;">,clsn,CEO
</span>&lt;1>&lt;0>&lt;2><span style="color: #000000;">,znix,CTO
</span>&lt;1>&lt;0>&lt;3><span style="color: #000000;">,Nmtui,COO
</span>&lt;1>&lt;0>&lt;4><span style="color: #000000;">,yy,CFO
</span>&lt;1>&lt;0>&lt;5>,hehe,CIO</pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; g<span style="font-family: 新宋体;">表示把</span>sed<span style="font-family: 新宋体;">命令找到的内容进行替换，<span style="background: lime;">不加</span></span><span style="background: lime;">g </span><span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">只替换找到的第一个</span><span style="font-family: 新宋体;">。</span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed 's#[0-9]#&lt;&>#' person.txt</span>
&lt;1>01<span style="color: #000000;">,clsn,CEO
</span>&lt;1>02<span style="color: #000000;">,znix,CTO
</span>&lt;1>03<span style="color: #000000;">,Nmtui,COO
</span>&lt;1>04<span style="color: #000000;">,yy,CFO
</span>&lt;1>05,hehe,CIO</pre>
</div>

### <span id="123">1.2.3 <span style="font-family: 新宋体;">把前面正则表达式找到的<span style="background: lime;">第二列</span>内容进行替换</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed 's#[0-9]#&lt;&>#2' person.txt</span>
1&lt;0>1<span style="color: #000000;">,clsn,CEO
</span>1&lt;0>2<span style="color: #000000;">,znix,CTO
</span>1&lt;0>3<span style="color: #000000;">,Nmtui,COO
</span>1&lt;0>4<span style="color: #000000;">,yy,CFO
</span>1&lt;0>5,hehe,CIO</pre>
</div>

### <span id="124">1.2.4 <span style="font-family: 新宋体;">把前面正则表达式找到的<span style="background: lime;">第二列以后</span>内容进行替换</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed 's#[0-9]#&lt;&>#2g' person.txt</span>
1&lt;0>&lt;1><span style="color: #000000;">,clsn,CEO
</span>1&lt;0>&lt;2><span style="color: #000000;">,znix,CTO
</span>1&lt;0>&lt;3><span style="color: #000000;">,Nmtui,COO
</span>1&lt;0>&lt;4><span style="color: #000000;">,yy,CFO
</span>1&lt;0>&lt;5>,hehe,CIO</pre>
</div>

## <span id="13">1.3 <span style="font-family: 新宋体;">单引号</span> <span style="font-family: 新宋体;">双引号</span> <span style="font-family: 新宋体;">不加引号的区别</span></span>

### <span id="131_nbsp">1.3.1 <span style="font-family: 新宋体;">单引号：</span>&nbsp;<span style="font-family: 新宋体;">所见即所得</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo '$LANG $(hostname) {1..3}'</span>
$LANG $(hostname) {1..3}</pre>
</div>

### <span id="132_nbsp">1.3.2 <span style="font-family: 新宋体;">双引号：</span>&nbsp;<span style="font-family: 新宋体;">对特殊符号进行解析</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo "$LANG $(hostname) {1..3}"</span>
en_US.UTF-8 znix {1..3}</pre>
</div>

### <span id="133">1.3.3 <span style="font-family: 新宋体;">不加引号：支持通配符</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo $LANG $(hostname) {1..3}</span>
en_US.UTF-8 znix 1 2 3</pre>
</div>

## <span id="14_sed">1.4 sed<span style="font-family: 新宋体;">与变量</span></span>

### <span id="141">1.4.1 <span style="font-family: 新宋体;">在变量中放入一行内容</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> a=hello</span>
[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> a='hello world'</span>
[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo $a</span>
hello world</pre>
</div>

### <span id="142">1.4.2 <span style="font-family: 新宋体;">查看下文件的内容</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat person.txt</span>
101<span style="color: #000000;">,clsn,CEO
</span>102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

### <span id="143">1.4.3 <span style="font-family: 新宋体;">定义一个变量，对变量进行替换</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">双引号里面，能够对变量进行解析</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sub=clsn</span>
[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed "s#$sub#znix#g" person.txt</span>
101<span style="color: #000000;">,znix,CEO
</span>102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

### <span id="144">1.4.4 <span style="font-family: 新宋体;">将两个变量分别放置，用变量替换变量。</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sub=clsn</span>
[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> aim=znix</span>
[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed "s#$sub#$aim#g" person.txt</span>
101<span style="color: #000000;">,znix,CEO
</span>102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

## <span id="15">1.5 <span style="font-family: 新宋体;">【<span style="background: aqua;">企业案例</span>】系统开机启动项优化</span></span>

<p style="text-indent: 18.0pt;">
  <span style="font-family: 新宋体;">将</span>chkconfig<span style="font-family: 新宋体;">中的除</span> sshd|network|crond|rsyslog|sysstat <span style="font-family: 新宋体;">之外的全部关闭。</span>
</p>

### <span id="151">1.5.1 <span style="font-family: 新宋体;">各项服务的含义</span></span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">crond   定时任务
sshd    远程连接服务
network 网络
sysstat 系统工具
rsyslog 系统日志服务 system log
        centos </span>6.x 7<span style="color: #000000;">.x 中系统日志服务为rsyslog
        centos </span>5.x 里面系统日志服务为 syslog</pre>
</div>

### <span id="152">1.5.2 <span style="font-family: 新宋体;">第一步把想要保留的排除走</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chkconfig |sed -r  '/sshd|network|crond|rsyslog|sysstat/d'</span>
abrt-ccpp      0:off   1:off   2:off   3:off   4:off   5:off   6<span style="color: #000000;">:off
abrtd          0:off   </span>1:off   2:off   3:off   4:off   5:off   6<span style="color: #000000;">:off
acpid          0:off   </span>1:off   2:off   3:off   4:off   5:off   6<span style="color: #000000;">:off
atd            0:off   </span>1:off   2:off   3:off   4:off   5:off   6<span style="color: #000000;">:off
auditd         0:off   </span>1:off   2:off   3:off   4:off   5:off   6<span style="color: #000000;">:off
blk</span>-availability    0:off   1:on    2:off   3:off   4:off   5:off   6<span style="color: #000000;">:off
cpuspeed       0:off   </span>1:on    2:off   3:off   4:off   5:off   6<span style="color: #000000;">:off
&hellip;&hellip;</span></pre>
</div>

### <span id="153">1.5.3 <span style="font-family: 新宋体;">第二步取出服务的名字</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chkconfig |sed -r  '/sshd|network|crond|rsyslog|sysstat/d'|sed -r 's#(^.*)0:.*#\1#g'  </span>
abrt-<span style="color: #000000;">ccpp     
abrtd         
acpid         
atd           
auditd        
blk</span>-<span style="color: #000000;">availability   
cpuspeed      
&hellip;&hellip;</span></pre>
</div>

### <span id="154">1.5.4 <span style="font-family: 新宋体;">第三步拼接出想要的形状</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chkconfig |sed -r  '/sshd|network|crond|rsyslog|sysstat/d'|sed -r 's#(^.*)0:.*#chkconfig \1 off #g'</span>
chkconfig abrt-<span style="color: #000000;">ccpp          off
chkconfig abrtd              off
chkconfig acpid              off
chkconfig atd                off
chkconfig auditd             off
chkconfig blk</span>-<span style="color: #000000;">availability  off
&hellip;&hellip;</span></pre>
</div>

### <span id="155_bash">1.5.5 <span style="font-family: 新宋体;">第四步交给</span>bash<span style="font-family: 新宋体;">执行</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chkconfig |sed -r  '/sshd|network|crond|rsyslog|sysstat/d'|sed -r 's#(^.*)0:.*#\1#g|bash</span></pre>
</div>

### <span id="156">1.5.6 <span style="font-family: 新宋体;">第五步检查结果</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chkconfig |grep "3:on"</span>
crond          0:off   1:off   2:on    3:on    4:on    5:on    6<span style="color: #000000;">:off
network        0:off   </span>1:off   2:on    3:on    4:on    5:on    6<span style="color: #000000;">:off
rsyslog        0:off   </span>1:off   2:on    3:on    4:on    5:on    6<span style="color: #000000;">:off
sshd           0:off   </span>1:off   2:on    3:on    4:on    5:on    6<span style="color: #000000;">:off
sysstat        0:off   </span>1:on    2:on    3:on    4:on    5:on    6:off</pre>
</div>

### <span id="157">1.5.7 <span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">简化命令</span></span>

#### <span id="1571nbsp">1.5.7.1&nbsp;<span style="font-family: 新宋体;">示例一：</span></span>

<span style="font-family: 新宋体;">&nbsp;<span class="cnblogs_code">[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chkconfig |sed -r '/sshd|network|crond|rsyslog|sysstat/d;s#(^.*)0:.*#chkconfig \1 off#g'|bash</span></span>&nbsp;</span>

#### <span id="1572nbsp">1.5.7.2&nbsp;<span style="font-family: 新宋体;">示例二</span></span>

<span style="font-family: 新宋体;">&nbsp;<span class="cnblogs_code">[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> chkconfig |sed -rn '/sshd|network|crond|rsyslog|sysstat/!s#^(.*)0:.*#chkconfig \1 off#gp'|bash</span></span>&nbsp;</span>

## <span id="16">1.6 &<span style="font-family: 新宋体;">符号的使用</span></span>

<p style="text-indent: 18.0pt;">
  <span style="background: lime;">&</span><span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">符号找东西会</span><span style="font-family: 新宋体; times new roman"4times new roman"; background: aqua;">把剩下的显示出来</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo clsn123</span>
<span style="color: #000000;">clsn123

[root@znix </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo clsn123|sed 's#.*1#&#g'</span>
<span style="color: #000000;">clsn123

[root@znix </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo clsn123|sed 's#.*1#{&}#g'</span>
{clsn1}23</pre>
</div>

## <span id="17_persontxt_yy">1.7 <span style="font-family: 新宋体;">【<span style="background: aqua;">练习题</span>】把</span>person.txt <span style="font-family: 新宋体;">中包含</span>yy<span style="font-family: 新宋体;">的行</span> <span style="font-family: 新宋体;">这一行里面的数字替换为空</span></span>

### <span id="171">1.7.1 <span style="font-family: 新宋体;">文件内容</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> cat person.txt</span>
101<span style="color: #000000;">,clsn,CEO
</span>102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
</span>104<span style="color: #000000;">,yy,CFO
</span>105,hehe,CIO</pre>
</div>

### <span id="172_yyyysg">1.7.2 /yy/<span style="font-family: 新宋体;">查找</span>yy<span style="font-family: 新宋体;">这行，使用</span>s###g<span style="font-family: 新宋体;">对文件内容进行替换</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed -r '/yy/s#[0-9]##g' person.txt</span>
101<span style="color: #000000;">,clsn,CEO
</span>102<span style="color: #000000;">,znix,CTO
</span>103<span style="color: #000000;">,Nmtui,COO
,yy,CFO
</span>105,hehe,CIO</pre>
</div>

### <span id="173_yy">1.7.3 <span style="font-family: 新宋体;">将不包含</span>yy<span style="font-family: 新宋体;">的行进行替换</span></span>

<p style="margin-left: 21.0pt;">
  -n <span style="font-family: 新宋体;">取消默认输出，所以</span>yy<span style="font-family: 新宋体;">那一行不会输出</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sed -rn '/yy/!s#[0-9]##gp' person.txt</span>
<span style="color: #000000;">,clsn,CEO
,znix,CTO
,Nmtui,COO
,hehe,CIO</span></pre>
</div>

## <span id="18_sedinfo">1.8 <span style="font-family: 新宋体;">查看</span>sed<span style="font-family: 新宋体;">更多的帮助信息<span style="background: aqua;">【</span></span><span style="background: aqua;">info</span><span style="font-family: 新宋体; times new roman"4times new roman";background: aqua;">】</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> info sed</span>
faq 经常遇到的问题，经常有人问的问题</pre>
</div>

# <span id="2_shell">第2章 shell <span style="font-family: 新宋体;">编程</span></span>

## <span id="21_shell">2.1 <span style="font-family: 新宋体;">什么是</span>shell</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">命令大礼包</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">判断</span> <span style="font-family: 新宋体;">循环</span>

### <span id="211_shellnbspnbspnbspnbsp">2.1.1 shell<span style="font-family: 新宋体;">的作用：</span>&nbsp;&nbsp;&nbsp;&nbsp;</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">为重复性的工作节约时间，省事</span>

## <span id="22">2.2 <span style="font-family: 新宋体;">如何<span style="background: aqua;">查看当前用户的命令解释器</span></span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo $SHELL</span>
/bin/bash</pre>
</div>

#### <span id="2211nbspshell_sh">2.2.1.1&nbsp;shell<span style="font-family: 新宋体;">修改为</span> sh <span style="font-family: 新宋体;">会有一些问题</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> sh</span>
sh-4.1<span style="color: #008000;">#</span><span style="color: #008000;"> bash</span>
<span style="color: #000000;">
[root@znix </span>~]<span style="color: #008000;">#</span></pre>
</div>

## <span id="23_shell">2.3 <span style="font-family: 新宋体;">书写</span>shell<span style="font-family: 新宋体;">脚本的要求</span></span>

<p style="text-indent: 18.0pt;">
  <span style="font-family: 新宋体;">位置<span style="background: yellow;">统一存放，便于管理</span></span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> pwd</span>
/server/scripts</pre>
</div>

<span style="font-family: 新宋体;">脚本内容</span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> vim show.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash    ##使用的命令解释器</span><span style="color: #008000;">
#</span><span style="color: #008000;">filename:show.sh  ##文件名</span><span style="color: #008000;">
#</span><span style="color: #008000;">desc: miaoshu      ##描述</span>

/sbin/ifconfig eth0|awk -F <span style="color: #800000;">"</span><span style="color: #800000;">[: ]+</span><span style="color: #800000;">"</span> <span style="color: #800000;">'</span><span style="color: #800000;">NR==2{print $4}</span><span style="color: #800000;">'</span></pre>
</div>

<span style="font-family: 新宋体;">脚本中尽量<span style="background: yellow;">使用命令的绝对路径</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> sh show.sh</span>
10.0.0.201</pre>
</div>

## <span id="24_shell">2.4 shell<span style="font-family: 新宋体;">脚本之变量</span></span>

### <span id="241">2.4.1 <span style="font-family: 新宋体;">什么是变量</span></span>

<span style="font-family: 新宋体;">举个栗子：</span>

<div class="cnblogs_code">
  <pre><span style="color: #000000;">znix                变量的名字
$znix               查看变量里的内容
znix</span>=<span style="color: #800000;">"</span><span style="color: #800000;">access</span><span style="color: #800000;">"</span>       修改变量的内容</pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">修改变量的时候最好使用引号将内容引起来。</span>

### <span id="242">2.4.2 <span style="font-family: 新宋体;">环境变量（全局变量）</span></span>

#### <span id="2421nbsp">2.4.2.1&nbsp;<span style="font-family: 新宋体;">特点</span></span>

<p style="margin-left: 36.0pt;">
  1<span style="font-family: 新宋体;">）大写</span>
</p>

<p style="margin-left: 36.0pt;">
  2<span style="font-family: 新宋体;">）在</span>linux<span style="font-family: 新宋体;">里面都生效</span>
</p>

#### <span id="2422nbsp">2.4.2.2&nbsp;<span style="font-family: 新宋体;">查看系统中的环境变量</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">使用</span>env<span style="font-family: 新宋体;">命令，可以列出系统中，所有的变量</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> env</span>
HOSTNAME=<span style="color: #000000;">znix
TERM</span>=<span style="color: #000000;">linux
SHELL</span>=/bin/<span style="color: #000000;">bash
HISTSIZE</span>=1000<span style="color: #000000;">
SSH_CLIENT</span>=10.0.0.1 3156 22<span style="color: #000000;">
SSH_TTY</span>=/dev/pts/1<span style="color: #000000;">
USER</span>=<span style="color: #000000;">root
&hellip;&hellip;</span></pre>
</div>

## <span id="25">2.5 <span style="font-family: 新宋体;">手动创建一个环境变量</span></span>

### <span id="251">2.5.1 <span style="font-family: 新宋体;">创建一个普通变量</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> ZNIX=clsn</span>
[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> echo $ZNIX</span>
clsn</pre>
</div>

### <span id="252">2.5.2 <span style="font-family: 新宋体;">临时创建环境变量</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">关键：</span><span style="background: lime;">export</span> <span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">是创建环境变量使用的</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> export ZNIX=clsn</span>
[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> env|grep clsn</span>
ZNIX=clsn</pre>
</div>

### <span id="253">2.5.3 <span style="font-family: 新宋体;">让环境变量永久生效</span></span>

#### <span id="2531nbsp_export_ZNIXclsn_etcprofile">2.5.3.1&nbsp;<span style="font-family: 新宋体;">将</span> export ZNIX=clsn <span style="font-family: 新宋体;">放入</span> /etc/profile</span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> echo 'export ZNIX=clsn' >> /etc/profile</span></pre>
</div>

#### <span id="2532nbsp_source_etcprofile">2.5.3.2&nbsp;<span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">让写入的内容生效</span> <span style="font-family: 新宋体;">，使用</span>source /etc/profile</span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> source /etc/profile</span></pre>
</div>

## <span id="26_shell">2.6 shell<span style="font-family: 新宋体;">脚本与变量</span></span>

### <span id="261">2.6.1 <span style="font-family: 新宋体;">脚本的内容：</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> cat show2.sh</span>

<span style="color: #008000;">#</span><span style="color: #008000;">!/bin/bash</span>
<span style="color: #000000;">
echo $a</span></pre>
</div>

### <span id="262_shell">2.6.2 shell<span style="font-family: 新宋体;">与普通变量</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">只在当前的</span>shell<span style="font-family: 新宋体;">中生效，<span style="background: lime;">执行脚本的时候</span>，很产生一个新的</span>shell<span style="font-family: 新宋体;">环境（<span style="background: lime;">子</span></span><span style="background: lime;">shell</span><span style="font-family: 新宋体;">）。普通变量不能对系统中其他的</span>shell<span style="font-family: 新宋体;">环境产生影响，<span style="background: lime;">普通变量没用了</span>。</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> a=100</span>
<span style="color: #000000;">
[root@znix scripts]</span><span style="color: #008000;">#</span><span style="color: #008000;"> sh show2.sh</span></pre>
</div>

### <span id="263_shell">2.6.3 shell<span style="font-family: 新宋体;">与全局变量</span></span>

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">全局变量对系统中所有的</span>shell<span style="font-family: 新宋体;">环境都有效，</span>export <span style="font-family: 新宋体;">在系统任何一个地方都承认他。</span>
</p>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> export a=100</span>
[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> sh show2.sh</span>
100</pre>
</div>

## <span id="27">2.7 <span style="font-family: 新宋体;">与用户有关的环境变量配置文件</span>/<span style="font-family: 新宋体;">目录</span></span>

<p style="text-indent: 18.0pt;">
  <span style="color: #ffc000; background: darkblue;">/etc/motd </span><span style="font-family: 新宋体;">用户登陆到系统后显示的信息</span>
</p>

### <span id="271">2.7.1 <span style="font-family: 新宋体;">全局环境变量配置文件</span></span>

<div class="cnblogs_code">
  <pre>    /etc/<span style="color: #000000;">profile
    </span>/etc/<span style="color: #000000;">bashrc
    </span>/etc/profile.d/     （目录）</pre>
</div>

### <span id="272">2.7.2 <span style="font-family: 新宋体;">用户环境变量</span></span>

<div class="cnblogs_code">
  <pre>    ~/<span style="color: #000000;">.bash_proflie
    </span>~/.bashrc</pre>
</div>

## <span id="28">2.8 <span style="font-family: 新宋体;">变量命名规则</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 新宋体;">变量名可以是字母、数字或下划线</span> <span style="font-family: 新宋体;">的组合。</span>
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">但是<span style="background: yellow;">不能是以数字开头</span>。</span>&nbsp;&nbsp;&nbsp;&nbsp;

<p style="text-indent: 21.0pt;">
  <span style="font-family: 新宋体; times new roman"4times new roman"; background: lime;">可以以下划线开头</span><span style="font-family: 新宋体;">。</span>
</p>

### <span id="281">2.8.1 <span style="font-family: 新宋体; times new roman"4times new roman";background: aqua;">取变量的时候将变量用</span><span style="background: aqua;">{ } </span><span style="font-family: 新宋体; times new roman"4times new roman";background: aqua;">包起来</span></span>

<div class="cnblogs_code">
  <pre>[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> www=123</span>
[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo $www</span>
123<span style="color: #000000;">

[root@znix </span>~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo $wwwday</span>
[root@znix ~]<span style="color: #008000;">#</span><span style="color: #008000;"> echo ${www}day</span>
123day</pre>
</div>

## <span id="29_shell">2.9 shell<span style="font-family: 新宋体;">中的特殊变量</span></span>

### <span id="291__0">2.9.1 $<span style="font-family: 新宋体;">数字</span> <span style="font-family: 新宋体;">与</span> $0</span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> cat para.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash</span>
echo $1 $2 $3<span style="color: #000000;"> ... $0

[root@znix scripts]</span><span style="color: #008000;">#</span><span style="color: #008000;"> sh para.sh  a b c</span>
a b c ... para.sh</pre>
</div>

<p style="text-indent: 21.0pt;">
  <span style="background: lime;">$1 </span>&nbsp;<span style="font-family: 新宋体;">添加到</span>Shell<span style="font-family: 新宋体;">的各参数值。</span>$1<span style="font-family: 新宋体;">是第</span>1<span style="font-family: 新宋体;">参数、</span>$2<span style="font-family: 新宋体;">是第</span>2<span style="font-family: 新宋体;">参数</span>
</p>

<p style="text-indent: 21.0pt;">
  <span style="background: lime;">$0 </span>&nbsp;<span style="font-family: 新宋体;">脚本文件的名字</span>
</p>

### <span id="292">2.9.2 [<span style="font-family: 新宋体; times new roman"4times new roman";background: aqua;">练习</span>] <span style="font-family: 新宋体;">使用变量写一个简单的计算器。</span></span>

#### <span id="2921nbsp">2.9.2.1&nbsp;<span style="font-family: 新宋体;">先写出一个模板。</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  cal.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash</span>
<span style="color: #000000;">
echo </span>1+2|<span style="color: #000000;">bc

[root@znix scripts]</span><span style="color: #008000;">#</span><span style="color: #008000;"> sh cal.sh</span>
3</pre>
</div>

#### <span id="2922nbsp">2.9.2.2&nbsp;<span style="font-family: 新宋体;">将期中的内容替换成为变量</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> cat  cal.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash</span>
<span style="color: #000000;">
echo $</span>1 + $2|<span style="color: #000000;">bc

[root@znix scripts]</span><span style="color: #008000;">#</span><span style="color: #008000;"> sh cal.sh 100 50</span>
150</pre>
</div>

#### <span id="2923nbsp">2.9.2.3&nbsp;<span style="font-family: 新宋体;">将里面的计算方式增加。</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> vim cal.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash</span>
echo $1 + $2|<span style="color: #000000;">bc
echo $</span>1 - $2|<span style="color: #000000;">bc
echo $</span>1*$2|bc      <span style="color: #008000;">#</span><span style="color: #008000;">## *在这里有不能有空格</span>
echo $1 / $2|<span style="color: #000000;">bc
echo $</span>1 ^ $2|bc</pre>
</div>

#### <span id="2924nbsp">2.9.2.4&nbsp;<span style="font-family: 新宋体;">执行脚本，进行计算。</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> sh  cal.sh  4 6</span>
10
-2
24<span style="color: #000000;">
0
</span>4096</pre>
</div>

### <span id="293_awk">2.9.3 awk<span style="font-family: 新宋体;">的计算方法</span></span>

#### <span id="2931nbspawk_-v">2.9.3.1&nbsp;awk<span style="font-family: 新宋体;">使用</span> -v <span style="font-family: 新宋体;">参数</span> <span style="font-family: 新宋体;">指定变量。</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> awk -va=1 -vb=10 'BEGIN{print a/b }'</span>
0.1</pre>
</div>

#### <span id="2932nbspawk">2.9.3.2&nbsp;<span style="font-family: 新宋体;">将</span>awk<span style="font-family: 新宋体;">命令放入脚本中</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> tail -2 cal.sh</span><span style="color: #008000;">
#</span><span style="color: #008000;">!/bin/bash</span>
a=$1<span style="color: #000000;">
b</span>=$2<span style="color: #000000;">

awk </span>-vnum1=$a -vnum2=$b <span style="color: #800000;">'</span><span style="color: #800000;">BEGIN{print num1/num2}</span><span style="color: #800000;">'</span></pre>
</div>

#### <span id="2933nbsp">2.9.3.3&nbsp;<span style="font-family: 新宋体;">测试脚本，检查脚本的执行结果。</span></span>

<div class="cnblogs_code">
  <pre>[root@znix scripts]<span style="color: #008000;">#</span><span style="color: #008000;"> sh cal.sh 10 23</span>
0.434783</pre>
</div>

# <span id="3_vim">第3章 vim <span style="font-family: 新宋体;">高级使用技巧</span></span>

## <span id="31_vim">3.1 vim<span style="font-family: 新宋体;">中进行查找替换</span></span>

<table style="border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 169.85pt; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" valign="top" width="226">
      <p>
        <strong><span style="font-family: 新宋体; times new roman"4times new roman"; color: white;">命令</span></strong>
      </p>
    </td>
    
    <td style="width: 352.95pt; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" valign="top" width="471">
      <p>
        <strong><span style="font-family: 新宋体; times new roman"4times new roman"; color: white;">含义</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 169.85pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="226">
      <p>
        <strong>:4,$s#$1#$a#g&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </strong>
      </p>
    </td>
    
    <td style="width: 352.95pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="471">
      <p>
        <span style="font-family: 新宋体;">从第</span>4<span style="font-family: 新宋体;">行到最后一行进行替换</span>&nbsp;&nbsp;&nbsp;
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 169.85pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="226">
      <p>
        <strong>:5,$s#$1#$a#g&nbsp;&nbsp;&nbsp;&nbsp; </strong>
      </p>
    </td>
    
    <td style="width: 352.95pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="471">
      <p>
        <span style="font-family: 新宋体;">从第</span>5<span style="font-family: 新宋体;">行到最后一行进行替换</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 169.85pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="226">
      <p>
        <strong>:1,$s#$1#$a#g&nbsp;&nbsp;&nbsp;&nbsp; </strong>
      </p>
    </td>
    
    <td style="width: 352.95pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="471">
      <p>
        <span style="font-family: 新宋体;">从第一行到最后一行进行替换</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 169.85pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" valign="top" width="226">
      <p>
        <strong>:%s#$1#$a#g&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </strong>
      </p>
    </td>
    
    <td style="width: 352.95pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="471">
      <p>
        <span style="font-family: 新宋体;">从第一行到最后一行进行替换</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 169.85pt; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" valign="top" width="226">
      <p>
        <strong>:.s,$s#echo#sed#g&nbsp; </strong>
      </p>
    </td>
    
    <td style="width: 352.95pt; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="471">
      <p>
        <span style="font-family: 新宋体;">从当前行到最后一行（</span>. <span style="font-family: 新宋体;">表示当前行）</span>
      </p>
    </td>
  </tr>
</table>

&nbsp;3.2<a name="_Toc493689992"></a><span lang="EN-US">&nbsp;</span><span lang="EN-US">vim </span><span style="font-family: 新宋体;">快捷键</span>

&nbsp;

<p style="margin-left: 48.0pt;">
  <span style="background: lime;">ctrl + v </span>&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">按</span>d<span style="font-family: 新宋体;">批量删除</span>
</p>

<p style="margin-left: 48.0pt;">
  <span style="background: lime;">ctrl + v </span>&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">按</span>x<span style="font-family: 新宋体;">批量删除</span>
</p>

<p style="margin-left: 48.0pt;">
  <span style="background: lime;">dd&nbsp;&nbsp;&nbsp;&nbsp; </span>&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">删除光标所在行</span>
</p>

<p style="margin-left: 48.0pt;">
  <span style="background: lime;">dG&nbsp;&nbsp;&nbsp; </span>&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">删除光标所在行到最后一行</span>
</p>

<p style="margin-left: 48.0pt;">
  <span style="background: lime;">D &nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">光标所在位置到行尾的内容</span>
</p>

<p style="margin-left: 48.0pt;">
  <span style="background: lime;">x &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">删除光标所在位置的一个字符</span>
</p>

## <span id="vim">vim命令粘贴带#号或注释信息格式会出现混乱情况解决？</span>

<span style="background-color: #00ff00;"><strong>问题说明：</strong></span>  
　　每次复制代码时，如果代码里有 //或# 这样的注释就容易让格式乱掉，显示的内容不整齐，并不是所期望的显示格式。

<span style="background-color: #00ff00;"><strong>原因分析：</strong></span>  
　　是由于vim编辑命令的自动缩进功能所影响，因此粘贴带注释的代码时可以取消自动缩进

**<span style="background-color: #00ff00;">问题解决：</span>**  
　　vim在粘贴代码时会自动缩进，把代码搞得一团糟糕，甚至可能因为某行的一个注释造成后面的代码全部被注释掉；最初的解决办法为：用vi去打开文件再粘贴上去，但其实是可以对vim编辑器进行设置修改的。

<div class="cnblogs_code">
  <pre>vim clsn.txt #&lt;--<span style="color: #000000;">编辑一个文件
:</span><span style="color: #0000ff;">set</span> paste #&lt;--在vim的命令行模式输入，关闭vim缩进功能</pre>
</div>

　　说明：然后再进入插入模式粘贴，代码就不会被自动缩进了，可以敲代码的时候需要自动缩进，所以还需要改回来

<div class="cnblogs_code">
  <pre>:<span style="color: #0000ff;">set</span> nopaste #&lt;--开启vim缩进功能</pre>
</div>

　　比较方便的方法就是修改用户家目录下的 .vimrc配置文件：

<div class="cnblogs_code">
  <pre><span style="color: #0000ff;">set</span> pastetoggle=&lt;F9></pre>
</div>

　　说明：

　　　　以后在插入模式下，只要按F9键就可以快速切换自动缩进模式了

## <span id="nbspsedawk">&nbsp;用sed和awk实现将文本中的上下两行合并为一行</span>

　　文本内容

<div class="cnblogs_code">
  <pre>[root@MongoDB tmp]# cat -n /tmp/<span style="color: #000000;">test.txt 
     </span><span style="color: #800080;">1</span>    bss_data <span style="color: #800080;">1</span>
     <span style="color: #800080;">2</span>    Data  <span style="color: #800080;">1</span> <span style="color: #800080;">2</span> <span style="color: #800080;">3</span> <span style="color: #800080;">4</span> <span style="color: #800080;">5</span> <span style="color: #800080;">6</span> <span style="color: #800080;">7</span> 
     <span style="color: #800080;">3</span>    bss_data <span style="color: #800080;">2</span>
     <span style="color: #800080;">4</span>    Data  <span style="color: #800080;">1</span> <span style="color: #800080;">2</span> <span style="color: #800080;">3</span> <span style="color: #800080;">4</span> <span style="color: #800080;">5</span> <span style="color: #800080;">6</span> <span style="color: #800080;">7</span> </pre>
</div>

　　使用sed命令实现

<div class="cnblogs_code">
  <pre>[root@MongoDB tmp]#  sed -n <span style="color: #800000;">'</span><span style="color: #800000;">{N;s#\n#\t#p}</span><span style="color: #800000;">'</span> test.txt|cat -<span style="color: #000000;">n 
     </span><span style="color: #800080;">1</span>    bss_data <span style="color: #800080;">1</span>    Data  <span style="color: #800080;">1</span> <span style="color: #800080;">2</span> <span style="color: #800080;">3</span> <span style="color: #800080;">4</span> <span style="color: #800080;">5</span> <span style="color: #800080;">6</span> <span style="color: #800080;">7</span> 
     <span style="color: #800080;">2</span>    bss_data <span style="color: #800080;">2</span>    Data  <span style="color: #800080;">1</span> <span style="color: #800080;">2</span> <span style="color: #800080;">3</span> <span style="color: #800080;">4</span> <span style="color: #800080;">5</span> <span style="color: #800080;">6</span> <span style="color: #800080;">7</span> </pre>
</div>

> 　　N 命令，将下一行读入并附加到当前行后面，以 \n （换行符）分隔，一起存在模式缓冲区内。

　　awk命令实现

<div class="cnblogs_code">
  <pre>[root@MongoDB tmp]# awk <span style="color: #800000;">'</span><span style="color: #800000;">{tmp=$0;getline;print tmp"\t"$0}</span><span style="color: #800000;">'</span> test.txt|cat -<span style="color: #000000;">n
     </span><span style="color: #800080;">1</span>    bss_data <span style="color: #800080;">1</span>    Data  <span style="color: #800080;">1</span> <span style="color: #800080;">2</span> <span style="color: #800080;">3</span> <span style="color: #800080;">4</span> <span style="color: #800080;">5</span> <span style="color: #800080;">6</span> <span style="color: #800080;">7</span> 
     <span style="color: #800080;">2</span>    bss_data <span style="color: #800080;">2</span>    Data  <span style="color: #800080;">1</span> <span style="color: #800080;">2</span> <span style="color: #800080;">3</span> <span style="color: #800080;">4</span> <span style="color: #800080;">5</span> <span style="color: #800080;">6</span> <span style="color: #800080;">7</span> </pre>
</div>

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1_sed">第1章 sed命令详解</a><ul>
        <li>
          <a href="#11">1.1 查找固定的某一行</a><ul>
            <li>
              <a href="#111_awk">1.1.1 awk命令方法</a>
            </li>
            <li>
              <a href="#112_grep">1.1.2 grep方法</a>
            </li>
            <li>
              <a href="#113_sed">1.1.3 sed方法</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#12_sed">1.2 sed的替换</a><ul>
            <li>
              <a href="#121_clsnclsnedu">1.2.1 将clsn替换程clsnedu</a>
            </li>
            <li>
              <a href="#122">1.2.2 把文件中的数字都替换成样式。</a>
            </li>
            <li>
              <a href="#123">1.2.3 把前面正则表达式找到的第二列内容进行替换</a>
            </li>
            <li>
              <a href="#124">1.2.4 把前面正则表达式找到的第二列以后内容进行替换</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#13">1.3 单引号 双引号 不加引号的区别</a><ul>
            <li>
              <a href="#131_nbsp">1.3.1 单引号：&nbsp;所见即所得</a>
            </li>
            <li>
              <a href="#132_nbsp">1.3.2 双引号：&nbsp;对特殊符号进行解析</a>
            </li>
            <li>
              <a href="#133">1.3.3 不加引号：支持通配符</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#14_sed">1.4 sed与变量</a><ul>
            <li>
              <a href="#141">1.4.1 在变量中放入一行内容</a>
            </li>
            <li>
              <a href="#142">1.4.2 查看下文件的内容</a>
            </li>
            <li>
              <a href="#143">1.4.3 定义一个变量，对变量进行替换</a>
            </li>
            <li>
              <a href="#144">1.4.4 将两个变量分别放置，用变量替换变量。</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#15">1.5 【企业案例】系统开机启动项优化</a><ul>
            <li>
              <a href="#151">1.5.1 各项服务的含义</a>
            </li>
            <li>
              <a href="#152">1.5.2 第一步把想要保留的排除走</a>
            </li>
            <li>
              <a href="#153">1.5.3 第二步取出服务的名字</a>
            </li>
            <li>
              <a href="#154">1.5.4 第三步拼接出想要的形状</a>
            </li>
            <li>
              <a href="#155_bash">1.5.5 第四步交给bash执行</a>
            </li>
            <li>
              <a href="#156">1.5.6 第五步检查结果</a>
            </li>
            <li>
              <a href="#157">1.5.7 简化命令</a><ul>
                <li>
                  <a href="#1571nbsp">1.5.7.1&nbsp;示例一：</a>
                </li>
                <li>
                  <a href="#1572nbsp">1.5.7.2&nbsp;示例二</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#16">1.6 &符号的使用</a>
        </li>
        <li>
          <a href="#17_persontxt_yy">1.7 【练习题】把person.txt 中包含yy的行 这一行里面的数字替换为空</a><ul>
            <li>
              <a href="#171">1.7.1 文件内容</a>
            </li>
            <li>
              <a href="#172_yyyysg">1.7.2 /yy/查找yy这行，使用s###g对文件内容进行替换</a>
            </li>
            <li>
              <a href="#173_yy">1.7.3 将不包含yy的行进行替换</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#18_sedinfo">1.8 查看sed更多的帮助信息【info】</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2_shell">第2章 shell 编程</a><ul>
        <li>
          <a href="#21_shell">2.1 什么是shell</a><ul>
            <li>
              <a href="#211_shellnbspnbspnbspnbsp">2.1.1 shell的作用：&nbsp;&nbsp;&nbsp;&nbsp;</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#22">2.2 如何查看当前用户的命令解释器</a><ul>
            <li>
              <ul>
                <li>
                  <a href="#2211nbspshell_sh">2.2.1.1&nbsp;shell修改为 sh 会有一些问题</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#23_shell">2.3 书写shell脚本的要求</a>
        </li>
        <li>
          <a href="#24_shell">2.4 shell脚本之变量</a><ul>
            <li>
              <a href="#241">2.4.1 什么是变量</a>
            </li>
            <li>
              <a href="#242">2.4.2 环境变量（全局变量）</a><ul>
                <li>
                  <a href="#2421nbsp">2.4.2.1&nbsp;特点</a>
                </li>
                <li>
                  <a href="#2422nbsp">2.4.2.2&nbsp;查看系统中的环境变量</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#25">2.5 手动创建一个环境变量</a><ul>
            <li>
              <a href="#251">2.5.1 创建一个普通变量</a>
            </li>
            <li>
              <a href="#252">2.5.2 临时创建环境变量</a>
            </li>
            <li>
              <a href="#253">2.5.3 让环境变量永久生效</a><ul>
                <li>
                  <a href="#2531nbsp_export_ZNIXclsn_etcprofile">2.5.3.1&nbsp;将 export ZNIX=clsn 放入 /etc/profile</a>
                </li>
                <li>
                  <a href="#2532nbsp_source_etcprofile">2.5.3.2&nbsp;让写入的内容生效 ，使用source /etc/profile</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#26_shell">2.6 shell脚本与变量</a><ul>
            <li>
              <a href="#261">2.6.1 脚本的内容：</a>
            </li>
            <li>
              <a href="#262_shell">2.6.2 shell与普通变量</a>
            </li>
            <li>
              <a href="#263_shell">2.6.3 shell与全局变量</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#27">2.7 与用户有关的环境变量配置文件/目录</a><ul>
            <li>
              <a href="#271">2.7.1 全局环境变量配置文件</a>
            </li>
            <li>
              <a href="#272">2.7.2 用户环境变量</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#28">2.8 变量命名规则</a><ul>
            <li>
              <a href="#281">2.8.1 取变量的时候将变量用{ } 包起来</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#29_shell">2.9 shell中的特殊变量</a><ul>
            <li>
              <a href="#291__0">2.9.1 $数字 与 $0</a>
            </li>
            <li>
              <a href="#292">2.9.2 [练习] 使用变量写一个简单的计算器。</a><ul>
                <li>
                  <a href="#2921nbsp">2.9.2.1&nbsp;先写出一个模板。</a>
                </li>
                <li>
                  <a href="#2922nbsp">2.9.2.2&nbsp;将期中的内容替换成为变量</a>
                </li>
                <li>
                  <a href="#2923nbsp">2.9.2.3&nbsp;将里面的计算方式增加。</a>
                </li>
                <li>
                  <a href="#2924nbsp">2.9.2.4&nbsp;执行脚本，进行计算。</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#293_awk">2.9.3 awk的计算方法</a><ul>
                <li>
                  <a href="#2931nbspawk_-v">2.9.3.1&nbsp;awk使用 -v 参数 指定变量。</a>
                </li>
                <li>
                  <a href="#2932nbspawk">2.9.3.2&nbsp;将awk命令放入脚本中</a>
                </li>
                <li>
                  <a href="#2933nbsp">2.9.3.3&nbsp;测试脚本，检查脚本的执行结果。</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3_vim">第3章 vim 高级使用技巧</a><ul>
        <li>
          <a href="#31_vim">3.1 vim中进行查找替换</a>
        </li>
        <li>
          <a href="#vim">vim命令粘贴带#号或注释信息格式会出现混乱情况解决？</a>
        </li>
        <li>
          <a href="#nbspsedawk">&nbsp;用sed和awk实现将文本中的上下两行合并为一行</a>
        </li>
      </ul>
    </li>
  </ul>
</div>