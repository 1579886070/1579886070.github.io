---
title: '[Oracle]基本操作-2'
date: 2019-10-09 23:02:51
categories: Sql
tags: Oracle
---

继<span style="text-decoration: underline;"><a href="http://zx21.xyz/2019/10/09/Oracle-%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C-1/">Oracle基本操作-1</a></span>该篇文章的后续与补充！<!--more-->
<h2>select语句</h2>
<h3>1.查询全部列</h3>
<pre class="lang:default decode:true " title="a_table表">select * from a_table</pre>
[caption id="" align="aligncenter" width="296"]<a href="http://image.xiaoxinyes.club/2019-01-04_092517.png"><img class="size-medium" src="http://image.xiaoxinyes.club/2019-01-04_092517.png" width="296" height="74" /></a> a_table[/caption]

&nbsp;
<h3>2.查询特定的列</h3>
<pre class="lang:default decode:true" title="b_table">select b_id,b_name,a_id from b_table</pre>
[caption id="" align="aligncenter" width="224"]<a href="http://image.xiaoxinyes.club/2019-01-04_092912.png"><img class="size-medium" src="http://image.xiaoxinyes.club/2019-01-04_092912.png" width="224" height="77" /></a> b_table[/caption]

&nbsp;
<h3>3.别名</h3>
在查询中，给查询结果的列起一个自定义名称。
<pre class="lang:default decode:true">select a_money as "月薪" from a_table</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_093250.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_093250.png" width="99" height="77" /></a>

&nbsp;
<h3>4.算术运算符</h3>
<table class=" aligncenter" style="height: 157px;" width="177">
<tbody>
<tr>
<td style="width: 80.15px;">运算符</td>
<td style="width: 80.15px;">描述</td>
</tr>
<tr>
<td style="width: 80.15px;">+</td>
<td style="width: 80.15px;">加</td>
</tr>
<tr>
<td style="width: 80.15px;">-</td>
<td style="width: 80.15px;">减</td>
</tr>
<tr>
<td style="width: 80.15px;">*</td>
<td style="width: 80.15px;">乘</td>
</tr>
<tr>
<td style="width: 80.15px;">/</td>
<td style="width: 80.15px;">除</td>
</tr>
</tbody>
</table>
举例：计算年薪
<pre class="lang:default decode:true ">select a_id,a_name,a_age,a_money*12 as "年薪" from a_table</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_093732.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_093732.png" width="275" height="79" /></a>

&nbsp;
<h3>5.字符串连接</h3>
能将指定的列进行连接。
<pre class="lang:default decode:true ">select a_id,a_name ||'-'|| a_money as "姓名与工资" from a_table</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_094122.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_094122.png" width="205" height="81" /></a>

&nbsp;
<h3>6.消除重复行</h3>
先查询下a_table表
<pre class="lang:default decode:true ">select a_money from a_table</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_094651.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_094651.png" width="124" height="74" /></a>

能看到有相同的数额，现在使用关键字<span style="color: #ff0000;">distinct</span>
<pre class="lang:default decode:true ">select distinct a_money from a_table</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_094830.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_094830.png" width="125" height="66" /></a>

&nbsp;
<h3>7.where限定</h3>
我们可以使用where进行过滤。

1).查询工资大于4000的所有信息。
<pre class="lang:default decode:true">select * from a_table where a_money&gt;4000</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_095212.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_095212.png" width="292" height="35" /></a>

&nbsp;

2).查询姓名为“wangwu”的信息。
<pre class="lang:default decode:true ">select * from a_table where a_name='wangwu'</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_095419.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_095419.png" width="298" height="34" /></a>

&nbsp;

除了常用的&gt;、&lt;、&gt;=等等这类比较运算符还包括下列的：
<table class="NormalTable aligncenter" style="width: 239.35px;">
<tbody>
<tr>
<td style="width: 90px;"><span class="fontstyle0">操作符</span></td>
<td style="width: 133.35px;"><span class="fontstyle0">含义</span></td>
</tr>
<tr>
<td style="width: 90px;"><span class="fontstyle1">BETWEEN
...AND...</span></td>
<td style="width: 133.35px;"><span class="fontstyle0">在两个值之间 </span><span class="fontstyle3">(</span><span class="fontstyle0">包含边界</span><span class="fontstyle3">)</span></td>
</tr>
<tr>
<td style="width: 90px;"><span class="fontstyle1">IN(set)</span></td>
<td style="width: 133.35px;"><span class="fontstyle0">等于值列表中的一个</span></td>
</tr>
<tr>
<td style="width: 90px;"><span class="fontstyle1">LIKE</span></td>
<td style="width: 133.35px;"><span class="fontstyle0">模糊查询</span></td>
</tr>
<tr>
<td style="width: 90px;"><span class="fontstyle1">IS NULL</span></td>
<td style="width: 133.35px;"><span class="fontstyle0">空值</span></td>
</tr>
</tbody>
</table>
&nbsp;

3).查询年龄在20-28之间的信息。
<pre class="lang:default decode:true" title="between...and">select * from a_table where a_age between 20 and 28</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_100247.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_100247.png" width="301" height="65" /></a>

&nbsp;

4).查询金额是3400与3500。
<pre class="lang:default decode:true" title="in">select * from a_table where a_money in(3400,3500)</pre>
&nbsp;

5).模糊查询姓名以w开头的人的信息。
<pre class="lang:default decode:true" title="like">select * from a_table where a_name like 'w%'</pre>
&nbsp;

