---
title: Mysql 备份恢复与xtrabackup备份
author: 惨绿少年
type: post
date: 2017-12-30T17:50:00+00:00
url: /clsn/lx346.html
Baidusubmit:
  - 1
views:
  - 444
zm_favorites:
  - 1
categories:
  - Linux运维
  - MySQL
  - 数据库
  - 运维基本功

---
## <span id="i">?<span style="font-family: 楷体;">新年贺词</span>?</span>

> <span style="font-family: 楷体; font-size: 16px;">　　2017年即将过去，新年的钟声即将敲响。</span><span style="font-family: 楷体; font-size: 16px;">在这辞旧迎新的美好时刻，我向全国各族人民，向香港特别行政区同胞、澳门特别行政区同胞，向台湾同胞和海外侨胞，向工作在一线的运维工程师们，向为开源事业做出贡献的朋友们，向世界各国各地区的朋友们，致以新年的祝福！</span>
> 
> <span style="font-family: 楷体; font-size: 16px;">　　今天是2017的最后一天，在这样一个特殊的日子里，希望大家都能事事顺心，快乐常在，希望在2018年里都能有所成就，创造不一样的价值。</span>
> 
> <span style="font-family: 楷体; font-size: 16px;">　　让我们满怀信心和期待，一起迎接新年的钟声！</span>
> 
> <span style="font-family: 楷体; font-size: 16px;">　　谢谢大家。</span>

## <span id="11">1.1 备份的原因</span>

　　备份是数据安全的最后一道防线，对于任何数据丢失的场景，备份虽然不一定能恢复百分之百的数据(取决于备份周期)，但至少能将损失降到最低。衡量备份恢复有两个重要的指标：恢复点目标(RPO)和恢复时间目标(RTO)，前者重点关注能恢复到什么程度，而后者则重点关注恢复需要多长时间。

### <span id="111">1.1.1 备份的目录</span>

　　做灾难恢复：对损坏的数据进行恢复和还原

　　需求改变：因需求改变而需要把数据还原到改变以前

　　测试：测试新功能是否可用

### <span id="112">1.1.2 备份中需要考虑的问题</span>

　　可以容忍丢失多长时间的数据；

　　恢复数据要在多长时间内完；

　　恢复的时候是否需要持续提供服务；

　　恢复的对象，是整个库，多个表，还是单个库，单个表。

### <span id="113">1.1.3 备份的类型</span>

<span style="background-color: #ffff00;"><strong>热备份：</strong></span>

　　这些动态备份在读取或修改数据的过程中进行，很少中断或者不中断传输或处理数据的功能。使用热备份时，系统仍可供读取和修改数据的操作访问。

<span style="background-color: #ffff00;"><strong>冷备份：</strong></span>

　　这些备份在用户不能访问数据时进行，因此无法读取或修改数据。这些脱机备份会阻止执行任何使用数据的活动。这些类型的备份不会干扰正常运行的系统的性能。但是，对于某些应用程序，会无法接受必须在一段较长的时间里锁定或完全阻止用户访问数据。

<span style="background-color: #ffff00;"><strong>温备份：</strong></span>

　　这些备份在读取数据时进行，但在多数情况下，在进行备份时不能修改数据本身。这种中途备份类型的优点是不必完全锁定最终用户。但是，其不足之处在于无法在进行备份时修改数据集，这可能使这种类型的备份不适用于某些应用程序。在备份过程中无法修改数据可能产生性能问题。

## <span id="12">1.2 备份的方式</span>

### <span id="121">1.2.1 冷备份</span>

　　最简单的备份方式就是，关闭MySQL服务器，然后将data目录下面的所有文件进行拷贝保存，需要恢复时，则将目录拷贝到需要恢复的机器即可。这种方式确实方便，但是在生产环境中基本没什么作用。因为所有的机器都是要提供服务的，即使是Slave有时候也需要提供只读服务，所以关闭MySQL停服备份是不现实的。与冷备份相对应的一个概念是热备份，所谓热备份是在不影响MySQL对外服务的情况下，进行备份。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 冷备份及停止业务进行备份。

### <span id="122">1.2.2 快照备份</span>

　　首先要介绍的热备份是快照备份，快照备份是指通过文件系统支持的快照功能对数据库进行备份。备份的原理是将所有的数据库文件放在同一分区中，然后对该分区执行快照工作，对于Linux而言，需要通过LVM(Logical Volumn Manager)来实现。LVM使用写时复制(copy-on-write)技术来创建快照，例如，对整个卷的某个瞬间的逻辑副本，类似于数据库中的innodb存储引擎的MVCC，只不过LVM的快照在文件系统层面，而MVCC在数据库层面，而且仅支持innodb存储引擎。

　　LVM有一个快照预留区域，如果原始卷数据有变化时，LVM保证在任何变更写入之前，会复制受影响块到快照预留区域。简单来说，快照区域内保留了快照点开始时的一致的所有old数据。对于更新很少的数据库，快照也会非常小。

　　对于MySQL而言，为了使用快照备份，需要将数据文件，日志文件都放在一个逻辑卷中，然后对该卷快照备份即可。由于快照备份，只能本地，因此，如果本地的磁盘损坏，则快照也就损坏了。快照备份更偏向于对误操作防范，可以将数据库迅速恢复到快照产生的时间点，然后结合二进制日志可以恢复到指定的时间点。基本原理如下图：

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171228214012100-1623137642.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Mysql 备份恢复与xtrabackup备份" alt="" />&nbsp;
</p>

### <span id="123_SQL">1.2.3 逻辑备份（文本表示：SQL 语句）</span>

　　冷备份和快照备份由于其弊端在生产环境中很少使用，使用更多是MySQL自带的逻辑备份和物理备份工具，这节主要讲逻辑备份，MySQL官方提供了Mysqldump逻辑备份工具，虽然已经足够好，但存在单线程备份慢的问题。在社区提供了更优秀的逻辑备份工具mydumper，它的优势主要体现在多线程备份，备份速度更快。

### <span id="124">1.2.4 其他常用的备份方式</span>

　<span style="background-color: #00ff00;">　物理备份</span>（数据文件的二进制副本）

<span style="text-decoration: underline; background-color: #00ff00;">全量备份概念</span>

　　　　全量数据就是数据库中所有的数据（或某一个库的全部数据）；

　　　　全量备份就是把数据库中所有的数据进行备份。

　　　　mysqldump会取得一个时刻的一致性数据.

<span style="text-decoration: underline; background-color: #00ff00;">增量备份（刷新二进制日志）</span>

　　　　增量数据就是指上一次全量备份数据之后到下一次全备之前数据库所更新的数据

　　　　对于mysqldump,binlog就是增量数据.

### <span id="125">1.2.5 备份工具的介绍</span>

　　1、mysqldump: mysql原生自带很好用的逻辑备份工具

　　2、mysqlbinlog: 实现binlog备份的原生态命令

　　3、xtrabackup: precona公司开发的性能很高的物理备份工具

## <span id="13_mysqldump">1.3 mysqldump备份介绍</span>

**_<span style="text-decoration: underline;">备份的基本流程如下</span>_**：

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #800080;">1</span>.调用FTWRL(flush tables with read <span style="color: #0000ff;">lock</span><span style="color: #000000;">)，全局禁止读写
</span><span style="color: #800080;">2</span><span style="color: #000000;">.开启快照读，获取此时的快照(仅对innodb表起作用)
</span><span style="color: #800080;">3</span>.备份非innodb表数据(*.frm,*.myi,*<span style="color: #000000;">.myd等)
</span><span style="color: #800080;">4</span><span style="color: #000000;">.非innodb表备份完毕后，释放FTWRL锁
</span><span style="color: #800080;">5</span><span style="color: #000000;">.逐一备份innodb表数据
</span><span style="color: #800080;">6</span>.备份完成。</pre>
  </div>
  
  <p>
    　　整个过程，可以参考我同事的一张图，但他的这张图只考虑innodb表的备份情况，实际上在unlock tables执行完毕之前，非innodb表已经备份完毕，后面的t1,t2和t3实质都是innodb表，而且5.6的mysqldump利用保存点机制，每备份完一个表就将一个表上的MDL锁释放，避免对一张表锁更长的时间。
  </p>
</div>

### <span id="131_mysqldump">1.3.1 mysqldump备份流程</span>

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171228214200881-1349563736.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Mysql 备份恢复与xtrabackup备份" alt="" />&nbsp;
</p>

### <span id="132">1.3.2 常用的备份参数</span>

&nbsp;

