---
title: '[GO]条件与循环语句-2'
date: 2019-10-08 22:39:14
categories: Go
tags: Golang
---

go的条件语句和循环语句，是不需要括号括起来的。<!--more-->
<h3>if</h3>
<pre class="lang:go decode:true ">func main(){
	num := 0
	if num &gt; 10{
		fmt.Println(10)
	}else {
		fmt.Println(0)
	}
}</pre>
&nbsp;
<h3>同样的运算，我们也可以写下面的形式</h3>
<strong>先执行赋值num=5,在进行判断。这种方式，num的作用域只存在if语句中。</strong>
<pre class="lang:go decode:true">func main() {
	if num := 5;num &gt; 10{
		fmt.Println(10)
	}else{
		fmt.Println(0)
	}
}
</pre>

<hr />

&nbsp;

&nbsp;
<h3>switch</h3>
<pre class="lang:go decode:true">func main() {
	var result int
	var symbol = "*"
	a,b := 10,5
	switch symbol {
	case "+":
		result = a + b
	case "-":
		result = a - b
	case "*":
		result = a * b
	case "/":
		result = a / b
	default:
		panic("有误..."+symbol)
	}
	fmt.Println(result)
}</pre>
&nbsp;

我们可以看到，不需要break，是因为会自动break。

<strong>switch后面也可以不加表达式。</strong>
<pre class="lang:go decode:true ">func main() {
	var result= 80
	var level = ""
	switch {
	case result &lt; 60:
		level = "D"
	case result &lt; 80:
		level = "C"
	case result &lt; 90:
		level = "B"
	case result &lt;= 100:
		level = "A"
	case result &lt; 0 || result &gt; 100:
		panic(fmt.Sprint("错误！"))
	}
	fmt.Printf("%s\n",level)
}</pre>

<hr />

&nbsp;

&nbsp;
<h3>for语句。</h3>
<strong>不需要括号，初始条件，结束条件，表达式都可以省略。</strong>

1加到100
<pre class="lang:go decode:true">func main() {
	sum := 0
	for i:=1;i&lt;=100;i++{
		sum += i
	}
	fmt.Println(sum)
}
</pre>
&nbsp;
<h3>go语言中除去了while,直接一个for就完事,如下一个死循环</h3>
<pre class="lang:go decode:true">for{
	fmt.println("aa")
}</pre>
&nbsp;
