---
title: MySQL Replication 主从复制全方位解决方案
author: 惨绿少年
type: post
date: 2018-01-02T21:33:00+00:00
url: /clsn/lx300.html
Baidusubmit:
  - 1
views:
  - 201
categories:
  - Linux运维
  - MySQL
  - 数据库
  - 运维基本功

---
## <span id="11">1.1 主从复制基础概念</span>

　　　　在了解主从复制之前必须要了解的就是数据库的二进制日志(binlog),主从复制架构大多基于二进制日志进行，二进制日志相关信息参考：<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/8087678.html#_label6"  target="_blank">http://www.cnblogs.com/clsn/p/8087678.html#_label6</a>

### <span id="111">1.1.1 二进制日志管理说明</span>

**　　<span style="background-color: #00ffff;">二进制日志在哪？如何设置位置和命名？</span>**

<div>
  <p class="a4">
    　　　　在my.cnf文件中使用 log-bin = 指定；命名规则为 mysql-bin.000000 （后为6位数字）
  </p>
</div>

　　<span style="background-color: #00ffff;">二进制日志位置</span>

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> show variables <span style="color: #808080;">like</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">%log_bin%</span><span style="color: #ff0000;">'</span><span style="color: #000000;"> ;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-------------------------------+-----------------------------------------+</span>
<span style="color: #808080;">|</span> Variable_name                   <span style="color: #808080;">|</span> Value                                   <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-------------------------------+-----------------------------------------+</span>
<span style="color: #808080;">|</span> log_bin                         <span style="color: #808080;">|</span> <span style="color: #0000ff;">ON</span>                                      <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> log_bin_basename                <span style="color: #808080;">|</span> <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span>mysql<span style="color: #808080;">-</span>bin       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> log_bin_index                   <span style="color: #808080;">|</span> <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>data<span style="color: #808080;">/</span>mysql<span style="color: #808080;">-</span>bin.<span style="color: #0000ff;">index</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> log_bin_trust_function_creators <span style="color: #808080;">|</span> <span style="color: #0000ff;">OFF</span>                                     <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> log_bin_use_v1_row_events       <span style="color: #808080;">|</span> <span style="color: #0000ff;">OFF</span>                                     <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> sql_log_bin                     <span style="color: #808080;">|</span> <span style="color: #0000ff;">ON</span>                                      <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-------------------------------+-----------------------------------------+</span>
<span style="color: #800000; font-weight: bold;">6</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.06</span> sec)</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 日志命名

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> show <span style="color: #0000ff;">binary</span><span style="color: #000000;"> logs;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-----------+</span>
<span style="color: #808080;">|</span> Log_name         <span style="color: #808080;">|</span> File_size <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-----------+</span>
<span style="color: #808080;">|</span> mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000001</span> <span style="color: #808080;">|</span>      <span style="color: #800000; font-weight: bold;">2979</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000002</span> <span style="color: #808080;">|</span>       <span style="color: #800000; font-weight: bold;">120</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-----------+</span>
<span style="color: #800000; font-weight: bold;">2</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
  
  <p>
    　　<span style="background-color: #00ffff;"><strong>二进制日志记录什么？</strong></span>
  </p>
</div>

<div>
  <p class="a4">
    　　　　二进制日志中记录的是一个个完成的事件
  </p>
</div>

**　　<span style="background-color: #00ffff;">二进制日志格式是怎样的？</span>**

<div>
  <p class="a4">
    　　　　推荐使用row格式
  </p>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 查看当前使用的日志 格式。

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> show variables <span style="color: #808080;">like</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">%format%</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------------+-------------------+</span>
<span style="color: #808080;">|</span> Variable_name            <span style="color: #808080;">|</span> Value             <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------------+-------------------+</span>
<span style="color: #808080;">|</span> binlog_format            <span style="color: #808080;">|</span> ROW               <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> date_format              <span style="color: #808080;">|</span> <span style="color: #808080;">%</span>Y<span style="color: #808080;">-%</span>m<span style="color: #808080;">-%</span>d          <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> datetime_format          <span style="color: #808080;">|</span> <span style="color: #808080;">%</span>Y<span style="color: #808080;">-%</span>m<span style="color: #808080;">-%</span>d <span style="color: #808080;">%</span>H:<span style="color: #808080;">%</span>i:<span style="color: #808080;">%</span>s <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> default_week_format      <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;"></span>                 <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> innodb_file_format       <span style="color: #808080;">|</span> Antelope          <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> innodb_file_format_check <span style="color: #808080;">|</span> <span style="color: #0000ff;">ON</span>                <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> innodb_file_format_max   <span style="color: #808080;">|</span> Antelope          <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> time_format              <span style="color: #808080;">|</span> <span style="color: #808080;">%</span>H:<span style="color: #808080;">%</span>i:<span style="color: #808080;">%</span>s          <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------------+-------------------+</span>
<span style="color: #800000; font-weight: bold;">8</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
  
  <p>
    　　<span style="background-color: #00ffff;"><strong>二进制日志如何滚动？</strong></span>
  </p>
</div>

<div>
  <p class="a4">
    　　　　每次重启都会刷新日志，也可以通过命令进行刷新 &nbsp;reset master;
  </p>
</div>

**　　<span style="background-color: #00ffff;">二进制日志用来干嘛？</span>**

<div>
  <p class="a4">
    　　　　备份恢复
  </p>
  
  <p class="a4">
    　　　　起始点的备份恢复
  </p>
</div>

**　　<span style="background-color: #00ffff;">二进制日志的操作命令？</span>**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;　　&nbsp; 查看都有哪些二进制日志

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> show <span style="color: #0000ff;">binary</span><span style="color: #000000;"> logs;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-----------+</span>
<span style="color: #808080;">|</span> Log_name         <span style="color: #808080;">|</span> File_size <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-----------+</span>
<span style="color: #808080;">|</span> mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000001</span> <span style="color: #808080;">|</span>      <span style="color: #800000; font-weight: bold;">2979</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000002</span> <span style="color: #808080;">|</span>       <span style="color: #800000; font-weight: bold;">167</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000003</span> <span style="color: #808080;">|</span>       <span style="color: #800000; font-weight: bold;">120</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-----------+</span>
<span style="color: #800000; font-weight: bold;">3</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
  
  <p>
    　　&nbsp; &nbsp; &nbsp; &nbsp;查看当前使用的二进制日志文件
  </p>
</div>

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span><span style="color: #000000;"> show master status;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+----------+--------------+------------------+-------------------+</span>
<span style="color: #808080;">|</span> <span style="color: #0000ff;">File</span>             <span style="color: #808080;">|</span> Position <span style="color: #808080;">|</span> Binlog_Do_DB <span style="color: #808080;">|</span> Binlog_Ignore_DB <span style="color: #808080;">|</span> Executed_Gtid_Set <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+----------+--------------+------------------+-------------------+</span>
<span style="color: #808080;">|</span> mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000003</span> <span style="color: #808080;">|</span>      <span style="color: #800000; font-weight: bold;">120</span> <span style="color: #808080;">|</span>              <span style="color: #808080;">|</span>                  <span style="color: #808080;">|</span>                   <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+----------+--------------+------------------+-------------------+</span>
<span style="color: #800000; font-weight: bold;">1</span> row <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
  
  <p>
    　　　　<em>binlog</em><em>相关详情参照：http://www.cnblogs.com/clsn/p/8087678.html#_label6</em>
  </p>
</div>

### <span id="112_mysql">1.1.2 mysql传统备份方式和缺陷</span>

　　1、二进制日志备份

　　2、mysqldump

<div>
  <p class="a4">
    &nbsp;&nbsp; 　　　　a)必须有数据库服务器完成逻辑工作，需要更多地cpu周期
  </p>
  
  <p class="a4">
    　　　　&nbsp; &nbsp;b)逻辑备份还原速度慢：需要MySQL加载和解释语句、转化存储格式、重建引擎
  </p>
</div>

　　3、xtrabackup

<div>
  <p class="a4">
    &nbsp;　　　　&nbsp;&nbsp;a)文件大
  </p>
  
  <p class="a4">
    　　　　　b)不总是可以跨平台、操作系统和MySQL版本
  </p>
</div>

### <span id="113_MySQL">1.1.3 MySQL主从复制能为我们做什么</span>

　　　　　高可用、辅助备份、分担负载

## <span id="12_MySQL">1.2 MySQL主从复制介绍</span>

### <span id="121">1.2.1 复制技术</span>

<span style="background-color: #99cc00;"><strong>作用</strong></span>

<div>
  <blockquote>
    <p class="a4">
      　　1.保证数据安全(异机实时备份)
    </p>
    
    <p class="a4">
      　　2.保证服务持续运行（宕机接管）
    </p>
  </blockquote>
</div>

<span style="background-color: #99cc00;"><strong>主从复制实现基本原理</strong></span>