<table class="MsoTable15Grid4Accent1" style="width: 100%; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 35.2%; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #5b9bd5; border-bottom-color: #5b9bd5; border-left-color: #5b9bd5; border-right: none; background: #5b9bd5; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: center; text-indent: 15.75pt; mso-yfti-cnfc: 5;" align="center">
        <strong><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; color: white; mso-themecolor: background1;">参数</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #5b9bd5; border-right-color: #5b9bd5; border-bottom-color: #5b9bd5; border-left: none; background: #5b9bd5; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: center; text-indent: 15.75pt; mso-yfti-cnfc: 1;" align="center">
        <strong><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; color: white; mso-themecolor: background1;">参数说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-A</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">备份全库</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-B</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">备某一个数据库下的所有表</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-R, --routines</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">备份存储过程和函数数据</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--triggers</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">备份触发器数据</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--master-data={1|2}</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">告诉你备份后时刻的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">binlog</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">位置</span>
      </p>
      
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">如果等于</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">1</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，则将其打印为</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">CHANGE MASTER</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">命令</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">; </span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">如果等于</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">2</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，那么该命令将以注释符号为前缀。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--single-transaction</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">对</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">引擎进行热备</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-F, --flush-logs</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">刷新</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">binlog</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">日志</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-x, --lock-all-tables </span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">锁定所有数据库的所有表。</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">这是通过在整个转储期间采用全局读锁来实现的。</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-l, --lock-tables</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">锁定所有表以供读取</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-d </span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">仅表结构</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">-t </span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">仅数据</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 35.2%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="35%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
        <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--compact</span></strong>
      </p>
    </td>
    
    <td style="width: 64.8%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" width="64%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">减少无用数据输出</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">(</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">调试</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">) </span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 100%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" colspan="2" width="100%">
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; color: #c00000;">一个完整的备份语句：</span>
      </p>
      
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">&nbsp; innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">引擎的备份命令如下：</span>
      </p>
      
      <div class="cnblogs_code">
        <pre>mysqldump -A -R --triggers --master-data=<span style="color: #800080;">2</span> --single-transaction |<span style="color: #0000ff;">gzip</span> >/opt/all_$(<span style="color: #0000ff;">date</span> +%F).sql.gz </pre>
      </div>
      
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        <span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">&nbsp; </span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">适合多引擎混合（例如：</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">myisam</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">与</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">混合）的备份命令如下：</span>
      </p>
      
      <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
        &nbsp;
      </p>
      
      <div class="cnblogs_code">
        <pre>mysqldump -A -R --triggers --master-data=<span style="color: #800080;">2</span> |<span style="color: #0000ff;">gzip</span>   >/opt/all_$(<span style="color: #0000ff;">date</span> +%F).sql.gz  </pre>
      </div>
    </td>
  </tr>
</table>

### <span id="133_-A">1.3.3 -A 参数</span>

　　备份全库，备份语句

<div>
  <div class="cnblogs_code">
    <pre>　　mysqldump <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #808080;">-</span>A  <span style="color: #808080;">></span> <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span><span style="color: #0000ff;">full</span>.sql</pre>
  </div>
</div>

### <span id="134_-B">1.3.4 -B 参数</span>

　　备某一个数据库下的所有表

　　增加建库（create）及&ldquo;use库&rdquo;的语句，可以直接接多个库名，同时备份多个库* -B 库1 库2

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #808080;">-</span>B world  <span style="color: #808080;">></span> <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span>worldb.sql</pre>
  </div>
</div>

&nbsp;备份语句：

<div>
  <div class="cnblogs_code">
    <pre>    <span style="color: #0000ff;">create</span> <span style="color: #0000ff;">database</span> <span style="color: #0000ff;">if</span> <span style="color: #808080;">not</span><span style="color: #000000;"> 存在
    </span><span style="color: #0000ff;">use</span><span style="color: #000000;"> db1
    </span><span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">table</span>
    <span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span>
    <span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span></pre>
  </div>
  
  <p>
    　　不加-B备份数据库时，只是备份数据库下的所有表，不会创建数据库
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 只能<span style="text-decoration: underline;">备份单独的数据库</span>（一般用于备份单表时使用）

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 world  <span style="color: #808080;">></span> <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span>world.sql</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="text-decoration: underline;">备份单表</span>

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 world  city  <span style="color: #808080;">></span> <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span>world_city.sql</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 对于单表备份的粒度，再恢复数据库数据时速度最快。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="text-decoration: underline;">备份多个表</span>

<div>
  <div class="cnblogs_code">
    <pre>mysqldump 库1 表1 表2 表3 <span style="color: #808080;">></span><span style="color: #000000;">库1.sql
mysqldump 库2 表1 表2 表3 </span><span style="color: #808080;">></span>库2.sql</pre>
  </div>
</div>

<span style="text-decoration: underline;">分库备份:for</span><span style="text-decoration: underline;">循环</span>

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p<span style="color: #ff0000;">'</span><span style="color: #ff0000;">mysql123</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">-</span><span style="color: #000000;">B mysql ...
mysqldump </span><span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p<span style="color: #ff0000;">'</span><span style="color: #ff0000;">mysql123</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">-</span><span style="color: #000000;">B mysql_utf8 ...
mysqldump </span><span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p<span style="color: #ff0000;">'</span><span style="color: #ff0000;">mysql123</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">-</span><span style="color: #000000;">B mysql ...
......</span></pre>
  </div>
</div>

<span style="text-decoration: underline;">分库备份</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">for</span> name <span style="color: #808080;">in</span> `mysql <span style="color: #808080;">-</span>e "show databases;"<span style="color: #808080;">|</span><span style="color: #000000;">sed 1d`
do
 mysqldump </span><span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p<span style="color: #ff0000;">'</span><span style="color: #ff0000;">mysql123</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">-</span><span style="color: #000000;">B $name
done</span></pre>
  </div>
</div>

### <span id="135_--master-data12">1.3.5 --master-data={1|2}参数</span>

　　告诉你备份后时刻的binlog位置

　　　　2为注释&nbsp; 1为非注释，要执行的(主从复制)

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 logs</span><span style="color: #ff0000;">]</span># sed <span style="color: #808080;">-</span>n <span style="color: #ff0000;">'</span><span style="color: #ff0000;">22p</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">/</span>opt<span style="color: #808080;">/</span><span style="color: #000000;">t.sql
CHANGE MASTER </span><span style="color: #0000ff;">TO</span> MASTER_LOG_FILE<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">clsn-bin.000005</span><span style="color: #ff0000;">'</span>, MASTER_LOG_POS<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">344</span><span style="color: #000000;">;
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 logs</span><span style="color: #ff0000;">]</span># mysqldump <span style="color: #808080;">-</span>B <span style="color: #008080;">--</span><span style="color: #008080;">master-data=2 clsn >/opt/t.sql</span></pre>
  </div>
</div>

### <span id="136_--single-transaction">1.3.6 --single-transaction 参数</span>

　　对innodb引擎进行热备

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="background-color: #ffff00;">只支持innodb引擎</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 使用该参数会单独开启一个事务进行备份，利用事务的快照技术实现。

　　基于事务引擎:不用锁表就可以获得一致性的备份.

　　体现了ACID四大特性中的隔离性，生产中99% 使用innodb事务引擎.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 虽然支持热备，并不意味着你可以再任意时间点进行备份，特别是业务繁忙期，不要做备份策略，一般夜里进行备份。

　　innodb引擎的备份命令如下：

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>A <span style="color: #808080;">-</span>B <span style="color: #808080;">-</span>R <span style="color: #008080;">--</span><span style="color: #008080;">triggers --master-data=2 --single-transaction |gzip >/opt/all.sql.gz</span></pre>
  </div>
</div>

### <span id="137_--flush-logs-F">1.3.7 --flush-logs参数/-F</span>

　　刷新binlog日志

　　　　每天晚上0点备份数据库

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>A <span style="color: #808080;">-</span>B <span style="color: #808080;">-</span>F <span style="color: #808080;">>/</span>opt<span style="color: #808080;">/</span>$(date <span style="color: #808080;">+%</span>F).sql</pre>
  </div>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span># ll <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>logs<span style="color: #808080;">/</span>
<span style="color: #808080;">-</span>rw<span style="color: #808080;">-</span>rw<span style="color: #008080;">--</span><span style="color: #008080;">-- 1 mysql mysql 168 Jun 21 12:06 clsn-bin.000001</span>
<span style="color: #808080;">-</span>rw<span style="color: #808080;">-</span>rw<span style="color: #008080;">--</span><span style="color: #008080;">-- 1 mysql mysql 168 Jun 21 12:06 clsn-bin.000002</span>
<span style="color: #808080;">-</span>rw<span style="color: #808080;">-</span>rw<span style="color: #008080;">--</span><span style="color: #008080;">-- 1 mysql mysql 210 Jun 21 12:07 clsn-bin.index</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 提示:每个库都会刷新一次.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### <span id="138">1.3.8 压缩备份</span>

压缩备份命令:

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>B <span style="color: #008080;">--</span><span style="color: #008080;">master-data=2 clsn|gzip >/opt/t.sql.gz</span></pre>
  </div>
</div>

　　解压:

<div>
  <div class="cnblogs_code">
    <pre>zcat t.sql.gz <span style="color: #808080;">></span><span style="color: #000000;">t1.sql
gzip </span><span style="color: #808080;">-</span><span style="color: #000000;">d t.sql.gz #删压缩包
gunzip alL_2017</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">22</span>.sql.gz </pre>
  </div>
</div>

**一个完整的备份语句**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; innodb引擎的备份命令如下：

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>A <span style="color: #808080;">-</span>R <span style="color: #008080;">--</span><span style="color: #008080;">triggers --master-data=2 --single-transaction |gzip >/opt/all.sql.gz</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 适合多引擎混合（例如：myisam与innodb混合）的备份命令如下：

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>A <span style="color: #808080;">-</span>R <span style="color: #008080;">--</span><span style="color: #008080;">triggers --master-data=2 |gzip   >/opt/alL_$(date +%F).sql.gz</span></pre>
  </div>
