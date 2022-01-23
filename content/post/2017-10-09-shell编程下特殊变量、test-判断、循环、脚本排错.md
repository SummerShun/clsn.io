---
title: 'shell编程下  特殊变量、test / [  ]判断、循环、脚本排错'
author: 惨绿少年
type: post
date: 2017-10-08T18:14:00+00:00
url: /clsn/lx960.html
Baidusubmit:
  - 1
views:
  - 203
specs_zan:
  - 1
categories:
  - Linux运维
  - Shell编程
  - 玩转Linux
  - 运维基本功

---
# <span id="1_shell">第1章 shell<span style="font-family: 新宋体;">中的特殊变量</span></span>

## <span id="11">1.1 $#</span>

<p style="text-indent: 18.0pt;">
  $# <span style="font-family: 新宋体;">表示参数的个数</span>
</p>

### <span id="111">1.1.1 <span style="font-family: 新宋体;">【<span style="background: lime;">示例</span>】脚本内容</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# cat /server/scripts/show2.sh
  </p>
  
  <p>
    #!/bin/bash
  </p>
  
  <p>
    echo $1 $2 <span style="background: lime;">$#</span>
  </p>
  
  <p>
    if [ $? == 0 ];then
  </p>
  
  <p>
    &nbsp;&nbsp; echo "OK"
  </p>
  
  <p>
    fi
  </p>
</div>

<p style="margin-left: 0cm; text-indent: 0cm;">
  实例1-1 <span style="font-family: 新宋体;">执行的不同结果</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sh&nbsp; /server/scripts/show2.sh&nbsp; <span style="background: lime;">1 2 3 4 6 </span>
  </p>
  
  <p>
    1 2 <span style="background: lime;">5</span>
  </p>
  
  <p>
    [root@znix ~]# sh&nbsp; /server/scripts/show2.sh&nbsp; <span style="background: lime;">aa bb</span>
  </p>
  
  <p>
    aa bb 2
  </p>
  
  <p>
    [root@znix ~]# sh&nbsp; /server/scripts/show2.sh &nbsp;<span style="background: lime;">aa bb cc</span>
  </p>
  
  <p>
    aa bb <span style="background: lime;">3</span>
  </p>
</div>

## <span id="12">1.2 $?</span>

<p style="text-indent: 18.0pt;">
  $?<span style="font-family: 新宋体;">表示：上一个命令执行后的状态</span>
</p>

<p style="margin-left: 42.0pt; text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">状态<span style="background: lime;">为</span></span><span style="background: lime;"></span> <span style="font-family: 新宋体;">表示执行<span style="background: lime;">正确</span></span>
</p>

<p style="margin-left: 42.0pt; text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">状态<span style="background: yellow;">非</span></span><span style="background: yellow;"></span> <span style="font-family: 新宋体;">表示执行<span style="background: yellow;">错误</span></span>
</p>

### <span id="121">1.2.1 <span style="font-family: 新宋体;">【<span style="background: lime;">示例</span>】</span>$?<span style="font-family: 新宋体;">的使用</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# aaa
  </p>
  
  <p>
    -bash: aaa: command not found
  </p>
  
  <p>
    [root@znix ~]# echo <span style="background: lime;">$?</span>
  </p>
  
  <p>
    <span style="background: yellow;">127</span>
  </p>
  
  <p>
    [root@znix ~]# sed
  </p>
  
  <p>
    Usage: sed [OPTION]...&nbsp;
  </p>
  
  <p>
    [root@znix ~]# echo <span style="background: lime;">$?</span>
  </p>
  
  <p>
    <span style="background: yellow;">4</span>
  </p>
  
  <p>
    [root@znix ~]# sh&nbsp; /server/scripts/show2.sh&nbsp; aa bb
  </p>
  
  <p>
    aa bb 2
  </p>
  
  <p>
    [root@znix ~]# echo <span style="background: lime;">$?</span>
  </p>
  
  <p>
    <span style="background: yellow;"></span>
  </p>
</div>

## <span id="13">1.3 <span style="font-family: 新宋体;">【了解】编译安装的过程</span></span>

<p style="margin-left: 42.0pt;">
  <span style="font-family: 新宋体;">切菜备菜</span> <span style="background: lime;">&nbsp;./configure</span>
</p>

<p style="margin-left: 42.0pt;">
  <span style="font-family: 新宋体;">炒菜</span>&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: lime;">&nbsp;make</span>
</p>

<p style="margin-left: 42.0pt;">
  <span style="font-family: 新宋体;">上菜</span>&nbsp;&nbsp;&nbsp;&nbsp; <span style="background: lime;">&nbsp;make install</span>
</p>

## <span id="14_---read">1.4 <span style="font-family: 新宋体;">如何向变量中存放内容</span>---<span style="font-family: 新宋体;">【</span><span style="background: aqua;">read</span><span style="font-family: 新宋体; times new roman"4times new roman";background: aqua;">命令</span><span style="font-family: 新宋体;">】</span></span>

### <span id="141_read">1.4.1 read<span style="font-family: 新宋体;">命令使用</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# read a
  </p>
  
  <p>
    <span style="font-family: 新宋体;">惨绿少年</span>
  </p>
  
  <p>
    [root@znix ~]# echo $a
  </p>
  
  <p>
    <span style="font-family: 新宋体;">惨绿少年</span>
  </p>
</div>

### <span id="142">1.4.2 <span style="font-family: 新宋体;">让执行命令后出现提示信息</span></span>

<p style="margin-left: 15.0pt; text-indent: 6.0pt;">
  -p <span style="font-family: 新宋体;">为显示提示信息。</span><span style="color: red;">p</span><span style="font-family: 新宋体; times new roman"4times new roman";color: red;">作为输出</span><span style="font-family: 新宋体;">，<span style="color: red;">一定要写在其他参数的最后面</span>。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# read <span style="background: lime;">-p "</span><span style="font-family: 新宋体;">请输入：</span>" a
  </p>
  
  <p>
    <span style="font-family: 新宋体;">请输入：呵呵</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  </p>
  
  <p>
    [root@znix ~]# echo <span style="background: lime;">$a</span>
  </p>
  
  <p>
    <span style="font-family: 新宋体;">呵呵</span>
  </p>
</div>

### <span id="143">1.4.3 <span style="font-family: 新宋体;">设置等待</span>(<span style="font-family: 新宋体;">超时</span>)<span style="font-family: 新宋体;">的时间</span></span>

