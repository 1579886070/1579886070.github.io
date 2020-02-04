---
title: '[JS]运算符和变量定义'
date: 2019-10-07 21:35:25
categories: Web
tags: js
---

讲讲JavaScript中的变量定义和各种运算符。<!--more-->

jsz中定义变量用的关键字是：**var**
** 定义变量。说下js是弱类型的。**
<pre class="lang:c decode:true">var x = 4; //var不写也行，因为js是非严谨的语言，但建议严谨定义。
 x = "abc"; //重新赋值为字符串abc。
 x = 3.1415926; //赋值为小数，数字类型。
 x = true; //赋值为boolean类型。
 x = 'c'; //赋值为字符串。
 alert("x="+x); //这是一个函数，将具体的参数通过对话框进行显示。</pre>
&nbsp;

* * *

接下来讲讲运算符。实际运算符跟我们学过得设计语言基本大同小异，也许只是表现形式不相同，但思想基本一致。

运算符：
> <span style="font-size: 10pt;">1，算术运算符。</span>
> 
> <span style="font-size: 10pt;">+ - * / % ++ --</span>
> 
> <span style="font-size: 10pt;">2，赋值运算符。</span>
> 
> <span style="font-size: 10pt;">= += -= /= *= %=</span>
> 
> <span style="font-size: 10pt;">3，比较运算符。</span>
> 
> <span style="font-size: 10pt;">&gt; &lt; &gt;= &lt;= == != (运算完的结果false或者true)</span>
> 
> <span style="font-size: 10pt;">4，逻辑运算符。</span>
> 
> <span style="font-size: 10pt;">&amp;&amp; || ! (用来连接两个布尔型的表达式)</span>
> 
> <span style="font-size: 10pt;">5，位运算符。</span>
> 
> <span style="font-size: 10pt;">&amp; | ^ &gt;&gt; &lt;&lt; &gt;&gt;&gt; ~</span>
> 
> <span style="font-size: 10pt;">6，三元运算符。</span>
> 
> <span style="font-size: 10pt;">? :</span>
接下来，分别对这些运算符举例：
**<span style="font-size: 10pt;">1，算数运算符演示。</span>**
<pre class="lang:c decode:true"> var a = 5600;
 alert("a="+a/100*100); //a=5600;
 var a1 = 3.4,b1 = 6.6;
 alert("a1+b1="+(a1+b1)); //a1+b1=10;

 alert("30"+1); //301;
 alert("30"-1); //29；
 alert(true+1); //2 //因为在js中false为0，或者null。非0，非null。为true，默认是1；</pre>

* * *

**2，赋值运算符**
<pre class="lang:c decode:true">var i = 3;
 i+=2;
 alert(i);
</pre>

* * *

**<span style="font-size: 10pt;">3，比较运算符</span>**
<pre class="lang:c decode:true"> var z = 10;
 alert(z!=5);</pre>

* * *

**<span style="font-size: 10pt;">4，逻辑运算符</span>**
<pre class="lang:c decode:true">var t = 5;
alert(t&gt;1 &amp;&amp; t&lt;10);
alert(!true);</pre>

* * *

**<span style="font-size: 10pt;">5，位运算符</span>**
<pre class="lang:c decode:true">var c = 6;
alert(c&amp;3); ///2
alert(5^3^3); //5
alert(c&gt;&gt;&gt;1); //6/2(1);
alert(c&lt;&lt;2); //24</pre>

* * *

**<span style="font-size: 10pt;">6，三元运算符</span>**
<pre class="lang:c decode:true"> alert(3&gt;100?100:200);
</pre>

* * *

**其他小知识点：**
**undefined:未定义。**
<pre class="lang:c decode:true">var xx;
alert(xx); //undefined
alert(xx==undefined);
</pre>
**想要获取具体值得类型。可以通过typeof来完成。**
<pre class="lang:c decode:true">alert(typeof(2.5)); //number
alert(typeof(true)); //boolean
alert(typeof(78)); //number
alert(typeof('9')); //string</pre>