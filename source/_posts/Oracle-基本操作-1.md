---
title: '[Oracle]基本操作-1'
date: 2019-10-09 23:01:46
categories: Sql
tags: Oracle
---

此篇文章包含sql语句增删改查，表结构的更改等，其他操作将在下篇数据库的文章列出。<!--more-->
<h2>管理表</h2>
<h3>1.创建表</h3>
<h4>命名规则</h4>
<p style="padding-left: 30px;"><span style="color: #ff0000;">表名和列名:</span>
<span style="color: #ff0000;">• 必须以字母开头</span>
<span style="color: #ff0000;">• 必须在 1–30 个字符之间</span>
<span style="color: #ff0000;">• 必须只能包含 A–Z, a–z, 0–9, _, $, 和 #</span>
<span style="color: #ff0000;">• 必须不能和用户定义的其他对象重名</span>
<span style="color: #ff0000;">• 必须 不能是Oracle 的保留字</span></p>

<pre class="lang:default decode:true" title="a_table">create table a_table(
       a_id int not null primary key,
       a_name varchar2(20),
       a_age int,
       a_money varchar2(30)
)</pre>
&nbsp;
<h3>2.添加新列add</h3>
<pre class="lang:default decode:true ">alter table a_table add(sex varchar2(2))</pre>
&nbsp;
<h3>3.修改列modify</h3>
<pre class="lang:default decode:true ">alter table a_table  modify(a_money number)</pre>
&nbsp;
<h3>4.删除列drop column</h3>
<pre class="lang:default decode:true">alter table a_table drop column sex</pre>
&nbsp;
<h3>5.删除表</h3>
<pre class="lang:default decode:true ">drop table a_table</pre>
&nbsp;
<h3>6.清空表</h3>
<p style="padding-left: 30px;"><span style="color: #ff0000;">• TRUNCATE TABLE </span>
<span style="color: #ff0000;">– 删除表中所有的数据</span>
<span style="color: #ff0000;">– 释放表的存储空间</span>
<span style="color: #ff0000;">• TRUNCATE语句不能回滚</span>
<span style="color: #ff0000;">• 可以使用 DELETE 语句删除数据</span></p>

<pre class="lang:default decode:true ">truncate  table a_table</pre>

<hr />

&nbsp;
<h2>关于约束</h2>
<p style="padding-left: 30px;"><span style="color: #ff0000;">– NOT NULL，不为空</span>
<span style="color: #ff0000;">– UNIQUE，唯一不可重复</span>
<span style="color: #ff0000;">– PRIMARY KEY，主键</span>
<span style="color: #ff0000;">– FOREIGN KEY，外键</span>
<span style="color: #ff0000;">– CHECK，添加条件</span></p>
<p style="padding-left: 30px;"><span class="fontstyle0">创建约束</span><span class="fontstyle1">
</span><span class="fontstyle2">– </span><span class="fontstyle0">建表的同时
</span><span class="fontstyle2">– </span><span class="fontstyle0">建表之后
</span><span class="fontstyle0">表级或列级定义约束
</span></p>

<h3>1.not null</h3>
<pre class="lang:default decode:true ">a_id int not null</pre>
&nbsp;
<h3>2.UNIQUE</h3>
<pre class="lang:default decode:true ">a_name varchar2(20) unique</pre>
或者
<pre class="lang:default decode:true ">create table a_table(
       a_id int not null primary key,
       a_name varchar2(20),
       a_age int,
       a_money varchar2(30),
       constraint a_name_uk unique(a_name)
)
</pre>
&nbsp;
<h3>3.PRIMARY KEY</h3>
<pre class="lang:default decode:true">a_id int primary key</pre>
或者
<pre class="lang:default decode:true ">create table a_table(
       a_id int not null,
       a_name varchar2(20),
       a_age int,
       a_money varchar2(30),
       constraint a_name_uk unique(a_name),
       constraint a_id_pk_ primary key(a_id)
)</pre>
&nbsp;
<h3>4.FOREIGN KEY</h3>
外键来自另一张表进行关联。插入数据时，需要先插入不是外键表的那张表。删除表时，需要先删除外键表。
<pre class="lang:default decode:true">create table a_table(
       a_id int not null,
       a_name varchar2(20),
       a_age int,
       a_money number,
       constraint a_name_uk unique(a_name),
       constraint a_id_pk_ primary key(a_id)
)
create table b_table(
       b_id int not null,
       b_name varchar2(20),
       a_id int,
       constraint b_id_fk foreign key(a_id) references a_table(a_id)
)
</pre>
&nbsp;
<h3>5.CHECK</h3>
<pre class="lang:default decode:true ">create table a_table(
       a_id int not null,
       a_name varchar2(20),
       a_age int,
       a_money number(2),
       constraint a_money_min check (a_money&gt;0)
)</pre>
&nbsp;
<h3>6.添加约束</h3>
<pre class="lang:default decode:true ">alter table a_table add constraint a_name_uk unique(a_name)</pre>
<pre class="lang:default decode:true ">alter table a_table add constraint a_id_fk primary key(a_id)</pre>
<pre class="lang:default decode:true ">alter table b_table add constraint b_id_fk foreign key(a_id) references a_table(a_id)</pre>
&nbsp;
<h3>7.删除约束</h3>
<pre class="lang:default decode:true ">alter table a_table drop constraint a_name_uk</pre>

<hr />

<h2></h2>
<h2>数据的增删改查</h2>
<p style="padding-left: 30px;"><span style="color: #ff0000;">insert</span>
<span style="color: #ff0000;">delete</span>
<span style="color: #ff0000;">update</span>
<span style="color: #ff0000;">select</span></p>

<h3>1.插入数据</h3>
<pre class="lang:default decode:true ">insert into a_table values(1,'xiaoxin',22,3)</pre>
&nbsp;
<h3>2.删除数据</h3>
如果省略where，将删除表中所有数据
<pre class="lang:default decode:true ">delete from a_table where a_name='xiaoxin'</pre>
<pre class="lang:default decode:true ">delete from a_table</pre>
&nbsp;
<h3>3.更新数据</h3>
如果省略where，将更新表中所有数据
<pre class="lang:default decode:true ">update  a_table set a_name='zhaoliu' where a_id=2</pre>
<pre class="lang:default decode:true ">update  a_table set a_name='zhaoliu'</pre>
&nbsp;
<h3>4.查询数据</h3>
查询所有数据
<pre class="lang:default decode:true ">select * from a_table</pre>
查询指定的数据
<pre class="lang:default decode:true ">select a_name from a_table</pre>
&nbsp;

&nbsp;

<span style="color: #808080;">由于查询单独涉及的内容比较多，下次会将逻辑运算，常用的函数和查询以及视图总结下。</span>