<p style="text-indent: 21.0pt;">
  -t5&nbsp; <span style="font-family: 新宋体;">等待</span>5<span style="font-family: 新宋体;">秒</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# read <span style="background: lime;">-t5 <span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">-p</span> "</span><span style="font-family: 新宋体;">请输入密码：</span>" a
  </p>
  
  <p>
    <span style="font-family: 新宋体;">请输入密码：</span>
  </p>
  
  <p>
    [root@znix ~]#
  </p>
  
  <p>
    [root@znix ~]# read -t5 -p "<span style="font-family: 新宋体;">请输入密码：</span>" a
  </p>
  
  <p>
    <span style="font-family: 新宋体;">请输入密码：</span>123
  </p>
</div>

### <span id="144">1.4.4 <span style="font-family: 新宋体;">不显示输入的内容</span></span>

<p style="text-indent: 21.0pt;">
  -s&nbsp; <span style="font-family: 新宋体;">输入的时候不显示输入内容。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# read <span style="background: lime;">-s <span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">-t50</span> <span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">-p</span> "</span><span style="font-family: 新宋体;">请输入密码：</span>" a
  </p>
  
  <p>
    <span style="font-family: 新宋体;">请输入密码：</span>
  </p>
  
  <p>
    [root@znix ~]# echo $a
  </p>
  
  <p>
    admin
  </p>
</div>

## <span id="15_read">1.5 <span style="font-family: 新宋体;">将</span>read<span style="font-family: 新宋体;">命令运用到脚本上</span></span>

### <span id="151">1.5.1 <span style="font-family: 新宋体;">修改<span style="color: #548dd4;">计算器</span>脚本的内容，让他能够更智能。</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# vim&nbsp; /server/scripts/cal.sh
  </p>
  
  <p>
    #!/bin/bash
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    <span style="background: lime;">read -p </span>"input 1st num:" <span style="background: lime;">a</span>
  </p>
  
  <p>
    <span style="background: lime;">read -p </span>"input 2st num:" <span style="background: lime;">b</span>
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    &nbsp;
  </p>
</div>

### <span id="152">1.5.2 <span style="font-family: 新宋体;">测试脚本</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sh&nbsp; /server/scripts/cal.sh
  </p>
  
  <p>
    <span style="color: #7030a0;">input 1st num:</span>1
  </p>
  
  <p>
    <span style="color: #7030a0;">input 2st num:</span>2
  </p>
  
  <p>
    0.5
  </p>
  
  <p>
    3
  </p>
  
  <p>
    -1
  </p>
  
  <p>
    2
  </p>
</div>

### <span id="153">1.5.3 <span style="font-family: 新宋体;">简化命令</span></span>

#### <span id="1531nbspread">1.5.3.1&nbsp;read <span style="font-family: 新宋体;">可以同时读入多个字符。</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# read -p "<span style="font-family: 新宋体;">输入第一个</span> <span style="font-family: 新宋体;">第二关数字：</span>" a b
  </p>
  
  <p>
    <span style="font-family: 新宋体;">输入第一个</span> <span style="font-family: 新宋体;">第二关数字：</span><span style="background: lime;">123</span> <span style="background: lime;">456&nbsp;&nbsp; </span>
  </p>
  
  <p>
    [root@znix ~]# echo $a $b
  </p>
  
  <p>
    123 456
  </p>
</div>

#### <span id="1532nbsp">1.5.3.2&nbsp;<span style="font-family: 新宋体;">修改脚本的内容。</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">使用一条</span>read<span style="font-family: 新宋体;">命令，读取两个参数。</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# vim /server/scripts/cal.sh
  </p>
  
  <p>
    #!/bin/bash
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    <span style="background: lime;">read -p "input 1st&2st num:" a b</span>
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    awk -vnum1=$a -vnum2=$b 'BEGIN{print num1/num2}'
  </p>
  
  <p>
    awk -vnum1=$a -vnum2=$b 'BEGIN{print num1+num2}'
  </p>
  
  <p>
    awk -vnum1=$a -vnum2=$b 'BEGIN{print num1-num2}'
  </p>
  
  <p>
    awk -vnum1=$a -vnum2=$b 'BEGIN{print num1*num2}'
  </p>
  
  <p>
    "/server/scripts/cal.sh" 15L, 355C written&nbsp;
  </p>
</div>

#### <span id="1533nbsp">1.5.3.3&nbsp;<span style="font-family: 新宋体;">测试脚本。</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">⚠</span><span style="font-family: 新宋体; times new roman"4times new roman";color: red;">注意</span> <span style="font-family: 新宋体; times new roman"4times new roman";color: red;">：</span> <span style="font-family: 新宋体; times new roman"4times new roman";color: red;">同时传入两个参数的时候，参数之间要使用空格分割</span><span style="color: red;">.</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sh /server/scripts/cal.sh
  </p>
  
  <p>
    input 1st&2st num:1 2
  </p>
  
  <p>
    0.5
  </p>
  
  <p>
    3
  </p>
  
  <p>
    -1
  </p>
  
  <p>
    2
  </p>
</div>

# <span id="2">第2章 <span style="font-family: 新宋体;">判断一个</span> <span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">文件</span><span style="background: lime;">/</span><span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">目录</span> <span style="font-family: 新宋体;">是否存在</span></span>

## <span id="21">2.1 <span style="font-family: 新宋体;">几种思路</span>:</span>

<p style="margin-left: 18.0pt;">
  cat + $?'
</p>

<p style="margin-left: 18.0pt;">
  ls&nbsp; + $?
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; test<span style="font-family: 新宋体;">命令</span>

## <span id="22_test">2.2 test<span style="font-family: 新宋体;">命令实例</span></span>

<p style="margin-left: 21.0pt; text-indent: 21.0pt;">
  test<span style="font-family: 新宋体;">命令不会自己输出执行的结果</span>,<span style="font-family: 新宋体;">配合</span>$?<span style="font-family: 新宋体;">查询上一条命令是否执行成功</span>,<span style="font-family: 新宋体;">就能够判断是否存在这个文件或目录</span>.
</p>

### <span id="221">2.2.1 <span style="font-family: 新宋体;">判断一个文件是否存在</span></span>