<div>
  <blockquote>
    <p class="a4">
      　　1.自带功能，复制是 MySQL 的一项功能，允许服务器将更改从一个实例复制到另一个实例。
    </p>
    
    <p class="a4">
      　　2.主服务器将所有数据和结构更改记录到二进制日志中。
    </p>
    
    <p class="a4">
      　　3.从属服务器从主服务器请求该二进制日志并在本地应用其内容。即通过把主库的binlog传送到从库，从新解析应用到从库。
    </p>
  </blockquote>
</div>

### <span id="122">1.2.2 复制架构</span>

mysql复制的应用常见场景：

<div>
  <blockquote>
    <div>
      <p class="a">
        应用场景1：从服务器作为主服务器的实时数据备份
      </p>
      
      <p class="a">
        应用场景2：主从服务器实现读写分离，从服务器实现负载均衡
      </p>
      
      <p class="a">
        应用场景3：把多个从服务器根据业务重要性进行拆分访问
      </p>
    </div>
  </blockquote>
</div>

#### <span id="1221nbsp_ndash"><strong>1.2.2.1&nbsp; </strong><strong>主&ndash;从复制</strong></span>

　　传统的 MySQL 复制提供了一种简单的主&ndash;从复制方法。 有一个主，以及一个或多个从。 主节点执行和提交事务，然后将它们（异步地）发送到从节点，以重新执行（在基于语句的复制中）或应用（在基于行的复制中）。 这是一个 shared-nothing 的系统，默认情况下所有 server 成员都有一个完整的数据副本。

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230145845570-1921897724.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
</p>

<p align="center">
  (图)MySQL 异步复制
</p>

　　还有一个半同步复制，它在协议中添加了一个同步步骤。 这意味着主节点在提交时需要等待从节点确认它已经接收到事务。只有这样，主节点才能继续提交操作。

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230145902382-366690047.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />&nbsp;
</p>

<p align="center">
  (图)MySQL 异步复制
</p>

　　在上面的两个图片中，可以看到传统异步 MySQL 复制协议（以及半同步）的图形展示。 蓝色箭头表示在不同 server 之间或者 server 与 client 应用之间的信息交互。

### <span id="123_MySQL">1.2.3 MySQL主从复制原理介绍</span>

<span style="background-color: #ffff00;"><strong>复制过程：</strong></span>

<div>
  <blockquote>
    <div>
      <p class="a">
        1、开启binlog日志，通过把主库的binlog传送到从库，从新解析应用到从库。
      </p>
      
      <p class="a">
        2、复制需要3个线程（dump、io、sql）完成，5.6从库多个sql。
      </p>
      
      <p class="a">
        3、复制是异步的过程。主从复制是异步的逻辑的SQL语句级的复制。
      </p>
    </div>
  </blockquote>
</div>

<span style="background-color: #ffff00;"><strong>复制前提：</strong></span>

<div>
  <blockquote>
    <div>
      <p class="a">
        1、主服务期一定要打开二进制日志
      </p>
      
      <p class="a">
        2、必须两台服务器（或者是多个实例）
      </p>
      
      <p class="a">
        3、从服务器需要一次<strong><em>数据初始化</em></strong>
      </p>
      
      <p class="a">
        &nbsp; &nbsp;&nbsp;&nbsp;3.1如果主从服务器都是新搭建的话，可以不做初始化
      </p>
      
      <p class="a">
        &nbsp; &nbsp; &nbsp;3.2如果主服务器已经运行了很长时间了，可以通过备份将主库数据恢复到从库。
      </p>
      
      <p class="a">
        4、主库必须要有对从库复制请求的用户。
      </p>
      
      <p class="a">
        5、从库需要有relay-log设置，存放从主库传送过来的二进制日志 <em>show variables&nbsp; like '%relay%';</em>
      </p>
      
      <p class="a">
        6、在第一次的时候，从库需要change master to 去连接主库。
      </p>
      
      <p class="a">
        7、change master信息需要存放到master.info中 &nbsp;<em>show variables&nbsp; like '%master_info%';</em>
      </p>
      
      <p class="a">
        8、从库怎么知道，主库发生了新的变化?通过relay-log.info记录的已经应用过的relay-log信息。
      </p>
      
      <p class="a">
        <strong>9</strong><strong>、在复制过程中涉及到的线程</strong>
      </p>
      
      <p class="a">
        &nbsp; &nbsp; &nbsp; 从库会开启一个IO thread(线程)，负责连接主库，请求binlog，接收binlog并写入relay-log。
      </p>
      
      <p class="a">
        &nbsp; &nbsp; &nbsp; 从库会开启一个SQL thread(线程)，负责执行relay-log中的事件。
      </p>
      
      <p class="a">
        &nbsp; &nbsp; &nbsp; 主库会开启一个dump thrad(线程)，负责响应从IO thread的请求。
      </p>
    </div>
  </blockquote>
</div>

<span style="background-color: #ffff00;"><strong>主从怎么实现的？</strong></span>

> 1、通过二进制日志
> 
> 2、至少两台（主、从）
> 
> 3、主服务器的二进制日志&ldquo;拿&rdquo;到从服务器上再运行一遍。
> 
> 4、通过网络连接两台机器，一般都会出现延迟的状态。也可以说是异步的。

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230150035101-381337558.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
</p>

### <span id="124">1.2.4 执行原理--第一次开启主从过程</span>

<div>
  <blockquote>
    <p class="a">
      1、 从库通过手工执行change master to 语句连接主库，提供了连接的用户一切条件
    </p>
    
    <p class="a">
      <em>（user</em><em>、password</em><em>、port</em><em>、ip</em><em>）</em>
    </p>
    
    <p class="a">
      &nbsp;&nbsp;&nbsp; <em>并且让从库知道，二进制日志的起点位置（file</em><em>名&nbsp; position</em><em>号）</em>
    </p>
    
    <p class="a">
      &nbsp;&nbsp;&nbsp; <em>start slave</em>
    </p>
    
    <p class="a">
      2、从库的IO和主库的dump线程建立连接
    </p>
    
    <p class="a">
      3、从库根据change master to 语句提供的file名和position号，IO线程向主库发起binlog的请求
    </p>
    
    <p class="a">
      4、主库dump线程根据从库的请求，将本地binlog以events的方式发给从库IO线程
    </p>
    
    <p class="a">
      5、从库IO线程接收binlog evnets，并存放到本地relay-log中，传送过来的信息，会记录到master.info中。
    </p>
    
    <p class="a">
      6、从库SQL线程应用relay-log，并且把应用过的记录到relay-log.info,默认情况下，已经应用过的relay会自动被清理purge。
    </p>
    
    <p class="a">
      <span style="background-color: #ffff00; color: #ff0000;"><strong>到此位置，一次主从复制就完成</strong></span>
    </p>
  </blockquote>
</div>

<p class="a">
  　　一旦主从运行起来：
</p>

<p class="a">
  &nbsp;&nbsp;&nbsp;&nbsp;　　　　就不需要手工执行change master to，
</p>

<p class="a">
  &nbsp;　　&nbsp;&nbsp;　　 因为信息都会被存放到master.info
</p>

<p class="a">
  　　　　　　（user、password、port、ip，上次获取过的binlog信息file和position）中
</p>

<p class="a">
  &nbsp;&nbsp;&nbsp;　　　　&nbsp;其他的过程都是一样的
</p>

#### <span id="1241nbsp_mysql_replication_nbspnbsp"><strong>1.2.4.1&nbsp; </strong><strong>详细的mysql replication </strong><strong>过程&nbsp;&nbsp; </strong></span>

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230150155007-1749757503.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
</p>

## <span id="13">1.3 主从搭建配置</span>

　　　　本次主从搭建使用mysql多实例进行实验。_多实例配置参考文档进行配置：_<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/8038964.html#_label8"  target="_blank"><em>http://www.cnblogs.com/clsn/p/8038964.html#_label8</em></a>

### <span id="131_slave">1.3.1 多实例数据库slave配置</span>

<span style="background-color: #00ff00; color: #ffffff;"><strong><em><span style="text-decoration: underline;">系统环境说明：</span></em></strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@db02 ~]# <span style="color: #0000ff;">cat</span> /etc/redhat-<span style="color: #000000;">release 
CentOS release </span><span style="color: #800080;">6.9</span><span style="color: #000000;"> (Final)
[root@db02 </span>~]# <span style="color: #0000ff;">uname</span> -<span style="color: #000000;">r
</span><span style="color: #800080;">2.6</span>.<span style="color: #800080;">32</span>-<span style="color: #800080;">696</span><span style="color: #000000;">.el6.x86_64
[root@db02 </span>~]# /etc/init.d/<span style="color: #000000;">iptables status 
iptables: Firewall is not running.  # 注意：务必关闭防火墙（iptables  selinux）
[root@db02 </span>~<span style="color: #000000;">]# getenforce  
Disabled
[root@db02 </span>~]# mysql --<span style="color: #000000;">version
mysql  Ver </span><span style="color: #800080;">14.14</span> Distrib <span style="color: #800080;">5.6</span>.<span style="color: #800080;">36</span>, <span style="color: #0000ff;">for</span> Linux (x86_64) using  EditLine wrapper</pre>
  </div>
