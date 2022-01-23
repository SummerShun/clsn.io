---
title: Hadoop+HBase 集群搭建
author: 惨绿少年
type: post
date: 2019-01-21T11:13:51+00:00
url: /clsn/lx1490.html
Baidusubmit:
  - 1
categories:
  - 云计算
  - 存储
  - 数据库
  - 玩转Linux
  - 运维基本功

---
<div id="log-box">
  <div id="catalog">
    <ul id="catalog-ul">
      <li>
        <i class="be be-arrowright"></i> <a href="#title-0" title="6.2.1 hbase-env.sh">6.2.1 hbase-env.sh</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-1" title="6.2.3 regionservers">6.2.3 regionservers</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-2" title="6.2.4 分发配置到其他节点">6.2.4 分发配置到其他节点</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-3" title="6.3.1 启动hbase">6.3.1 启动hbase</a>
      </li>
      <li>
        <i class="be be-arrowright"></i> <a href="#title-4" title="6.3.2 启动hbase 客户端">6.3.2 启动hbase 客户端</a>
      </li>
    </ul>
    
    <span class="log-zd"><span class="log-close"><a title="隐藏目录"><i class="be be-cross"></i><strong>目录</strong></a></span></span>
  </div>
</div>

# <span id="HadoopHBase">Hadoop+HBase 集群搭建</span>

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1545622365883-0b7d8d3b-e38b-4ecc-b1df-9e69a8a279bf.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Hadoop+HBase 集群搭建" alt="图片.png | center | 400x98.66666666666667" title="" /> 

## <span id="1">1. 环境准备</span>

说明：本次集群搭建使用系统版本Centos 7.5 ，软件版本 V3.1.1。

### <span id="11">1.1 配置说明</span>

本次集群搭建共三台机器，具体说明下:

<div class="bi-table">
  <table>
    <colgroup> <col width="111px" /> <col width="115px" /> <col width="523px" /> </colgroup> <tr height="34px">
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            主机名
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            IP
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            说明
          </div></p>
        </div>
      </td>
    </tr>
    
    <tr height="34px">
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            hadoop01
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            10.0.0.10
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="p">
          DataNode、NodeManager、NameNode
        </div>
      </td>
    </tr>
    
    <tr height="34px">
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            hadoop02
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            10.0.0.11
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="p">
          DataNode、NodeManager、ResourceManager、SecondaryNameNode
        </div>
      </td>
    </tr>
    
    <tr height="34px">
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            hadoop03
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="alignment" data-value="center" style="text-align:center">
          <div data-type="p">
            10.0.0.12
          </div></p>
        </div>
      </td>
      
      <td rowspan="1" colSpan="1">
        <div data-type="p">
          DataNode、NodeManager
        </div>
      </td>
    </tr>
  </table>
</div>

### <span id="12">1.2 机器配置说明</span>

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /home/clsn]
$cat  /etc/redhat-release
CentOS Linux release 7.5.1804 (Core)

[clsn@hadoop01 /home/clsn]
$uname  -r
3.10.0-862.el7.x86_64

[clsn@hadoop01 /home/clsn]
$sestatus
SELinux status:                 disabled

[clsn@hadoop01 /home/clsn]
$systemctl  status  firewalld.service
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)

[clsn@hadoop01 /home/clsn]
$id clsn
uid=1000(clsn) gid=1000(clsn) 组=1000(clsn)

[clsn@hadoop01 /home/clsn]
$cat  /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.0.0.10   hadoop01
10.0.0.11   hadoop02
10.0.0.12   hadoop03
</code></pre>

注：本集群内所有进程均由clsn用户启动

### <span id="13_ssh">1.3 ssh互信配置</span>

<pre><code class="language-bash line-numbers">ssh-keygen
ssh-copy-id -i ~/.ssh/id_rsa.pub  127.0.0.1
scp -rp ~/.ssh hadoop02:/home/clsn
scp -rp ~/.ssh hadoop03:/home/clsn
</code></pre>