</div>

### <span id="139_Mysqldump">1.3.9 使用Mysqldump备份进行恢复实践</span>

备份innodb引擎数据库clsn并压缩：

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>B <span style="color: #808080;">-</span>R <span style="color: #008080;">--</span><span style="color: #008080;">triggers --master-data=2 clsn|gzip >/opt/all_$(date +%F).sql.gz</span></pre>
  </div>
</div>

人为删除clsn数据库：

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 opt</span><span style="color: #ff0000;">]</span># mysql <span style="color: #808080;">-</span>e &ldquo;<span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">database</span><span style="color: #000000;"> clsn;&rdquo;
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 opt</span><span style="color: #ff0000;">]</span># mysql <span style="color: #808080;">-</span>e &ldquo;show databases;&rdquo;</pre>
  </div>
</div>

恢复数据库：

<div>
  <div class="cnblogs_code">
    <pre>使用gzip解压 gzip <span style="color: #808080;">-</span><span style="color: #000000;">d xxx.gz
shell</span><span style="color: #808080;">></span> mysql <span style="color: #808080;">&lt;/</span>opt<span style="color: #808080;">/</span>all_2017<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">1222</span><span style="color: #000000;">.sql
或
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">set</span> sql_log_bin<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;"></span><span style="color: #000000;">;
Query OK, </span><span style="color: #800000; font-weight: bold;"></span> rows affected (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)
mysql</span><span style="color: #808080;">></span> source <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span>alL_2017<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">22</span>.sql</pre>
  </div>
</div>

验证数据：

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 opt</span><span style="color: #ff0000;">]</span>#  mysql <span style="color: #808080;">-</span>e &ldquo;<span style="color: #0000ff;">use</span> clsn;<span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> test;&rdquo;</pre>
  </div>
</div>

## <span id="14">1.4 【模拟】增量恢复企业案例</span>

### <span id="141">1.4.1 前提条件:</span>

　　1.具备全量备份（mysqldump）。

　　2.除全量备份以外，还有全量备份之后产生的的所有binlog增量日志。

### <span id="142">1.4.2 环境准备</span>

(1)准备环境：

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">database</span><span style="color: #000000;"> clsn;
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">DATABASE</span><span style="color: #000000;"> clsn;
</span><span style="color: #0000ff;">USE</span><span style="color: #000000;"> `clsn`;
</span><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TABLE</span><span style="color: #000000;"> `test` (
  `id` </span><span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">4</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;"> AUTO_INCREMENT,
  `name` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">20</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  </span><span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> (`id`)
) ENGINE</span><span style="color: #808080;">=</span>InnoDB AUTO_INCREMENT<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">7</span> <span style="color: #0000ff;">DEFAULT</span> CHARSET<span style="color: #808080;">=</span><span style="color: #000000;">utf8;
</span><span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> `test` <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">1</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">clsn</span><span style="color: #ff0000;">'</span>),(<span style="color: #800000; font-weight: bold;">2</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">znix</span><span style="color: #ff0000;">'</span>),(<span style="color: #800000; font-weight: bold;">3</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">inca</span><span style="color: #ff0000;">'</span>),(<span style="color: #800000; font-weight: bold;">4</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">zuma</span><span style="color: #ff0000;">'</span>),(<span style="color: #800000; font-weight: bold;">5</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">kaka</span><span style="color: #ff0000;">'</span>);</pre>
  </div>
</div>

　　查看创建好的数据

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> test;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> clsn <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> znix <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> inca <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">4</span> <span style="color: #808080;">|</span> zuma <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">5</span> <span style="color: #808080;">|</span> kaka <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #800000; font-weight: bold;">5</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
</div>

(2)模拟环境：

<div>
  <div class="cnblogs_code">
    <pre>mkdir <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span> <span style="color: #808080;">-</span><span style="color: #000000;">p
date </span><span style="color: #808080;">-</span>s "<span style="color: #800000; font-weight: bold;">2017</span><span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">22</span>"</pre>
  </div>
</div>

全备份:

<div>
  <div class="cnblogs_code">
    <pre>mysqldump <span style="color: #808080;">-</span>B <span style="color: #008080;">--</span><span style="color: #008080;">master-data=2 --single-transaction clsn|gzip>/data/backup/clsn_$(date +%F).sql.gz</span></pre>
  </div>
</div>

　　模拟增量:

<div>
  <div class="cnblogs_code">
    <pre>mysql <span style="color: #808080;">-</span>e "<span style="color: #0000ff;">use</span> clsn;<span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">6</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">haha</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);"
mysql </span><span style="color: #808080;">-</span>e "<span style="color: #0000ff;">use</span> clsn;<span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">7</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">hehe</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);"
mysql </span><span style="color: #808080;">-</span>e "<span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span> clsn.test;"</pre>
  </div>
</div>

(3)模拟误删数据：

<div>
  <div class="cnblogs_code">
    <pre>date <span style="color: #808080;">-</span>s "<span style="color: #800000; font-weight: bold;">2017</span><span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">22</span> <span style="color: #800000; font-weight: bold;">11</span>:<span style="color: #800000; font-weight: bold;">40</span><span style="color: #000000;">"
mysql  </span><span style="color: #808080;">-</span>e "<span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">database</span> clsn;show databases;"</pre>
  </div>
  
  <p>
    　　出现问题10分钟后,发现问题,删除了数据库了.
  </p>
</div>

### <span id="143">1.4.3 恢复数据准备</span>

(1)采用iptables防火墙屏蔽所有应用程序的写入。

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@clsn ~</span><span style="color: #ff0000;">]</span># iptables <span style="color: #808080;">-</span>I INPUT <span style="color: #808080;">-</span>p tcp <span style="color: #008080;">--</span><span style="color: #008080;">dport 3306 ! -s 172.16.1.51 -j DROP </span>
#<span style="color: #808080;">&lt;==</span>非172.<span style="color: #800000; font-weight: bold;">16.1</span>.51禁止访问数据库3306端口。</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 或采用mysql 配置参数，但是需要重启数据库

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #008080;">--</span><span style="color: #008080;">skip-networking</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 复制二进制日志文件

<div>
  <div class="cnblogs_code">
    <pre>cp <span style="color: #808080;">-</span>a <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>logs<span style="color: #808080;">/</span>clsn<span style="color: #808080;">-</span>bin.<span style="color: #808080;">*</span> <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 截取日志

<div>
  <div class="cnblogs_code">
    <pre>zcat clsn_2017<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">22</span>.sql.gz <span style="color: #808080;">></span>clsn_2017<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">22</span><span style="color: #000000;">.sql
sed </span><span style="color: #808080;">-</span>n <span style="color: #ff0000;">'</span><span style="color: #ff0000;">22p</span><span style="color: #ff0000;">'</span> clsn_2017<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">22</span><span style="color: #000000;">.sql
mysqlbinlog </span><span style="color: #808080;">-</span>d clsn <span style="color: #008080;">--</span><span style="color: #008080;">start-position=339 clsn-bin.000008 -r bin.sql</span></pre>
  </div>
</div>

_<span style="text-decoration: underline;">需要恢复的日志:</span>_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #800000; font-weight: bold;">1</span>.clsn_2017<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">22</span><span style="color: #000000;">.sql
</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">.bin.sql
grep </span><span style="color: #808080;">-</span>i <span style="color: #0000ff;">drop</span><span style="color: #000000;"> bin.sql 
sed </span><span style="color: #808080;">-</span>i <span style="color: #ff0000;">'</span><span style="color: #ff0000;">/^drop.*/d</span><span style="color: #ff0000;">'</span> bin.sql</pre>
  </div>
</div>

### <span id="144">1.4.4 进行数据恢复</span>

恢复数据

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 backup</span><span style="color: #ff0000;">]</span># mysql <span style="color: #808080;">&lt;</span>clsn_2017<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">12</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">22</span><span style="color: #000000;">.sql
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 backup</span><span style="color: #ff0000;">]</span># mysql <span style="color: #808080;">-</span><span style="color: #000000;">e "show databases;"
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> <span style="color: #0000ff;">Database</span>           <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> information_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> mysql              <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> clsn               <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> znix               <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> performance_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span></pre>
  </div>
</div>

查看数据库

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> test;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> clsn <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> znix <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> inca <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">4</span> <span style="color: #808080;">|</span> zuma <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">5</span> <span style="color: #808080;">|</span> kaka <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #800000; font-weight: bold;">5</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
</div>

恢复增量数据：

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 backup</span><span style="color: #ff0000;">]</span># mysql clsn <span style="color: #808080;">&lt;</span><span style="color: #000000;">bin.sql
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> test;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span> id <span style="color: #808080;">|</span> name <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">|</span> clsn <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">2</span> <span style="color: #808080;">|</span> znix <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">3</span> <span style="color: #808080;">|</span> inca <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">4</span> <span style="color: #808080;">|</span> zuma <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">5</span> <span style="color: #808080;">|</span> kaka <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">6</span> <span style="color: #808080;">|</span> haha <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span>  <span style="color: #800000; font-weight: bold;">7</span> <span style="color: #808080;">|</span> hehe <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--+------+</span>
<span style="color: #800000; font-weight: bold;">7</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
  
  <p>
    　　恢复完毕。
  </p>
</div>