</div>

**1****、启动多实例数据库**

<div>
  <div class="cnblogs_code">
    <pre>[root@db02 ~]# /data/<span style="color: #800080;">3306</span>/<span style="color: #000000;">mysql start
Starting MySQL...
[root@db02 </span>~]# /data/<span style="color: #800080;">3307</span>/<span style="color: #000000;">mysql start
Starting MySQL...</span></pre>
  </div>
</div>

**2****、配置文件说明：**

<span style="background-color: #ffff00;"><strong><em>master </em></strong></span>**_<span style="background-color: #ffff00;">配置文件说明</span>：_**

<div>
  <div class="cnblogs_code">
    <pre>[root@db02 ~]# <span style="color: #0000ff;">cat</span> /data/<span style="color: #800080;">3306</span>/<span style="color: #000000;">my.cnf 
[client]
port            </span>= <span style="color: #800080;">3306</span><span style="color: #000000;">
socket          </span>= /data/<span style="color: #800080;">3306</span>/<span style="color: #000000;">mysql.sock

[mysqld]
user    </span>=<span style="color: #000000;"> mysql
port    </span>= <span style="color: #800080;">3306</span><span style="color: #000000;">
socket  </span>= /data/<span style="color: #800080;">3306</span>/<span style="color: #000000;">mysql.sock
basedir </span>= /application/<span style="color: #000000;">mysql
datadir </span>= /data/<span style="color: #800080;">3306</span>/<span style="color: #000000;">data
log</span>-bin = /data/<span style="color: #800080;">3306</span>/mysql-<span style="color: #000000;">bin
server</span>-<span style="color: #0000ff;">id</span> = <span style="color: #800080;">6</span>  # server <span style="color: #0000ff;">id</span><span style="color: #000000;"> 不能相同
skip_name_resolve </span>= <span style="color: #800080;"></span><span style="color: #000000;"> # 跳过域名解析参数


[mysqld_safe]
log</span>-error=/data/<span style="color: #800080;">3306</span>/<span style="color: #000000;">mysql_3306.err
pid</span>-<span style="color: #0000ff;">file</span>=/data/<span style="color: #800080;">3306</span>/mysqld.pid</pre>
  </div>
</div>

<span style="background-color: #ffff00;"><strong><em>slave </em></strong><strong><em>配置文件说明：</em></strong></span>

<div>
  <div class="cnblogs_code">
    <pre>[root@db02 ~]# <span style="color: #0000ff;">cat</span> /data/<span style="color: #800080;">3307</span>/<span style="color: #000000;">my.cnf 
[client]
port            </span>= <span style="color: #800080;">3307</span><span style="color: #000000;">
socket          </span>= /data/<span style="color: #800080;">3307</span>/<span style="color: #000000;">mysql.sock

[mysqld]
user    </span>=<span style="color: #000000;"> mysql
port    </span>= <span style="color: #800080;">3307</span><span style="color: #000000;">
socket  </span>= /data/<span style="color: #800080;">3307</span>/<span style="color: #000000;">mysql.sock
basedir </span>= /application/<span style="color: #000000;">mysql
datadir </span>= /data/<span style="color: #800080;">3307</span>/<span style="color: #000000;">data
log</span>-bin = /data/<span style="color: #800080;">3307</span>/mysql-<span style="color: #000000;">bin
server</span>-<span style="color: #0000ff;">id</span> = <span style="color: #800080;">7</span>  # server <span style="color: #0000ff;">id</span><span style="color: #000000;"> 不能相同
skip_name_resolve </span>= <span style="color: #800080;"></span><span style="color: #000000;">  # 跳过域名解析参数
read_only </span>= <span style="color: #800080;">1</span><span style="color: #000000;">  # 从库只读 （非root用户 ）

[mysqld_safe]
log</span>-error=/data/<span style="color: #800080;">3307</span>/<span style="color: #000000;">mysql_3307.err
pid</span>-<span style="color: #0000ff;">file</span>=/data/<span style="color: #800080;">3307</span>/mysqld.pid</pre>
  </div>
</div>

**3****、在主库创建复制用户**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 登陆到主数据库中：

<div>
  <div class="cnblogs_code">
    <pre>mysql <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #808080;">-</span>S <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">3306</span><span style="color: #808080;">/</span>mysql.sock</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 创建授权用户，注意是slave用户。

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">grant</span> <span style="color: #0000ff;">replication</span> slave  <span style="color: #0000ff;">on</span> <span style="color: #808080;">*</span>.<span style="color: #808080;">*</span> <span style="color: #0000ff;">to</span> repl@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">10.0.0.%</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span>;</pre>
  </div>
</div>

**4****、初始化从库数据**

<span style="background-color: #ffff00;">备份主库当前数据</span>

<div>
  <div class="cnblogs_code">
    <pre> mysqldump <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #808080;">-</span>A <span style="color: #808080;">-</span>B <span style="color: #808080;">-</span>F <span style="color: #008080;">--</span><span style="color: #008080;">master-data=2  -S /data/3306/mysql.sock >/tmp/full.sql</span></pre>
  </div>
</div>

_<span style="text-decoration: underline;">&nbsp;部分</span>__<span style="text-decoration: underline;">参数说明：（详细参照<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/8138015.html#_label2"  target="_blank">http://www.cnblogs.com/clsn/p/8138015.html#_label2</a>）</span>_

　　　　-F 刷新二进制日志

　　　　--master-data [=＃]这会导致二进制日志的位置和文件名被追加到输出中。

　　　　如果等于1，则将其打印为CHANGE MASTER命令; 如果等于2，那么该命令将以注释符号为前缀。

<span style="background-color: #ffff00;">到从库进行恢复</span>

<div>
  <div class="cnblogs_code">
    <pre> mysql <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #808080;">-</span>S <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">3307</span><span style="color: #808080;">/</span>mysql.sock</pre>
  </div>
  
  <p class="ad">
    # 恢复备份的数据
  </p>
  
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">set</span> sql_log_bin<span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;"></span><span style="color: #000000;">;
source </span><span style="color: #808080;">/</span>tmp<span style="color: #808080;">/</span><span style="color: #0000ff;">full</span>.sql</pre>
  </div>
</div>

**5****、开启从库复制**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 查看备份的当前使用的文件及POS号

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span><span style="color: #000000;"> show master status;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+----------+--------------+------------------+-------------------+</span>
<span style="color: #808080;">|</span> <span style="color: #0000ff;">File</span>             <span style="color: #808080;">|</span> Position <span style="color: #808080;">|</span> Binlog_Do_DB <span style="color: #808080;">|</span> Binlog_Ignore_DB <span style="color: #808080;">|</span> Executed_Gtid_Set <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+----------+--------------+------------------+-------------------+</span>
<span style="color: #808080;">|</span> mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000012</span> <span style="color: #808080;">|</span>      <span style="color: #800000; font-weight: bold;">120</span> <span style="color: #808080;">|</span>              <span style="color: #808080;">|</span>                  <span style="color: #808080;">|</span>                   <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+----------+--------------+------------------+-------------------+</span>
<span style="color: #800000; font-weight: bold;">1</span> row <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 登入数据库，进行slave配置。

<div>
  <div class="cnblogs_code">
    <pre>mysql <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #808080;">-</span>S <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">3307</span><span style="color: #808080;">/</span><span style="color: #000000;">mysql.sock 
CHANGE MASTER </span><span style="color: #0000ff;">TO</span><span style="color: #000000;">
  MASTER_HOST</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">10.0.0.52</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_USER</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">repl</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_PASSWORD</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_PORT</span><span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">3306</span><span style="color: #000000;">,
  MASTER_LOG_FILE</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">mysql-bin.000012</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_LOG_POS</span><span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">120</span><span style="color: #000000;">;
start  slave;  # 启动从库复制</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 该配置想关说明可以通过 help 获得。

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> help CHANGE MASTER <span style="color: #0000ff;">TO</span><span style="color: #000000;">
CHANGE MASTER </span><span style="color: #0000ff;">TO</span><span style="color: #000000;">
  MASTER_HOST</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">master2.mycompany.com</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_USER</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">replication</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_PASSWORD</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">bigs3cret</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_PORT</span><span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">3306</span><span style="color: #000000;">,
  MASTER_LOG_FILE</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">master2-bin.001</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  MASTER_LOG_POS</span><span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">,
  MASTER_CONNECT_RETRY</span><span style="color: #808080;">=</span><span style="color: #800000; font-weight: bold;">10</span>;</pre>
  </div>
</div>

### <span id="132">1.3.2 测试主从同步</span>