### <span id="14_jdk">1.4 配置jdk</span>

在三台机器上都需要操作

<pre><code class="language-bash line-numbers">tar xf jdk-8u191-linux-x64.tar.gz -C  /usr/local/
ln -s /usr/local/jdk1.8.0_191 /usr/local/jdk
sed -i.ori '$a export JAVA_HOME=/usr/local/jdk\nexport PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH\nexport CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar' /etc/profile
. /etc/profile
</code></pre>

## <span id="2_hadoop">2. 安装hadoop</span>

### <span id="21_Binary">2.1 安装包下载（Binary）</span>

<pre><code class="language-bash line-numbers">wget http://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.1.1/hadoop-3.1.1.tar.gz
</code></pre>

### <span id="22">2.2 安装</span>

<pre><code class="language-bash line-numbers">tar xf hadoop-3.1.1.tar.gz -C /usr/local/
ln -s /usr/local/hadoop-3.1.1  /usr/local/hadoop
sudo  chown  -R clsn.clsn /usr/local/hadoop-3.1.1/
</code></pre>

## <span id="3hadoop">3.修改hadoop配置</span>

配置文件全部位于 /usr/local/hadoop/etc/hadoop 文件夹下

### <span id="31_hadoop-envsh">3.1 hadoop-env.sh</span>

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hadoop/etc/hadoop]
$ head hadoop-env.sh
.  /etc/profile
#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
</code></pre>

### <span id="32_core-sitexml">3.2 core-site.xml</span>

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hadoop/etc/hadoop]
$ cat core-site.xml
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="configuration.xsl"?&gt;

&lt;!-- Put site-specific property overrides in this file. --&gt;

&lt;configuration&gt;
    &lt;!-- 指定HDFS老大（namenode）的通信地址 --&gt;
    &lt;property&gt;
        &lt;name&gt;fs.defaultFS&lt;/name&gt;
        &lt;value&gt;hdfs://hadoop01:9000&lt;/value&gt;
    &lt;/property&gt;
    &lt;!-- 指定hadoop运行时产生文件的存储路径 --&gt;
    &lt;property&gt;
        &lt;name&gt;hadoop.tmp.dir&lt;/name&gt;
        &lt;value&gt;/data/tmp&lt;/value&gt;
    &lt;/property&gt;
&lt;/configuration&gt;

</code></pre>

### <span id="33_hdfs-sitexml">3.3 hdfs-site.xml</span>

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hadoop/etc/hadoop]
$ cat hdfs-site.xml
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="configuration.xsl"?&gt;

&lt;!-- Put site-specific property overrides in this file. --&gt;

&lt;configuration&gt;
    &lt;!-- 设置namenode的http通讯地址 --&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.namenode.http-address&lt;/name&gt;
        &lt;value&gt;hadoop01:50070&lt;/value&gt;
    &lt;/property&gt;

    &lt;!-- 设置secondarynamenode的http通讯地址 --&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.namenode.secondary.http-address&lt;/name&gt;
        &lt;value&gt;hadoop02:50090&lt;/value&gt;
    &lt;/property&gt;

    &lt;!-- 设置namenode存放的路径 --&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.namenode.name.dir&lt;/name&gt;
        &lt;value&gt;/data/name&lt;/value&gt;
    &lt;/property&gt;

    &lt;!-- 设置hdfs副本数量 --&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.replication&lt;/name&gt;
        &lt;value&gt;2&lt;/value&gt;
    &lt;/property&gt;
    &lt;!-- 设置datanode存放的路径 --&gt;
    &lt;property&gt;
        &lt;name&gt;dfs.datanode.data.dir&lt;/name&gt;
        &lt;value&gt;/data/datanode&lt;/value&gt;
    &lt;/property&gt;

    &lt;property&gt;
        &lt;name&gt;dfs.permissions&lt;/name&gt;
        &lt;value&gt;false&lt;/value&gt;
    &lt;/property&gt;

&lt;/configuration&gt;
</code></pre>

