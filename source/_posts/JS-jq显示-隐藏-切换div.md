---
title: '[JS]jq显示/隐藏/切换div'
date: 2019-10-08 22:28:40
categories: Web
tags: js
---

Jquery是一个javascript的框架，操作起来十分方便。<!--more-->

<strong>show：显示</strong>
<strong>hide：隐藏</strong>
<strong>toggle：切换</strong>

&nbsp;
<pre class="lang:js decode:true ">&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
	&lt;meta charset="UTF-8"&gt;
	&lt;title&gt;Document&lt;/title&gt;
	&lt;script src="jquery.min.js"&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;script type="text/javascript"&gt;
	$(function(){
		$("#a1").click(function(){
			$("#d").hide();
		});
		$("#a2").click(function(){
			$("#d").show();
		});
		$("#a3").click(function(){
			$("#d").toggle();
		});
	});
&lt;/script&gt;
&lt;body&gt;
	&lt;input type="button" id="a1" value="隐藏"&gt;
	&lt;input type="button" id="a2" value="显示"&gt;
	&lt;input type="button" id="a3" value="切换"&gt;
	&lt;div id="d"&gt;
		我出来啦！！！
	&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
效果

<a href="http://image.xiaoxinyes.club/2018-9-1.gif"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2018-9-1.gif" width="598" height="107" /></a>

<hr />

&nbsp;

也可以加上<strong>毫秒</strong>值，这些显示出来会有<strong>延迟</strong>效果。
<pre class="lang:js decode:true ">&lt;script type="text/javascript"&gt;
	$(function(){
		$("#a1").click(function(){
			$("#d").hide(1000);
		});
		$("#a2").click(function(){
			$("#d").show(1000);
		});
		$("#a3").click(function(){
			$("#d").toggle(1000);
		});
	});
&lt;/script&gt;</pre>
效果

<a href="http://image.xiaoxinyes.club/2018-9-1-1.gif"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2018-9-1-1.gif" width="598" height="107" /></a>