<p style="margin-left: 21.0pt;">
  -f <span style="font-family: 新宋体;">为判断的对象是文件</span>,0<span style="font-family: 新宋体;">为存在</span>,1<span style="font-family: 新宋体;">为不存在</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# test <span style="background: lime;">-f /clsn/clsn.txt </span>
  </p>
  
  <p>
    [root@znix ~]# echo $?
  </p>
  
  <p>
    <span style="background: yellow;"></span>
  </p>
  
  <p>
    [root@znix ~]# test -f /clsn/clsn
  </p>
  
  <p>
    [root@znix ~]# echo $?
  </p>
  
  <p>
    <span style="background: yellow;">1</span>
  </p>
</div>

### <span id="222">2.2.2 <span style="font-family: 新宋体;">判断一个目录是否存在</span></span>

<p style="margin-left: 21.0pt;">
  -d <span style="font-family: 新宋体;">为对目录进行判断</span>,0<span style="font-family: 新宋体;">为存在</span>,1<span style="font-family: 新宋体;">为不存在</span>.
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# test <span style="background: lime;">-d /znix</span>
  </p>
  
  <p>
    [root@znix ~]# echo $?
  </p>
  
  <p>
    <span style="background: yellow;">1</span>
  </p>
  
  <p>
    [root@znix ~]# test `<span style="background: lime;">-d /clsn/</span>
  </p>
  
  <p>
    [root@znix ~]# echo $?
  </p>
  
  <p>
    <span style="background: yellow;"></span>
  </p>
</div>

## <span id="23__nbsp">2.3 <span style="font-family: 新宋体;">使用</span> <span style="background: lime;">[</span> &nbsp;<span style="background: lime;">]</span> <span style="font-family: 新宋体;">进行判断</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 'Segoe UI Emoji',sans-serif;">⚠</span><span style="font-family: 新宋体;">使用</span>[&nbsp; ]<span style="font-family: 新宋体;">的时候<span style="color: red;">注意中间的空格</span></span>,<span style="font-family: 新宋体;">两边都要有两个空格</span>
</p>

<p style="margin-left: 18.0pt;">
  [&nbsp; ]<span style="font-family: 新宋体;">与</span>test<span style="font-family: 新宋体;">命令的功能相似</span>,<span style="font-family: 新宋体;">可以进行判断</span>,<span style="font-family: 新宋体;">相比</span>test<span style="font-family: 新宋体;">命令更为便捷</span>
</p>

### <span id="231_nbsp">2.3.1 [&nbsp; ]<span style="font-family: 新宋体;">判断一个文件是否存在</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">与</span>test<span style="font-family: 新宋体;">命令一样</span> [&nbsp; ] <span style="font-family: 新宋体;">判断的结果也</span><span style="font-family: 新宋体;">为存在</span>,1<span style="font-family: 新宋体;">为不存在</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# [ <span style="background: lime;">-f</span> /zinx.txt ]
  </p>
  
  <p>
    [root@znix ~]# echo $?
  </p>
  
  <p>
    1
  </p>
  
  <p>
    [root@znix ~]# [ <span style="background: lime;">-f</span> /root/clsn.txt ]
  </p>
  
  <p>
    [root@znix ~]# echo $?
  </p>
  
  <p>
  </p>
</div>

### <span id="232">2.3.2 <span style="font-family: 新宋体;">判断成功后</span>,<span style="font-family: 新宋体;">执行下一个命令</span> &&</span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">判断文件</span>/<span style="font-family: 新宋体;">目录是否存在</span>,<span style="font-family: 新宋体;">判断成功执行后面的内容</span>,<span style="font-family: 新宋体;">输出</span>ok ;<span style="font-family: 新宋体;">判断失败</span>,<span style="font-family: 新宋体;">不执行后面的命令</span>.
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# [ -f /clsn/znix.txt ] <span style="background: lime;">&& echo "ok"</span>
  </p>
  
  <p>
    [root@znix ~]# [ -d /clsn ] <span style="background: lime;">&& echo "ok"</span>
  </p>
  
  <p>
    ok
  </p>
</div>

### <span id="233">2.3.3 <span style="font-family: 新宋体;">判断不成功后执行下一条命令</span> ||</span>

<p style="margin-left: 15.0pt; text-indent: 21.0pt;">
  <span style="font-family: 新宋体;">如果这个文件不存在，就创建</span> <span style="font-family: 新宋体;">（使用</span> || <span style="font-family: 新宋体;">）</span>, || <span style="font-family: 新宋体;">表示前面的执行错误了，执行后面的命令</span>.
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# [ <span style="background: aqua;">-f</span> /clsn/znix.txt ] <span style="background: lime;">||</span> touch /clsn/znix.txt
  </p>
  
  <p>
    [root@znix ~]# ls -l /clsn/znix.txt
  </p>
  
  <p>
    -rw-r--r-- 1 root root 0 Sep 20 10:31 /clsn/znix.txt
  </p>
</div>

#### <span id="2331nbsp1rootclsndir_nbspnbspnbspnbspnbsp">2.3.3.1&nbsp;[<span style="font-family: 新宋体;">示例</span>1]<span style="font-family: 新宋体;">如果</span>/root/clsndir <span style="font-family: 新宋体;">目录不存在就创建</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# [ <span style="background: aqua;">-d</span> /root/clsndir ] <span style="background: lime;">||</span> mkdir -p /root/clsndir
  </p>
  
  <p>
    [root@znix ~]# ls -ld /root/clsndir
  </p>
  
  <p>
    drwxr-xr-x 2 root root 4096 Sep 20 10:36 /root/clsndir
  </p>
</div>

#### <span id="2332nbsp2rootclsndir_nbspnbsp">2.3.3.2&nbsp;[<span style="font-family: 新宋体;">示例</span>2]<span style="font-family: 新宋体;">如果</span>/root/clsndir <span style="font-family: 新宋体;">目录不存在</span>,<span style="font-family: 新宋体;">就创建这个目录</span>&nbsp;&nbsp;</span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">使用</span> && ,<span style="font-family: 新宋体;">前面的执行成功</span>,<span style="font-family: 新宋体;">执行后面的</span>; ! <span style="font-family: 新宋体;">表示非</span>,<span style="font-family: 新宋体;">不存在</span>.
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# [ <span style="background: yellow;">!</span> -d /root/clsndir/1 ] <span style="background: lime;">&&</span> mkdir&nbsp; -p /root/clsndir/1
  </p>
  
  <p>
    [root@znix ~]# ll -d /root/clsndir/1
  </p>
  
  <p>
    drwxr-xr-x 2 root root 4096 Sep 20 10:41 /root/clsndir/1
  </p>
</div>

# <span id="3">第3章 <span style="font-family: 新宋体;">判断在脚本的使用</span></span>

