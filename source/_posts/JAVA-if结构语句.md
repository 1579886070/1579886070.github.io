---
title: '[JAVA]if结构语句'
date: 2019-10-11 20:36:02
categories: JAVA
tags: java
---

我们经常遇到某种事需要做出判断，像过马路需要对路况进行判断，红灯停绿灯行。而在java中<!--more-->，可以用选择语句来作出判断。

选择语句有if语句与switch语句，现在对if语句进行说明。

### **1. if条件语句**

if语句是指如果满足某条件就执行什么动作。语法格式如下：
![](http://image.xiaoxinyes.club/java_3_14.png)
如上图的三段伪代码所示，分解理解。“如果”相当于java关键字中的”if”。获取的值是一个布尔值，非真即假。分析上图可知。当前方是红灯的时候，接下来执行"我就停下来"的操作。
接下来简单看下if语句的用法

[![](http://image.xiaoxinyes.club/java_3_14_2.png)](http://image.xiaoxinyes.club/java_3_14_2.png)

如上图所示，给n变量赋值了10，其运行思路为：如果10大于5，那么打印，显而易见结果是"这是一个大于5的数呢"!

&nbsp;

### **2. if…else语句条件语句**

if…else语句如果满足某种条件，就进行某种处理，否则进行另一种处理。，语法格式如下：
[![](http://image.xiaoxinyes.club/java_3_14__3_1.png)](http://image.xiaoxinyes.club/java_3_14__3_1.png)

通过一个实例来了解

[![](http://image.xiaoxinyes.club/java_3_14_3.png)](http://image.xiaoxinyes.club/java_3_14_3.png)

上面代码中，实现的是判断一个数是偶数还是奇数。思路是：定义一个数为10，假如10除以2的余数正好等于0，那么打印”这个数是偶数”，否则就是10除以2不等于0的情况(else相当于否则)，那么打印”这个数是奇数”。


&nbsp;

### **3. if…else if…else条件语句**

if…else if…else是在多个选择中进行多种不同的处理。语法格式如下：

[![](http://image.xiaoxinyes.club/java_3_14_3_2.png)](http://image.xiaoxinyes.club/java_3_14_3_2.png)
通过一个实例来了解if…else if…else的具体用法

[![](http://image.xiaoxinyes.club/java_3_14_4.png)](http://image.xiaoxinyes.club/java_3_14_4.png)

其结果如图。
定义成绩如果这个成绩大于90，则输入”成绩为优！”，不然会执行下面的else if看是否大于60，假如也不是，则最后输出”成绩为差”。(定义的变量值为75，所以成绩为良)

