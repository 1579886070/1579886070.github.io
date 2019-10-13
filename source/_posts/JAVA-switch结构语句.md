---
title: '[JAVA]switch结构语句'
date: 2019-10-13 22:51:26
categories: JAVA
tags: java
---

讲过if语句，接下来讲讲switch，它也是常用的选择语句，作用于对一个表达式进行判断，来决定执行什么代码。
**来看下语句的基本格式**

[![](http://image.xiaoxinyes.club/java_3_15_1.png)](http://image.xiaoxinyes.club/java_3_15_1.png)

在上面的格式中,switch中的表达式的值会和每个case的值进行匹配，如果值相同，则会执行对应的case后的执行语句若都没有找到，则会执行default后的语句。break在这里起到的跳出switch语句的作用。

&nbsp;

**接下来做一个练习更加了解switch语句**

[![](http://image.xiaoxinyes.club/java_3_15_2.png)](http://image.xiaoxinyes.club/java_3_15_2.png)

分析：定义了一个变量n值为3，将3传入swithc()中，代码会先执行第一条case语句，看是否值与定义的3相等，不相等的话执行下一条，假如都不匹配，最后执行default后的代码。我定义的值是3，所以会匹配case 3后面的语句，打印”星期三”。

&nbsp;

**接下来看switch中另一种情况，如图**

[![](http://image.xiaoxinyes.club/java_3_15_3.png)](http://image.xiaoxinyes.club/java_3_15_3.png)

这种情况属于当n值为1.2.3.4.5中的任意一个值得时候，都打印”上学”，当n为6.7任意值都打印”休息”。

&nbsp;

<span style="color: #999999; font-size: 8pt;">内容声明：博主原创</span>