**　<span style="background-color: #ffff00;">　调整iptables</span>**<span style="background-color: #ffff00;"><strong>允许用户访问.</strong></span>

### <span id="145_binlog">1.4.5 多个binlog问题</span>

<div>
  <div class="cnblogs_code">
    <pre>mysqlbinlog <span style="color: #808080;">-</span>d clsn <span style="color: #008080;">--</span><span style="color: #008080;">start-position=339 clsn-bin.000009 clsn-bin.0000010 -r bin1.sql</span>
<span style="color: #000000;">
mysql clsn </span><span style="color: #808080;">&lt;</span>bin1.sql</pre>
  </div>
</div>

## <span id="15_mysql">1.5 mysql数据库实际生产惨案</span>

### <span id="151">1.5.1 发生背景</span>

　　1、mysql服务器会在每天夜里0点全量备份

　　2、某个开发人员某个阳光明媚的上午，喝着茶，优雅的误删除了clsn_oss（核心）数据库。

　　3、导致公司业务异常停止，无法正常提供服务。

### <span id="152">1.5.2 怎么解决的</span>

　　1、当前系统进行评估。

　　　　什么损坏了，有没有备份，

　　　　恢复数据时间（误操作的数据有关，备份、恢复策略），

　　　　恢复业务时间

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2、恢复方案

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （1）恢复0点的全备，到测试库

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （2）恢复0点开始到故障时间点的binlog，到测试库

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （3）将误操作的数据导出，恢复到生产库。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （4）检验数据是不是完整的（开发测试环境测试恢复成功数据库）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （5）检验完成之后，重新开启生产业务

### <span id="153">1.5.3 项目总结</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1、经过我的恢复处理，30分钟整体业务重新提供服务（速度慢。。。）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2、在以后的工作中制定严格的开发规范，开发，开发。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3、将来制定更好的架构方案。

## <span id="16">1.6 备份工具的选择</span>

　　　　数据量范围：30G&nbsp; --> TB级别&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### <span id="161_nbspnbspnbsp">1.6.1 数据量大，变换量小&nbsp;&nbsp;&nbsp;</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （1）全备分花费的成本较高，mysqldump+binlog实现全备 + 增量备份，缺点是恢复成本比备份时间成本还高

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （2）xtrabackup：可以较长时间做一次全备，其余时间都是增量，全量备份空间成本很高如果数据量在30G-->TB级别的话，更推荐使用xtrabackup工具。

### <span id="162">1.6.2 数据量小，变化量大</span>

　　　　只需要考虑时间成本。

　　　　只用全备备份即可，两种工具选择都可以。恢复成本上xtrabackup小一些

### <span id="163">1.6.3 数据量、变化量都大</span>

　　　　时间成本和空间成本都要考虑了。

　　　数据量达到PB或更高时（facebook），mysqldump可能成为首选，占用空间小，但技术成本高。需要对mysqldump进行二次开发（大数据量公司首选）。

## <span id="17_xtrabackup">1.7 xtrabackup备份软件</span>

percona公司官网 &nbsp;https://www.percona.com/

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171228215514163-227995886.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Mysql 备份恢复与xtrabackup备份" alt="" />
</p>

### <span id="171_Xtrabackup">1.7.1 Xtrabackup介绍</span>

　　Xtrabackup是由percona开源的免费数据库热备份软件，它能对InnoDB数据库和XtraDB存储引擎的数据库非阻塞地备份（对于MyISAM的备份同样需要加表锁）；mysqldump备份方式是采用的逻辑备份，其最大的缺陷是备份和恢复速度较慢，如果数据库大于50G，mysqldump备份就不太适合。

　　Xtrabackup安装完成后有4个可执行文件，其中2个比较重要的备份工具是**innobackupex****、xtrabackup**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">）xtrabackup 是专门用来备份InnoDB表的，和mysql server没有交互；
</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">）innobackupex 是一个封装xtrabackup的Perl脚本，支持同时备份innodb和myisam，但在对myisam备份时需要加一个全局的读锁。
</span><span style="color: #800000; font-weight: bold;">3</span><span style="color: #000000;">）xbcrypt 加密解密备份工具
</span><span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">）xbstream 流传打包传输工具，类似tar
</span><span style="color: #800000; font-weight: bold;">5</span>）物理备份工具，在同级数据量基础上，都要比逻辑备份性能好的多，特别是在数据量较大的时候，体现的更加明显。</pre>
  </div>
</div>

### <span id="171_Xtrabackup-2">1.7.1 Xtrabackup优点</span>

　　1）备份速度快，物理备份可靠

　　2）备份过程不会打断正在执行的事务（无需锁表）

　　3）能够基于压缩等功能节约磁盘空间和流量

　　4）自动备份校验

　　5）还原速度快

　　6）可以流传将备份传输到另外一台机器上

　　7）在不增加服务器负载的情况备份数据

　　8）<span style="background-color: #00ffff;">物理备份工具，在同级数据量基础上，都要比逻辑备份性能要好的多。几十G到不超过TB级别的条件下。但在同数据量级别，物理备份恢复数据上有一定优势。</span>

### <span id="172">1.7.2 备份原理</span>

　　拷贝数据文件、拷贝数据页

<span style="background-color: #ffff00;"><strong>对于innodb</strong><strong>表可以实现热备。</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>    (<span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">)在数据库还有修改操作的时刻，直接将数据文件备走，此时，备份走的数据对于当前mysql来讲是不一致的。
    (</span><span style="color: #800000; font-weight: bold;">2</span><span style="color: #000000;">)将备份过程中的redo和undo一并备走。
    (</span><span style="color: #800000; font-weight: bold;">3</span>)为了恢复的时候，只要保证备份出来的数据页lsn能和redo lsn匹配，将来恢复的就是一致的数据。redo应用和undo应用。</pre>
  </div>
</div>

<span style="background-color: #ffff00;"><strong>对于myisam</strong><strong>表实现自动锁表拷贝文件。</strong></span>

　　备份开始时首先会开启一个后台检测进程，实时检测mysql redo的变化，一旦发现有新的日志写入，立刻将日志记入后台日志文件xtrabackup\_log中，之后复制innodb的数据文件一系统表空间文件ibdatax，复制结束后，将执行flush tables with readlock,然后复制.frm MYI MYD等文件，最后执行unlock tables,最终停止xtrabackup\_log

### <span id="173_xtrabackup">1.7.3 xtrabackup的安装</span>

　　安装依赖关系

<div>
  <div class="cnblogs_code">
    <pre>wget <span style="color: #808080;">-</span>O <span style="color: #808080;">/</span>etc<span style="color: #808080;">/</span>yum.repos.d<span style="color: #808080;">/</span>epel.repo http:<span style="color: #808080;">//</span>mirrors.aliyun.com<span style="color: #808080;">/</span>repo<span style="color: #808080;">/</span>epel<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;">.repo
yum </span><span style="color: #808080;">-</span>y install perl perl<span style="color: #808080;">-</span>devel libaio libaio<span style="color: #808080;">-</span>devel perl<span style="color: #808080;">-</span>Time<span style="color: #808080;">-</span>HiRes perl<span style="color: #808080;">-</span>DBD<span style="color: #808080;">-</span>MySQL</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 下载软件包，并安装软件

<div>
  <div class="cnblogs_code">
    <pre>wget https:<span style="color: #808080;">//</span>www.percona.com<span style="color: #808080;">/</span>downloads<span style="color: #808080;">/</span>XtraBackup<span style="color: #808080;">/</span>Percona<span style="color: #808080;">-</span>XtraBackup<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">2.4</span>.<span style="color: #800000; font-weight: bold;">4</span><span style="color: #808080;">/</span><span style="color: #0000ff;">binary</span><span style="color: #808080;">/</span>redhat<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">6</span><span style="color: #808080;">/</span>x86_64<span style="color: #808080;">/</span>percona<span style="color: #808080;">-</span>xtrabackup<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">24</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">2.4</span>.<span style="color: #800000; font-weight: bold;">4</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">.el6.x86_64.rpm
yum </span><span style="color: #808080;">-</span>y install percona<span style="color: #808080;">-</span>xtrabackup<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">24</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">2.4</span>.<span style="color: #800000; font-weight: bold;">4</span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">1</span>.el6.x86_64.rpm</pre>
  </div>
</div>

## <span id="18_xtrabackup">1.8 xtrabackup实践操作</span>

### <span id="181">1.8.1 全量备份与恢复</span>

　　这一阶段会启动xtrabackup内嵌的innodb实例，回放xtrabackup日志xtrabackup_log，将提交的事务信息变更应用到innodb数据/表空间，同时回滚未提交的事务(这一过程类似innodb的实例恢复）。恢复过程如下图：

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171228215804459-377954285.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Mysql 备份恢复与xtrabackup备份" alt="" />&nbsp;
</p>

<span style="background-color: #ffff00;"><strong>备份</strong></span>

　　创建备份目录

<div>
  <div class="cnblogs_code">
    <pre>mkdir  <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span> <span style="color: #808080;">-</span>p</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 进行第一次全量备份

<div>
  <div class="cnblogs_code">
    <pre><span style="background-color: #ffff00;">innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">defaults-file=/etc/my.cnf --user=root --password=123 --socket=/application/mysql/tmp/mysql.sock --no-timestamp /backup/xfull</span></span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<span style="background-color: #ffff00;"><strong>恢复前准备</strong></span>

　　恢复数据前的准备(合并xtabackup\_log\_file和备份的物理文件)

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --use-memory=32M /backup/xfull/</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 查看合并后的 checkpoints 其中的类型变为 full-prepared 即为可恢复。

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 full</span><span style="color: #ff0000;">]</span><span style="color: #000000;"># cat xtrabackup_checkpoints 
backup_type </span><span style="color: #808080;">=</span> <span style="color: #0000ff;">full</span><span style="color: #808080;">-</span><span style="color: #000000;">prepared
from_lsn </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;"></span><span style="color: #000000;">
to_lsn </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">4114824</span><span style="color: #000000;">
last_lsn </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">4114824</span><span style="color: #000000;">
compact </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;"></span><span style="color: #000000;">
recover_binlog_info </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;"></span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　破坏数据库数据文件

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 full</span><span style="color: #ff0000;">]</span># cd <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span>
<span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 data</span><span style="color: #ff0000;">]</span><span style="color: #000000;"># ls
auto.cnf  db02.pid  ibdata2      mysql             mysql</span><span style="color: #808080;">-</span>bin.<span style="color: #0000ff;">index</span><span style="color: #000000;">     world
clsn      haha      ib_logfile0  mysql</span><span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000001</span><span style="color: #000000;">  oldboy
db02.err  ibdata1   ib_logfile1  mysql</span><span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000002</span><span style="color: #000000;">  performance_schema
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 data</span><span style="color: #ff0000;">]</span># \rm <span style="color: #808080;">-</span>rf .<span style="color: #008080;">/*</span><span style="color: #008080;"> 
[root@db02 data]# ls
[root@db02 data]# killall mysql</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<span style="background-color: #ffff00;"><strong>恢复方法</strong></span>