### <span id="34_mapred-sitexml">3.4 mapred-site.xml</span>

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hadoop/etc/hadoop]
$ cat mapred-site.xml
&lt;?xml version="1.0"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="configuration.xsl"?&gt;
&lt;!-- Put site-specific property overrides in this file. --&gt;

&lt;configuration&gt;
    &lt;!-- 通知框架MR使用YARN --&gt;
    &lt;property&gt;
        &lt;name&gt;mapreduce.framework.name&lt;/name&gt;
        &lt;value&gt;yarn&lt;/value&gt;
    &lt;/property&gt;
    &lt;property&gt;
        &lt;name&gt;mapreduce.application.classpath&lt;/name&gt;
        &lt;value&gt;
        /usr/local/hadoop/etc/hadoop,
        /usr/local/hadoop/share/hadoop/common/*,
        /usr/local/hadoop/share/hadoop/common/lib/*,
        /usr/local/hadoop/share/hadoop/hdfs/*,
        /usr/local/hadoop/share/hadoop/hdfs/lib/*,
        /usr/local/hadoop/share/hadoop/mapreduce/*,
        /usr/local/hadoop/share/hadoop/mapreduce/lib/*,
        /usr/local/hadoop/share/hadoop/yarn/*,
        /usr/local/hadoop/share/hadoop/yarn/lib/*
        &lt;/value&gt;
    &lt;/property&gt;

&lt;/configuration&gt;
</code></pre>

### <span id="35_yarn-sitexml">3.5 yarn-site.xml</span>

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hadoop/etc/hadoop]
$ cat yarn-site.xml
&lt;?xml version="1.0"?&gt;

&lt;configuration&gt;
    &lt;property&gt;
        &lt;name&gt;yarn.resourcemanager.hostname&lt;/name&gt;
        &lt;value&gt;hadoop02&lt;/value&gt;
    &lt;/property&gt;

    &lt;property&gt;
        &lt;description&gt;The http address of the RM web application.&lt;/description&gt;
        &lt;name&gt;yarn.resourcemanager.webapp.address&lt;/name&gt;
        &lt;value&gt;${yarn.resourcemanager.hostname}:8088&lt;/value&gt;
    &lt;/property&gt;

    &lt;property&gt;
        &lt;description&gt;The address of the applications manager interface in the RM.&lt;/description&gt;
        &lt;name&gt;yarn.resourcemanager.address&lt;/name&gt;
        &lt;value&gt;${yarn.resourcemanager.hostname}:8032&lt;/value&gt;
    &lt;/property&gt;

    &lt;property&gt;
        &lt;description&gt;The address of the scheduler interface.&lt;/description&gt;
        &lt;name&gt;yarn.resourcemanager.scheduler.address&lt;/name&gt;
        &lt;value&gt;${yarn.resourcemanager.hostname}:8030&lt;/value&gt;
    &lt;/property&gt;

    &lt;property&gt;
        &lt;name&gt;yarn.resourcemanager.resource-tracker.address&lt;/name&gt;
        &lt;value&gt;${yarn.resourcemanager.hostname}:8031&lt;/value&gt;
    &lt;/property&gt;

    &lt;property&gt;
        &lt;description&gt;The address of the RM admin interface.&lt;/description&gt;
        &lt;name&gt;yarn.resourcemanager.admin.address&lt;/name&gt;
        &lt;value&gt;${yarn.resourcemanager.hostname}:8033&lt;/value&gt;
    &lt;/property&gt;

&lt;/configuration&gt;
</code></pre>

### <span id="36_masters_slaves">3.6 masters & slaves</span>

<pre><code class="language-plain line-numbers">echo 'hadoop02' &gt;&gt; /usr/local/hadoop/etc/hadoop/masters
echo 'hadoop03
hadoop01'  &gt;&gt; /usr/local/hadoop/etc/hadoop/slaves

</code></pre>

### <span id="37">3.7 启动脚本修改</span>

启动脚本文件全部位于 /usr/local/hadoop/sbin 文件夹下：  
（1）修改 start-dfs.sh stop-dfs.sh 文件添加：

<pre><code class="language-bash line-numbers">HDFS_DATANODE_USER=clsn
HADOOP_SECURE_DN_USER=hdfs
HDFS_NAMENODE_USER=clsn
HDFS_SECONDARYNAMENODE_USER=clsn
</code></pre>

（2）修改start-yarn.sh 和 stop-yarn.sh文件添加：

<pre><code class="language-bash line-numbers">YARN_RESOURCEMANAGER_USER=clsn
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=clsn
</code></pre>

## <span id="4">4. 启动前准备</span>

### <span id="41">4.1 创建文件目录</span>

<pre><code class="language-bash line-numbers">mkdir -p /data/tmp
mkdir -p /data/name
mkdir -p /data/datanode
chown -R clsn.clsn /data 
</code></pre>

在集群内所有机器上都进行创建，也可以复制文件夹

<pre><code class="language-bash line-numbers">for i in hadoop02 hadoop03
    do 
        sudo scp -rp /data $i:/
done 
</code></pre>

### <span id="42_hadoop">4.2 复制hadoop配置到其他机器</span>

<pre><code class="language-bash line-numbers">for i in hadoop02 hadoop03
    do 
        sudo scp -rp  /usr/local/hadoop-3.1.1 $i:/usr/local/
done 
</code></pre>

### <span id="43_hadoop">4.3 启动hadoop集群</span>

(1)第一次启动前需要格式化

<pre><code class="language-bash line-numbers">/usr/local/hadoop/bin/hdfs namenode -format 
</code></pre>

(2)启动集群

<pre><code class="language-plain line-numbers">cd /usr/local/hadoop/sbin 
./start-all.sh
</code></pre>

## <span id="5">5.集群启动成功</span>

（1）使用jps查看集群中各个角色，是否与预期相一致

<pre><code class="language-python line-numbers">[clsn@hadoop01 /home/clsn]
$ pssh  -ih  cluster  "`which jps`"
[1] 11:30:31 [SUCCESS] hadoop03
7947 DataNode
8875 Jps
8383 NodeManager
[2] 11:30:31 [SUCCESS] hadoop01
20193 DataNode
20665 NodeManager
21017 NameNode
22206 Jps
[3] 11:30:31 [SUCCESS] hadoop02
8896 DataNode
9427 NodeManager
10883 Jps
9304 ResourceManager
10367 SecondaryNameNode

</code></pre>

（2）浏览器访问[http://hadoop02:8088/cluster/nodes][1]  
该页面为ResourceManager 管理界面，在上面可以看到集群中的三台Active Nodes。

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1545622408816-c6e4a343-1d60-47dc-bfee-e2e71a5c38e0.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Hadoop+HBase 集群搭建" alt="图片.png | center | 747x378" title="" />  
(3) 浏览器访问[http://hadoop01:50070/dfshealth.html#tab-datanode][2]  
该页面为NameNode管理页面

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1545622631989-e939563d-1b0a-4219-a7a8-79bf62489b8c.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Hadoop+HBase 集群搭建" alt="图片.png | center | 747x438" title="" /> 

## <span id="6Hbase">6.Hbase配置</span>

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1545710952464-634a68e8-15b1-460c-aacc-47bdf0927641.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Hadoop+HBase 集群搭建" alt="image.png | center | 300x73.88059701492537" title="" /> 

### <span id="61_Hbase">6.1 部署Hbase包</span>

<pre><code class="language-bash line-numbers">cd /opt/
wget  http://mirrors.tuna.tsinghua.edu.cn/apache/hbase/1.4.9/hbase-1.4.9-bin.tar.gz
tar xf  hbase-1.4.9-bin.tar.gz -C  /usr/local/
ln -s /usr/local/hbase-1.4.9 /usr/local/hbase
</code></pre>

### <span id="62">6.2 修改配置文件</span>

<span class="directory"></span>

#### <span id="621_hbase-envsh">6.2.1 hbase-env.sh</span> {#title-0}

<pre><code class="language-bash line-numbers"># 添加一行
. /etc/profile
</code></pre>

6.2.2

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hbase/conf]
$ cat  hbase-site.xml
&lt;?xml version="1.0"?&gt;
&lt;?xml-stylesheet type="text/xsl" href="configuration.xsl"?&gt;

&lt;configuration&gt;
&lt;property&gt;
    &lt;name&gt;hbase.rootdir&lt;/name&gt;
    &lt;!-- hbase存放数据目录 --&gt;
    &lt;value&gt;hdfs://hadoop01:9000/hbase/hbase_db&lt;/value&gt;
    &lt;!-- 端口要和Hadoop的fs.defaultFS端口一致--&gt;
&lt;/property&gt;
&lt;property&gt;
    &lt;name&gt;hbase.cluster.distributed&lt;/name&gt; 
    &lt;!-- 是否分布式部署 --&gt;
    &lt;value&gt;true&lt;/value&gt;
&lt;/property&gt;
&lt;property&gt;
    &lt;name&gt;hbase.zookeeper.quorum&lt;/name&gt;
    &lt;!-- zookooper 服务启动的节点，只能为奇数个 --&gt;
    &lt;value&gt;hadoop01,hadoop02,hadoop03&lt;/value&gt;
&lt;/property&gt;
&lt;property&gt;
    &lt;!--zookooper配置、日志等的存储位置，必须为以存在 --&gt;
    &lt;name&gt;hbase.zookeeper.property.dataDir&lt;/name&gt;
    &lt;value&gt;/data/hbase/zookeeper&lt;/value&gt;
&lt;/property&gt;
&lt;property&gt;
    &lt;!--hbase web 端口 --&gt;
    &lt;name&gt;hbase.master.info.port&lt;/name&gt;
    &lt;value&gt;16610&lt;/value&gt;
&lt;/property&gt;
&lt;/configuration&gt;
</code></pre>

注意:

> zookeeper有这样一个特性：  
> 集群中只要有过半的机器是正常工作的，那么整个集群对外就是可用的。  
> 也就是说如果有2个zookeeper，那么只要有1个死了zookeeper就不能用了，因为1没有过半，所以2个zookeeper的死亡容忍度为0；  
> 同理，要是有3个zookeeper，一个死了，还剩下2个正常的，过半了，所以3个zookeeper的容忍度为1；  
> 再多列举几个：2->0 ; 3->1 ; 4->1 ; 5->2 ; 6->2 会发现一个规律，2n和2n-1的容忍度是一样的，都是n-1，所以为了更加高效，何必增加那一个不必要的zookeeper 

<span class="directory"></span>

#### <span id="623_regionservers">6.2.3 regionservers</span> {#title-1}

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hbase/conf]
$ cat regionservers
hadoop01
hadoop02
hadoop03
</code></pre>

<span class="directory"></span>

#### <span id="624">6.2.4 分发配置到其他节点</span> {#title-2}

<pre><code class="language-bash line-numbers">for i in hadoop02 hadoop03
    do 
        sudo scp -rp  /usr/local/hbase-1.4.9 $i:/usr/local/
done 
</code></pre>

### <span id="63_hbase">6.3 启动hbase集群</span>

<span class="directory"></span>

#### <span id="631_hbase">6.3.1 启动hbase</span> {#title-3}

<pre><code class="language-bash line-numbers">[clsn@hadoop01 /usr/local/hbase/bin]
$ sudo  ./start-hbase.sh
hadoop03: running zookeeper, logging to /usr/local/hbase-1.4.9/bin/../logs/hbase-root-zookeeper-hadoop03.out
hadoop02: running zookeeper, logging to /usr/local/hbase-1.4.9/bin/../logs/hbase-root-zookeeper-hadoop02.out
hadoop01: running zookeeper, logging to /usr/local/hbase-1.4.9/bin/../logs/hbase-root-zookeeper-hadoop01.out
running master, logging to /usr/local/hbase-1.4.9/bin/../logs/hbase-root-master-hadoop01.out
hadoop02: running regionserver, logging to /usr/local/hbase-1.4.9/bin/../logs/hbase-root-regionserver-hadoop02.out
hadoop03: running regionserver, logging to /usr/local/hbase-1.4.9/bin/../logs/hbase-root-regionserver-hadoop03.out
hadoop01: running regionserver, logging to /usr/local/hbase-1.4.9/bin/../logs/hbase-root-regionserver-hadoop01.out

</code></pre>

访问 [http://hadoop01:16610/master-status][3] 查看hbase状态

<img data-original="https://cdn.nlark.com/yuque/0/2018/png/206952/1545710312002-60c440c8-ebe8-448e-800f-9d089c79ddce.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="Hadoop+HBase 集群搭建" alt="image.png | center | 747x445" title="" /> 

<span class="directory"></span>

#### <span id="632_hbase">6.3.2 启动hbase 客户端</span> {#title-4}

<pre><code class="language-sql line-numbers">[clsn@hadoop01 /usr/local/hbase/bin]
$ ./hbase shell  #启动hbase客户端
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/local/hbase-1.4.9/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/local/hadoop-3.1.1/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
Version 1.4.9, rd625b212e46d01cb17db9ac2e9e927fdb201afa1, Wed Dec  5 11:54:10 PST 2018

hbase(main):001:0&gt; create 'clsn','cf'   #创建一个clsn表，一个cf 列簇
0 row(s) in 7.8790 seconds

=&gt; Hbase::Table - clsn    

hbase(main):003:0&gt; list    #查看hbase 所有表
TABLE
clsn
1 row(s) in 0.0860 seconds

=&gt; ["clsn"]
hbase(main):004:0&gt; put 'clsn','1000000000','cf:name','clsn'    #put一条记录到表clsn，rowkey 为 1000000000，放到 name列上
0 row(s) in 0.3390 seconds

hbase(main):005:0&gt; put 'clsn','1000000000','cf:sex','male'    #put一条记录到表clsn，rowkey 为 1000000000，放到sex列上
0 row(s) in 0.0300 seconds

hbase(main):006:0&gt; put 'clsn','1000000000','cf:age','24'     #put一条记录到表clsn，rowkey 为 1000000000，放到age列上
0 row(s) in 0.0290 seconds

hbase(main):007:0&gt; count  'clsn'     
1 row(s) in 0.2100 seconds

=&gt; 1
hbase(main):008:0&gt;  get 'clsn','cf'
COLUMN                        CELL
0 row(s) in 0.1050 seconds

hbase(main):009:0&gt; get 'clsn','1000000000'      #获取数据
COLUMN                        CELL
 cf:age                       timestamp=1545710530665, value=24
 cf:name                      timestamp=1545710495871, value=clsn
 cf:sex                       timestamp=1545710509333, value=male
1 row(s) in 0.0830 seconds

hbase(main):010:0&gt; list
TABLE
clsn
1 row(s) in 0.0240 seconds

=&gt; ["clsn"]
hbase(main):011:0&gt; drop  clsn
NameError: undefined local variable or method `clsn' for #&lt;Object:0x6f731759&gt;

hbase(main):012:0&gt; drop  'clsn'

ERROR: Table clsn is enabled. Disable it first.

Here is some help for this command:
Drop the named table. Table must first be disabled:
  hbase&gt; drop 't1'
  hbase&gt; drop 'ns1:t1'


hbase(main):013:0&gt; list
TABLE
clsn
1 row(s) in 0.0330 seconds

=&gt; ["clsn"]

hbase(main):015:0&gt; disable 'clsn'
0 row(s) in 2.4710 seconds

hbase(main):016:0&gt; list
TABLE
clsn
1 row(s) in 0.0210 seconds

=&gt; ["clsn"]
</code></pre>

## <span id="7">7. 参考文献</span>

> <https://hadoop.apache.org/releases.html>  
> <https://my.oschina.net/orrin/blog/1816023>  
> <https://www.yiibai.com/hadoop/>  
> [http://blog.fens.me/hadoop-family-roadmap/][4]  
> [http://www.cnblogs.com/Springmoon-venn/p/9054006.html][5]  
> <https://github.com/googlehosts/hosts>  
> [http://abloz.com/hbase/book.html][6] 

<div id="toc_container" class="toc_white have_bullets">
  <ul class="toc_list">
    <li>
      <a href="#HadoopHBase">Hadoop+HBase 集群搭建</a><ul>
        <li>
          <a href="#1">1. 环境准备</a><ul>
            <li>
              <a href="#11">1.1 配置说明</a>
            </li>
            <li>
              <a href="#12">1.2 机器配置说明</a>
            </li>
            <li>
              <a href="#13_ssh">1.3 ssh互信配置</a>
            </li>
            <li>
              <a href="#14_jdk">1.4 配置jdk</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#2_hadoop">2. 安装hadoop</a><ul>
            <li>
              <a href="#21_Binary">2.1 安装包下载（Binary）</a>
            </li>
            <li>
              <a href="#22">2.2 安装</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#3hadoop">3.修改hadoop配置</a><ul>
            <li>
              <a href="#31_hadoop-envsh">3.1 hadoop-env.sh</a>
            </li>
            <li>
              <a href="#32_core-sitexml">3.2 core-site.xml</a>
            </li>
            <li>
              <a href="#33_hdfs-sitexml">3.3 hdfs-site.xml</a>
            </li>
            <li>
              <a href="#34_mapred-sitexml">3.4 mapred-site.xml</a>
            </li>
            <li>
              <a href="#35_yarn-sitexml">3.5 yarn-site.xml</a>
            </li>
            <li>
              <a href="#36_masters_slaves">3.6 masters & slaves</a>
            </li>
            <li>
              <a href="#37">3.7 启动脚本修改</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#4">4. 启动前准备</a><ul>
            <li>
              <a href="#41">4.1 创建文件目录</a>
            </li>
            <li>
              <a href="#42_hadoop">4.2 复制hadoop配置到其他机器</a>
            </li>
            <li>
              <a href="#43_hadoop">4.3 启动hadoop集群</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#5">5.集群启动成功</a>
        </li>
        <li>
          <a href="#6Hbase">6.Hbase配置</a><ul>
            <li>
              <a href="#61_Hbase">6.1 部署Hbase包</a>
            </li>
            <li>
              <a href="#62">6.2 修改配置文件</a><ul>
                <li>
                  <a href="#621_hbase-envsh">6.2.1 hbase-env.sh</a>
                </li>
                <li>
                  <a href="#623_regionservers">6.2.3 regionservers</a>
                </li>
                <li>
                  <a href="#624">6.2.4 分发配置到其他节点</a>
                </li>
              </ul>
            </li>
            
            <li>
              <a href="#63_hbase">6.3 启动hbase集群</a><ul>
                <li>
                  <a href="#631_hbase">6.3.1 启动hbase</a>
                </li>
                <li>
                  <a href="#632_hbase">6.3.2 启动hbase 客户端</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#7">7. 参考文献</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

 [1]: /wp-content/themes/clsn-003/inc/go.php?url=http://hadoop02:8088/cluster/nodes
 [2]: /wp-content/themes/clsn-003/inc/go.php?url=http://hadoop01:50070/dfshealth.html#tab-datanode
 [3]: /wp-content/themes/clsn-003/inc/go.php?url=http://hadoop01:16610/master-status
 [4]: /wp-content/themes/clsn-003/inc/go.php?url=http://blog.fens.me/hadoop-family-roadmap/
 [5]: /wp-content/themes/clsn-003/inc/go.php?url=http://www.cnblogs.com/Springmoon-venn/p/9054006.html
 [6]: /wp-content/themes/clsn-003/inc/go.php?url=http://abloz.com/hbase/book.html