### <span id="311">3.1.1 <span style="font-family: 新宋体;">中文示例</span>(<span style="font-family: 新宋体;">更好理解</span>)</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2; margin-left: 18.0pt; margin-right: 0cm;">
  <p style="margin-left: 0cm; text-indent: 31.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: 新宋体;">如果</span> [ <span style="font-family: 新宋体;">这个文件存在</span> ];<span style="font-family: 新宋体;">然后</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">提示文件存在</span>
  </p>
  
  <p style="margin-left: 0cm; text-indent: 31.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: 新宋体;">否则</span>
  </p>
  
  <p>
    &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">创建这个文件</span>
  </p>
  
  <p style="margin-left: 0cm; text-indent: 31.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="font-family: 新宋体;">果如</span>
  </p>
</div>

## <span id="32">3.2 [<span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">示例</span>]<span style="font-family: 新宋体;">判断文件是否存在</span>,<span style="font-family: 新宋体;">不存在就创建</span></span>

### <span id="321">3.2.1 <span style="font-family: 新宋体;">第一步</span> <span style="font-family: 新宋体;">按照格式书写判断语句</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    if [ <span style="background: lime;">-f /root/clsn.txt ];then</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; echo "<span style="font-family: 新宋体;">文件存在</span>"
  </p>
  
  <p>
    else
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; touch /root/clsn.txt
  </p>
  
  <p>
    fi
  </p>
</div>

## <span id="33">3.3 <span style="font-family: 新宋体;">第二步</span> <span style="font-family: 新宋体;">写入脚本：</span></span>

<p style="margin-left: 18.0pt;">
  <span style="font-family: 新宋体;">写入脚本的时候</span>,<span style="font-family: 新宋体;">注意脚本的基本格式</span>.
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# vim /server/scripts/if.sh
  </p>
  
  <p>
    #!/bin/bash
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    if [ -f /root/clsn.txt ];then
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; echo "<span style="font-family: 新宋体;">文件存在</span>"
  </p>
  
  <p>
    else
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; touch /root/clsn.txt
  </p>
  
  <p>
    fi
  </p>
</div>

### <span id="331">3.3.1 <span style="font-family: 新宋体;">第三步</span> <span style="font-family: 新宋体;">测试脚本</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sh&nbsp; /server/scripts/if.sh
  </p>
  
  <p>
    <span style="font-family: 新宋体;">文件存在</span>
  </p>
</div>

## <span id="34">3.4 <span style="font-family: 新宋体;">在脚本中进行判断的格式</span></span>

<div align="center">
  <table style="width: 74%; border-collapse: collapse; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
    <tr>
      <td style="width: 16.28%; border-top: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: 1pt solid #9bbb59; border-right: none; background: #9bbb59; padding: 0cm 5.4pt;" width="16%">
        <p style="text-align: center;" align="center">
          <strong><span style="font-family: 新宋体; times new roman"4times new roman";color: white;">符号</span></strong>
        </p>
      </td>
      
      <td style="width: 21.22%; border-top: solid #9BBB59 1.0pt; border-left: none; border-bottom: solid #9BBB59 1.0pt; border-right: none; background: #9BBB59; padding: 0cm 5.4pt 0cm 5.4pt;" width="21%">
        <p style="text-align: center;" align="center">
          <strong><span style="font-family: 新宋体; times new roman"4times new roman";color: white; background: lime;">参数</span></strong>
        </p>
      </td>
      
      <td style="width: 24.48%; border-top: solid #9BBB59 1.0pt; border-left: none; border-bottom: solid #9BBB59 1.0pt; border-right: none; background: #9BBB59; padding: 0cm 5.4pt 0cm 5.4pt;" width="24%">
        <p style="text-align: center;" align="center">
          <strong><span style="font-family: 新宋体; times new roman"4times new roman";color: white;">含义</span></strong>
        </p>
      </td>
      
      <td style="width: 38.02%; border-top: 1pt solid #9bbb59; border-right: 1pt solid #9bbb59; border-bottom: 1pt solid #9bbb59; border-left: none; background: #9bbb59; padding: 0cm 5.4pt;" width="38%">
        <p style="text-align: center;" align="center">
          <strong><span style="font-family: 新宋体; times new roman"4times new roman";color: white;">英文</span></strong>
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 16.28%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="16%">
        <p style="text-align: center;" align="center">
          <strong>>&nbsp;</strong>
        </p>
      </td>
      
      <td style="width: 21.22%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="21%">
        <p style="text-align: center;" align="center">
          <strong>-gt</strong>
        </p>
      </td>
      
      <td style="width: 24.48%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="24%">
        <p>
          <span style="font-family: 新宋体;">大于</span>&nbsp;&nbsp;
        </p>
      </td>
      
      <td style="width: 38.02%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="38%">
        <p>
          great than
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 16.28%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="16%">
        <p style="text-align: center;" align="center">
          <strong>>=</strong>
        </p>
      </td>
      
      <td style="width: 21.22%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="21%">
        <p style="text-align: center;" align="center">
          <strong>-ge</strong>
        </p>
      </td>
      
      <td style="width: 24.48%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="24%">
        <p>
          <span style="font-family: 新宋体;">大于等于</span>
        </p>
      </td>
      
      <td style="width: 38.02%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="38%">
        <p>
          great equal
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 16.28%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="16%">
        <p style="text-align: center;" align="center">
          <strong><&nbsp;</strong>
        </p>
      </td>
      
      <td style="width: 21.22%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="21%">
        <p style="text-align: center;" align="center">
          <strong>-lt</strong>
        </p>
      </td>
      
      <td style="width: 24.48%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="24%">
        <p>
          <span style="font-family: 新宋体;">小于</span>
        </p>
      </td>
      
      <td style="width: 38.02%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="38%">
        <p>
          less than
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 16.28%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="16%">
        <p style="text-align: center;" align="center">
          <strong><=</strong>
        </p>
      </td>
      
      <td style="width: 21.22%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="21%">
        <p style="text-align: center;" align="center">
          <strong>-le</strong>
        </p>
      </td>
      
      <td style="width: 24.48%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="24%">
        <p>
          <span style="font-family: 新宋体;">小于等于</span>
        </p>
      </td>
      
      <td style="width: 38.02%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="38%">
        <p>
          less than or equal
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 16.28%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; background: #eaf1dd; padding: 0cm 5.4pt;" width="16%">
        <p style="text-align: center;" align="center">
          <strong>==</strong>
        </p>
      </td>
      
      <td style="width: 21.22%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" width="21%">
        <p style="text-align: center;" align="center">
          <strong>-eq</strong>
        </p>
      </td>
      
      <td style="width: 24.48%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="24%">
        <p>
          <span style="font-family: 新宋体;">等于</span>&nbsp;&nbsp;
        </p>
      </td>
      
      <td style="width: 38.02%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; background: #EAF1DD; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="38%">
        <p>
          equal&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        </p>
      </td>
    </tr>
    
    <tr>
      <td style="width: 16.28%; border-right: 1pt solid #c2d69b; border-bottom: 1pt solid #c2d69b; border-left: 1pt solid #c2d69b; border-top: none; padding: 0cm 5.4pt;" width="16%">
        <p style="text-align: center;" align="center">
          <strong>!=</strong>
        </p>
      </td>
      
      <td style="width: 21.22%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" width="21%">
        <p style="text-align: center;" align="center">
          <strong>-ne / ! -eq</strong>
        </p>
      </td>
      
      <td style="width: 24.48%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="24%">
        <p>
          <span style="font-family: 新宋体;">不等于</span>
        </p>
      </td>
      
      <td style="width: 38.02%; border-top: none; border-left: none; border-bottom: solid #C2D69B 1.0pt; border-right: solid #C2D69B 1.0pt; padding: 0cm 5.4pt 0cm 5.4pt;" valign="top" width="38%">
        <p>
          not equal&nbsp;
        </p>
      </td>
    </tr>
  </table>
