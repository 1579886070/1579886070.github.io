---
title: '[GO]结构体分析-6'
date: 2019-10-09 22:50:38
categories: Go
tags: Golang
---

结构体有点类似与java中的类，是用户定义的类型。<!--more-->

当用户声明一个新的类型时，这个声明就给编译器提供了一个框架，告知必要的内存大小和表示的信息。

&nbsp;

## 结构体的使用

#### 1.使用关键字struct

使用关键字type，后面时类型的名称，然后时struct。这个结构体中有3个字段。
<pre class="lang:default decode:true">type user struct {
	name  string
	age   int
	email string
}
</pre>

* * *

&nbsp;

#### 2.结构类型声明变量，初始化为零值

当声明的user类型给变量的值时，这个值要么用指定的值初始化，要么是变量类型的默认值(数值类型为0，字符串为空，布尔为false)。(:=)含义为声明一个变量，并且初始化。
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

type user struct {
	name  string
	age   int
	email string
}

func main() {
	u := user{}
	fmt.Println(u)
}
------------
{ 0 }</pre>

* * *

&nbsp;

#### 3.字面量初始化

可以指定字段并且初始化值得方式，这种形式对声明的顺序是没有要求的。注意逗号！
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

type user struct {
	name  string
	age   int
	email string
}

func main() {
	u := user{
		name:  "xiaoxin",
		email: "xiaoxin1218@qq.com",
		age:   19,
	}
	fmt.Println(u)
}
--------------
{xiaoxin 19 xiaoxin1218@qq.com}</pre>
&nbsp;

这里我们还可以采用第二种形式，无需字段名，只声明对应的值。但是这种形式顺序必须要和结构体中字段的顺序一致才行。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

type user struct {
	name  string
	age   int
	email string
}

func main() {
	u := user{"aaa", 19, "123456@163.com"}
	fmt.Println(u)
}
----------
{aaa 19 123456@163.com}</pre>
&nbsp;

不在同一行也是可行的。
<pre class="lang:default decode:true ">func main() {
	u := user{
		"aaa",
		11,
		"123456@163.com",
	}</pre>

* * *

&nbsp;

#### 4.赋值取值

可以使用这种方式取值。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

type user struct {
	name  string
	age   int
	email string
}

func main() {
	u := user{
		name: "xiaoxin",
	}
	u.age = 20
	u.email = "88888@qq.com"
	fmt.Println(u)
	fmt.Println(u.name)
}
----------
{xiaoxin 20 88888@qq.com}
xiaoxin</pre>

* * *

&nbsp;

#### 5.内嵌结构体

把一个结构体放入另一个结构体当作字段来用。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

type user struct {
	name  string
	age   int
	email string
}

type admin struct {
	person     user
	profession string
}

func main() {
	u := admin{
		person: user{
			name:  "wangwu",
			age:   20,
			email: "9999@qq.com",
		},
		profession: "java",
	}

	fmt.Println(u)
	fmt.Println(u.person.name)
}
-------------
&nbsp;

同时也可以匿名写成下面这种
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

type admin struct {
	person struct {
		name string
		age  int
	}
	profession string
}

func main() {
	u := admin{
		profession: "c",
	}
	u.person.name = "lisi"
	u.person.age = 20

	fmt.Println(u)
}
</pre>
&nbsp;