　　方法一:直接将备份文件复制回来

<div>
  <div class="cnblogs_code">
    <pre>cp <span style="color: #808080;">-</span>a <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span><span style="color: #0000ff;">full</span><span style="color: #808080;">/</span> <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span><span style="color: #000000;">data
chown </span><span style="color: #808080;">-</span>R mysql.mysql <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data</pre>
  </div>
  
  <p>
    　　方法二:使用innobackupex命令进行恢复<strong>(</strong><strong>推荐)</strong>
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 mysql</span><span style="color: #ff0000;">]</span># innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">copy-back /backup/xfull</span>
<span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 mysql</span><span style="color: #ff0000;">]</span># chown <span style="color: #808080;">-</span>R mysql.mysql <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span></pre>
  </div>
  
  <p>
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;说明：无论使用那种恢复方法都要恢复后需改属组属主，保持与程序一致。
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 data</span><span style="color: #ff0000;">]</span># cd <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span>
<span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 data</span><span style="color: #ff0000;">]</span><span style="color: #000000;"># ls
clsn     ibdata2      ibtmp1  performance_schema            xtrabackup_info
haha     ib_logfile0  mysql   world
ibdata1  ib_logfile1  oldboy  xtrabackup_binlog_pos_innodb</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 启动是数据库

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 data</span><span style="color: #ff0000;">]</span>#  <span style="color: #808080;">/</span>etc<span style="color: #808080;">/</span>init.d<span style="color: #808080;">/</span>mysqld start</pre>
  </div>
</div>

### <span id="182">1.8.2 增量备份与恢复</span>

　　innobackupex增量备份过程中的"增量"处理，其实主要是相对innodb而言，对myisam和其他存储引擎而言，它仍然是全拷贝(全备份)

　　"增量"备份的过程主要是通过拷贝innodb中有变更的"页"（这些变更的数据页指的是"页"的LSN大于xtrabackup_checkpoints中给定的LSN）。增量备份是基于全备的，第一次增备的数据必须要基于上一次的全备，之后的每次增备都是基于上一次的增备，最终达到一致性的增备。增量备份的过程如下，和全备的过程很类似，区别仅在第2步。

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171228220033850-708438958.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Mysql 备份恢复与xtrabackup备份" alt="" />&nbsp;
</p>

**增量备份从哪增量？**

　　基于上一次的备份进行增量。

　　redo默认情况下是一组两个文件，并且有固定大小。其使用的文件是一种轮询使用方式，他不是永久的，文件随时可能被覆盖。

**　　<span style="color: #ff0000;">注意：千万不要在业务繁忙时做备份。</span>**

**备份什么内容**

　　1、可以使用binlog作为增量

　　2、自带的增量备份，基于上次备份后的变化的数据页，还要备份在备份过程中的undo、redo变化

<span style="background-color: #ffff00;"><strong>怎么备份</strong></span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _1__、<span style="text-decoration: underline;">先进行第一次全备</span>_

<div>
  <div class="cnblogs_code">
    <pre>innobackupex  <span style="color: #008080;">--</span><span style="color: #008080;">user=root --password=123 --no-timestamp /bakcup/xfull</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 对原库做了修改，修改了小红那行然后commit。

_&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2__、<span style="text-decoration: underline;">再进行增量备份</span>_

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">user=root --password=123  --incremental --no-timestamp --incremental-basedir=/backup/xfull/  /backup/xinc1</span></pre>
  </div>
</div>

**怎么恢复**

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171228220210991-652940594.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Mysql 备份恢复与xtrabackup备份" alt="" />
</p>

　　1、先应用全备日志（--apply-log，暂时不需要做回滚操作--redo-only）

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --redo-only /backup/xfull/ </span>&nbsp; &nbsp;&nbsp;</pre>
  </div>
</div>

　　2、合并增量到全备中（一致性的合并）

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --incremental-dir=/backup/xinc1 /backup/xfull/</span>
innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log /backup/xfull</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　3、合并完成进行恢复

　　　　方法一:直接将备份文件复制回来

<div>
  <div class="cnblogs_code">
    <pre>cp <span style="color: #808080;">-</span>a <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span><span style="color: #0000ff;">full</span><span style="color: #808080;">/</span> <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span><span style="color: #000000;">data
chown </span><span style="color: #808080;">-</span>R mysql.mysql <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　　　方法二:使用innobackupex命令进行恢复**(****推荐)**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 mysql</span><span style="color: #ff0000;">]</span># innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">copy-back /backup/xfull</span>
<span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 mysql</span><span style="color: #ff0000;">]</span># chown <span style="color: #808080;">-</span>R mysql.mysql <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 说明：无论使用那种恢复方法都要恢复后需改属组属主，保持与程序一致。

### <span id="183">1.8.3 数据库备份策略</span>

　　　　每周的周日进行一次全备；周一到周六每天做上一天增量，每周轮询一次。

<div>
  <div class="cnblogs_code">
    <pre>xfull       <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --redo-only   保证last-lsn=周一增量开始lsn</span>
xinc1        合并周一的增量到全备，并apply<span style="color: #808080;">-</span><span style="color: #ff00ff;">log</span> <span style="color: #008080;">--</span><span style="color: #008080;">redo-only  保证last-lsn=周二增量开始lsn</span>
xinc2        合并周二的增量到全备，并apply<span style="color: #808080;">-</span><span style="color: #ff00ff;">log</span> <span style="color: #008080;">--</span><span style="color: #008080;">redo-only  保证last-lsn=周三增量开始lsn</span>
xinc3       合并周三的增量到全备，并apply<span style="color: #808080;">-</span><span style="color: #ff00ff;">log</span> <span style="color: #008080;">--</span><span style="color: #008080;">redo-only  保证last-lsn=周四增量开始lsn</span>
xinc4       合并周四的增量到全备，并apply<span style="color: #808080;">-</span><span style="color: #ff00ff;">log</span> <span style="color: #008080;">--</span><span style="color: #008080;">redo-only  保证last-lsn=周五增量开始lsn</span>
xinc5       合并周五的增量到全备，并apply<span style="color: #808080;">-</span><span style="color: #ff00ff;">log</span> <span style="color: #008080;">--</span><span style="color: #008080;">redo-only  保证last-lsn=周六增量开始lsn</span>
xinc6        合并周六的增量到全备，<span style="color: #008080;">--</span><span style="color: #008080;">apply-log  准备恢复即可</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

### <span id="184">1.8.4 真实生产实战案例分析</span>

**　　背景：**某物流公司网站核心系统，数据量是220G，每日更新量100M-200M

**　　备份方案：** xtrabackup全备+增量

**　　备份策略（crontab****）**：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<div>
  <p class="a4">
    　　　　1、周六 晚上0点全备&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  </p>
  
  <p class="a4">
    　　　　　　&nbsp;<span class="cnblogs_code"><span style="color: #800000; font-weight: bold;"></span> <span style="color: #800000; font-weight: bold;"></span> <span style="color: #808080;">*</span> <span style="color: #808080;">*</span> <span style="color: #800000; font-weight: bold;">6</span> zjs_full.sh <span style="color: #008080;">--</span><span style="color: #008080;">-这行可以没有</span></span>&nbsp;
  </p>
  
  <p class="a4">
    　　　　2、周一至周五、周日&nbsp; 是增量，基于上一天增量
  </p>
  
  <p class="a4">
    　　　　　　&nbsp;<span class="cnblogs_code"><span style="color: #800000; font-weight: bold;"></span> <span style="color: #800000; font-weight: bold;">1</span> <span style="color: #808080;">*</span> <span style="color: #808080;">*</span> <span style="color: #800000; font-weight: bold;"></span><span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">5</span> zjs_inc.sh<span style="color: #008080;">--</span><span style="color: #008080;">-这行可以没有 </span></span>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;
  </p>