6).模糊查询倒数第二个字母的姓名为o的人的信息。
<pre class="lang:default decode:true" title="like">select * from a_table where a_name like '%o_'</pre>
&nbsp;

7).查询姓名中包含0的所有人的信息。
<pre class="lang:default decode:true " title="like">select * from a_table where a_name like '%o%'</pre>
&nbsp;

8).查询是否为空和不为空。
<pre class="lang:default decode:true ">select * from a_table where a_name is null
select * from a_table where a_name is not null</pre>
&nbsp;
<h3>8.逻辑运算</h3>
<table class="NormalTable">
<tbody>
<tr>
<td width="43"><span class="fontstyle0">操作符</span></td>
<td width="43"><span class="fontstyle0">含义</span></td>
</tr>
<tr>
<td width="91"><span class="fontstyle1">AND</span></td>
<td width="91"><span class="fontstyle0">逻辑并</span></td>
</tr>
<tr>
<td width="86"><span class="fontstyle1">OR</span></td>
<td width="86"><span class="fontstyle0">逻辑或</span></td>
</tr>
<tr>
<td width="82"><span class="fontstyle1">NOT</span></td>
<td width="82"><span class="fontstyle0">逻辑否</span></td>
</tr>
</tbody>
</table>
&nbsp;

1).查询年龄大于20并且工资大于4000的人。
<pre class="lang:default decode:true" title="and">select * from a_table where a_age &gt; 20 and a_money &gt; 4000</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_101628.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_101628.png" width="286" height="30" /></a>

&nbsp;

2).查询年龄为20或者工资大于4000的人。
<pre class="lang:default decode:true ">select * from a_table where a_age = 20 or a_money &gt; 4000</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_101839.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_101839.png" width="288" height="47" /></a>

&nbsp;
<h3>9.排序查询</h3>
<p style="padding-left: 30px;"><span style="color: #ff0000;"> <span class="fontstyle0">使用 </span><span class="fontstyle2">ORDER BY </span><span class="fontstyle0">子句排序
</span><span class="fontstyle3">– </span><span class="fontstyle4">ASC: </span><span class="fontstyle0">升序
</span><span class="fontstyle3">– </span><span class="fontstyle4">DESC: </span><span class="fontstyle0">降序
</span><span class="fontstyle3">• </span><span class="fontstyle2">ORDER BY </span><span class="fontstyle0">子句在</span><span class="fontstyle4">SELECT</span><span class="fontstyle0">语句的结尾</span></span></p>

<pre class="lang:default decode:true ">select * from a_table  order by a_money desc</pre>
&nbsp;
<h3>10.等值连接</h3>
可以从多张表中取出数据，但是表中必须要有所关联。
<pre class="lang:default decode:true ">select * from b_table b,a_table a where b.a_id=a.a_id</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_103033.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_103033.png" width="470" height="77" /></a>

&nbsp;
<h3>11.子查询</h3>
查询谁的工资高于wangwu。
<pre class="lang:default decode:true ">select a_name from a_table where a_money&gt;(
       select a_money  from a_table where a_name='wangwu' )</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_103655.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_103655.png" width="112" height="31" /></a>

<hr />

&nbsp;
<h2>分组函数</h2>
<h3>1.统计函数</h3>
<p style="padding-left: 30px;"><span style="color: #ff0000;"> <span class="fontstyle0">• </span><span class="fontstyle2">AVG
</span><span class="fontstyle0">• </span><span class="fontstyle2">COUNT
</span><span class="fontstyle0">• </span><span class="fontstyle2">MAX
</span><span class="fontstyle0">• </span><span class="fontstyle2">MIN
</span><span class="fontstyle0">• </span><span class="fontstyle2">STDDEV
</span><span class="fontstyle0">• </span><span class="fontstyle2">SUM</span></span></p>

<pre class="lang:default decode:true ">select max(a_money),min(a_money),avg(a_money),sum(a_money) from a_table</pre>
&nbsp;

查询年龄大于20的人数。(<span style="color: #ff0000;"> <span class="fontstyle0">COUNT(DISTINCT expr) </span><span class="fontstyle2">返回 </span><span class="fontstyle3">expr</span><span class="fontstyle2">非空且不重复的记录总数</span></span> )
<pre class="lang:default decode:true ">select count(*) from a_table where a_age&gt;20</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_104441.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_104441.png" width="124" height="34" /></a>

&nbsp;
<h3>2.分组查询</h3>
<pre class="lang:default decode:true " title="group by">select a_id,avg(a_money) from a_table group by a_id</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_105327.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_105327.png" width="197" height="77" /></a>

&nbsp;
<h3>3.过滤分组</h3>
<span class="fontstyle0" style="color: #ff0000;">having</span>
<pre class="lang:default decode:true " title="having">select a_id,max(a_money) from a_table group by a_id
       having max(a_money) &gt;= 3500</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_105718.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_105718.png" width="202" height="66" /></a>

<hr />

&nbsp;
<h2>视图</h2>
<h3>1.创建视图</h3>
<pre class="lang:default decode:true">create view a_view
as(
   select a.a_id,a.a_name,a.a_age,a.a_money from a_table a
)</pre>
&nbsp;
<h3>2.查询视图</h3>
<pre class="lang:default decode:true ">select * from a_view</pre>
<a href="http://image.xiaoxinyes.club/2019-01-04_110303.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2019-01-04_110303.png" width="293" height="76" /></a>

&nbsp;
<h3>3.删除视图</h3>
<pre class="lang:default decode:true ">drop view a_view</pre>
&nbsp;
