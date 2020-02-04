---
title: '[JS]实现简单的计算器'
date: 2019-10-07 21:49:25
categories: Web
tags: js
---

通过获取两个文本的值，判断选择的运算符，根据运算符的不同，分别进行计算。<!--more-->
<pre class="lang:js decode:true">&lt;%--
  Created by IntelliJ IDEA.
  User: 小信
  Date: 2018/8/25
  Time: 18:41
  To change this template use File | Settings | File Templates.
--%&gt;
&lt;%@ page contentType="text/html;charset=UTF-8" language="java" %&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;$Title$&lt;/title&gt;
&lt;/head&gt;
&lt;style&gt;
    input{
        font-size:1.4em;
        width: 80px;
        border-radius:4px;
        border:1px solid #c8cccf;
        color:#6a6f77;
    }
&lt;/style&gt;
&lt;script type="text/javascript"&gt;
    function add() {
        var sum;
        var num1 = parseInt(document.getElementById("num1").value);
        var num2 = parseInt(document.getElementById("num2").value);
        var symbol = document.getElementById("symbol").value;
        switch (symbol) {
            case '+':
                sum = num1 + num2;
                break;
            case '-':
                sum = num1 - num2;
                break;
            case '*':
                sum = num1 * num2;
                break;
            case '/':
                sum = num1 / num2;
                break;
        }
        document.getElementById("num3").value = sum;
    }
&lt;/script&gt;
&lt;body&gt;
&lt;form&gt;
    &lt;input type="text" id="num1" value=""&gt;
    &lt;select id="symbol"&gt;
        &lt;option value="+"&gt;+&lt;/option&gt;
        &lt;option value="-"&gt;-&lt;/option&gt;
        &lt;option value="*"&gt;*&lt;/option&gt;
        &lt;option value="/"&gt;/&lt;/option&gt;
    &lt;/select&gt;
    &lt;input type="text" id="num2" value=""&gt;=
    &lt;input type="text" id="num3" value=""&gt;
    &lt;input type="button" value="计算" onclick="add()"&gt;
    &lt;input type="reset" name="ret" value="清空"&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>
&nbsp;

样式如下

[![](http://image.xiaoxinyes.club/2018-8-30.gif)](http://image.xiaoxinyes.club/2018-8-30.gif)