</div>

## <span id="35_cal2sh2">3.5 [<span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">实例</span>]<span style="font-family: 新宋体;">如果</span>cal2.sh<span style="font-family: 新宋体;">脚本的参数个数不等于</span>2<span style="font-family: 新宋体;">，就显示</span> "<span style="font-family: 新宋体;">命令错误</span>"</span>

### <span id="351">3.5.1 <span style="font-family: 新宋体;">书写脚本</span></span>

<p style="margin-left: 21.0pt;">
  !=&nbsp; <span style="font-family: 新宋体;">表示判断两个是否不相等</span>,<span style="font-family: 新宋体;">如果不想等就显示命令错误</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    #/bin/bash
  </p>
  
  <p>
    if [ $# != &nbsp;2 ];then
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; echo "<span style="font-family: 新宋体;">命令错误</span>"
  </p>
  
  <p>
    fi
  </p>
</div>

### <span id="352">3.5.2 <span style="font-family: 新宋体;">对脚本进行测试</span>.</span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">当参数的个数为</span>2<span style="font-family: 新宋体;">的时候</span>,<span style="font-family: 新宋体;">不会输出</span>,<span style="font-family: 新宋体;">个数不为二的时候输出</span> <span style="font-family: 新宋体;">命令错误</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sh /server/scripts/cal2.sh
  </p>
  
  <p>
    <span style="font-family: 新宋体;">命令错误</span>
  </p>
  
  <p>
    [root@znix ~]# sh /server/scripts/cal2.sh 1 2
  </p>
  
  <p>
    [root@znix ~]# sh /server/scripts/cal2.sh 1 2 3
  </p>
  
  <p>
    <span style="font-family: 新宋体;">命令错误</span>
  </p>
</div>

&nbsp;

## <span id="36_22">3.6 <span style="font-family: 新宋体;">修改之前的计算器，进行两个数字的加减乘除，在计算器前面加上参数个数判断。当输入的参数是</span>2<span style="font-family: 新宋体;">个的时候执行计算</span>,<span style="font-family: 新宋体;">不为</span>2<span style="font-family: 新宋体;">的时候显示参数错误</span>.</span>

### <span id="361">3.6.1 <span style="font-family: 新宋体;">书写脚本</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">当输入的参数不为</span>2<span style="font-family: 新宋体;">的时候</span> <span style="font-family: 新宋体;">执行</span> <span style="color: #e36c0a;">echo "Usage: NUM1 NUM2", </span><span style="font-family: 新宋体;">然后执行</span> <span style="color: #e36c0a;">exit..</span>
</p>

<p style="margin-left: 21.0pt;">
  exit <span style="font-family: 新宋体;">表示前面的执行完成后就结束</span>(<span style="font-family: 新宋体;">跳出</span>),<span style="font-family: 新宋体;">不会再执行脚本后面的内容</span>.
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# vim&nbsp; /server/scripts/cal2.sh&nbsp;&nbsp;&nbsp;
  </p>
  
  <p>
    #<span style="font-family: 新宋体;">！</span>/bin/bash
  </p>
  
  <p>
    <span style="color: #e36c0a;">if [ $# -ne 2 ];then</span>
  </p>
  
  <p>
    <span style="color: #e36c0a;">&nbsp;&nbsp;&nbsp; echo "<span style="background: lime;">Usage: NUM1 NUM2</span>"</span>
  </p>
  
  <p>
    <span style="color: #e36c0a;">&nbsp;&nbsp;&nbsp; exit</span>
  </p>
  
  <p>
    <span style="color: #e36c0a;">fi</span>
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    a=$1
  </p>
  
  <p>
    b=$2
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    awk -vh=$a -vz=$b 'BEGIN{print h/z}'
  </p>
  
  <p>
    awk -vh=$a -vz=$b 'BEGIN{print h+z}'
  </p>
  
  <p>
    awk -vh=$a -vz=$b 'BEGIN{print h-z}'
  </p>
  
  <p>
    awk -vh=$a -vz=$b 'BEGIN{print h*z}'
  </p>
</div>

### <span id="362">3.6.2 <span style="font-family: 新宋体;">测试脚本</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">当输入的参数个数不为</span>2<span style="font-family: 新宋体;">的时候执行</span>echo <span style="font-family: 新宋体;">命令</span>
</p>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">当输入的参数个数为</span>2<span style="font-family: 新宋体;">的时候进行计算</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sh&nbsp;&nbsp; /server/scripts/cal2.sh
  </p>
  
  <p>
    <span style="color: #e36c0a; background: lime;">Usage: NUM1 NUM2</span> &nbsp;&nbsp;<span style="background: lime;">##</span><span style="font-family: 新宋体; background: lime;">提示信息</span>
  </p>
  
  <p>
    [root@znix ~]# sh&nbsp;&nbsp; /server/scripts/cal2.sh 22 22
  </p>
  
  <p>
    1
  </p>
  
  <p>
    44
  </p>
  
  <p>
  </p>
  
  <p>
    484
  </p>
</div>

# <span id="4">第4章 <span style="font-family: 新宋体;">循环语句</span></span>

## <span id="41_20170501201705202017-05-01txt">4.1 [<span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">示例</span>]<span style="font-family: 新宋体;">修改系统时间为</span>20170501<span style="font-family: 新宋体;">到</span>20170520<span style="font-family: 新宋体;">，然后创建文件的名字为</span>2017-05-01.txt</span>

### <span id="411">4.1.1 <span style="font-family: 新宋体;">基础姿势</span></span>

#### <span id="4111nbspfor">4.1.1.1&nbsp;for<span style="font-family: 新宋体;">循环可以从后面的数组中读取内容进行操作</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">循环的格式</span> <span style="font-family: 新宋体;">：</span><span style="background: lime;">&nbsp; &nbsp;&nbsp;</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="background: lime;">for</span> hd <span style="background: lime;">in&nbsp; znix_{a..z}</span>
  </p>
  
  <p>
    <span style="background: lime;">do</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; echo $hd
  </p>
  
  <p>
    <span style="background: lime;">done</span>
  </p>
</div>

#### <span id="4112nbspfor">4.1.1.2&nbsp;<span style="font-family: 新宋体;">执行</span>for <span style="font-family: 新宋体;">循环语句</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">for</span> hd <span style="background: lime;">in</span>&nbsp; znix_{a..z}
  </p>
  
  <p>
    > <span style="background: lime;">do</span>
  </p>
  
  <p>
    >&nbsp;&nbsp;&nbsp;&nbsp; echo $hd
  </p>
  
  <p>
    > <span style="background: lime;">done</span>
  </p>
  
  <p>
    znix_a
  </p>
  
  <p>
    znix_b
  </p>
  
  <p>
    znix_c
  </p>
  
  <p>
    znix_d
  </p>
  
  <p>
    <span style="font-family: 新宋体;">&hellip;&hellip;</span>
  </p>
  
  <p>
    znix_z
  </p>
</div>

### <span id="412">4.1.2 <span style="font-family: 新宋体;">书写循环语句</span></span>

<p style="margin-left: 18.0pt;">
  do<span style="font-family: 新宋体;">里面可以两条命令同时执行</span>.
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="background: lime;">for</span> hd <span style="background: lime;">in {01..20}</span>
  </p>
  
  <p>
    <span style="background: lime;">do</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; date -s "201705$hd"
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; touch /tmp/date/$(date +%F).txt
  </p>
  
  <p>
    <span style="background: lime;">done</span>
  </p>
</div>

### <span id="413">4.1.3 <span style="font-family: 新宋体;">测试循环语句</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">for</span> hd <span style="background: lime;">in</span> {01..20}
  </p>
  
  <p>
    > <span style="background: lime;">do</span>
  </p>
  
  <p>
    >&nbsp;&nbsp;&nbsp;&nbsp; date -s "201705$hd"
  </p>
  
  <p>
    >&nbsp;&nbsp;&nbsp;&nbsp; touch /tmp/date/$(date +%F).txt
  </p>
  
  <p>
    > <span style="background: lime;">done</span>
  </p>
  
  <p>
    Mon May&nbsp; 1 00:00:00 CST 2017
  </p>
  
  <p>
    Tue May&nbsp; 2 00:00:00 CST 2017
  </p>
  
  <p>
    Wed May&nbsp; 3 00:00:00 CST 2017
  </p>
  
  <p>
    <span style="font-family: 新宋体;">&hellip;&hellip;</span>
  </p>
  
  <p>
    Sat May 20 00:00:00 CST 2017
  </p>
</div>

### <span id="414">4.1.4 <span style="font-family: 新宋体;">检查执行结果</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# ls /tmp/date/
  </p>
  
  <p>
    2017-05-01.txt&nbsp; 2017-05-05.txt&nbsp; 2017-05-09.txt&nbsp; 2017-05-13.txt&nbsp; 2017-05-17.txt
  </p>
  
  <p>
    2017-05-02.txt&nbsp; 2017-05-06.txt&nbsp; 2017-05-10.txt&nbsp; 2017-05-14.txt&nbsp; 2017-05-18.txt
  </p>
  
  <p>
    2017-05-03.txt&nbsp; 2017-05-07.txt&nbsp; 2017-05-11.txt&nbsp; 2017-05-15.txt&nbsp; 2017-05-19.txt
  </p>
  
  <p>
    2017-05-04.txt&nbsp; 2017-05-08.txt&nbsp; 2017-05-12.txt&nbsp; 2017-05-16.txt&nbsp; 2017-05-20.txt
  </p>
</div>

## <span id="42_linux-for">4.2 [<span style="font-family: 新宋体; times new roman"4times new roman";background: lime;">实例</span>]<span style="font-family: 新宋体; times new roman"4times new roman";color: red;">优化</span><span style="color: red;">linux</span><span style="font-family: 新宋体; times new roman"4times new roman";color: red;">开机启动项目</span>-for<span style="font-family: 新宋体;">循环方法</span></span>

### <span id="421">4.2.1 <span style="font-family: 新宋体;">第一步：确定目标</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    <span style="background: lime;">for</span> name <span style="background: lime;">in </span><span style="font-family: 新宋体;">要关闭的服务的名字</span>
  </p>
  
  <p>
    <span style="background: lime;">do</span>
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; chkconfig $name off
  </p>
  
  <p>
    <span style="background: lime;">done</span>
  </p>
</div>

### <span id="422">4.2.2 <span style="font-family: 新宋体;">第二步</span>:<span style="font-family: 新宋体;">取出要关闭服务的名字</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# chkconfig |egrep -v "sshd|network|crond|rsyslog|sysstat"|awk '{print $1}'
  </p>
  
  <p>
    abrt-ccpp
  </p>
  
  <p>
    abrtd
  </p>
  
  <p>
    acpid
  </p>
  
  <p>
    <span style="font-family: 新宋体;">&hellip;&hellip;</span>
  </p>
</div>

### <span id="423_for">4.2.3 <span style="font-family: 新宋体;">第三步</span> <span style="font-family: 新宋体;">把名字与</span>for<span style="font-family: 新宋体;">循环结合</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    for name in <span style="background: lime;">`chkconfig |egrep -v "sshd|network|crond|rsyslog|sysstat"|awk '{print $1}'<span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">`</span></span>
  </p>
  
  <p>
    do
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; chkconfig $name off
  </p>
  
  <p>
    done
  </p>
</div>

# <span id="5">第5章 <span style="font-family: 新宋体;">脚本执行排错</span></span>

## <span id="51_sh_-x">5.1 sh -x <span style="font-family: 新宋体;">显示脚本详细的执行过程</span></span>

&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">前面有</span> + &nbsp;&nbsp;&nbsp;&nbsp;<span style="font-family: 新宋体;">表示执行过的命令的</span>

&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">前面没有东西，表示输出到屏幕上的内容。</span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# <span style="background: lime;">sh -x</span> /server/scripts/cal2.sh
  </p>
  
  <p>
    + '[' 0 -ne 2 ']'
  </p>
  
  <p>
    + echo 'Usage: NUM1 NUM2'
  </p>
  
  <p>
    <span style="background: aqua;">Usage: NUM1 NUM2</span>
  </p>
  
  <p>
    + exit
  </p>
</div>

### <span id="511">5.1.1 <span style="font-family: 新宋体;">检查脚本运行中的错误</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sh -x /server/scripts/cal2.sh&nbsp; 10 20
  </p>
  
  <p>
    + '[' 2 -ne 2 ']'
  </p>
  
  <p>
    + a=10
  </p>
  
  <p>
    + b=20
  </p>
  
  <p>
    + awk -vh=10 -vz=20 'BEGIN{print h/z}'
  </p>
  
  <p>
    <span style="background: aqua;">0.5</span>
  </p>
  
  <p>
    + awk -vh=10 -vz=20 'BEGIN{print h+z}'
  </p>
  
  <p>
    <span style="background: aqua;">30</span>
  </p>
  
  <p>
    + awk -vh=10 -vz=20 'BEGIN{print h-z}'
  </p>
  
  <p>
    <span style="background: aqua;">-10</span>
  </p>
  
  <p>
    + awk -vh=10 -vz=20 'BEGIN{print h*z}'
  </p>
  
  <p>
    <span style="background: aqua;">200</span>
  </p>
</div>

# <span id="6_vim">第6章 vim <span style="font-family: 新宋体;">快捷键</span></span>

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

# <span id="7">第7章 <span style="font-family: 新宋体;">昨日回顾</span></span>

## <span id="71_sed">7.1 <span style="font-family: 新宋体;">【</span>sed<span style="font-family: 新宋体;">命令】删除文件中的空行或只有空格的行。</span></span>

### <span id="711">7.1.1 <span style="font-family: 新宋体;">文件内容</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# cat -A test.txt
  </p>
  
  <p>
    znix$
  </p>
  
  <p>
    &nbsp;&nbsp;&nbsp; $
  </p>
  
  <p>
    linux$
  </p>
  
  <p>
    $
  </p>
  
  <p>
    good$
  </p>
  
  <p>
    &nbsp; $
  </p>
  
  <p>
    n$
  </p>
</div>

### <span id="712_0">7.1.2 * <span style="font-family: 新宋体;">表示前一个字符出现了</span><span style="font-family: 新宋体;">次或以上</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">次的时候即为</span>^$ <span style="font-family: 新宋体;">表示空行，以上表示空格的行</span>
</p>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sed '/<span style="background: aqua;">^ <span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">*$</span>/d' test.txt </span>
  </p>
  
  <p>
    znix
  </p>
  
  <p>
    linux
  </p>
  
  <p>
    good
  </p>
  
  <p>
    n
  </p>
</div>

### <span id="713">7.1.3 +<span style="font-family: 新宋体;">表示连续出现至少一次</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sed -r '<span style="background: lime;">/<span style="background: aqua;">^</span> <span style="background: aqua;">+$</span><span style="background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">|^$/</span>d' test.txt</span>
  </p>
  
  <p>
    znix
  </p>
  
  <p>
    linux
  </p>
  
  <p>
    good
  </p>
  
  <p>
    n
  </p>
</div>

### <span id="714">7.1.4 <span style="font-family: 新宋体;">！表示取反</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sed -n '/^ *$/<span style="background: red;">!p' test.txt</span>
  </p>
  
  <p>
    znix
  </p>
  
  <p>
    linux
  </p>
  
  <p>
    good
  </p>
  
  <p>
    n
  </p>
</div>

### <span id="715">7.1.5 <span style="font-family: 新宋体;">同样用来取反</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p>
    [root@znix ~]# sed -nr '/^$|^ +$/<span style="background: red;">!p' test.txt</span>
  </p>
  
  <p>
    znix
  </p>
  
  <p>
    linux
  </p>
  
  <p>
    good
  </p>
  
  <p>
    n
  </p>
</div>

## <span id="72_shell">7.2 shell<span style="font-family: 新宋体;">编程中的变量</span></span>

### <span id="721_shell">7.2.1 <span style="font-family: 新宋体;">特殊变量（</span>shell<span style="font-family: 新宋体;">脚本中）</span></span>

<p style="margin-left: 21.0pt;">
  $1,$2...&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">用来传递参数</span>
</p>

<p style="margin-left: 21.0pt;">
  $0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">脚本文件的文件名</span>
</p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">脚本中参数的个数</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $?&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="font-family: 新宋体;">前一条命令是否执行成功</span>

### <span id="722">7.2.2 <span style="font-family: 新宋体;">全局变量（环境变量）</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">常用的</span> LANG&nbsp;&nbsp; PATH&nbsp; PS1 <span style="font-family: 新宋体;">&hellip;&hellip;</span>
</p>

### <span id="723">7.2.3 <span style="font-family: 新宋体;">普通变量</span></span>

<p style="margin-left: 21.0pt;">
  <span style="font-family: 新宋体;">自己定义的变量</span>
</p>

## <span id="73_shell">7.3 shell<span style="font-family: 新宋体;">编程中与用户有关的环境变量的文件和目录</span></span>

<div style="border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; background: #F2F2F2;">
  <p style="text-indent: 18pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    /etc/profile
  </p>
  
  <p style="text-indent: 18pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    <span style="background: lime;">/etc/profile.d</span>&nbsp;&nbsp;&nbsp; (<span style="font-family: 新宋体;">目录</span>)
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    /etc/bashrc
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    ~/.bashrc
  </p>
  
  <p style="text-indent: 21pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
    ~/.bash_profile
  </p>
</div>

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#1_shell">第1章 shell中的特殊变量</a><ul>
        <li>
          <a href="#11">1.1 $#</a><ul>
            <li>
              <a href="#111">1.1.1 【示例】脚本内容</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#12">1.2 $?</a><ul>
            <li>
              <a href="#121">1.2.1 【示例】$?的使用</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#13">1.3 【了解】编译安装的过程</a>
        </li>
        <li>
          <a href="#14_---read">1.4 如何向变量中存放内容---【read命令】</a><ul>
            <li>
              <a href="#141_read">1.4.1 read命令使用</a>
            </li>
            <li>
              <a href="#142">1.4.2 让执行命令后出现提示信息</a>
            </li>
            <li>
              <a href="#143">1.4.3 设置等待(超时)的时间</a>
            </li>
            <li>
              <a href="#144">1.4.4 不显示输入的内容</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#15_read">1.5 将read命令运用到脚本上</a><ul>
            <li>
              <a href="#151">1.5.1 修改计算器脚本的内容，让他能够更智能。</a>
            </li>
            <li>
              <a href="#152">1.5.2 测试脚本</a>
            </li>
            <li>
              <a href="#153">1.5.3 简化命令</a><ul>
                <li>
                  <a href="#1531nbspread">1.5.3.1&nbsp;read 可以同时读入多个字符。</a>
                </li>
                <li>
                  <a href="#1532nbsp">1.5.3.2&nbsp;修改脚本的内容。</a>
                </li>
                <li>
                  <a href="#1533nbsp">1.5.3.3&nbsp;测试脚本。</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#2">第2章 判断一个 文件/目录 是否存在</a><ul>
        <li>
          <a href="#21">2.1 几种思路:</a>
        </li>
        <li>
          <a href="#22_test">2.2 test命令实例</a><ul>
            <li>
              <a href="#221">2.2.1 判断一个文件是否存在</a>
            </li>
            <li>
              <a href="#222">2.2.2 判断一个目录是否存在</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#23__nbsp">2.3 使用 [ &nbsp;] 进行判断</a><ul>
            <li>
              <a href="#231_nbsp">2.3.1 [&nbsp; ]判断一个文件是否存在</a>
            </li>
            <li>
              <a href="#232">2.3.2 判断成功后,执行下一个命令 &&</a>
            </li>
            <li>
              <a href="#233">2.3.3 判断不成功后执行下一条命令 ||</a><ul>
                <li>
                  <a href="#2331nbsp1rootclsndir_nbspnbspnbspnbspnbsp">2.3.3.1&nbsp;[示例1]如果/root/clsndir 目录不存在就创建&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</a>
                </li>
                <li>
                  <a href="#2332nbsp2rootclsndir_nbspnbsp">2.3.3.2&nbsp;[示例2]如果/root/clsndir 目录不存在,就创建这个目录&nbsp;&nbsp;</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#3">第3章 判断在脚本的使用</a><ul>
        <li>
          <ul>
            <li>
              <a href="#311">3.1.1 中文示例(更好理解)</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#32">3.2 [示例]判断文件是否存在,不存在就创建</a><ul>
            <li>
              <a href="#321">3.2.1 第一步 按照格式书写判断语句</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#33">3.3 第二步 写入脚本：</a><ul>
            <li>
              <a href="#331">3.3.1 第三步 测试脚本</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#34">3.4 在脚本中进行判断的格式</a>
        </li>
        <li>
          <a href="#35_cal2sh2">3.5 [实例]如果cal2.sh脚本的参数个数不等于2，就显示 "命令错误"</a><ul>
            <li>
              <a href="#351">3.5.1 书写脚本</a>
            </li>
            <li>
              <a href="#352">3.5.2 对脚本进行测试.</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#36_22">3.6 修改之前的计算器，进行两个数字的加减乘除，在计算器前面加上参数个数判断。当输入的参数是2个的时候执行计算,不为2的时候显示参数错误.</a><ul>
            <li>
              <a href="#361">3.6.1 书写脚本</a>
            </li>
            <li>
              <a href="#362">3.6.2 测试脚本</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#4">第4章 循环语句</a><ul>
        <li>
          <a href="#41_20170501201705202017-05-01txt">4.1 [示例]修改系统时间为20170501到20170520，然后创建文件的名字为2017-05-01.txt</a><ul>
            <li>
              <a href="#411">4.1.1 基础姿势</a><ul>
                <li>
                  <a href="#4111nbspfor">4.1.1.1&nbsp;for循环可以从后面的数组中读取内容进行操作</a>
                </li>
                <li>
                  <a href="#4112nbspfor">4.1.1.2&nbsp;执行for 循环语句</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#412">4.1.2 书写循环语句</a>
            </li>
            <li>
              <a href="#413">4.1.3 测试循环语句</a>
            </li>
            <li>
              <a href="#414">4.1.4 检查执行结果</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#42_linux-for">4.2 [实例]优化linux开机启动项目-for循环方法</a><ul>
            <li>
              <a href="#421">4.2.1 第一步：确定目标</a>
            </li>
            <li>
              <a href="#422">4.2.2 第二步:取出要关闭服务的名字</a>
            </li>
            <li>
              <a href="#423_for">4.2.3 第三步 把名字与for循环结合</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#5">第5章 脚本执行排错</a><ul>
        <li>
          <a href="#51_sh_-x">5.1 sh -x 显示脚本详细的执行过程</a><ul>
            <li>
              <a href="#511">5.1.1 检查脚本运行中的错误</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#6_vim">第6章 vim 快捷键</a>
    </li>
    <li>
      <a href="#7">第7章 昨日回顾</a><ul>
        <li>
          <a href="#71_sed">7.1 【sed命令】删除文件中的空行或只有空格的行。</a><ul>
            <li>
              <a href="#711">7.1.1 文件内容</a>
            </li>
            <li>
              <a href="#712_0">7.1.2 * 表示前一个字符出现了0次或以上</a>
            </li>
            <li>
              <a href="#713">7.1.3 +表示连续出现至少一次</a>
            </li>
            <li>
              <a href="#714">7.1.4 ！表示取反</a>
            </li>
            <li>
              <a href="#715">7.1.5 同样用来取反</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#72_shell">7.2 shell编程中的变量</a><ul>
            <li>
              <a href="#721_shell">7.2.1 特殊变量（shell脚本中）</a>
            </li>
            <li>
              <a href="#722">7.2.2 全局变量（环境变量）</a>
            </li>
            <li>
              <a href="#723">7.2.3 普通变量</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#73_shell">7.3 shell编程中与用户有关的环境变量的文件和目录</a>
        </li>
      </ul>
    </li>
  </ul>
</div>