</div>

**　　故障场景：**

<div>
  <p class="a4">
    　　　　周三的时候，下午两点，开发人员误删除了一张表zjs_base，大约10G。
  </p>
</div>

**　　项目职责：**

　　　　1)&nbsp; 指定恢复方案、利用现有备份;

　　　　2)&nbsp; 恢复误删除数据;

　　　　3)&nbsp; 制定运维、开发流程规范。

**　　恢复流程：**

<div class="cnblogs_code">
  <pre><span style="color: #000000;">    a)    准备上周六全备。
    b)    合并周日、周一 、周二增量。
    c)    在测试库恢复以上数据，数据的目前状态应该周三凌晨1:</span><span style="color: #800000; font-weight: bold;">00</span><span style="color: #000000;">
    d)    需要恢复的数据状态是，下午2点钟左右
    e)    从1点开始的binlog恢复到删除之前转台
    f)    导出删除的表zjs_base，恢复到生产库，验证数据可用性、完整性。
    g)    启动应用连接数据库。</span></pre>
</div>

　　**总结**：经过30分钟将误删表恢复了。服务总共停止40分钟。

### <span id="185">1.8.5 故障恢复小结</span>

**_&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_** **_恢复思路：_**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1、首先确保断开所有应用，保证数据的安全。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2、检查用于恢复的备份存在吗。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3、设计快速、安全恢复简单方案，制定突发问题解决办法。

**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;** **具体恢复流程：**

<div>
  <div class="cnblogs_code">
    <pre>        <span style="color: #800000; font-weight: bold;">1</span>、准备上周六全备，并<span style="color: #008080;">--</span><span style="color: #008080;">apply-log --redo-only</span>
        <span style="color: #800000; font-weight: bold;">2</span>、合并增量，周日、周一 、周二  <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --redo-only 周三 --apply-log</span>
        <span style="color: #800000; font-weight: bold;">3</span>、在测试库恢复以上数据，数据的目前状态应该周三凌晨1:<span style="color: #800000; font-weight: bold;">00</span>
        <span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">、需要恢复的数据状态是，下午2点钟左右，删除zjs_base之前的数据状态
            　　从1点开始的binlog恢复到删除之前的那个events的position。
        </span><span style="color: #800000; font-weight: bold;">5</span><span style="color: #000000;">、导出删除的表zjs_base，恢复到生产库，验证数据可用性、完整性。
        </span><span style="color: #800000; font-weight: bold;">6</span>、启动应用连接数据库。</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **确定恢复所需时间**

<div>
  <div class="cnblogs_code">
    <pre>恢复窗口要多长？<span style="color: #008080;">--</span><span style="color: #008080;">--> 预计3小时</span>
        和你恢复<span style="color: #808080;">+</span>验证<span style="color: #808080;">+</span><span style="color: #000000;">意外情况有关。
业务停多长时间？</span><span style="color: #008080;">--</span><span style="color: #008080;">--> 6小时？或者更多？更少？    </span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

### <span id="186"><span style="color: #ff0000; background-color: #ffff00;">1.8.6 【模拟】生产事故恢复</span></span>

**　数据创建阶段**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

　　1、创建备份需要的目录