查看slave库的状态

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 主要查看&nbsp;&nbsp; Slave\_IO\_Running 与 Slave\_SQL\_Running 是否都为Yes

<div>
  <div class="cnblogs_code" onclick="cnblogs_code_show('1936e928-1ef2-4118-b99c-8ac93940265b')">
    <img id="code_img_closed_1936e928-1ef2-4118-b99c-8ac93940265b" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-5.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" /><img id="code_img_opened_1936e928-1ef2-4118-b99c-8ac93940265b" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('1936e928-1ef2-4118-b99c-8ac93940265b',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-5.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" /></p> 
    
    <div id="cnblogs_code_open_1936e928-1ef2-4118-b99c-8ac93940265b" class="cnblogs_code_hide">
      <pre><span style="color: #008080;"> 1</span> mysql<span style="color: #808080;">></span><span style="color: #000000;"> show slave status\G
</span><span style="color: #008080;"> 2</span> <span style="color: #808080;">***************************</span> <span style="color: #800000; font-weight: bold;">1</span>. row <span style="color: #808080;">***************************</span>
<span style="color: #008080;"> 3</span>                Slave_IO_State: Waiting <span style="color: #0000ff;">for</span> master <span style="color: #0000ff;">to</span><span style="color: #000000;"> send event
</span><span style="color: #008080;"> 4</span>                   Master_Host: <span style="color: #800000; font-weight: bold;">10.0</span>.<span style="color: #800000; font-weight: bold;">0.52</span>
<span style="color: #008080;"> 5</span> <span style="color: #000000;">                  Master_User: repl
</span><span style="color: #008080;"> 6</span>                   Master_Port: <span style="color: #800000; font-weight: bold;">3306</span>
<span style="color: #008080;"> 7</span>                 Connect_Retry: <span style="color: #800000; font-weight: bold;">60</span>
<span style="color: #008080;"> 8</span>               Master_Log_File: mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000010</span>
<span style="color: #008080;"> 9</span>           Read_Master_Log_Pos: <span style="color: #800000; font-weight: bold;">842</span>
<span style="color: #008080;">10</span>                Relay_Log_File: <span style="color: #800000; font-weight: bold;">3307</span><span style="color: #808080;">-</span>relay<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000018</span>
<span style="color: #008080;">11</span>                 Relay_Log_Pos: <span style="color: #800000; font-weight: bold;">283</span>
<span style="color: #008080;">12</span>         Relay_Master_Log_File: mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000010</span>
<span style="color: #008080;">13</span> <span style="color: #000000;">             Slave_IO_Running: Yes
</span><span style="color: #008080;">14</span> <span style="color: #000000;">            Slave_SQL_Running: Yes
</span><span style="color: #008080;">15</span> <span style="color: #000000;">              Replicate_Do_DB: 
</span><span style="color: #008080;">16</span> <span style="color: #000000;">          Replicate_Ignore_DB: 
</span><span style="color: #008080;">17</span> <span style="color: #000000;">           Replicate_Do_Table: 
</span><span style="color: #008080;">18</span> <span style="color: #000000;">       Replicate_Ignore_Table: 
</span><span style="color: #008080;">19</span> <span style="color: #000000;">      Replicate_Wild_Do_Table: 
</span><span style="color: #008080;">20</span> <span style="color: #000000;">  Replicate_Wild_Ignore_Table: 
</span><span style="color: #008080;">21</span>                    Last_Errno: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">22</span> <span style="color: #000000;">                   Last_Error: 
</span><span style="color: #008080;">23</span>                  Skip_Counter: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">24</span>           Exec_Master_Log_Pos: <span style="color: #800000; font-weight: bold;">842</span>
<span style="color: #008080;">25</span>               Relay_Log_Space: <span style="color: #800000; font-weight: bold;">455</span>
<span style="color: #008080;">26</span> <span style="color: #000000;">              Until_Condition: None
</span><span style="color: #008080;">27</span> <span style="color: #000000;">               Until_Log_File: 
</span><span style="color: #008080;">28</span>                 Until_Log_Pos: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">29</span> <span style="color: #000000;">           Master_SSL_Allowed: No
</span><span style="color: #008080;">30</span> <span style="color: #000000;">           Master_SSL_CA_File: 
</span><span style="color: #008080;">31</span> <span style="color: #000000;">           Master_SSL_CA_Path: 
</span><span style="color: #008080;">32</span> <span style="color: #000000;">              Master_SSL_Cert: 
</span><span style="color: #008080;">33</span> <span style="color: #000000;">            Master_SSL_Cipher: 
</span><span style="color: #008080;">34</span> <span style="color: #000000;">               Master_SSL_Key: 
</span><span style="color: #008080;">35</span>         Seconds_Behind_Master: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">36</span> <span style="color: #000000;">Master_SSL_Verify_Server_Cert: No
</span><span style="color: #008080;">37</span>                 Last_IO_Errno: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">38</span> <span style="color: #000000;">                Last_IO_Error: 
</span><span style="color: #008080;">39</span>                Last_SQL_Errno: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">40</span> <span style="color: #000000;">               Last_SQL_Error: 
</span><span style="color: #008080;">41</span> <span style="color: #000000;">  Replicate_Ignore_Server_Ids: 
</span><span style="color: #008080;">42</span>              Master_Server_Id: <span style="color: #800000; font-weight: bold;">6</span>
<span style="color: #008080;">43</span>                   Master_UUID: 4f344556<span style="color: #808080;">-</span>e0ab<span style="color: #808080;">-</span>11e7<span style="color: #808080;">-</span><span style="color: #800000; font-weight: bold;">9138</span><span style="color: #808080;">-</span><span style="color: #000000;">000c29d60ab3
</span><span style="color: #008080;">44</span>              Master_Info_File: <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">3307</span><span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #000000;">master.info
</span><span style="color: #008080;">45</span>                     SQL_Delay: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">46</span>           SQL_Remaining_Delay: <span style="color: #0000ff;">NULL</span>
<span style="color: #008080;">47</span>       Slave_SQL_Running_State: Slave has <span style="color: #0000ff;">read</span> <span style="color: #808080;">all</span> relay <span style="color: #ff00ff;">log</span>; waiting <span style="color: #0000ff;">for</span> the slave I<span style="color: #808080;">/</span>O thread <span style="color: #0000ff;">to</span> <span style="color: #0000ff;">update</span><span style="color: #000000;"> it
</span><span style="color: #008080;">48</span>            Master_Retry_Count: <span style="color: #800000; font-weight: bold;">86400</span>
<span style="color: #008080;">49</span> <span style="color: #000000;">                  Master_Bind: 
</span><span style="color: #008080;">50</span> <span style="color: #000000;">      Last_IO_Error_Timestamp: 
</span><span style="color: #008080;">51</span> <span style="color: #000000;">     Last_SQL_Error_Timestamp: 
</span><span style="color: #008080;">52</span> <span style="color: #000000;">               Master_SSL_Crl: 
</span><span style="color: #008080;">53</span> <span style="color: #000000;">           Master_SSL_Crlpath: 
</span><span style="color: #008080;">54</span> <span style="color: #000000;">           Retrieved_Gtid_Set: 
</span><span style="color: #008080;">55</span> <span style="color: #000000;">            Executed_Gtid_Set: 
</span><span style="color: #008080;">56</span>                 Auto_Position: <span style="color: #800000; font-weight: bold;"></span>
<span style="color: #008080;">57</span> <span style="color: #800000; font-weight: bold;">1</span> row <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
    </div>
    
    <p>
      <span class="cnblogs_code_collapse">View Code 从库状态信息</span></div> </div> 
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在主库进行操作，在从库验证
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span># mysql <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123  <span style="color: #808080;">-</span>S <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">3306</span><span style="color: #808080;">/</span><span style="color: #000000;">mysql.sock
mysql</span><span style="color: #808080;">></span><span style="color: #000000;"> show databases;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> <span style="color: #0000ff;">Database</span>           <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> information_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> mysql              <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> performance_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> test               <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #800000; font-weight: bold;">4</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span><span style="color: #000000;"> sec)
mysql</span><span style="color: #808080;">></span> <span style="color: #0000ff;">create</span> <span style="color: #0000ff;">database</span><span style="color: #000000;"> clsn;
Query OK, </span><span style="color: #800000; font-weight: bold;">1</span> row affected (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在从库上可以看到该数据库已创建
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span># mysql <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #808080;">-</span>S <span style="color: #808080;">/</span>data<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">3307</span><span style="color: #808080;">/</span><span style="color: #000000;">mysql.sock
mysql</span><span style="color: #808080;">></span><span style="color: #000000;"> show databases;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> <span style="color: #0000ff;">Database</span>           <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> information_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> clsn               <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> mysql              <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> performance_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> test               <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #800000; font-weight: bold;">5</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
        </div>
        
        <p>
          　　<span style="background-color: #ffff00; color: #ff0000;"><strong><em><span style="text-decoration-line: underline;">至此mysql</span></em></strong><strong><em><span style="text-decoration-line: underline;">主从复制就搭建完成</span></em></strong></span>
        </p>
      </div>
      
      <h3>
        <span id="133">1.3.3 忘记数据库密码？</span>
      </h3>
      
      <div>
        <div class="cnblogs_code">
          <pre>shell<span style="color: #808080;">></span> <span style="color: #808080;">/</span>application<span style="color: #808080;">/</span>mysql<span style="color: #808080;">/</span>bin<span style="color: #808080;">/</span>mysqld_safe  <span style="color: #008080;">--</span><span style="color: #008080;">defaults-file=/data/3306/my.cnf    --skip-grant-tables --skip-networking  &</span>
mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">update</span> <span style="color: #ff00ff;">user</span> <span style="color: #0000ff;">set</span> password<span style="color: #808080;">=</span>password(<span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span>) <span style="color: #0000ff;">where</span> <span style="color: #ff00ff;">user</span><span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">root</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">and</span> host<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">localhost</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
mysql</span><span style="color: #808080;">></span> flush <span style="color: #0000ff;">privileges</span>;</pre>
        </div>
      </div>
      
      <h3>
        <span id="134">1.3.4 主从复制状态失败的原因？</span>
      </h3>
      
      <div>
        <div class="cnblogs_code">
          <pre>Last_IO_Error: error reconnecting <span style="color: #0000ff;">to</span> master <span style="color: #ff0000;">'</span><span style="color: #ff0000;">repl@10.0.0.52:3306</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">-</span> retry<span style="color: #808080;">-</span>time: <span style="color: #800000; font-weight: bold;">60</span>  retries: <span style="color: #800000; font-weight: bold;">1</span></pre>
        </div>
      </div>
      
      <p>
        原因：
      </p>
      
      <div>
        <blockquote>
          <div>
            <p class="a">
              1、主机没启动，或者宕机，检查主库的状态。
            </p>
            
            <p class="a">
              2、网络通信问题，使用ping命令进行检查；或使用mysql命令进行shell端登陆测试
            </p>
            
            <p class="a">
              3、防火墙，selinux（务必检查）。
            </p>
            
            <p class="a">
              4、复制用户和密码、端口号、地址有问题，使用mysql命令进行shell端登陆测试。
            </p>
            
            <p class="a">
              5、mysql自动解析，会将连接的IP解析成主机名（skip-name-resolve = 0）写入my.cnf文件即可。
            </p>
            
            <p class="a">
              6、从库IO异常关闭，通过show slave status \G 进行查看。
            </p>
          </div>
        </blockquote>
      </div>
      
      <h2>
        <span id="14_MySQL">1.4 MySQL主从复制常见问题</span>
      </h2>
      
      <h3>
        <span id="141_binlogbinlog">1.4.1 从库binlog落后主库binlog？</span>
      </h3>
      
      <p>
        　　从库记录的已经主库已经给我传送的binlog事件的坐标，一般在繁忙的生产环境下会落后于主库
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>show master status\G  <span style="color: #008080;">--</span><span style="color: #008080;">- 主</span>
show slave status \G  <span style="color: #008080;">--</span><span style="color: #008080;">- 从</span>
Master_Log_File: mysql<span style="color: #808080;">-</span>bin.<span style="color: #800000; font-weight: bold;">000007</span><span style="color: #000000;">
Read_Master_Log_Pos: </span><span style="color: #800000; font-weight: bold;">729</span></pre>
        </div>
      </div>
      
      <p>
        <strong>落后太远的原因：</strong>
      </p>
      
      <blockquote>
        <p>
          硬件条件有关的，机器磁盘IO性能不足。
        </p>
        
        <p>
          主要还是网络问题，网络传输的性能。
        </p>
        
        <p>
          主库存放二进制日志的存储性能太低，建议binlog日志存咋SSD中。
        </p>
        
        <p>
          主库DUMP线程太繁忙，主要发生在一主多从的环境下。
        </p>
        
        <p>
          从库IO线程太忙
        </p>
        
        <p>
          人为控制（delay节点、延时节点 ）
        </p>
      </blockquote>
      
      <h3>
        <span id="142_update">1.4.2 主库update，从库迟迟的没有更新。</span>
      </h3>
      
      <p>
        <strong><em>特殊情况：</em></strong>日志已经传过来了，数据并没有同步
      </p>
      
      <p>
        <em><span style="text-decoration: underline;"><strong>一般情况：</strong></span></em>
      </p>
      
      <div>
        <blockquote>
          <div>
            <p class="a">
              1、没开启SQL线程
            </p>
            
            <p class="a">
              2、传的东西有问题（你要做的事情，我提前已经做了，不想重复做了，然后他就死了）
            </p>
            
            <p class="a">
              3、SQL线程忙。
            </p>
            
            <p class="a">
              4、人为控制了【delay(从库)节点、延时节点，一般生产设置为3-6小时之间，可以保证过去3-6小时之间的误操作，可以避免】。
            </p>
          </div>
        </blockquote>
      </div>
      
      <h3>
        <span id="143">1.4.3 主从复制延时配置(从库配置)</span>
      </h3>
      
      <p>
        停止从库复制
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span><span style="color: #000000;">stop slave;
Query OK, </span><span style="color: #800000; font-weight: bold;"></span> rows affected (<span style="color: #800000; font-weight: bold;">0.01</span> sec)</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 修改延时参数，MASTER_DELAY，单位位S （秒）。
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span>CHANGE MASTER <span style="color: #0000ff;">TO</span> MASTER_DELAY <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">30</span><span style="color: #000000;">;
Query OK, </span><span style="color: #800000; font-weight: bold;"></span> rows affected (<span style="color: #800000; font-weight: bold;">0.07</span> sec)</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 启动从库复制
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span><span style="color: #000000;">start slave;
Query OK, </span><span style="color: #800000; font-weight: bold;"></span> rows affected (<span style="color: #800000; font-weight: bold;">0.07</span> sec)</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 查看配置是否生效
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span><span style="color: #000000;"> show slave status \G
&hellip;&hellip;
              SQL_Delay: </span><span style="color: #800000; font-weight: bold;">30</span></pre>
        </div>
      </div>
      
      <h3>
        <span id="144">1.4.4 从库安全配置（其他用户只读）</span>
      </h3>
      
      <p>
        修改my.cnf配置文件，添加只读参数
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>read_only <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1</span>    <span style="color: #808080;">====></span><span style="color: #000000;"> 控制普通用户
innodb_read_only </span><span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1</span>  <span style="color: #808080;">====></span> 控制root用户，正常情况不要加</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 添加完成后重启数据库
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span> show variables <span style="color: #808080;">like</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">%read_only%</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-------+</span>
<span style="color: #808080;">|</span> Variable_name    <span style="color: #808080;">|</span> Value <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-------+</span>
<span style="color: #808080;">|</span> innodb_read_only <span style="color: #808080;">|</span> <span style="color: #0000ff;">OFF</span>   <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> read_only        <span style="color: #808080;">|</span> <span style="color: #0000ff;">ON</span>    <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> tx_read_only     <span style="color: #808080;">|</span> <span style="color: #0000ff;">OFF</span>   <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----------------+-------+</span>
<span style="color: #800000; font-weight: bold;">3</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
        </div>
        
        <p>
          　　延时从库: delay节点、延时节点
        </p>
      </div>
      
      <h3>
        <span id="145">1.4.5 主从复制故障及解决（跳过错误）</span>
      </h3>
      
      <p>
        命令行设置
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>stop slave; #<span style="color: #808080;">&lt;==</span><span style="color: #000000;">临时停止同步开关。
</span><span style="color: #0000ff;">set</span> global sql_slave_skip_counter <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1</span> ; #<span style="color: #808080;">&lt;==</span><span style="color: #000000;">将同步指针向下移动一个，如果多次不同步，可以重复操作。
start slave;</span></pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在配置文件修改，设置要跳过的pos
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #808080;">/</span>etc<span style="color: #808080;">/</span><span style="color: #000000;">my.cnf
slave</span><span style="color: #808080;">-</span>skip<span style="color: #808080;">-</span>errors <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1032</span>,<span style="color: #800000; font-weight: bold;">1062</span>,<span style="color: #800000; font-weight: bold;">1007</span></pre>
        </div>
        
        <p>
          　　在mysql中可以跳过某些错误,但是最好的解决办法，重新搭建主从复制。
        </p>
      </div>
      
      <h3>
        <span id="146_--_SQL">1.4.6 延时节点概念 --> SQL线程延时？</span>
      </h3>
      
      <div class="cnblogs_code">
        <pre>Last_SQL_Errno: <span style="color: #800000; font-weight: bold;"></span><span style="color: #000000;">
Last_SQL_Error:</span></pre>
      </div>
      
      <p>
        <strong><em><span style="text-decoration: underline;">原因:</span></em></strong>
      </p>
      
      <blockquote>
        <p>
          1、主库做操作的对象，在从库不存在
        </p>
        
        <p>
          2、主库做操作的对象属性不一致。
        </p>
        
        <p>
          3、主库做操作的对象，从库已经存在
        </p>
        
        <p>
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &hellip;&hellip;
        </p>
      </blockquote>
      
      <h3>
        <span id="147_Slave__Running">1.4.7 Slave_*_Running：？</span>
      </h3>
      
      <blockquote>
        <div>
          <p>
            1、Slave_IO_Running I/O 线程正在运行、未运行还是正在运行但尚未连接到主服务器。可能值分别为Yes、No 或 Connecting。
          </p>
          
          <p>
            2、Slave_SQL_Running SQL 线程当前正在运行、未运行,可能值分别为 Yes、No 主服务器日志坐标：
          </p>
          
          <p>
            3、Master_Log_File 和 Read_Master_Log_Pos 标识主服务器二进制日志中 I/O 线程已经传输的最近事件的坐标。
          </p>
          
          <p>
            4、如果Master_Log_File和Read_Master_Log_Pos 的值远远落后于主服务器上的那些值，这表示主服务器与从属服务器之间事件的网络传输可能存在延迟。
          </p>
        </div>
      </blockquote>
      
      <h3>
        <span id="148">1.4.8 中继日志坐标</span>
      </h3>
      
      <blockquote>
        <p>
          a)&nbsp; Relay_Log_File 和 Relay_Log_Pos 列标识从属服务器中继日志中 SQL 线程已经执行的最近事件的坐标。这些坐标对应于 Relay_Master_Log_File 和 Exec_Master_Log_Pos 列标识的主服务器二进制日志中的坐标。
        </p>
        
        <p>
          b)&nbsp; 如果 Relay_Master_Log_File 和 Exec_Master_Log_Pos 列的输出远远落后于 Master_Log_File 和Read_Master_Log_Pos 列（表示 I/O 线程的坐标），这表示 SQL 线程（而不是 I/O 线程）中存在延迟。即，它表示复制日志事件快于执行这些事件。
        </p>
      </blockquote>
      
      <h3>
        <span id="149">1.4.9 单一主从需要改变的地方</span>
      </h3>
      
      <p>
        <span style="background-color: #ffff00;"><strong>从库的作用</strong></span>
      </p>
      
      <blockquote>
        <p>
          1、相当于实时备份
        </p>
        
        <p>
          2、使用从库备份
        </p>
        
        <p>
          3、一主多从应对读多的业务需求
        </p>
      </blockquote>
      
      <p>
        <strong>&nbsp; &nbsp; &nbsp;</strong> 如果，从库只做备份服务器用，那么主库的压力会不减反增。因为，所有的业务都在主库实现，读和写，dump线程读取并投递binlog
      </p>
      
      <p>
        <span style="background-color: #ffff00;"><strong>解决方案：</strong></span>
      </p>
      
      <blockquote>
        <p>
          （1）可不可以挪走一部分读业务到从库，读写分离
        </p>
        
        <p>
          （2） 一主多从应对读多的业务需求，一旦发展成这个架构，dump线程投递binlog的压力更大
        </p>
        
        <p>
          （3） 多级主从，采用中间库缓解主库dump的压力，会出现中间库瓶颈的问题，选择blackhole引擎，看性能与安全的权衡
        </p>
        
        <p>
          （4）双主模型：缓解，数据一致性难保证
        </p>
        
        <p>
          （5）环装复制
        </p>
      </blockquote>
      
      <h2>
        <span id="15">1.5 【生产案例】主从复制事故</span>
      </h2>
      
      <h3>
        <span id="151">1.5.1 发生背景</span>
      </h3>
      
      <blockquote>
        <p>
          1、有一台已经工作很久的单机mysql数据。在2017年12月24日的平安夜，我司购物网站宕机了。机器物损坏，系统硬盘报废。
        </p>
        
        <p>
          2、在接到一条短信告知<span style="background-color: #ffff00; color: #ff0000;">服务器宕机</span>，数据库连不上。当时的我一脸懵逼的还在开party，谁能想到在这样一个阖家欢乐的时刻发生这样的事情。&nbsp;&nbsp;
        </p>
        
        <p>
          3、随之我火速赶回公司处理事故。首先更换硬盘，从备份服务器上拉取备份数据，用备份恢复宕机的时刻数据，经历40分钟后所有应用恢复正常。
        </p>
        
        <p>
          4、经历这次事故，决心修改数据库架构，我跟领导承诺，保证改完之后，出现类似故障能在5-10分钟恢复业务。把原来的停机时间缩短4-8倍。
        </p>
      </blockquote>
      
      <h3>
        <span id="152">1.5.2 搭建流程</span>
      </h3>
      
      <h4>
        <span id="1521nbsp"><strong>1.5.2.1&nbsp; </strong><strong>架构设计</strong></span>
      </h4>
      
      <p align="center">
        &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230151833742-2105293480.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 修改架构采用数据库主从同步，能保证数据的安全，提高事故发生的恢复速度。
      </p>
      
      <h4>
        <span id="1522nbsp"><strong>1.5.2.2&nbsp; </strong><strong>架构实施</strong></span>
      </h4>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （1）准备一台新机器，配置、系统、环境等与原数据库保持一致。
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （2）在主库检查binlog开关，没有开启将其开启 ，检查server_id 与 auto.cnf文件中的uuid 是否唯一。
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （3）主库创建授权复制的用户，授权 replication slave。
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （4）备份主库上现有数据，恢复到从库中，推荐使用mysqldump，在访问低谷的时候做。
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （5）在从库上开启binlog和relaylog,server_id。
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （6）在从库配置change master to 信息：在第一次开启主从的时候，告诉从库user password host&nbsp; port，复制binlog的起点file、position。
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; （7）start slave 开启主从复制。
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 到此历经千辛万苦主从复制搭建完成。
      </p>
      
      <h3>
        <span id="153">1.5.3 测试主从切换</span>
      </h3>
      
      <blockquote>
        <p>
          （1） 主从的可用性测试：在主库中插入数据，在从库查看有没有。&nbsp;&nbsp;
        </p>
        
        <p>
          （2） 主从快速恢复演练
        </p>
        
        <p>
          　　a)&nbsp; 在一个月黑风高夜选一个业务不繁忙时间点，人工宕掉主库。
        </p>
        
        <p>
          　　b)&nbsp; 将从库定为主库，查看从库的日志量（master.info、relay-log.info）
        </p>
        
        <p>
          　　c)&nbsp; 判断主从日志的差距（master.info,show master status）
        </p>
        
        <p>
          　　d)&nbsp; 恢复后发现偏差，就人为登录到主库（备份服务器也行）中，把截取差距的binlog日志，并传送到从库进行数据补偿。
        </p>
        
        <p>
          　　e)&nbsp; 此时从库数据现在已经和主库一致。
        </p>
        
        <p>
          　　f)&nbsp; reset master，reset slave
        </p>
        
        <p>
          　　g)&nbsp; 应用割接到从库，将应用数据库IP指向从库IP，测试应用。
        </p>
        
        <p>
          （3）小结：经历里这次测试，主从见的切换历时6分32秒，比之前缩短许多，但是感觉还差点什么，以后再补吧。
        </p>
      </blockquote>
      
      <h2>
        <span id="16_mysql">1.6 mysql半同步复制</span>
      </h2>
      
      <p>
        　　MySQL复制默认是异步复制，存在一定的概率备库与主库的数据是不对等的,如果Master宕机，事务在Master上已提交，但很可能这些事务没有传到任何的Slave上，此时Slave也可能会丢失事务。在半同步复制的架构下，当master在将自己binlog发给slave上的时候，要确保slave已经接受到了这个二进制日志以后，才会返回数据给客户端。
      </p>
      
      <p>
        　　半同步复制介于异步复制和全同步复制之间，主库在执行完客户端提交的事务后不是立刻返回给客户端，而是等待至少一个从库接收到并写到relay log中才返回给客户端。相对于异步复制，半同步复制提高了数据的安全性，同时它也造成了一定程度的延迟，这个延迟最少是一个TCP/IP往返的时间。所以，半同步复制最好在低延时的网络中使用。
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230151931851-594607212.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p align="center">
        半同步复制的原理图
      </p>
      
      <p>
        　　在5.6中加入了group commit特性之后，性能不比传统的异步复制差。
      </p>
      
      <h3>
        <span id="161">1.6.1 半同步复制的潜在问题</span>
      </h3>
      
      <p>
        　　客户端事务在存储引擎层提交后，在得到从库确认的过程中，主库宕机了，此时，可能的情况有两种
      </p>
      
      <p>
        <strong>事务还没发送到从库上</strong>
      </p>
      
      <p>
        　　此时，客户端会收到事务提交失败的信息，客户端会重新提交该事务到新的主上，当宕机的主库重新启动后，以从库的身份重新加入到该主从结构中，会发现，该事务在从库中被提交了两次，一次是之前作为主的时候，一次是被新主同步过来的。
      </p>
      
      <p>
        <strong>事务已经发送到从库上</strong>
      </p>
      
      <p>
        　　此时，从库已经收到并应用了该事务，但是客户端仍然会收到事务提交失败的信息，重新提交该事务到新的主上。
      </p>
      
      <h3>
        <span id="162">1.6.2 半同步架构搭建</span>
      </h3>
      
      <p>
        <span style="background-color: #ffff00;"><strong>加载使用的插件</strong></span>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #000000;">主库:
