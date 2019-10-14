---
title: '[JAVA]谈谈重定向与转发'
date: 2019-10-14 19:15:24
categories: JAVA
tags: java
---


转发与重定向都能指定到另外一个URl。

## 重定向

<pre class="lang:java decode:true">response.sendRedirect(request.getContextPath()+"/redirect.jsp");</pre>
&nbsp;

## 转发

<pre class="lang:java decode:true">request.getRequestDispatcher(request.getContextPath()+"/forwarding.jsp").forward(request,response);</pre>
&nbsp;

request.getContextPath()获得的是web项目的根路径。

* * *

&nbsp;

&nbsp;

演示说明：

web.xml配置的是&lt;url-pattern&gt;/aaa&lt;/url-pattern&gt;。
为了区分明显，项目中有一个redirect.jsp(表示重定向)和forwarding.jsp(表示转发),接下来通过动图分别查看路径得变化情况。

&nbsp;

**重定向：**

[![](http://image.xiaoxinyes.club/redirect.gif)](http://image.xiaoxinyes.club/redirect.gif)

&nbsp;

&nbsp;

**转发：**

[![](http://image.xiaoxinyes.club/forwarding.gif)](http://image.xiaoxinyes.club/forwarding.gif)

&nbsp;

可以很清楚的看到**重定向的地址发生变化而转发的未改变**。

接下来看一张简单的图

[![](http://image.xiaoxinyes.club/2018-08-10_1.png)](http://image.xiaoxinyes.club/2018-08-10_1.png)

&nbsp;

## 总结

### <span style="color: #ff0000;">转发与重定向的区别：</span>

*   **重定向是两次请求，转发只有一次请求;**
*   **重新定向可以访问外部网站 转发只能访问内部资源;**
*   **重定向的地址会发生改变变化，转发地址不变;**
*   **转发性能高于重定向。**
&nbsp;

我们可以抽象理解：

**重定向**：你问我借钱，我没钱，我告诉你谁有钱你去找他。

**转发**：你问我借钱，我没钱，我去帮你借给你。