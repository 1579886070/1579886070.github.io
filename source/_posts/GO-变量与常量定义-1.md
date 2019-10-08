---
title: '[GO]变量与常量定义-1'
date: 2019-10-08 22:31:26
categories: Go
tags: Golang
---

使用的关键字：var,跟常规的编程语言不同，golang定义变量时变量名在前变量类型在后。<!--more-->并且定义的变量必须使用。
<h3>定义变量</h3>
<pre class="lang:go decode:true">var a int
var b string</pre>

<hr />

&nbsp;
<h3>定义变量并赋初值</h3>
<pre class="lang:go decode:true">var a int = 3
var b string = "abc"</pre>

<hr />

&nbsp;
<h3>同时赋值多个</h3>
<pre class="lang:go decode:true">var a,b string = "hehe","haha"
</pre>

<hr />

&nbsp;
<h3>直接省略定义的类型，编译器自己识别</h3>
<pre class="lang:go decode:true">var a = 6
var c,d,e = true,"pupu",10</pre>

<hr />

&nbsp;
<h3>使用var集中变量</h3>
<pre class="lang:go decode:true">var(
	bb = 3
	cc=true
)</pre>

<hr />

&nbsp;
<h3>使用 :=定义变量(只能在函数内部使用)</h3>
<pre class="lang:go decode:true ">a, b, c, d := 3, 4, true, "heihei"</pre>

<hr />

&nbsp;

&nbsp;
<h3>常量定义的关键字 const</h3>
<pre class="lang:default decode:true ">const a = "a.text"
const b int = 5</pre>

<hr />

&nbsp;
<h3>枚举类型定义</h3>
<pre class="lang:default decode:true">const(
	app = 0
	java = 1
	c = 2
)</pre>
<h3>同样的go为这种提供了简化。iota表示自增</h3>
<pre class="lang:default decode:true ">const(
	app = iota
	java
	c
)</pre>
&nbsp;