INSTALL PLUGIN rpl_semi_sync_master SONAME </span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">semisync_master.so</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
从从:
INSTALL PLUGIN rpl_semi_sync_slave SONAME </span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">semisync_slave.so</span><span style="color: #ff0000;">'</span>;</pre>
        </div>
      </div>
      
      <p>
        <strong>查看是否加载成功</strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>show plugins;</pre>
        </div>
      </div>
      
      <p>
        <strong>启动半同步复制</strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #000000;">主库:
</span><span style="color: #0000ff;">SET</span> GLOBAL rpl_semi_sync_master_enabled <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">;
从库:
</span><span style="color: #0000ff;">SET</span> GLOBAL rpl_semi_sync_slave_enabled <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">1</span>;</pre>
        </div>
      </div>
      
      <p>
        <strong>重启从库上的IO</strong><strong>线程</strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #000000;">STOP SLAVE IO_THREAD;
START SLAVE IO_THREAD;</span></pre>
        </div>
      </div>
      
      <p>
        <strong>查看是否在运行</strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #000000;">主库:
show status </span><span style="color: #808080;">like</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Rpl_semi_sync_master_status</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
从库:
show status </span><span style="color: #808080;">like</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Rpl_semi_sync_slave_status</span><span style="color: #ff0000;">'</span>;</pre>
        </div>
      </div>
      
      <p>
        <strong>测试半同步复制</strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #000000;">查看延迟时间:
show variables </span><span style="color: #808080;">like</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">%rpl_semi_sync%</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
从库:
stop slave;
主库:
</span><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">database</span> clsn;</pre>
        </div>
        
        <p>
          　　如果创建库的时间是设置的时间就成功了。
        </p>
      </div>
      
      <h2>
        <span id="17">1.7 主从复制架构的演变</span>
      </h2>
      
      <h3>
        <span id="171">1.7.1 基本结构</span>
      </h3>
      
      <p>
        （1）一主一从
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152117226-2068861123.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        （2）一主多从
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152145445-1265499500.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />&nbsp;
      </p>
      
      <p>
        （3）多级主从
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152203710-42573272.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        （4）双主
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152220898-423986887.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        （5）多主一从（5.7之后开始支持）
      </p>
      
      <p align="center">
        &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152232179-1765608793.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        （6）循环复制
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 对于循环数据库镜像，就是多个数据库A、B、C、D等，对其中任一个数据库的修改，都要同时镜像到其它的数据库里。&nbsp;<span class="cnblogs_code"> <span style="color: #ff00ff;">replicate</span><span style="color: #808080;">-</span>same<span style="color: #808080;">-</span>server<span style="color: #808080;">-</span>id <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;"></span></span>&nbsp;
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152241507-635336480.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />&nbsp;
      </p>
      
      <h3>
        <span id="172">1.7.2 高级应用架构演变</span>
      </h3>
      
      <p>
        <span style="background-color: #ffff00;"><strong>（1</strong><strong>）读写分离&mdash;&mdash;MySQL proxy</strong><strong>、amoeba</strong><strong>、xx-dbproxy</strong><strong>等。</strong></span>
      </p>
      
      <p align="center">
        &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152306820-59568261.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        　　一般来说都是通过主从复制（Master-Slave）的方式来同步数据，再通过读写分离（MySQL-Proxy）来提升数据库的并发负载能力这样的方案来进行部署与实施的。　　
      </p>
      
      <p>
        <span style="background-color: #ffff00;"><strong>（2</strong><strong>）分库分表&mdash;&mdash;cobar</strong><strong>、自主研发等。</strong></span>
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152317273-92340670.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />&nbsp;
      </p>
      
      <p>
        　　1) 系统对外提供的数据库名是dbtest,并且其中有两张表tb1和tb2。
      </p>
      
      <p>
        　　2) tb1表的数据被映射到物理数据库dbtest1的tb1上。
      </p>
      
      <p>
        　　3) tb2表的一部分数据被映射到物理数据库dbtest2的tb2上，另外一部分数据被映射到物理数据库dbtest3的tb2上。
      </p>
      
      <p>
        <span style="background-color: #ffff00;"><strong>（3</strong><strong>）MMM</strong><strong>架构&mdash;&mdash;mysql-mmm</strong><strong>（google</strong><strong>）-</strong><strong>使用极少</strong></span>
      </p>
      
      <p align="center">
        &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152332335-1783684620.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        　　MMM（Master-Master replication manager for MySQL）是一套支持双主故障切换和双主日常管理的脚本程序。MMM使用Perl语言开发，主要用来监控和管理MySQL Master-Master（双主）复制，可以说是mysql主主复制管理器。
      </p>
      
      <p>
        　　虽然叫做双主复制，但是业务上同一时刻只允许对一个主进行写入，另一台备选主上提供部分读服务，以加速在主主切换时刻备选主的预热，可以说MMM这套脚本程序一方面实现了故障切换的功能，另一方面其内部附加的工具脚本也可以实现多个slave的read负载均衡。
      </p>
      
      <p>
        　　关于mysql主主复制配置的监控、故障转移和管理的一套可伸缩的脚本套件（在任何时候只有一个节点可以被写入），这个套件也能对居于标准的主从配置的任意数量的从服务器进行读负载均衡，所以你可以用它来在一组居于复制的服务器启动虚拟ip，除此之外，它还有实现数据备份、节点之间重新同步功能的脚本。
      </p>
      
      <p>
        　　对于那些对数据的一致性要求很高的业务，非常不建议采用MMM这种高可用架构。
      </p>
      
      <p>
        <span style="background-color: #ffff00;"><strong>（4</strong><strong>）MHA</strong><strong>架构&mdash;&mdash;mysql-master-ha</strong><strong>（日本DeNa</strong><strong>）</strong></span>
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152405476-1810433091.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />&nbsp;
      </p>
      
      <p>
        　　MHA（Master High Availability）目前在MySQL高可用方面是一个相对成熟的解决方案，它由日本DeNA公司youshimaton（现就职于Facebook公司）开发，是一套优秀的作为MySQL高可用性环境下故障切换和主从提升的高可用软件。在MySQL故障切换过程中，MHA能做到在0~30秒之内自动完成数据库的故障切换操作，并且在进行故障切换的过程中，MHA能在最大程度上保证数据的一致性，以达到真正意义上的高可用。
      </p>
      
      <p>
        　　该软件由两部分组成：MHA Manager（管理节点）和MHA Node（数据节点）。
      </p>
      
      <p>
        <span style="background-color: #ffff00;"><strong>（5</strong><strong>）MGR-- 5.7&nbsp;&nbsp;&nbsp;&nbsp; </strong><strong>新特性&nbsp;&nbsp;&nbsp; MySQL&nbsp;&nbsp;&nbsp; Group&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; replication</strong></span>
      </p>
      
      <p align="center">
        &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152430351-638052279.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p>
        　　基于传统异步复制和半同步复制的缺陷&mdash;&mdash;数据的一致性问题无法保证，MySQL官方在5.7.17版本正式推出组复制（MySQL Group Replication，简称MGR）。
      </p>
      
      <p>
        　　由若干个节点共同组成一个复制组，一个事务的提交，必须经过组内大多数节点（N / 2 + 1）决议并通过，才能得以提交。如上图所示，由3个节点组成一个复制组，Consensus层为一致性协议层，在事务提交过程中，发生组间通讯，由2个节点决议(certify)通过这个事务，事务才能够最终得以提交并响应。
      </p>
      
      <p>
        <span style="background-color: #ffff00;">（6）PXC、MySQL Cluster、galera cluster架构</span>
      </p>
      
      <p align="center">
        &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171230152443757-539991414.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL Replication 主从复制全方位解决方案" alt="" />
      </p>
      
      <p align="center">
        在PXC中，一次数据写入在各个节点间的验证/回滚流程
      </p>
      
      <p>
        <strong><em><span style="text-decoration: underline;">PXC</span></em></strong><strong><em><span style="text-decoration: underline;">架构的优点：</span></em></strong>
      </p>
      
      <blockquote>
        <p>
          a)&nbsp; 服务高可用；
        </p>
        
        <p>
          b)&nbsp; 数据同步复制(并发复制)，几乎无延迟；
        </p>
        
        <p>
          c)&nbsp; 多个可同时读写节点，可实现写扩展，不过最好事先进行分库分表，让各个节点分别写不同的表或者库，避免让galera解决数据冲突；
        </p>
        
        <p>
          d)&nbsp; 新节点可以自动部署，部署操作简单；
        </p>
        
        <p>
          e)&nbsp; 数据严格一致性，尤其适合电商类应用；
        </p>
        
        <p>
          f)&nbsp; 完全兼容MySQL；
        </p>
      </blockquote>
      
      <h3>
        <span id="173">1.7.3 分库分表简单实践</span>
      </h3>
      
      <p>
        　　实践中使用的为world数据库，为mysql官方提供，详情参照：<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/clsn/p/8087417.html#_label0"  target="_blank">http://www.cnblogs.com/clsn/p/8087417.html#_label0</a>
      </p>
      
      <p>
        <strong>第一个里程碑:创建新表</strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TABLE</span><span style="color: #000000;"> `country_1` (
  `Code` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">3</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span> <span style="color: #0000ff;">DEFAULT</span> <span style="color: #ff0000;">''</span><span style="color: #000000;">,
  `Name` </span><span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">52</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span> <span style="color: #0000ff;">DEFAULT</span> <span style="color: #ff0000;">''</span><span style="color: #000000;">,
  `Continent` enum(</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">Asia</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">Europe</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">North America</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">Africa</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">Oceania</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">Antarctica</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">South America</span><span style="color: #ff0000;">'</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span> <span style="color: #0000ff;">DEFAULT</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Asia</span><span style="color: #ff0000;">'</span><span style="color: #000000;">,
  </span><span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;"> (`Code`),
  </span><span style="color: #0000ff;">KEY</span><span style="color: #000000;"> `name_idx` (`Name`)
) ENGINE</span><span style="color: #808080;">=</span>InnoDB <span style="color: #0000ff;">DEFAULT</span> CHARSET<span style="color: #808080;">=</span>latin1</pre>
        </div>
      </div>
      
      <p>
        <strong>第二个里程碑：从旧表导入数据到新表</strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> country_1(code,name,continent) <span style="color: #0000ff;">select</span> code,name,continent <span style="color: #0000ff;">from</span> country;</pre>
        </div>
      </div>
      
      <p>
        <strong>第三个里程碑：横向拆表</strong>
      </p>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 创建与导出表相同格式的新表1
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> country_1_p1 <span style="color: #808080;">like</span> country_1;</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 将数据的前100行插入到新表1中
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> country_1_p1 <span style="color: #0000ff;">select</span> code,name,continent <span style="color: #0000ff;">from</span> country_1 <span style="color: #0000ff;">order</span> <span style="color: #0000ff;">by</span> code limit <span style="color: #800000; font-weight: bold;">100</span>;</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 创建与新表1相同格式的新表2
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">create</span> <span style="color: #0000ff;">table</span> country_1_p2 <span style="color: #808080;">like</span> country_1;</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 将100行之后的139行导入表2.
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">insert</span> <span style="color: #0000ff;">into</span> country_1_p2 <span style="color: #0000ff;">select</span> code,name,continent <span style="color: #0000ff;">from</span> country_1 <span style="color: #0000ff;">order</span> <span style="color: #0000ff;">by</span> code limit <span style="color: #800000; font-weight: bold;">139</span> offset <span style="color: #800000; font-weight: bold;">100</span>;</pre>
        </div>
      </div>
      
      <h2>
        <span id="18">1.8 参考文献</span>
      </h2>
      
      <div>
        <blockquote>
          <div>
            <p class="a">
              [1]&nbsp; http://blog.csdn.net/hguisu/article/details/7325124
            </p>
            
            <p class="a">
              [2]&nbsp; https://www.cnblogs.com/ivictor/p/5735580.html
            </p>
            
            <p class="a">
              [3]&nbsp; https://www.cnblogs.com/Aiapple/p/5792939.html
            </p>
            
            <p class="a">
              [4]&nbsp; http://heylinux.com/archives/1004.html 读写分离
            </p>
            
            <p class="a">
              [5]&nbsp; http://hualong.iteye.com/blog/2102798 Cobar逻辑层次图
            </p>
            
            <p class="a">
              [6]&nbsp; https://www.cnblogs.com/kevingrace/p/5662975.html MMM架构
            </p>
            
            <p class="a">
              [7]&nbsp; https://www.cnblogs.com/gomysql/p/3675429.html&nbsp; MHA架构
            </p>
            
            <p class="a">
              [8]&nbsp; http://blog.jobbole.com/70844/ &nbsp;&nbsp;MySQL在大型网站的应用架构演变
            </p>
            
            <p class="a">
              [9]&nbsp; https://www.cnblogs.com/dosmile/p/6681923.html&nbsp; MGR复制
            </p>
            
            <p class="a">
              [10] http://imysql.cn/tag/pxc &nbsp;&nbsp; PXC架构
            </p>
            
            <p class="a">
              [11] https://mp.weixin.qq.com/s/Ll0sdKS6Vbw1pXIKbW3-NQ&nbsp;
            </p>
          </div>
        </blockquote>
      </div>
      
      <div id="toc_container" class="toc_white have_bullets">
        <ul class="toc_list">
          <li>
            <a href="#11">1.1 主从复制基础概念</a><ul>
              <li>
                <a href="#111">1.1.1 二进制日志管理说明</a>
              </li>
              <li>
                <a href="#112_mysql">1.1.2 mysql传统备份方式和缺陷</a>
              </li>
              <li>
                <a href="#113_MySQL">1.1.3 MySQL主从复制能为我们做什么</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#12_MySQL">1.2 MySQL主从复制介绍</a><ul>
              <li>
                <a href="#121">1.2.1 复制技术</a>
              </li>
              <li>
                <a href="#122">1.2.2 复制架构</a><ul>
                  <li>
                    <a href="#1221nbsp_ndash">1.2.2.1&nbsp; 主&ndash;从复制</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#123_MySQL">1.2.3 MySQL主从复制原理介绍</a>
              </li>
              <li>
                <a href="#124">1.2.4 执行原理--第一次开启主从过程</a><ul>
                  <li>
                    <a href="#1241nbsp_mysql_replication_nbspnbsp">1.2.4.1&nbsp; 详细的mysql replication 过程&nbsp;&nbsp; </a>
                  </li>
                </ul>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#13">1.3 主从搭建配置</a><ul>
              <li>
                <a href="#131_slave">1.3.1 多实例数据库slave配置</a>
              </li>
              <li>
                <a href="#132">1.3.2 测试主从同步</a>
              </li>
              <li>
                <a href="#133">1.3.3 忘记数据库密码？</a>
              </li>
              <li>
                <a href="#134">1.3.4 主从复制状态失败的原因？</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#14_MySQL">1.4 MySQL主从复制常见问题</a><ul>
              <li>
                <a href="#141_binlogbinlog">1.4.1 从库binlog落后主库binlog？</a>
              </li>
              <li>
                <a href="#142_update">1.4.2 主库update，从库迟迟的没有更新。</a>
              </li>
              <li>
                <a href="#143">1.4.3 主从复制延时配置(从库配置)</a>
              </li>
              <li>
                <a href="#144">1.4.4 从库安全配置（其他用户只读）</a>
              </li>
              <li>
                <a href="#145">1.4.5 主从复制故障及解决（跳过错误）</a>
              </li>
              <li>
                <a href="#146_--_SQL">1.4.6 延时节点概念 --> SQL线程延时？</a>
              </li>
              <li>
                <a href="#147_Slave__Running">1.4.7 Slave_*_Running：？</a>
              </li>
              <li>
                <a href="#148">1.4.8 中继日志坐标</a>
              </li>
              <li>
                <a href="#149">1.4.9 单一主从需要改变的地方</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#15">1.5 【生产案例】主从复制事故</a><ul>
              <li>
                <a href="#151">1.5.1 发生背景</a>
              </li>
              <li>
                <a href="#152">1.5.2 搭建流程</a><ul>
                  <li>
                    <a href="#1521nbsp">1.5.2.1&nbsp; 架构设计</a>
                  </li>
                  <li>
                    <a href="#1522nbsp">1.5.2.2&nbsp; 架构实施</a>
                  </li>
                </ul>
              </li>
              
              <li>
                <a href="#153">1.5.3 测试主从切换</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#16_mysql">1.6 mysql半同步复制</a><ul>
              <li>
                <a href="#161">1.6.1 半同步复制的潜在问题</a>
              </li>
              <li>
                <a href="#162">1.6.2 半同步架构搭建</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#17">1.7 主从复制架构的演变</a><ul>
              <li>
                <a href="#171">1.7.1 基本结构</a>
              </li>
              <li>
                <a href="#172">1.7.2 高级应用架构演变</a>
              </li>
              <li>
                <a href="#173">1.7.3 分库分表简单实践</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#18">1.8 参考文献</a>
          </li>
        </ul>
      </div>