<div>
  <div class="cnblogs_code">
    <pre>mkdir <span style="color: #0000ff;">full</span>  inc1 inc2</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　2、周日全备

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">user=root --password=123 --no-timestamp /backup/xbackup/full/</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　3、模拟数据变化

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">use</span><span style="color: #000000;"> oldboy
</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> test(id <span style="color: #0000ff;">int</span>,name <span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">20</span>),age <span style="color: #0000ff;">int</span><span style="color: #000000;">);
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">8</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">outman</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">99</span><span style="color: #000000;">);
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">9</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">outgirl</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">100</span><span style="color: #000000;">);
</span><span style="color: #0000ff;">commit</span>;</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　4、周一增量备份

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">user=root --password=123 --incremental --no-timestamp --incremental-basedir=/backup/xbackup/full/ /backup/xbackup/inc1</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　5、模拟数据变化

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">use</span><span style="color: #000000;"> oldboy
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">8</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">outman1</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">119</span><span style="color: #000000;">);
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">9</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">outgirl1</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">120</span><span style="color: #000000;">);
</span><span style="color: #0000ff;">commit</span>;</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　6、周二的增量备份

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">user=root --password=123 --incremental --no-timestamp --incremental-basedir=/backup/xbackup/inc1 /backup/xbackup/inc2</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　7. 再插入新的行操作

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">use</span><span style="color: #000000;"> oldboy
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">10</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">outman2</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">19</span><span style="color: #000000;">);
</span><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> test <span style="color: #0000ff;">values</span>(<span style="color: #800000; font-weight: bold;">11</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">outgirl2</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">10</span><span style="color: #000000;">);
</span><span style="color: #0000ff;">commit</span>;</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<span style="background-color: #ffff00;"><strong>模拟误操作事故</strong></span>

　　模拟场景，周二下午2点误删除test表

<div>
  <div class="cnblogs_code">
    <pre>    <span style="color: #0000ff;">use</span><span style="color: #000000;"> oldboy;
    </span><span style="color: #0000ff;">drop</span> <span style="color: #0000ff;">table</span> test;</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<span style="background-color: #ffff00;"><strong>准备恢复数据</strong></span>

　　1.准备xtrabackup备份，合并备份

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --redo-only /backup/xbackup/full</span>
innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --redo-only --incremental-dir=/backup/xbackup/inc1 /backup/xbackup/full</span>
innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log  --incremental-dir=/backup/xbackup/inc2 /backup/xbackup/full</span>
innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log /backup/xbackup/full</span></pre>
  </div>
  
  <p>
    　　2．确认binlog起点，准备截取binlog。
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre> cd <span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span>xbackup<span style="color: #808080;">/</span>inc2<span style="color: #808080;">/</span><span style="color: #000000;">
 cat xtrabackup_binlog_info 
 mysql</span><span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000001</span>    <span style="color: #800000; font-weight: bold;">1121</span></pre>
  </div>
  
  <p>
    　　 3.截取到drop操作<strong>之前</strong>的binlog
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>    mysqlbinlog  <span style="color: #008080;">--</span><span style="color: #008080;">start-position=1121 /tmp/mysql-bin.000003 </span>
    找到drop之前的event和postion号做日志截取，假如 <span style="color: #800000; font-weight: bold;">1437</span><span style="color: #000000;">
    mysqlbinlog  </span><span style="color: #008080;">--</span><span style="color: #008080;">start-position=1121 --stop-position=1437    /tmp/mysql-bin.000003 >/tmp/incbinlog.sql</span>
    </pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　4．关闭数据库、备份二进制日志

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #808080;">/</span>etc<span style="color: #808080;">/</span>init.d<span style="color: #808080;">/</span><span style="color: #000000;">mysqld stop
cd </span><span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #000000;">
cp mysql</span><span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000001</span> <span style="color: #808080;">/</span>tmp</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

5.删除MySQL所有数据

<div>
  <div class="cnblogs_code">
    <pre>cd <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #000000;">
rm </span><span style="color: #808080;">-</span>rf <span style="color: #808080;">*</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<span style="background-color: #ffff00;"><strong>恢复数据</strong></span>

　　1．将全量备份的数据恢复到数据目录下

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">copy-back /backup/xbackup/full/</span>
chown <span style="color: #808080;">-</span>R mysql.mysql <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span>
<span style="color: #808080;">/</span>etc<span style="color: #808080;">/</span>init.d<span style="color: #808080;">/</span>mysqld start</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

　　2.恢复binlog记录

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">set</span> sql_log_bin<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;"></span><span style="color: #000000;">
source </span><span style="color: #808080;">/</span>tmp<span style="color: #808080;">/</span>incbinlog.sql</pre>
  </div>
</div>

### <span id="187_xtarbackup">1.8.7 xtarbackup导出</span>

　　(1)&ldquo;导出&rdquo;表 导出表是在备份的prepare阶段进行的，因此，一旦完全备份完成，就可以在prepare过程中通过--export选项将某表导出了：

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --export /path/to/backup</span></pre>
  </div>
  
  <p>
    　　&nbsp; 此命令会为每个innodb表的表空间创建一个以.exp结尾的文件，这些以.exp结尾的文件则可以用于导入至其它服务器。
  </p>
</div>

　　(2)&ldquo;导入&rdquo;表 要在mysql服务器上导入来自于其它服务器的某innodb表，需要先在当前服务器上创建一个跟原表表结构一致的表，而后才能实现将表导入：

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TABLE</span> mytable (...)  ENGINE<span style="color: #808080;">=</span>InnoDB; </pre>
  </div>
  
  <p>
    　　然后将此表的表空间删除：
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">ALTER</span> <span style="color: #0000ff;">TABLE</span> mydatabase.mytable  DISCARD TABLESPACE; </pre>
  </div>
  
  <p>
    　　接下来，将来自于&ldquo;导出&rdquo;表的服务器的mytable表的mytable.ibd和mytable.exp文件复制到当前服务器的数据目录，然后使用如下命令将其&ldquo;导入&rdquo;：(记得改权限)
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">ALTER</span> <span style="color: #0000ff;">TABLE</span> mydatabase.mytable  IMPORT TABLESPACE;</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

<span style="background-color: #ffff00;"><strong>示例:</strong></span>

<div>
  <div class="cnblogs_code">
    <pre>innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">user=root --password=123 --no-timestamp /backup/xbackup/full/</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;** **进入到全备的数据库目录下**

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 haha</span><span style="color: #ff0000;">]</span><span style="color: #000000;"># ls
db.opt  PENALTIES.frm  PENALTIES.ibd  PLAYERS.frm  PLAYERS.ibd
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 haha</span><span style="color: #ff0000;">]</span><span style="color: #000000;"># pwd
</span><span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span>xbackup<span style="color: #808080;">/</span><span style="color: #0000ff;">full</span><span style="color: #808080;">/</span>haha</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 导出表

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 haha</span><span style="color: #ff0000;">]</span># innobackupex <span style="color: #008080;">--</span><span style="color: #008080;">apply-log --export /backup/xbackup/full/  </span>
<span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 haha</span><span style="color: #ff0000;">]</span><span style="color: #000000;"># ls
db.opt         PENALTIES.</span><span style="color: #ff00ff;">exp</span>  PENALTIES.ibd  PLAYERS.<span style="color: #ff00ff;">exp</span><span style="color: #000000;">  PLAYERS.ibd
PENALTIES.cfg  PENALTIES.frm  PLAYERS.cfg    PLAYERS.frm</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 创建出同结构表

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TABLE</span><span style="color: #000000;"> `PLAYERS` (
  `PLAYERNO` </span><span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `NAME` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">15</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `INITIALS` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">3</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `BIRTH_DATE` date </span><span style="color: #0000ff;">DEFAULT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `SEX` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">1</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `JOINED` </span><span style="color: #0000ff;">smallint</span>(<span style="color: #800000; font-weight: bold;">6</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `STREET` </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">30</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `HOUSENO` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">4</span>) <span style="color: #0000ff;">DEFAULT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `POSTCODE` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">6</span>) <span style="color: #0000ff;">DEFAULT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `TOWN` </span><span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">30</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `PHONENO` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">13</span>) <span style="color: #0000ff;">DEFAULT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  `LEAGUENO` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">4</span>) <span style="color: #0000ff;">DEFAULT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,
  </span><span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> (`PLAYERNO`)
) ENGINE</span><span style="color: #808080;">=</span>InnoDB <span style="color: #0000ff;">DEFAULT</span> CHARSET<span style="color: #808080;">=</span>utf8</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 复制恢复数据到库下

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 haha</span><span style="color: #ff0000;">]</span># cp  PLAYERS.ibd  PLAYERS.<span style="color: #ff00ff;">exp</span>  <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span><span style="color: #000000;">
cp: overwrite `</span><span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #0000ff;">backup</span><span style="color: #808080;">/</span>PLAYERS.ibd<span style="color: #ff0000;">'</span><span style="color: #ff0000;">? y</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 恢复数据

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">ALTER</span> <span style="color: #0000ff;">TABLE</span> <span style="color: #0000ff;">backup</span><span style="color: #000000;">.PLAYERS  DISCARD TABLESPACE;
Query OK, </span><span style="color: #800000; font-weight: bold;"></span> rows affected (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

### <span id="188_innobackupex">1.8.8 innobackupex参数说明</span>

<div align="center">
  <div align="center">
    <table class="MsoTable15Grid4Accent1" style="width: 100%; border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
      <tr>
        <td style="width: 21.18%; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #5b9bd5; border-bottom-color: #5b9bd5; border-left-color: #5b9bd5; border-right: none; background: #5b9bd5; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: center; text-indent: 15.75pt; mso-yfti-cnfc: 5;" align="center">
            <strong><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; color: white; mso-themecolor: background1;">参数</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #5b9bd5; border-right-color: #5b9bd5; border-bottom-color: #5b9bd5; border-left: none; background: #5b9bd5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: center; text-indent: 15.75pt; mso-yfti-cnfc: 1;" align="center">
            <strong><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman'; color: white; mso-themecolor: background1;">参数说明</span></strong>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--compress</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示压缩</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">数据文件的备份。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--compress-threads&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示并行压缩</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">worker</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">线程的数量。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--compress-chunk-size </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示每个压缩线程</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">worker buffer</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的大小，单位是字节，默认是</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">64K</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--encrypt&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示通过</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">ENCRYPTION_ALGORITHM</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的算法加密</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">数据文件的备份，目前支持的算法有</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">ASE128,AES192,AES256</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--encrypt-threads&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示并行加密的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">worker</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">线程数量。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--encrypt-chunk-size&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示每个加密线程</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">worker buffer</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的大小，单位是字节，默认是</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">64K</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--encrypt-key&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项使用合适长度加密</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">key</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，因为会记录到命令行，所以不推荐使用。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--encryption-key-file </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示文件必须是一个简单二进制或者文本文件，加密</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">key</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">可通过以下命令行命令生成：</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">openssl rand -base64 24</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--include&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示使用正则表达式匹配表的名字</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">[db.tb]</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，要求为其指定匹配要备份的表的完整名称，即</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">databasename.tablename</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--user&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示备份账号。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--password&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示备份的密码。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--port&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示备份数据库的端口。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--host&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示备份数据库的地址。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--databases</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项接受的参数为数据名，如果要指定多个数据库，彼此间需要以空格隔开；如：</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">"xtra_test dba_test"</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，同时，在指定某数据库时，也可以只指定其中的某张表。如：</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">"mydatabase.mytable"</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。该选项对</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">引擎表无效，还是会备份所有</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">表。此外，此选项也可以接受一个文件为参数，文件中每一行为一个要备份的对象。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--tables-file&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示指定含有表列表的文件，格式为</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">database.table</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，该选项直接传给</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--tables-file</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--socket&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">mysql.sock</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">所在位置，以便备份进程登录</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">mysql</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--no-timestamp&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项可以表示不要创建一个时间戳目录来存储备份，指定到自己想要的备份文件夹。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--ibbackup&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项指定了使用哪个</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrabackup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">二进制程序。</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">IBBACKUP-BINARY</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">是运行</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">percona xtrabackup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的命令。这个选项适用于</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrbackup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">二进制不在你是搜索和工作目录，如果指定了该选项</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">,innoabackupex</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">自动决定用的二进制程序。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--slave-info&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示对</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">slave</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">进行备份的时候使用，打印出</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">master</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的名字和</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">binlog pos</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，同样将这些信息以</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">change master</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的命令写入</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrabackup_slave_info</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">文件。可以通过基于这份备份启动一个从库。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--safe-slave-backup</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示为保证一致性复制状态，这个选项停止</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">SQL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">线程并且等到</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">show status</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">中的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">slave_open_temp_tables</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">为</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US"></span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的时候开始备份，如果没有打开临时表，</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">bakcup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">会立刻开始，否则</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">SQL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">线程启动或者关闭知道没有打开的临时表。如果</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">slave_open_temp_tables</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">在</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--safe-slave-backup-timeount </span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">（默认</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">300</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">秒）秒之后不为</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US"></span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，从库</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">sql</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">线程会在备份完成的时候重启。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--kill-long-queries-timeout</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示从开始执行</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">FLUSH TABLES WITH READ LOCK</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">到</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">kill</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">掉阻塞它的这些查询之间等待的秒数。默认值为</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US"></span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，不会</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">kill</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">任何查询，使用这个选项</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrabackup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">需要有</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">Process</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">和</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">super</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">权限。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--kill-long-query-type&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">kill</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的类型，默认是</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">all</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，可选</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">select</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--ftwrl-wait-threshold&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示检测到长查询，单位是秒，表示长查询的阈值。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--ftwrl-wait-query-type&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示获得全局锁之前允许那种查询完成，默认是</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">ALL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，可选</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">update</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--galera-info&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示生成了包含创建备份时候本地节点状态的文件</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrabackup_galera_info</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">文件，该选项只适用于备份</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">PXC</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--stream&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示流式备份的格式，</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">backup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">完成之后以指定格式到</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">STDOUT</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，目前只支持</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">tar</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">和</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xbstream</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--defaults-file&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项指定了从哪个文件读取</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">MySQL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">配置，必须放在命令行第一个选项的位置。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--defaults-extra-file&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项指定了在标准</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">defaults-file</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">之前从哪个额外的文件读取</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">MySQL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">配置，必须在命令行的第一个选项的位置。一般用于存备份用户的用户名和密码的配置文件。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">----defaults-group&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示从配置文件读取的组，</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innobakcupex</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">多个实例部署时使用。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--no-lock</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示关闭</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">FTWRL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的表锁，只有在所有表都是</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">Innodb</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">表并且不关心</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">backup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">binlog pos</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">点，如果有任何</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">DDL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">语句正在执行或者非</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">InnoDB</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">正在更新时（包括</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">mysql</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">库下的表），都不应该使用这个选项，后果是导致备份数据不一致，如果考虑备份因为获得锁失败，可以考虑</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--safe-slave-backup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">立刻停止复制线程。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--tmpdir</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示指定</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--stream</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的时候，指定临时文件存在哪里，在</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">streaming</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">和拷贝到远程</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">server</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">之前，事务日志首先存在临时文件里。在</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">使用参数</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">stream=tar</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">备份的时候，你的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrabackup_logfile</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">可能会临时放在</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">/tmp</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">目录下，如果你备份的时候并发写入较大的话</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US"> xtrabackup_logfile</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">可能会很大</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">(5G+)</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，很可能会撑满你的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">/tmp</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">目录，可以通过参数</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--tmpdir</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">指定目录来解决这个问题。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--history&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">percona server </span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的备份历史记录在</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">percona_schema.xtrabackup_history</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">表。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示创建一个增量备份，需要指定</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental-basedir</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental-basedir&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示接受了一个字符串参数指定含有</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">full backup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">的目录为增量备份的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">base</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">目录，与</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">同时使用。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental-dir&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示增量备份的目录。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental-force-scan</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示创建一份增量备份时，强制扫描所有增量备份中的数据页。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental-lsn&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示指定增量备份的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">LSN</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，与</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">选项一起使用。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental-history-name</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示存储在</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">PERCONA_SCHEMA.xtrabackup_history</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">基于增量备份的历史记录的名字。</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">Percona Xtrabackup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">搜索历史表查找最近（</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innodb_to_lsn</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">）成功备份并且将</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">to_lsn</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">值作为增量备份启动出事</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">lsn.</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">与</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">innobackupex--incremental-history-uuid</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">互斥。如果没有检测到有效的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">lsn</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">，</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrabackup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">会返回</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">error</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--incremental-history-uuid</span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示存储在</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">percona_schema.xtrabackup_history</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">基于增量备份的特定历史记录的</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">UUID</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--close-files&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示关闭不再访问的文件句柄，当</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">xtrabackup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">打开表空间通常并不关闭文件句柄目的是正确的处理</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">DDL</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">操作。如果表空间数量巨大，这是一种可以关闭不再访问的文件句柄的方法。使用该选项有风险，会有产生不一致备份的可能。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; background: #deeaf6; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 68;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--compact&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; background: #deeaf6; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 64;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示创建一份没有辅助索引的紧凑的备份。</span>
          </p>
        </td>
      </tr>
      
      <tr>
        <td style="width: 21.18%; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #9cc2e5; border-bottom-color: #9cc2e5; border-left-color: #9cc2e5; border-top: none; padding: 0cm 5.4pt;" width="21%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt; mso-yfti-cnfc: 4;">
            <strong><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--throttle&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></strong>
          </p>
        </td>
        
        <td style="width: 78.82%; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #9cc2e5; border-right-width: 1pt; border-right-color: #9cc2e5; padding: 0cm 5.4pt;" valign="top" width="78%">
          <p class="MsoNormal" style="margin-bottom: .0001pt; text-align: justify; text-justify: inter-ideograph; text-indent: 15.75pt;">
            <span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">该选项表示每秒</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">IO</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">操作的次数，只作用于</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">bakcup</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">阶段有效。</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">apply-log</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">和</span><span style="font-size: 10.0pt; font-family: 'Times New Roman',serif; mso-fareast-font-family: 宋体;" lang="EN-US">--copy-back</span><span style="font-size: 10.0pt; font-family: 宋体; mso-ascii-font-family: 'Times New Roman'; mso-hansi-font-family: 'Times New Roman'; mso-bidi-font-family: 'Times New Roman';">不生效不要一起用。</span>
          </p>
        </td>
      </tr>
    </table>
  </div>
</div>

## <span id="19">1.9 参考文献</span>

<div>
  <div class="cnblogs_code">
    <pre>https:<span style="color: #808080;">//</span>www.cnblogs.com<span style="color: #808080;">/</span>cchust<span style="color: #808080;">/</span>p<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">5452557</span><span style="color: #000000;">.html
http:</span><span style="color: #808080;">//</span>www.cnblogs.com<span style="color: #808080;">/</span>gomysql<span style="color: #808080;">/</span>p<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">3650645</span><span style="color: #000000;">.html  xtrabackup 详解
https:</span><span style="color: #808080;">//</span>www.percona.com<span style="color: #808080;">/</span>software<span style="color: #808080;">/</span>mysql<span style="color: #808080;">-</span><span style="color: #0000ff;">database</span><span style="color: #808080;">/</span>percona<span style="color: #808080;">-</span><span style="color: #000000;">xtrabackup
https:</span><span style="color: #808080;">//</span>learn.percona.com<span style="color: #808080;">/</span>hubfs<span style="color: #808080;">/</span>Manuals<span style="color: #808080;">/</span>Percona_Xtra_Backup<span style="color: #808080;">/</span>Percona_XtraBackup_2.<span style="color: #800000; font-weight: bold;">4</span><span style="color: #808080;">/</span>Percona<span style="color: #808080;">-</span>XtraBackup<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">2.4</span>.<span style="color: #800000; font-weight: bold;">9</span>.pdf</pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

&nbsp;

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#i">?新年贺词?</a>
    </li>
    <li>
      <a href="#11">1.1 备份的原因</a><ul>
        <li>
          <a href="#111">1.1.1 备份的目录</a>
        </li>
        <li>
          <a href="#112">1.1.2 备份中需要考虑的问题</a>
        </li>
        <li>
          <a href="#113">1.1.3 备份的类型</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#12">1.2 备份的方式</a><ul>
        <li>
          <a href="#121">1.2.1 冷备份</a>
        </li>
        <li>
          <a href="#122">1.2.2 快照备份</a>
        </li>
        <li>
          <a href="#123_SQL">1.2.3 逻辑备份（文本表示：SQL 语句）</a>
        </li>
        <li>
          <a href="#124">1.2.4 其他常用的备份方式</a>
        </li>
        <li>
          <a href="#125">1.2.5 备份工具的介绍</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#13_mysqldump">1.3 mysqldump备份介绍</a><ul>
        <li>
          <a href="#131_mysqldump">1.3.1 mysqldump备份流程</a>
        </li>
        <li>
          <a href="#132">1.3.2 常用的备份参数</a>
        </li>
        <li>
          <a href="#133_-A">1.3.3 -A 参数</a>
        </li>
        <li>
          <a href="#134_-B">1.3.4 -B 参数</a>
        </li>
        <li>
          <a href="#135_--master-data12">1.3.5 --master-data={1|2}参数</a>
        </li>
        <li>
          <a href="#136_--single-transaction">1.3.6 --single-transaction 参数</a>
        </li>
        <li>
          <a href="#137_--flush-logs-F">1.3.7 --flush-logs参数/-F</a>
        </li>
        <li>
          <a href="#138">1.3.8 压缩备份</a>
        </li>
        <li>
          <a href="#139_Mysqldump">1.3.9 使用Mysqldump备份进行恢复实践</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#14">1.4 【模拟】增量恢复企业案例</a><ul>
        <li>
          <a href="#141">1.4.1 前提条件:</a>
        </li>
        <li>
          <a href="#142">1.4.2 环境准备</a>
        </li>
        <li>
          <a href="#143">1.4.3 恢复数据准备</a>
        </li>
        <li>
          <a href="#144">1.4.4 进行数据恢复</a>
        </li>
        <li>
          <a href="#145_binlog">1.4.5 多个binlog问题</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#15_mysql">1.5 mysql数据库实际生产惨案</a><ul>
        <li>
          <a href="#151">1.5.1 发生背景</a>
        </li>
        <li>
          <a href="#152">1.5.2 怎么解决的</a>
        </li>
        <li>
          <a href="#153">1.5.3 项目总结</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#16">1.6 备份工具的选择</a><ul>
        <li>
          <a href="#161_nbspnbspnbsp">1.6.1 数据量大，变换量小&nbsp;&nbsp;&nbsp;</a>
        </li>
        <li>
          <a href="#162">1.6.2 数据量小，变化量大</a>
        </li>
        <li>
          <a href="#163">1.6.3 数据量、变化量都大</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#17_xtrabackup">1.7 xtrabackup备份软件</a><ul>
        <li>
          <a href="#171_Xtrabackup">1.7.1 Xtrabackup介绍</a>
        </li>
        <li>
          <a href="#171_Xtrabackup-2">1.7.1 Xtrabackup优点</a>
        </li>
        <li>
          <a href="#172">1.7.2 备份原理</a>
        </li>
        <li>
          <a href="#173_xtrabackup">1.7.3 xtrabackup的安装</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#18_xtrabackup">1.8 xtrabackup实践操作</a><ul>
        <li>
          <a href="#181">1.8.1 全量备份与恢复</a>
        </li>
        <li>
          <a href="#182">1.8.2 增量备份与恢复</a>
        </li>
        <li>
          <a href="#183">1.8.3 数据库备份策略</a>
        </li>
        <li>
          <a href="#184">1.8.4 真实生产实战案例分析</a>
        </li>
        <li>
          <a href="#185">1.8.5 故障恢复小结</a>
        </li>
        <li>
          <a href="#186">1.8.6 【模拟】生产事故恢复</a>
        </li>
        <li>
          <a href="#187_xtarbackup">1.8.7 xtarbackup导出</a>
        </li>
        <li>
          <a href="#188_innobackupex">1.8.8 innobackupex参数说明</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#19">1.9 参考文献</a>
    </li>
  </ul>
</div>