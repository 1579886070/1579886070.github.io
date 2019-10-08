---
title: '[JS]语句结构'
date: 2019-10-07 21:50:25
categories: JS
tags: js
---

讲讲JavaScript中的语句结构,接下来针对各语句举例说明，跟其他程序语言思想基本一致。<!--more-->

语句：
<blockquote><span style="font-size: 10pt;">1，顺序结构。</span>
<span style="font-size: 10pt;"> 2，判断结构。</span>
<p style="padding-left: 30px;"><span style="font-size: 10pt;">代表语句：if
else if</span></p>
<span style="font-size: 10pt;">3，选择结构。</span>
<p style="padding-left: 30px;"><span style="font-size: 10pt;">switch</span></p>
<span style="font-size: 10pt;">4，循环结构。</span>
<p style="padding-left: 30px;"><span style="font-size: 10pt;">while, do while, for</span></p>
<span style="font-size: 10pt;">5，其他语句。</span>
<p style="padding-left: 30px;"><span style="font-size: 10pt;">break:跳出选择,跳出循环</span></p>
<p style="padding-left: 30px;"><span style="font-size: 10pt;">continue:用于循环语句，结束本次循环继续下次循环</span></p>
</blockquote>
&nbsp;

说明：

<strong> 1，顺序结构</strong>
<pre class="lang:c decode:true"><span style="font-size: 10pt;">alert("1"); 
alert("2"); 
alert("3");</span></pre>
按照顺序从上往下执行。

<hr />

<strong>2,判断结构</strong>
<pre class="lang:c decode:true"><span style="font-size: 10pt;">var x = 5;
if (2==x) {		//建议将常量放左边。这样出现错误会报错
	alert("yes");
}
else{
	alert("no");
}</span></pre>
<strong>else if</strong>
<pre class="lang:c decode:true"><span style="font-size: 10pt;">if(x&gt;1)
	alert("a");
else if(x&gt;2)
	alert("b");
else if(x&gt;5)
	alert("c");
else
	alert("d");</span></pre>

<hr />

<strong>3，选择结构 (都是先执行第一个case)</strong>
<pre class="lang:c decode:true">var x = "abc";
switch(x){
	case "aa":
		alert("a");
		break;
	case "abc":
		alert("b");
		break;
	default:
		alert("c");
		break;
}</pre>

<hr />

<strong>4，循环结构(这里do while就不举例了)</strong>
<pre class="lang:c decode:true"><code class="javascript">var x = 1;
document.write("&lt;font color='blue'&gt;");
while(x&lt;10){
	document.write("x="+x+"&lt;br/&gt;");//将数据直接写到当前得页面当中。
	x++;
}
document.write("&lt;/font&gt;");</code></pre>
<strong>for循环</strong>
<pre class="lang:c decode:true">for(var x = 0;x &lt; 3;x++){
	document.write("x="+x);
}</span></pre>

<hr />

<strong>5，其他语句</strong>
<pre class="lang:c decode:true">w:for (var x = 0; x&lt;3; x++) {
	for(var y = 0;y&lt;4;y++){
		document.write("x="+x);
		continue w;	//跳出当前循环
	}
 }</span></pre>
