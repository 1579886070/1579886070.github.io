---
title: '[JS]Js和Html相结合方式'
date: 2019-10-07 21:24:25
categories: JS
tags: js
---

### js和html相结合的方式
1，将javascript代码封装到&lt;script&gt;标签中。

2，将javascript代码封装到js文件中，并且通过&lt;script&gt;中的src属性进行导入。

注意：如果&lt;script&gt;标签中使用src属性，那么改标签中的javascript代码不会被执行。
所以通常导入js文件都是用单独&lt;script&gt;来完成。
<pre class="lang:c decode:true"> &lt;!--导入一个js文件，使用src导入--&gt;
 &lt;script type="text/javascript" src="demo.js"&gt;&lt;/script&gt;
</pre>
&nbsp;

* * *

<pre class="lang:c decode:true"> &lt;!--封装javascript代码--&gt;
 &lt;script type="text/javascript"&gt;
 alert("hello javascript");</pre>
[![周信的个人博客](http://image.xiaoxinyes.club/js_jiehe.png)](http://image.xiaoxinyes.club/js_jiehe.png)
