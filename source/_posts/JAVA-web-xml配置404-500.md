---
title: '[JAVA]web.xml配置404,500'
date: 2019-10-21 19:14:56
categories: JAVA
tags: java
---

javaWeb项目中，配置404，500，简简单单几句话就解决了。<!--more-->

**web.xml**
<pre class="lang:default decode:true ">&lt;error-page&gt;
    &lt;error-code&gt;404&lt;/error-code&gt;
    &lt;location&gt;/404.jsp&lt;/location&gt;
&lt;/error-page&gt;

&lt;error-page&gt;
    &lt;error-code&gt;500&lt;/error-code&gt;
    &lt;location&gt;/505.jsp&lt;/location&gt;
&lt;/error-page&gt;</pre>
&nbsp;

同时我的404.jsp直接接入腾讯公益的404页面。

腾讯404公益平台:[http://www.qq.com/404/](http://www.qq.com/404/)。

&nbsp;

**404.jsp**
<pre class="lang:default decode:true">&lt;%--
  Created by IntelliJ IDEA.
  User: 小信
  Date: 2018/9/10
  Time: 15:12
  To change this template use File | Settings | File Templates.
--%&gt;
&lt;%@ page contentType="text/html;charset=UTF-8" language="java" isErrorPage="true"%&gt;
&lt;html&gt;
&lt;script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" charset="utf-8"
        homePageUrl="/index.jsp" homePageName="重新回到主页"&gt;

&lt;/script&gt;
&lt;head&gt;
    &lt;title&gt;发生了错误&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;/body&gt;
&lt;/html&gt;
</pre>
&nbsp;
