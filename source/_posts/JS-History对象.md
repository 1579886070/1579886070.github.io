---
title: '[JS]History对象'
date: 2019-10-07 21:53:25
categories: Web
tags: js
---

History 对象用来记录用户曾浏览的历史记录，类似于浏览器中的后退前进功能。

<pre class="lang:js decode:true">&lt;%@ page contentType="text/html;charset=UTF-8" language="java" %&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;History &lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;script&gt;
    function goBack()
    {
        history.back();
    }

    function goBack2()
    {
        history.go(-2);
    }

    function goForward()
    {
        history.forward();
    }
&lt;/script&gt;

&lt;button onclick="goBack()"&gt;返回&lt;/button&gt;
&lt;button onclick="goBack2()"&gt;返回上上次&lt;/button&gt;
&lt;button onclick="goForward()"&gt;前进&lt;/button&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
&nbsp;

history.back();返回上一次的访问历史；

history.go(-2);根据数字不同，返回次数不同，-1返回上次，-2上上次，以此类推。

history.forward();模拟浏览器前进按钮。
