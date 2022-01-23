---
title: MySQL-Select语句高级应用
author: 惨绿少年
type: post
date: 2017-12-22T03:39:00+00:00
url: /clsn/lx403.html
Baidusubmit:
  - 1
views:
  - 280
categories:
  - Linux运维
  - MySQL
  - 数据库

---
## <span id="11_SELECT">1.1 SELECT高级应用</span>

### <span id="111">1.1.1 前期准备工作</span>

_本次测试使用的是world__数据库，由mysql__官方提供下载地址_：

<div>
  <p class="a1">
    　　　<a href="　https://dev.mysql.com/doc/index-other.html" target="_blank">　https://dev.mysql.com/doc/index-other.html</a>
  </p>
</div>

_world__文件导入方法，官方说明：_

<div>
  <p class="a1">
    　　　　<a href="https://dev.mysql.com/doc/world-setup/en/world-setup-installation.html" target="_blank">https://dev.mysql.com/doc/world-setup/en/world-setup-installation.html</a>
  </p>
</div>

&nbsp;&nbsp; _下载sqlyog_ _软件，用于之后的数据库管理用_：

<div>
  <p class="a1">
    　　　　<a href="/wp-content/themes/clsn-003/inc/go.php?url=http://www.webyog.com"  target="_blank">http://www.webyog.com</a>
  </p>
</div>

