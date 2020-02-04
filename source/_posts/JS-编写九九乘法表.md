---
title: '[JS]编写九九乘法表'
date: 2019-10-07 21:47:25
categories: Web
tags: js
---

学过其他语言的应该知道初级阶段写九九乘法表，下面用JavaScript实现。<!--more-->

代码如下
<pre class="lang:javascript decode:true">&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
	&lt;meta charset="UTF-8"&gt;
	&lt;title&gt;Document&lt;/title&gt;
	&lt;link rel="stylesheet" type="text/css" href="table.css"&gt;
&lt;/head&gt;
&lt;body&gt;
	&lt;script type="text/javascript"&gt;
		/*
		练习：在页面显示一个99乘法表
		*/
		document.write("&lt;table&gt;");
		for(var i = 1;i &lt;= 9;i++){
			document.write("&lt;tr&gt;");
			for(var j = 1;j &lt;= i;j++){
				document.write("&lt;td&gt;"+j+"*"+i+"="+j*i+"&lt;/td&gt;");
			}
			document.write("&lt;/tr&gt;");
		}
		document.write("&lt;/table&gt;");
	&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>

* * *

为了让显示得好看些，导入css,增加了边框效果。

代码如下
<pre class="lang:javascript decode:true ">table ,table td{
	border: solid #1690EE 1px;
	width: 600px;
}</pre>

* * *

效果如图

![爱生活爱语录](http://image.xiaoxinyes.club/js_4_22_2.png)