**创建用户**，能够让sqlyog登录数据库即可，注意权限控制。

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">grant</span> <span style="color: #808080;">all</span> <span style="color: #0000ff;">on</span> <span style="color: #808080;">*</span>.<span style="color: #808080;">*</span> <span style="color: #0000ff;">to</span> root@<span style="color: #ff0000;">'</span><span style="color: #ff0000;">%</span><span style="color: #ff0000;">'</span> identified <span style="color: #0000ff;">by</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">123</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
Query OK, </span><span style="color: #800000; font-weight: bold;"></span> rows affected (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
</div>

&nbsp;&nbsp; 授权用户后参看

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> <span style="color: #0000ff;">select</span> <span style="color: #ff00ff;">user</span>,host <span style="color: #0000ff;">from</span> mysql.<span style="color: #ff00ff;">user</span> <span style="color: #0000ff;">where</span> <span style="color: #ff00ff;">user</span> <span style="color: #808080;">like</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">root</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----+-----------+</span>
<span style="color: #808080;">|</span> <span style="color: #ff00ff;">user</span> <span style="color: #808080;">|</span> host      <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----+-----------+</span>
<span style="color: #808080;">|</span> root <span style="color: #808080;">|</span> <span style="color: #808080;">%</span>         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> root <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;">10.0</span>.<span style="color: #800000; font-weight: bold;">0.1</span>  <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> root <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;">127.0</span>.<span style="color: #800000; font-weight: bold;">0.1</span> <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> root <span style="color: #808080;">|</span> localhost <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">----+-----------+</span>
<span style="color: #800000; font-weight: bold;">4</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
  </div>
</div>

### <span id="112_select">1.1.2 select语法格式说明</span>

<div>
  <div class="cnblogs_code">
    <pre>mysql<span style="color: #808080;">></span> help <span style="color: #0000ff;">select</span><span style="color: #000000;">;
Name: </span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">SELECT</span><span style="color: #ff0000;">'</span><span style="color: #000000;">
Description:
Syntax:
</span><span style="color: #0000ff;">SELECT</span>
    <span style="color: #ff0000;">[</span><span style="color: #ff0000;">ALL | DISTINCT | DISTINCTROW </span><span style="color: #ff0000;">]</span>
      <span style="color: #ff0000;">[</span><span style="color: #ff0000;">HIGH_PRIORITY</span><span style="color: #ff0000;">]</span>
      <span style="color: #ff0000;">[</span><span style="color: #ff0000;">STRAIGHT_JOIN</span><span style="color: #ff0000;">]</span>
      <span style="color: #ff0000;">[</span><span style="color: #ff0000;">SQL_SMALL_RESULT</span><span style="color: #ff0000;">]</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">SQL_BIG_RESULT</span><span style="color: #ff0000;">]</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">SQL_BUFFER_RESULT</span><span style="color: #ff0000;">]</span>
      <span style="color: #ff0000;">[</span><span style="color: #ff0000;">SQL_CACHE | SQL_NO_CACHE</span><span style="color: #ff0000;">]</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">SQL_CALC_FOUND_ROWS</span><span style="color: #ff0000;">]</span><span style="color: #000000;">
    select_expr </span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">, select_expr ...</span><span style="color: #ff0000;">]</span>
    <span style="color: #ff0000;">[</span><span style="color: #ff0000;">FROM table_references
      [PARTITION partition_list</span><span style="color: #ff0000;">]</span>
    <span style="color: #ff0000;">[</span><span style="color: #ff0000;">WHERE where_condition</span><span style="color: #ff0000;">]</span>
    <span style="color: #ff0000;">[</span><span style="color: #ff0000;">GROUP BY {col_name | expr | position}
      [ASC | DESC</span><span style="color: #ff0000;">]</span>, ... <span style="color: #ff0000;">[</span><span style="color: #ff0000;">WITH ROLLUP</span><span style="color: #ff0000;">]</span><span style="color: #000000;">]
    </span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">HAVING where_condition</span><span style="color: #ff0000;">]</span>
    <span style="color: #ff0000;">[</span><span style="color: #ff0000;">ORDER BY {col_name | expr | position}
      [ASC | DESC</span><span style="color: #ff0000;">]</span><span style="color: #000000;">, ...]
    </span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">LIMIT {[offset,</span><span style="color: #ff0000;">]</span> row_count <span style="color: #808080;">|</span><span style="color: #000000;"> row_count OFFSET offset}]
    </span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">PROCEDURE procedure_name(argument_list)</span><span style="color: #ff0000;">]</span>
    <span style="color: #ff0000;">[</span><span style="color: #ff0000;">INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name</span><span style="color: #ff0000;">]</span><span style="color: #000000;">
        export_options
      </span><span style="color: #808080;">|</span> <span style="color: #0000ff;">INTO</span> DUMPFILE <span style="color: #ff0000;">'</span><span style="color: #ff0000;">file_name</span><span style="color: #ff0000;">'</span>
      <span style="color: #808080;">|</span> <span style="color: #0000ff;">INTO</span> var_name <span style="color: #ff0000;">[</span><span style="color: #ff0000;">, var_name</span><span style="color: #ff0000;">]</span><span style="color: #000000;">]
    </span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">FOR UPDATE | LOCK IN SHARE MODE</span><span style="color: #ff0000;">]</span>]</pre>
  </div>
</div>

## <span id="12_selectwhere">1.2 select中where子句使用</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span>  <span style="color: #808080;">*|</span>{<span style="color: #ff0000;">[</span><span style="color: #ff0000;">DISTINCT</span><span style="color: #ff0000;">]</span>  <span style="color: #0000ff;">column</span><span style="color: #808080;">|</span>select_expr <span style="color: #ff0000;">[</span><span style="color: #ff0000;">alias</span><span style="color: #ff0000;">]</span><span style="color: #000000;">, ...]} 
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">FROM [database.</span><span style="color: #ff0000;">]</span><span style="color: #0000ff;">table</span><span style="color: #000000;">]
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">WHERE conditions</span><span style="color: #ff0000;">]</span>; </pre>
  </div>
</div>

<span style="background-color: #00ff00;"><strong>&nbsp;where </strong><strong>条件的说明：</strong></span>

　　WHERE条件又叫做过滤条件，它从FROM子句的中间结果中去掉所有条件conditions不为TRUE（而为FALSE或者NULL）的行。

　　WHERE子句跟在FROM子句后面，不能在WHERE子句中使用列别名。

<span style="background-color: #00ff00;">【示例一】where字句的基本使用</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> world.`city` <span style="color: #0000ff;">WHERE</span> CountryCode<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">CHN</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;
</span><span style="color: #808080;">or</span>
<span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span> world.`city` <span style="color: #0000ff;">WHERE</span> CountryCode<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">chn</span><span style="color: #ff0000;">'</span>;</pre>
  </div>
</div>

&nbsp;&nbsp; sql说明：从数据库中查找是中国的城市。

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222191324318-1160902236.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />
</p>

注意：

　　WHERE中出现的字符串和日期字面量必须使用引号括起来

　　这里，字符串字面量写成大写或小写结果都一样，即不区分大小写进行查询。

　　这和ORACLE不同，ORACLE中WHERE条件中的字面量是区分大小写的

<span style="background-color: #00ff00;">【示例二】where字句中的逻辑操作符</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> world.`city` 
</span><span style="color: #0000ff;">WHERE</span> CountryCode<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">chn</span><span style="color: #ff0000;">'</span> <span style="color: #808080;">AND</span> district <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">shanxi</span><span style="color: #ff0000;">'</span>;</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明： 从数据库中查找是中国的并且是山西的城市

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222191417709-885140040.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

逻辑操作符介绍：

<table class="MsoTable15Grid4Accent1" style="border-width: initial; border-style: none; border-color: initial;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 91.9pt; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #4f81bd; border-bottom-color: #4f81bd; border-left-color: #4f81bd; border-right: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="123">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 5;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">逻辑操作符</span></strong>
      </p>
    </td>
    
    <td style="width: 430.9pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #4f81bd; border-right-color: #4f81bd; border-bottom-color: #4f81bd; border-left: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="575">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 1;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 91.9pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #95b3d7; border-bottom-color: #95b3d7; border-left-color: #95b3d7; border-top: none; background: #dbe5f1; padding: 0cm 5.4pt;" width="123">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 68;" align="center">
        <strong><span lang="EN-US">and</span></strong>
      </p>
    </td>
    
    <td style="width: 430.9pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #95b3d7; border-right-width: 1pt; border-right-color: #95b3d7; background: #dbe5f1; padding: 0cm 5.4pt;" valign="top" width="575">
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">逻辑与。只有当所有的子条件都为</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">时，</span><span lang="EN-US">and</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">才返回</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">。否则返回</span><span lang="EN-US">false</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">或</span><span lang="EN-US">null</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 91.9pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #95b3d7; border-bottom-color: #95b3d7; border-left-color: #95b3d7; border-top: none; padding: 0cm 5.4pt;" width="123">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 4;" align="center">
        <strong><span lang="EN-US">or</span></strong>
      </p>
    </td>
    
    <td style="width: 430.9pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #95b3d7; border-right-width: 1pt; border-right-color: #95b3d7; padding: 0cm 5.4pt;" valign="top" width="575">
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">逻辑或。只要有一个子条件为</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">，</span><span lang="EN-US">or</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">就返回</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">。否则返回</span><span lang="EN-US">false</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">或</span><span lang="EN-US">null</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 91.9pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #95b3d7; border-bottom-color: #95b3d7; border-left-color: #95b3d7; border-top: none; background: #dbe5f1; padding: 0cm 5.4pt;" width="123">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 68;" align="center">
        <strong><span lang="EN-US">not</span></strong>
      </p>
    </td>
    
    <td style="width: 430.9pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #95b3d7; border-right-width: 1pt; border-right-color: #95b3d7; background: #dbe5f1; padding: 0cm 5.4pt;" valign="top" width="575">
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">逻辑非。如果子条件为</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">，则返回</span><span lang="EN-US">false</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">；如果子条件为</span><span lang="EN-US">false</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">，则返回</span><span lang="EN-US">true</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 91.9pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #95b3d7; border-bottom-color: #95b3d7; border-left-color: #95b3d7; border-top: none; padding: 0cm 5.4pt;" width="123">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 4;" align="center">
        <strong><span lang="EN-US">xor</span></strong>
      </p>
    </td>
    
    <td style="width: 430.9pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #95b3d7; border-right-width: 1pt; border-right-color: #95b3d7; padding: 0cm 5.4pt;" valign="top" width="575">
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">逻辑异或。当一个子条件为</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">而另一个子条件为</span><span lang="EN-US">false</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">时，其结果为</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">；</span>
      </p>
      
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">当两个条件都为</span><span lang="EN-US">true</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">或都为</span><span lang="EN-US">false</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">时，结果为</span><span lang="EN-US">false</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">。否则，结果为</span><span lang="EN-US">null</span>
      </p>
    </td>
  </tr>
</table>

<span style="background-color: #00ff00;">【示例三】：where字句中的范围比较</span>

<div class="cnblogs_code">
  <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> world.`city` 
</span><span style="color: #0000ff;">WHERE</span><span style="color: #000000;"> 
population </span><span style="color: #808080;">BETWEEN</span> <span style="color: #800000; font-weight: bold;">100000</span> <span style="color: #808080;">AND</span> <span style="color: #800000; font-weight: bold;">200000</span> ;</pre>
</div>

&nbsp;&nbsp; &nbsp; &nbsp; sql说明： 从数据库中查找人口数量在 100000-200000 之间的城市

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222191519678-919149104.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />
</p>

<span style="background-color: #00ff00;">【示例四】：where字句中的IN</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> city
</span><span style="color: #0000ff;">WHERE</span> countrycode <span style="color: #808080;">IN</span> (<span style="color: #ff0000;">'</span><span style="color: #ff0000;">CHN</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">JPN</span><span style="color: #ff0000;">'</span>);</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明： 查询中国和日本的所有城市

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222191554896-1254277023.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />
</p>

<span style="background-color: #00ff00;">【示例五】：where字句中的like</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">USE</span><span style="color: #000000;"> world;
</span><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> city
</span><span style="color: #0000ff;">WHERE</span> countrycode <span style="color: #808080;">LIKE</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">ch%</span><span style="color: #ff0000;">'</span>;</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明： 从city表中找到国家是一ch开头的。

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222191616350-242017463.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

like的语法：

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #808080;">　　like</span> &lsquo;匹配模式字符串&rsquo;</pre>
  </div>
</div>

　　实现模式匹配查询或者模糊查询：测试一个列值是否匹配给出的模式

　　　　在&lsquo;匹配模式字符串&rsquo;中，可以有两个具有特殊含义的通配字符：

<div>
  <div class="cnblogs_code">
    <pre>        <span style="color: #808080;">%</span><span style="color: #000000;">：表示0个或者任意多个字符
        _：只表示一个任意字符</span></pre>
  </div>
</div>

## <span id="13_selectORDER_BY">1.3 select中ORDER BY子句</span>

### <span id="131_order_by">1.3.1 order by 子句的作用</span>

　　ORDER BY子句用来排序行

　　如果SELECT语句中没有ORDER BY子句，那么结果集中行的顺序是不可预料的

_语法：_

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;">  expr
</span><span style="color: #0000ff;">FROM</span>  <span style="color: #0000ff;">table</span>
<span style="color: #ff0000;">[</span><span style="color: #ff0000;">WHERE condition(s)</span><span style="color: #ff0000;">]</span>
<span style="color: #ff0000;">[</span><span style="color: #ff0000;">ORDER  BY  {column, expr, numeric_position} [Asc|DEsc</span><span style="color: #ff0000;">]</span>];</pre>
  </div>
</div>

<p style="text-align: left;">
  <em>部分参数说明：</em>&nbsp;
</p>

<table class="MsoTable15Grid4Accent1" style="border-width: initial; border-style: none; border-color: initial; margin-left: auto; margin-right: auto;" border="1" cellspacing="0" cellpadding="0">
  <tr>
    <td style="width: 99pt; border-top-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-top-color: #4f81bd; border-bottom-color: #4f81bd; border-left-color: #4f81bd; border-right: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="132">
      <p class="MsoNormal" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">参数</span></strong>
      </p>
    </td>
    
    <td style="width: 423.8pt; border-top-width: 1pt; border-right-width: 1pt; border-bottom-width: 1pt; border-top-color: #4f81bd; border-right-color: #4f81bd; border-bottom-color: #4f81bd; border-left: none; background: #4f81bd; padding: 0cm 5.4pt;" valign="top" width="565">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 1;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New'; color: white; mso-themecolor: background1;">参数说明</span></strong>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #95b3d7; border-bottom-color: #95b3d7; border-left-color: #95b3d7; border-top: none; background: #dbe5f1; padding: 0cm 5.4pt;" valign="top" width="132">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 68;" align="center">
        <strong><span lang="EN-US">Asc</span></strong>
      </p>
    </td>
    
    <td style="width: 423.8pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #95b3d7; border-right-width: 1pt; border-right-color: #95b3d7; background: #dbe5f1; padding: 0cm 5.4pt;" valign="top" width="565">
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">执行升序排序。默认值</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #95b3d7; border-bottom-color: #95b3d7; border-left-color: #95b3d7; border-top: none; padding: 0cm 5.4pt;" valign="top" width="132">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 4;" align="center">
        <strong><span lang="EN-US">DEsc</span></strong>
      </p>
    </td>
    
    <td style="width: 423.8pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #95b3d7; border-right-width: 1pt; border-right-color: #95b3d7; padding: 0cm 5.4pt;" valign="top" width="565">
      <p class="MsoNormal">
        <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">执行降序排序</span>
      </p>
    </td>
  </tr>
  
  <tr>
    <td style="width: 99pt; border-right-width: 1pt; border-bottom-width: 1pt; border-left-width: 1pt; border-right-color: #95b3d7; border-bottom-color: #95b3d7; border-left-color: #95b3d7; border-top: none; background: #92d050; padding: 0cm 5.4pt;" valign="top" width="132">
      <p class="MsoNormal" style="text-align: center; mso-yfti-cnfc: 68;" align="center">
        <strong><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">使用方法</span></strong>
      </p>
    </td>
    
    <td style="width: 423.8pt; border-top: none; border-left: none; border-bottom-width: 1pt; border-bottom-color: #95b3d7; border-right-width: 1pt; border-right-color: #95b3d7; background: #92d050; padding: 0cm 5.4pt;" valign="top" width="565">
      <p class="MsoNormal">
        <span lang="EN-US">ORDER BY</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">子句一般在</span><span lang="EN-US">SELECT</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 'Courier New'; mso-hansi-font-family: 'Courier New';">语句的最后面</span>
      </p>
    </td>
  </tr>
</table>

<span style="font-size: 1.17em;">1.3.2 order by 示例</span>

<span style="font-size: 1.17em; background-color: #00ff00;">【示例一】Order by基本使用</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> city
</span><span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> population;</pre>
  </div>
</div>

&nbsp;&nbsp; &nbsp;&nbsp; sql说明：将城市表按照人口数量升序排列

<p align="center">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222192121303-1432055736.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />
</p>

<span style="background-color: #00ff00;">【示例二】多个排序条件</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> city
</span><span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> population,countrycode;</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明： 按照人口和国家进行排序

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222192210303-281501689.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

【示例三】以select字句列编号排序

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> city
</span><span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> <span style="color: #800000; font-weight: bold;">5</span>;</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明：按照第5列进行排序

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222192232928-717216664.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

【示例四】desc实践

<div class="cnblogs_code">
  <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> city
</span><span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> <span style="color: #800000; font-weight: bold;">5</span> <span style="color: #0000ff;">DESC</span>;</pre>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明： 按照第列进行逆序排列

<p align="center">
  &nbsp;<img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222192253521-267645837.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />
</p>

**　<span style="color: #ff0000;">　说明：NULL</span>**<span style="color: #ff0000;"><strong>值的排序</strong></span>

　　　　在MySQL中，把NULL值当做一列值中的最小值对待。

　　　　因此，升序排序时，它出现在最前面。

## <span id="14_LIMIT">1.4 LIMIT子句</span>

特点说明：

<div>
  <div style="mso-element: para-border-div; border: solid windowtext 2.0pt; padding: 1.0pt 4.0pt 1.0pt 4.0pt; mso-border-shadow: yes; background: #F2F2F2; mso-shading: windowtext; mso-pattern: gray-5 auto; margin-left: 28.3pt; margin-right: 76.7pt;">
    <p class="a" style="margin: 1pt 0cm; text-indent: 42.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span lang="EN-US">MySQL</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">特有的子句。</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; text-indent: 42.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">它是</span><span lang="EN-US">SELECT</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">语句中的最后一个子句（在</span><span lang="EN-US">order by</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">后面）。</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; text-indent: 42.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">它用来表示从结果集中选取最前面或最后面的几行。</span>
    </p>
    
    <p class="a" style="margin: 1pt 0cm; text-indent: 42.5pt; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial;">
      <span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">偏移量</span><span lang="EN-US">offset</span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">的最小值为</span><span lang="EN-US"></span><span style="font-family: '微软雅黑',sans-serif; mso-ascii-font-family: 宋体; mso-hansi-font-family: 宋体;">。</span>
    </p>
  </div>
</div>

语法：

<div>
  <div class="cnblogs_code">
    <pre>limit  <span style="color: #808080;">&lt;</span>获取的行数<span style="color: #808080;">></span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">OFFSET &lt;跳过的行数></span><span style="color: #ff0000;">]</span><span style="color: #000000;">
或者 
limit </span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">&lt;跳过的行数>,</span><span style="color: #ff0000;">]</span> <span style="color: #808080;">&lt;</span>获取的行数<span style="color: #808080;">></span>  </pre>
  </div>
</div>

查询示例

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">FROM</span><span style="color: #000000;"> city
</span><span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> <span style="color: #800000; font-weight: bold;">5</span> <span style="color: #0000ff;">DEsc</span><span style="color: #000000;">
LIMIT </span><span style="color: #800000; font-weight: bold;">4</span>;</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明： 获取排序后的前4行

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222192407334-1597033677.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

<div>
  <p class="a3">
    　　　　注：先按照人口数量进行降序排序，然后使用limit从中挑出最前面的4行。
  </p>
  
  <p class="a3">
    　　　　　　如果没有order by子句，返回的4行就是不可预料的。
  </p>
</div>

## <span id="15">1.5 多表连接查询</span>

### <span id="151_where">1.5.1 传统的连接写法（使用where）</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> NAME,ci.countrycode ,cl.language ,ci.population
</span><span style="color: #0000ff;">FROM</span><span style="color: #000000;">  city ci , countrylanguage cl
</span><span style="color: #0000ff;">WHERE</span> ci.`CountryCode`<span style="color: #808080;">=</span>cl.countrycode;</pre>
  </div>
  
  <p>
    &nbsp; &nbsp; &nbsp; &nbsp;sql说明： city定别名为ci ，国家定别名问为cl，进行连表查询，NAME是共同的键值，使用where条件进行连接。
  </p>
</div>

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222192431865-1649603031.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

　　注意：一旦给表定义了别名，那么原始的表名就不能在出现在该语句的其它子句中了

### <span id="152_NATURALnbsp_JOIN">1.5.2 NATURAL&nbsp; JOIN子句</span>

　　自动到两张表中查找所有同名同类型的列拿来做连接列，进行相等连接

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> NAME,countrycode ,LANGUAGE ,population
</span><span style="color: #0000ff;">FROM</span>  city NATURAL  <span style="color: #808080;">JOIN</span><span style="color: #000000;">  countrylanguage
</span><span style="color: #0000ff;">WHERE</span> population <span style="color: #808080;">></span> <span style="color: #800000; font-weight: bold;">1000000</span>
<span style="color: #0000ff;">ORDER</span> <span style="color: #0000ff;">BY</span> population;</pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明：使用natural join 进行相等连接，两个表，条件为人口大于1000000的，进行升序排列。

<p align="center">
  <img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222192545068-233765473.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

　　注意：在select子句只能出现一个连接列

### <span id="153_using">1.5.3 使用using子句</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> NAME,countrycode ,LANGUAGE ,population
</span><span style="color: #0000ff;">FROM</span>  city <span style="color: #808080;">JOIN</span><span style="color: #000000;">  countrylanguage
USING(countrycode);</span></pre>
  </div>
</div>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明：使用join进行两表的来连接，using指定countrycode为关联列。

<p align="center">
  <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222192604709-1781821410.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
</p>

### <span id="154">1.5.4 集合操作</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">UNION</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">DISTINCT</span><span style="color: #ff0000;">]</span>
<span style="color: #0000ff;">UNION</span> <span style="color: #808080;">ALL</span></pre>
  </div>
  
  <p>
    &nbsp;
  </p>
</div>

语法：

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> ... 
</span><span style="color: #0000ff;">UNION</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">ALL | DISTINCT</span><span style="color: #ff0000;">]</span> 
<span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> ... 
</span><span style="color: #ff0000;">[</span><span style="color: #ff0000;">UNION [ALL | DISTINCT</span><span style="color: #ff0000;">]</span> 
<span style="color: #0000ff;">SELECT</span> ...]</pre>
  </div>
  
  <p>
    　　&nbsp; &nbsp;⛳ UNION用于把两个或者多个select查询的结果集合并成一个
  </p>
</div>

&nbsp; 　　&nbsp;⛳&nbsp;进行合并的两个查询，其SELECT列表必须在数量和对应列的数据类型上保持一致

&nbsp; 　　&nbsp;⛳&nbsp;默认会去掉两个查询结果集中的重复行

&nbsp; 　　&nbsp;⛳&nbsp;默认结果集不排序

&nbsp; 　&nbsp; &nbsp;&nbsp;⛳&nbsp;最终结果集的列名来自于第一个查询的SELECT列表

### <span id="155">1.5.5 分组操作及分组处理</span>

　　&ldquo;Group By&rdquo;从字面意义上理解就是根据&ldquo;By&rdquo;指定的规则对数据进行分组，所谓的分组就是将一个&ldquo;数据集&rdquo;划分成若干个&ldquo;小区域&rdquo;，然后针对若干个&ldquo;小区域&rdquo;进行数据处理。

<span style="background-color: #00ff00;">Having与Where的区别</span>

　　where 子句的作用是在对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，where条件中不能包含聚组函数，使用where条件过滤出特定的行。

　　having 子句的作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数，使用having 条件过滤出特定的组，也可以使用多个分组标准进行分组。

### <span id="156_select">1.5.6 【select高级应用】数据库备份脚本拼接</span>

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> CONCAT("mysqldump ","<span style="color: #808080;">-</span>uroot ","<span style="color: #808080;">-</span>p123 ",table_schema," ",table_name,"<span style="color: #808080;">>/</span>tmp<span style="color: #808080;">/</span><span style="color: #000000;">",table_schema,"_",table_name,".sql") 
</span><span style="color: #0000ff;">FROM</span><span style="color: #000000;"> information_schema.tables
</span><span style="color: #0000ff;">WHERE</span> table_schema<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">world</span><span style="color: #ff0000;">'</span>
<span style="color: #0000ff;">INTO</span> OUTFILE <span style="color: #ff0000;">'</span><span style="color: #ff0000;">/tmp/world_bak.sh</span><span style="color: #ff0000;">'</span></pre>
  </div>
  
  <p>
    &nbsp; &nbsp; &nbsp; &nbsp;使用concat进行拼接数据备份脚本。
  </p>
</div>

<p style="text-align: center;">
  &nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222192803521-1773133819.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />
</p>

-- 显示信息，可直接进行运算

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> CONCAT("<span style="color: #800000; font-weight: bold;">132</span><span style="color: #000000;">")；
</span><span style="color: #0000ff;">SELECT</span> CONCAT("<span style="color: #800000; font-weight: bold;">132</span><span style="color: #808080;">+</span><span style="color: #800000; font-weight: bold;">123</span><span style="color: #000000;">")；
</span><span style="color: #0000ff;">SELECT</span> CONCAT("<span style="color: #800000; font-weight: bold;">132</span><span style="color: #808080;">+</span><span style="color: #800000; font-weight: bold;">123</span>")；</pre>
  </div>
</div>

-- 查看引擎是innodb的表

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> TABLE_NAME  <span style="color: #0000ff;">FROM</span> TABLES <span style="color: #0000ff;">WHERE</span> ENGINE<span style="color: #808080;">=</span><span style="color: #ff0000;">'</span><span style="color: #ff0000;">innodb</span><span style="color: #ff0000;">'</span><span style="color: #000000;">;

</span><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> CHARACTER_SET_NAME, COLLATION_NAME
</span><span style="color: #0000ff;">FROM</span><span style="color: #000000;">   INFORMATION_SCHEMA.COLLATIONS
</span><span style="color: #0000ff;">WHERE</span>  IS_DEFAULT <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Yes</span><span style="color: #ff0000;">'</span>;</pre>
  </div>
  
  <p style="text-align: center;">
    <img data-original="https://images2017.cnblogs.com/blog/1190037/201712/1190037-20171222192836428-221565943.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
  </p>
</div>

-- 显示每个库下有多少表

<div>
  <div class="cnblogs_code">
    <pre><span style="color: #0000ff;">SELECT</span> TABLE_SCHEMA ,<span style="color: #ff00ff;">COUNT</span>(<span style="color: #808080;">*</span><span style="color: #000000;">)
</span><span style="color: #0000ff;">FROM</span><span style="color: #000000;"> information_schema.`TABLES`
</span><span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span> TABLE_SCHEMA;</pre>
  </div>
  
  <p style="text-align: center;">
    <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222192859381-2016937619.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
  </p>
</div>

### <span id="157">1.5.7 子查询</span>

<span style="background-color: #00ff00;"><strong>子查询定义</strong></span>

　　在一个表表达中可以调用另一个表表达式，这个被调用的表表达式叫做子查询（subquery），我么也称作子选择（subselect）或内嵌选择（inner select）。子查询的结果传递给调用它的表表达式继续处理。

　　子查询（inner&nbsp; query）先执行，然后执行主查询（outer&nbsp; query）

　　子查询按对返回结果集的调用方法，可分为：where型子查询，from型子查询及exists型子查询。

<span style="background-color: #00ff00;"><strong>使用子查询原则</strong></span>

　　一个子查询必须放在圆括号中。

　　将子查询放在比较条件的右边以增加可读性。

　　子查询不包含 ORDER BY 子句。对一个 SELECT 语句只能用一个 ORDER BY 子句，并且如果指定了它就必须放在主 SELECT 语句的最后。

　　在子查询中可以使用两种比较条件：单行运算符(>, =, >=, <, <>, <=) 和多行运算符(IN, ANY, ALL)。

<span style="background-color: #00ff00;"><strong>不相关子查询</strong></span>

　　子查询中没有使用到外部查询的表中的任何列。先执行子查询，然后执行外部查询

　　相关子查询(correlated subquery)

　　子查询中使用到了外部查询的表中的任何列。先执行外部查询，然后执行子查询

　　以上两种类型之下又可以分为：

<div class="cnblogs_code">
  <pre>　　行子查询(row subquery)：返回的结果集是 <span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;"> 行 N 列
　　列子查询(</span><span style="color: #0000ff;">column</span><span style="color: #000000;"> subquery)：返回的结果集是 N 行 1列 
　　表子查询(</span><span style="color: #0000ff;">table</span><span style="color: #000000;"> subquery)：返回的结果集是 N 行 N 列 
　　标量子查询(scalar subquery)：返回1行1列一个值</span></pre>
</div>

<span style="background-color: #ffff00; color: #ff0000;"><strong><em><span style="text-decoration: underline;">子查询示例</span></em></strong></span>

&nbsp;&nbsp; 创建数据表

<div>
  <div class="cnblogs_code" onclick="cnblogs_code_show('a997ac3d-2ca0-44b3-90f0-9c86160f0e8f')">
    <img id="code_img_closed_a997ac3d-2ca0-44b3-90f0-9c86160f0e8f" class="code_img_closed" src="https://clsn.io/wp-content/uploads/2018/03/ContractedBlock-7.gif" alt="MySQL-Select语句高级应用" alt="" /><img id="code_img_opened_a997ac3d-2ca0-44b3-90f0-9c86160f0e8f" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('a997ac3d-2ca0-44b3-90f0-9c86160f0e8f',event)" data-original="https://clsn.io/wp-content/uploads/2018/03/ExpandedBlockStart-7.gif" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" /></p> 
    
    <div id="cnblogs_code_open_a997ac3d-2ca0-44b3-90f0-9c86160f0e8f" class="cnblogs_code_hide">
      <pre><span style="color: #008080;"> 1</span> <span style="color: #0000ff;">CREATE</span> <span style="color: #0000ff;">TABLE</span><span style="color: #000000;"> PLAYERS  
</span><span style="color: #008080;"> 2</span>     (PLAYERNO      <span style="color: #0000ff;">INTEGER</span>      <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;"> 3</span>     NAME           <span style="color: #0000ff;">CHAR</span>(<span style="color: #800000; font-weight: bold;">15</span>)     <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;"> 4</span>     INITIALS       <span style="color: #0000ff;">CHAR</span>(<span style="color: #800000; font-weight: bold;">3</span>)      <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;"> 5</span> <span style="color: #000000;">    BIRTH_DATE     DATE                 ,  
</span><span style="color: #008080;"> 6</span>     SEX            <span style="color: #0000ff;">CHAR</span>(<span style="color: #800000; font-weight: bold;">1</span>)      <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;"> 7</span>     JOINED         <span style="color: #0000ff;">SMALLINT</span>     <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;"> 8</span>     STREET         <span style="color: #0000ff;">VARCHAR</span>(<span style="color: #800000; font-weight: bold;">30</span>)  <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;"> 9</span>     HOUSENO        <span style="color: #0000ff;">CHAR</span>(<span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">)              ,  
</span><span style="color: #008080;">10</span>     POSTCODE       <span style="color: #0000ff;">CHAR</span>(<span style="color: #800000; font-weight: bold;">6</span><span style="color: #000000;">)              ,  
</span><span style="color: #008080;">11</span>     TOWN           <span style="color: #0000ff;">VARCHAR</span>(<span style="color: #800000; font-weight: bold;">30</span>)  <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;">12</span>     PHONENO        <span style="color: #0000ff;">CHAR</span>(<span style="color: #800000; font-weight: bold;">13</span><span style="color: #000000;">)             ,  
</span><span style="color: #008080;">13</span>     LEAGUENO       <span style="color: #0000ff;">CHAR</span>(<span style="color: #800000; font-weight: bold;">4</span><span style="color: #000000;">)              ,  
</span><span style="color: #008080;">14</span>     <span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;">    (PLAYERNO));  
</span><span style="color: #008080;">15</span>   
<span style="color: #008080;">16</span> <span style="color: #0000ff;">CREATE</span>   <span style="color: #0000ff;">TABLE</span><span style="color: #000000;"> PENALTIES  
</span><span style="color: #008080;">17</span>         (PAYMENTNO      <span style="color: #0000ff;">INTEGER</span>      <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;">18</span>          PLAYERNO       <span style="color: #0000ff;">INTEGER</span>      <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;">19</span>          PAYMENT_DATE   DATE         <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;">20</span>          AMOUNT         <span style="color: #0000ff;">DECIMAL</span>(<span style="color: #800000; font-weight: bold;">7</span>,<span style="color: #800000; font-weight: bold;">2</span>) <span style="color: #808080;">NOT</span> <span style="color: #0000ff;">NULL</span><span style="color: #000000;">,  
</span><span style="color: #008080;">21</span>          <span style="color: #0000ff;">PRIMARY</span> <span style="color: #0000ff;">KEY</span><span style="color: #000000;">    (PAYMENTNO)); 
</span><span style="color: #008080;">22</span> 
<span style="color: #008080;">23</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">2</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Everett</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">R</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1948-09-01</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1975</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stoney Road</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">43</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">3575NH</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stratford</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-237893</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">2411</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">24</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">6</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Parmenter</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">R</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1964-06-25</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1977</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Haseltine Lane</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">80</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1234KK</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stratford</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-476537</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">8467</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">25</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">7</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Wise</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">GWS</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1963-05-11</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1981</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Edgecombe Way</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">39</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">9758VB</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stratford</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-347689</span><span style="color: #ff0000;">'</span>, <span style="color: #0000ff;">NULL</span><span style="color: #000000;">);  
</span><span style="color: #008080;">26</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">8</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Newcastle</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">B</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1962-07-08</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">F</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1980</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Station Road</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">4</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">6584WO</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Inglewood</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-458458</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">2983</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">27</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">27</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Collins</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">DD</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1964-12-28</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">F</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1983</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Long Drive</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">804</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">8457DK</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Eltham</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">079-234857</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">2513</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">28</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">28</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Collins</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">C</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1963-06-22</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">F</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1983</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Old Main Road</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">10</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1294QK</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Midhurst</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">010-659599</span><span style="color: #ff0000;">'</span>, <span style="color: #0000ff;">NULL</span><span style="color: #000000;">);  
</span><span style="color: #008080;">29</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">39</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Bishop</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">D</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1956-10-29</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1980</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Eaton Square</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">78</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">9629CD</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stratford</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-393435</span><span style="color: #ff0000;">'</span>, <span style="color: #0000ff;">NULL</span><span style="color: #000000;">);  
</span><span style="color: #008080;">30</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">44</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Baker</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">E</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1963-01-09</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1980</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Lewis Street</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">23</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">4444LJ</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Inglewood</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-368753</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1124</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">31</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">57</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Brown</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1971-08-17</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1985</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Edgecombe Way</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">16</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">4377CB</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stratford</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-473458</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">6409</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">32</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">83</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Hope</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">PK</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1956-11-11</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1982</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Magdalene Road</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">16A</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1812UP</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stratford</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-353548</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1608</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">33</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">95</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Miller</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">P</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1963-05-14</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1972</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">High Street</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">33A</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">5746OP</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Douglas</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-867564</span><span style="color: #ff0000;">'</span>, <span style="color: #0000ff;">NULL</span><span style="color: #000000;">);  
</span><span style="color: #008080;">34</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">100</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Parmenter</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">P</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1963-02-28</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">M</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1979</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Haseltine Lane</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">80</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">6494SG</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stratford</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">070-494593</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">6524</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">35</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">104</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Moorman</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">D</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1970-05-10</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">F</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1984</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Stout Street</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">65</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">9437AO</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Eltham</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">079-987571</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">7060</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">36</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PLAYERS <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">112</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Bailey</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">IP</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1963-10-01</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">F</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">1984</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Vixen Road</span><span style="color: #ff0000;">'</span>,<span style="color: #ff0000;">'</span><span style="color: #ff0000;">8</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">6392LK</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Plymouth</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">010-548745</span><span style="color: #ff0000;">'</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1319</span><span style="color: #ff0000;">'</span><span style="color: #000000;">);  
</span><span style="color: #008080;">37</span>   
<span style="color: #008080;">38</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">1</span>,  <span style="color: #800000; font-weight: bold;">6</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1980-12-08</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">100</span><span style="color: #000000;">);
</span><span style="color: #008080;">39</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">2</span>, <span style="color: #800000; font-weight: bold;">44</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1981-05-05</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">75</span><span style="color: #000000;">);
</span><span style="color: #008080;">40</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">3</span>, <span style="color: #800000; font-weight: bold;">27</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1983-09-10</span><span style="color: #ff0000;">'</span>,<span style="color: #800000; font-weight: bold;">100</span><span style="color: #000000;">);
</span><span style="color: #008080;">41</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">4</span>,<span style="color: #800000; font-weight: bold;">104</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1984-12-08</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">50</span><span style="color: #000000;">);
</span><span style="color: #008080;">42</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">5</span>, <span style="color: #800000; font-weight: bold;">44</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1980-12-08</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">25</span><span style="color: #000000;">);
</span><span style="color: #008080;">43</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">6</span>,  <span style="color: #800000; font-weight: bold;">8</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1980-12-08</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">25</span><span style="color: #000000;">);
</span><span style="color: #008080;">44</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">7</span>, <span style="color: #800000; font-weight: bold;">44</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1982-12-30</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">30</span><span style="color: #000000;">);
</span><span style="color: #008080;">45</span> <span style="color: #0000ff;">INSERT</span> <span style="color: #0000ff;">INTO</span> PENALTIES <span style="color: #0000ff;">VALUES</span> (<span style="color: #800000; font-weight: bold;">8</span>, <span style="color: #800000; font-weight: bold;">27</span>, <span style="color: #ff0000;">'</span><span style="color: #ff0000;">1984-11-12</span><span style="color: #ff0000;">'</span>, <span style="color: #800000; font-weight: bold;">75</span>);</pre>
    </div>
    
    <p>
      <span class="cnblogs_code_collapse">View Code 创建数据库语句</span></div> </div> 
      
      <p>
        例一、获取和100号球员性别相同并且居住在同一城市的球员号码。
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">select</span><span style="color: #000000;"> playerno   
</span><span style="color: #0000ff;">from</span><span style="color: #000000;"> players   
</span><span style="color: #0000ff;">where</span> (sex, town) <span style="color: #808080;">=</span><span style="color: #000000;"> (  
    </span><span style="color: #0000ff;">select</span><span style="color: #000000;"> sex, town   
    </span><span style="color: #0000ff;">from</span><span style="color: #000000;"> players   
    </span><span style="color: #0000ff;">where</span> playerno <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">100</span>);  </pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp; 例二、获取和27号球员出生在同一年的球员的号码
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">select</span><span style="color: #000000;"> playerno   
</span><span style="color: #0000ff;">from</span><span style="color: #000000;"> players   
</span><span style="color: #0000ff;">where</span> <span style="color: #ff00ff;">year</span>(birth_date) <span style="color: #808080;">=</span><span style="color: #000000;">   
    (</span><span style="color: #0000ff;">select</span> <span style="color: #ff00ff;">year</span><span style="color: #000000;">(birth_date)   
    </span><span style="color: #0000ff;">from</span><span style="color: #000000;"> players   
    </span><span style="color: #0000ff;">where</span> playerno <span style="color: #808080;">=</span> <span style="color: #800000; font-weight: bold;">27</span><span style="color: #000000;">)   
</span><span style="color: #808080;">and</span> playerno <span style="color: #808080;">&lt;></span> <span style="color: #800000; font-weight: bold;">27</span>;</pre>
        </div>
      </div>
      
      <p>
        例三、获取那些至少支付了一次罚款的球员的名字和首字母。
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre> <span style="color: #0000ff;">select</span><span style="color: #000000;"> name, initials   
</span><span style="color: #0000ff;">from</span><span style="color: #000000;"> players   
</span><span style="color: #0000ff;">where</span> <span style="color: #808080;">exists</span><span style="color: #000000;">   
    (</span><span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> penalties   
    </span><span style="color: #0000ff;">where</span> playerno <span style="color: #808080;">=</span> players.playerno);  </pre>
        </div>
      </div>
      
      <p>
        &nbsp;例四、获取那些从来没有罚款的球员的名字和首字母。
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre> <span style="color: #0000ff;">select</span><span style="color: #000000;"> name, initials   
</span><span style="color: #0000ff;">from</span><span style="color: #000000;"> players   
</span><span style="color: #0000ff;">where</span> <span style="color: #808080;">not</span> <span style="color: #808080;">exists</span><span style="color: #000000;">   
    (</span><span style="color: #0000ff;">select</span> <span style="color: #808080;">*</span> <span style="color: #0000ff;">from</span><span style="color: #000000;"> penalties   
    </span><span style="color: #0000ff;">where</span> playerno <span style="color: #808080;">=</span> players.playerno);  </pre>
        </div>
      </div>
      
      <h2>
        <span id="16_Informatica_schema">1.6 Informatica_schema获取元数据</span>
      </h2>
      
      <h3>
        <span id="161">1.6.1 元数据访问方法</span>
      </h3>
      
      <p>
        　　查询 INFORMATION_SCHEMA 数据库表。其中包含 MySQL 数据库服务器所管理的所有对象的相关数据
      </p>
      
      <p>
        　　使用 SHOW 语句。用于获取数据库和表信息的 MySQL 专用语句
      </p>
      
      <p>
        　　使用 DESCRIBE（或 DESC）语句。用于检查表结构和列属性的快捷方式
      </p>
      
      <p>
        　　使用 mysqlshow 客户端程序。SHOW 语法的命令行程序
      </p>
      
      <p>
        <span style="background-color: #00ff00;"><strong>INFORMATION_SCHEMA </strong><strong>数据库优点介绍</strong></span>
      </p>
      
      <p>
        　　充当数据库元数据的中央系统信息库，模式和模式对象，服务器统计信息（状态变量、设置、连接） 。
      </p>
      
      <p>
        　　采用表格式以实现灵活访问，使用任意 SELECT 语句。是&ldquo;虚拟数据库&rdquo;，表并非&ldquo;真实&rdquo;表（基表），而是&ldquo;系统视图&rdquo;，根据当前用户的特权动态填充表。
      </p>
      
      <p>
        <span style="background-color: #00ff00;"><strong>列出 INFORMATION_SCHEMA </strong><strong>数据库中所有的表：</strong></span>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span>  <span style="color: #0000ff;">USE</span><span style="color: #000000;"> information_schema;
</span><span style="color: #0000ff;">Database</span><span style="color: #000000;"> changed
mysql</span><span style="color: #808080;">></span><span style="color: #000000;">  SHOW TABLES;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-------------------------------------+</span>
<span style="color: #808080;">|</span> Tables_in_information_schema          <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-------------------------------------+</span>
<span style="color: #808080;">|</span> CHARACTER_SETS                        <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> COLLATIONS                            <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> COLLATION_CHARACTER_SET_APPLICABILITY <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> COLUMNS                               <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> COLUMN_PRIVILEGES                     <span style="color: #808080;">|</span></pre>
        </div>
      </div>
      
      <h3>
        <span id="162_INFORMATION_SCHEMA_SELECT">1.6.2 对 INFORMATION_SCHEMA 使用 SELECT</span>
      </h3>
      
      <p>
        示例一：
      </p>
      
      <p>
        查找引擎是innodb的表。
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> TABLE_NAME, ENGINE
</span><span style="color: #0000ff;">FROM</span><span style="color: #000000;">  INFORMATION_SCHEMA.TABLES
</span><span style="color: #0000ff;">WHERE</span> ENGINE<span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">innodb</span><span style="color: #ff0000;">'</span>;</pre>
        </div>
      </div>
      
      <p style="text-align: center;">
        &nbsp;&nbsp;<img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222193240725-1887587949.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />
      </p>
      
      <p>
        &nbsp;
      </p>
      
      <p>
        示例二:
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME
 </span><span style="color: #0000ff;">FROM</span><span style="color: #000000;">   INFORMATION_SCHEMA.COLUMNS
 </span><span style="color: #0000ff;">WHERE</span>  DATA_TYPE <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">set</span><span style="color: #ff0000;">'</span>;</pre>
        </div>
        
        <p class="a3">
          &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;sql说明：查找数据类型是set的表
        </p>
      </div>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222193318568-1915423025.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
      </p>
      
      <p>
        示例三：
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">SELECT</span><span style="color: #000000;"> CHARACTER_SET_NAME, COLLATION_NAME,IS_DEFAULT
</span><span style="color: #0000ff;">FROM</span><span style="color: #000000;">   INFORMATION_SCHEMA.COLLATIONS
</span><span style="color: #0000ff;">WHERE</span>  IS_DEFAULT <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Yes</span><span style="color: #ff0000;">'</span>;</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; sql说明：查看找默认为yes的表
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222193338537-1033401033.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
      </p>
      
      <p>
        示例四：
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">SELECT</span> TABLE_SCHEMA, <span style="color: #ff00ff;">COUNT</span>(<span style="color: #808080;">*</span><span style="color: #000000;">) 
 </span><span style="color: #0000ff;">FROM</span><span style="color: #000000;">   INFORMATION_SCHEMA.TABLES
 </span><span style="color: #0000ff;">GROUP</span> <span style="color: #0000ff;">BY</span> TABLE_SCHEMA;</pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sql说明：查看每个数据库下表的个数
      </p>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222193356100-1822960636.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
      </p>
      
      <p>
        使用 INFORMATION_SCHEMA 表获取有关创建 shell 命令的信息。
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #0000ff;">SELECT</span> CONCAT("mysqldump <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span><span style="color: #000000;">p ",
   TABLE_SCHEMA," ",  TABLE_NAME, " </span><span style="color: #808080;">>></span><span style="color: #000000;"> ",
   TABLE_SCHEMA,".bak.sql")
 </span><span style="color: #0000ff;">FROM</span> TABLES <span style="color: #0000ff;">WHERE</span> TABLE_NAME <span style="color: #808080;">LIKE</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">Country%</span><span style="color: #ff0000;">'</span></pre>
        </div>
      </div>
      
      <p align="center">
        <img data-original="https://clsn.io/wp-content/uploads/2018/03/1190037-20171222193416162-1760456399.png" src="/wp-content/themes/clsn-003/img/blank.gif" alt="MySQL-Select语句高级应用" alt="" />&nbsp;
      </p>
      
      <h3>
        <span id="163_mysql_SQL">1.6.3 使用 mysql 命令创建 SQL 语句。</span>
      </h3>
      
      <div class="cnblogs_code">
        <pre>mysql <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 <span style="color: #008080;">--</span><span style="color: #008080;">silent --skip-column-names -e "SELECT CONCAT('CREATE TABLE ', TABLE_SCHEMA, '.',TABLE_NAME, '_backup LIKE ', TABLE_SCHEMA, '.',TABLE_NAME, ';') </span>
<span style="color: #0000ff;">FROM</span><span style="color: #000000;"> INFORMATION_SCHEMA.TABLES 
</span><span style="color: #0000ff;">WHERE</span> TABLE_SCHEMA <span style="color: #808080;">=</span> <span style="color: #ff0000;">'</span><span style="color: #ff0000;">world_innodb</span><span style="color: #ff0000;">'</span>;"</pre>
      </div>
      
      <h3>
        <span id="164_MySQLshow">1.6.4 MySQL中的show语句</span>
      </h3>
      
      <div class="cnblogs_code">
        <pre><span style="color: #000000;">SOHW databases：列出所有数据库
SHOW TABLES：列出默认数据库中的表
SHOW TABLES FROM </span>&lt;database_name><span style="color: #000000;">：列出指定数据库中的表
SHOW COLUMNS FROM </span>&lt;table_name><span style="color: #000000;">：显示表的列结构
SHOW INDEX FROM </span>&lt;table_name><span style="color: #000000;">：显示表中有关索引和索引列的信息
SHOW CHARACTER SET：显示可用的字符集及其默认整理
SHOW COLLATION：显示每个字符集的整理
SHOW STATUS：列出当前数据库状态
SHOW VARIABLES：列出数据库中的参数定义值
SHOW PROCESSLIST  查看当前的连接数量</span></pre>
      </div>
      
      <h3>
        <span id="165_DESCRIBE">1.6.5 DESCRIBE 语句</span>
      </h3>
      
      <p>
        　　　　DESCRIBE 语句 等效于 SHOW COLUMNS
      </p>
      
      <p>
        <strong><em>一般语法：</em></strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span> DESCRIBE <span style="color: #808080;">&lt;</span>table_name<span style="color: #808080;">></span>;</pre>
        </div>
      </div>
      
      <p>
        显示 INFORMATION_SCHEMA 表信息
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>mysql<span style="color: #808080;">></span><span style="color: #000000;"> DESCRIBE INFORMATION_SCHEMA.CHARACTER_SETS;
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--------------------+-------------+------+-----+---------+-------+</span>
<span style="color: #808080;">|</span> Field                <span style="color: #808080;">|</span> Type        <span style="color: #808080;">|</span> <span style="color: #0000ff;">Null</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Key</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Default</span> <span style="color: #808080;">|</span> Extra <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--------------------+-------------+------+-----+---------+-------+</span>
<span style="color: #808080;">|</span> CHARACTER_SET_NAME   <span style="color: #808080;">|</span> <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">32</span>) <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> DEFAULT_COLLATE_NAME <span style="color: #808080;">|</span> <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">32</span>) <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> DESCRIPTION          <span style="color: #808080;">|</span> <span style="color: #0000ff;">varchar</span>(<span style="color: #800000; font-weight: bold;">60</span>) <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> MAXLEN               <span style="color: #808080;">|</span> <span style="color: #0000ff;">bigint</span>(<span style="color: #800000; font-weight: bold;">3</span>)   <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;"></span>       <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">--------------------+-------------+------+-----+---------+-------+</span>
<span style="color: #800000; font-weight: bold;">4</span> rows <span style="color: #808080;">in</span> <span style="color: #0000ff;">set</span> (<span style="color: #800000; font-weight: bold;">0.00</span> sec)</pre>
        </div>
        
        <p>
          　　有关数据库和表的结构的信息与 SHOW 语句相似
        </p>
      </div>
      
      <p>
        <strong><em>一般语法：</em></strong>
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre>shell<span style="color: #808080;">></span> mysqlshow <span style="color: #ff0000;">[</span><span style="color: #ff0000;">options</span><span style="color: #ff0000;">]</span> <span style="color: #ff0000;">[</span><span style="color: #ff0000;">db_name [table_name[column_name</span><span style="color: #ff0000;">]</span>]]</pre>
        </div>
      </div>
      
      <p>
        显示所有数据库或特定数据库、表和/或列的相关信息：
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span>#  mysqlshow <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span><span style="color: #000000;">p123
Warning: Using a password </span><span style="color: #0000ff;">on</span><span style="color: #000000;"> the command line interface can be insecure.
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span>     Databases      <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span>
<span style="color: #808080;">|</span> information_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> clsn               <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> haha               <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> mysql              <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> oldboy             <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> performance_schema <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> world              <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">------------------+</span></pre>
        </div>
      </div>
      
      <p>
        &nbsp;&nbsp; 查看数据库下的表
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span>#  mysqlshow <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span><span style="color: #000000;">p123 world
Warning: Using a password </span><span style="color: #0000ff;">on</span><span style="color: #000000;"> the command line interface can be insecure.
</span><span style="color: #0000ff;">Database</span><span style="color: #000000;">: world
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------------+</span>
<span style="color: #808080;">|</span>     Tables      <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------------+</span>
<span style="color: #808080;">|</span> PENALTIES       <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> PLAYERS         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> city            <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> country         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> countrylanguage <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------------+</span></pre>
        </div>
      </div>
      
      <p>
        查看数据库下表记录
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span>#  mysqlshow <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span><span style="color: #000000;">p123 world city
Warning: Using a password </span><span style="color: #0000ff;">on</span><span style="color: #000000;"> the command line interface can be insecure.
</span><span style="color: #0000ff;">Database</span>: world  <span style="color: #0000ff;">Table</span><span style="color: #000000;">: city
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----------+----------+-------------------+------+-----+---------+----------------+---------------------------------+---------+</span>
<span style="color: #808080;">|</span> Field       <span style="color: #808080;">|</span> Type     <span style="color: #808080;">|</span> Collation         <span style="color: #808080;">|</span> <span style="color: #0000ff;">Null</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Key</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Default</span> <span style="color: #808080;">|</span> Extra          <span style="color: #808080;">|</span> <span style="color: #0000ff;">Privileges</span>                      <span style="color: #808080;">|</span> Comment <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----------+----------+-------------------+------+-----+---------+----------------+---------------------------------+---------+</span>
<span style="color: #808080;">|</span> ID          <span style="color: #808080;">|</span> <span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>)  <span style="color: #808080;">|</span>                   <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span> PRI <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span> auto_increment <span style="color: #808080;">|</span> <span style="color: #0000ff;">select</span>,<span style="color: #0000ff;">insert</span>,<span style="color: #0000ff;">update</span>,<span style="color: #0000ff;">references</span> <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> Name        <span style="color: #808080;">|</span> <span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">35</span>) <span style="color: #808080;">|</span> latin1_swedish_ci <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>                <span style="color: #808080;">|</span> <span style="color: #0000ff;">select</span>,<span style="color: #0000ff;">insert</span>,<span style="color: #0000ff;">update</span>,<span style="color: #0000ff;">references</span> <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> CountryCode <span style="color: #808080;">|</span> <span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">3</span>)  <span style="color: #808080;">|</span> latin1_swedish_ci <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span> MUL <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>                <span style="color: #808080;">|</span> <span style="color: #0000ff;">select</span>,<span style="color: #0000ff;">insert</span>,<span style="color: #0000ff;">update</span>,<span style="color: #0000ff;">references</span> <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> District    <span style="color: #808080;">|</span> <span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">20</span>) <span style="color: #808080;">|</span> latin1_swedish_ci <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>                <span style="color: #808080;">|</span> <span style="color: #0000ff;">select</span>,<span style="color: #0000ff;">insert</span>,<span style="color: #0000ff;">update</span>,<span style="color: #0000ff;">references</span> <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>
<span style="color: #808080;">|</span> Population  <span style="color: #808080;">|</span> <span style="color: #0000ff;">int</span>(<span style="color: #800000; font-weight: bold;">11</span>)  <span style="color: #808080;">|</span>                   <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span>     <span style="color: #808080;">|</span> <span style="color: #800000; font-weight: bold;"></span>       <span style="color: #808080;">|</span>                <span style="color: #808080;">|</span> <span style="color: #0000ff;">select</span>,<span style="color: #0000ff;">insert</span>,<span style="color: #0000ff;">update</span>,<span style="color: #0000ff;">references</span> <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----------+----------+-------------------+------+-----+---------+----------------+---------------------------------+---------+</span></pre>
        </div>
      </div>
      
      <p>
        查看记录信息
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span>#  mysqlshow <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span><span style="color: #000000;">p123 world city CountryCode
Warning: Using a password </span><span style="color: #0000ff;">on</span><span style="color: #000000;"> the command line interface can be insecure.
</span><span style="color: #0000ff;">Database</span>: world  <span style="color: #0000ff;">Table</span><span style="color: #000000;">: city  Wildcard: CountryCode
</span><span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----------+---------+-------------------+------+-----+---------+-------+---------------------------------+---------+</span>
<span style="color: #808080;">|</span> Field       <span style="color: #808080;">|</span> Type    <span style="color: #808080;">|</span> Collation         <span style="color: #808080;">|</span> <span style="color: #0000ff;">Null</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Key</span> <span style="color: #808080;">|</span> <span style="color: #0000ff;">Default</span> <span style="color: #808080;">|</span> Extra <span style="color: #808080;">|</span> <span style="color: #0000ff;">Privileges</span>                      <span style="color: #808080;">|</span> Comment <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----------+---------+-------------------+------+-----+---------+-------+---------------------------------+---------+</span>
<span style="color: #808080;">|</span> CountryCode <span style="color: #808080;">|</span> <span style="color: #0000ff;">char</span>(<span style="color: #800000; font-weight: bold;">3</span>) <span style="color: #808080;">|</span> latin1_swedish_ci <span style="color: #808080;">|</span> NO   <span style="color: #808080;">|</span> MUL <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>       <span style="color: #808080;">|</span> <span style="color: #0000ff;">select</span>,<span style="color: #0000ff;">insert</span>,<span style="color: #0000ff;">update</span>,<span style="color: #0000ff;">references</span> <span style="color: #808080;">|</span>         <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">-----------+---------+-------------------+------+-----+---------+-------+---------------------------------+---------+</span></pre>
        </div>
      </div>
      
      <p>
        查看数据库类似like。
      </p>
      
      <div>
        <div class="cnblogs_code">
          <pre><span style="color: #ff0000;">[</span><span style="color: #ff0000;">root@db02 ~</span><span style="color: #ff0000;">]</span>#  mysqlshow <span style="color: #808080;">-</span>uroot <span style="color: #808080;">-</span>p123 "w<span style="color: #808080;">%</span><span style="color: #000000;">"
Warning: Using a password </span><span style="color: #0000ff;">on</span><span style="color: #000000;"> the command line interface can be insecure.
Wildcard: w</span><span style="color: #808080;">%</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------+</span>
<span style="color: #808080;">|</span> Databases <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------+</span>
<span style="color: #808080;">|</span> world     <span style="color: #808080;">|</span>
<span style="color: #808080;">+</span><span style="color: #008080;">--</span><span style="color: #008080;">---------+</span></pre>
        </div>
      </div>
      
      <h2>
        <span id="17">1.7 参考文献</span>
      </h2>
      
      <div>
        <div class="cnblogs_code">
          <pre>https:<span style="color: #808080;">//</span>dev.mysql.com<span style="color: #808080;">/</span>doc<span style="color: #808080;">/</span>refman<span style="color: #808080;">/</span><span style="color: #800000; font-weight: bold;">5.6</span><span style="color: #808080;">/</span>en<span style="color: #808080;">/</span><span style="color: #0000ff;">select</span>.html         SELECT语法官方说明</pre>
        </div>
        
        <p>
          &nbsp;
        </p>
      </div>
      
      <p>
        &nbsp;
      </p>
      
      <div id="toc_container" class="toc_white have_bullets">
        <ul class="toc_list">
          <li>
            <a href="#11_SELECT">1.1 SELECT高级应用</a><ul>
              <li>
                <a href="#111">1.1.1 前期准备工作</a>
              </li>
              <li>
                <a href="#112_select">1.1.2 select语法格式说明</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#12_selectwhere">1.2 select中where子句使用</a>
          </li>
          <li>
            <a href="#13_selectORDER_BY">1.3 select中ORDER BY子句</a><ul>
              <li>
                <a href="#131_order_by">1.3.1 order by 子句的作用</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#14_LIMIT">1.4 LIMIT子句</a>
          </li>
          <li>
            <a href="#15">1.5 多表连接查询</a><ul>
              <li>
                <a href="#151_where">1.5.1 传统的连接写法（使用where）</a>
              </li>
              <li>
                <a href="#152_NATURALnbsp_JOIN">1.5.2 NATURAL&nbsp; JOIN子句</a>
              </li>
              <li>
                <a href="#153_using">1.5.3 使用using子句</a>
              </li>
              <li>
                <a href="#154">1.5.4 集合操作</a>
              </li>
              <li>
                <a href="#155">1.5.5 分组操作及分组处理</a>
              </li>
              <li>
                <a href="#156_select">1.5.6 【select高级应用】数据库备份脚本拼接</a>
              </li>
              <li>
                <a href="#157">1.5.7 子查询</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#16_Informatica_schema">1.6 Informatica_schema获取元数据</a><ul>
              <li>
                <a href="#161">1.6.1 元数据访问方法</a>
              </li>
              <li>
                <a href="#162_INFORMATION_SCHEMA_SELECT">1.6.2 对 INFORMATION_SCHEMA 使用 SELECT</a>
              </li>
              <li>
                <a href="#163_mysql_SQL">1.6.3 使用 mysql 命令创建 SQL 语句。</a>
              </li>
              <li>
                <a href="#164_MySQLshow">1.6.4 MySQL中的show语句</a>
              </li>
              <li>
                <a href="#165_DESCRIBE">1.6.5 DESCRIBE 语句</a>
              </li>
            </ul>
          </li>
          
          <li>
            <a href="#17">1.7 参考文献</a>
          </li>
        </ul>
      </div>