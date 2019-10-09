---
title: '[GO]方法与接口-7'
date: 2019-10-09 22:54:05
categories: Go
tags: Golang
---

方法能给用户定义的类型添加新的行为，方法实际以包含了接收者的函数。但是函数在go中不等同于方法。<!--more-->
<h2>方法</h2>
我们可以看到自定义了一个user类型，然后将这个类型作为接收者给了一个名为notify的函数。通过对user类型的初始化再调用该方法。
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

//定义一个用户类型
type user struct {
	name string
	age  int
}

//使用值接收者实现一个方法
func (u user) notify() {
	fmt.Printf("方法执行--&gt;%s-%d\n", u.name, u.age)
}

func main() {
	user := user{
		name: "xiaoxin",
		age:  20,
	}
	user.notify()

}
--------
方法执行--&gt;xiaoxin-20</pre>
&nbsp;

上面这个例子是值接收，如果使用值接收者声明方法，调用时会使这个值的一个副本来执行，那么当前值修改不会影响原来的值。接下来看下值接收和指针类型的接收有何区别。
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

//定义一个用户类型
type user struct {
	name string
	age  int
}

//使用值接收者实现一个方法
func (u user) notify() {
	fmt.Printf("notify方法执行--&gt;%s-%d\n", u.name, u.age)
}

//使用值接收者实现一个方法
func (u user) newnotify(name string) {
	u.name = name
}

//使用指针接收者实现一个方法
func (u *user) newnotify2(name string) {
	u.name = name
}

func main() {
	user := user{"wangwu", 20}
	user.notify()

	user.newnotify("zhaoliu")
	user.notify()

	user.newnotify2("zhaoliu")
	user.notify()

}
------------
方法执行--&gt;wangwu-20
方法执行--&gt;wangwu-20
方法执行--&gt;zhaoliu-20</pre>
&nbsp;

我们能发现，使用指针接收者的方法时，这个方法会共享调用方法接收时接收者所指向的值。从而改变。

值接收者使用的值的副本来调用方法，而指针接收者使用实际的值来调用方法。

<hr />

&nbsp;
<h2>接口</h2>
接口定义着对象所具有的行为。
<h4>创建与实现接口</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

//定义了一个接口
type notifier interface {
	notify()
}

//定义一个用户类型
type user struct {
	name string
	age  int
}

func (u user) notify() {
	fmt.Printf("notify()执行了--&gt;%s-%d\n", u.name, u.age)
}

func main() {
	user := user{"lisi", 20}
	var n notifier
	n = user
	n.notify()
}
---------
notify()执行了--&gt;lisi-20</pre>
&nbsp;

如果时使用<span style="color: #ff0000;">指针接收者</span>来实现一个接口，那么只有指向那个类型的指针才能实现相对应的接口。如果是使用的<span style="color: #ff0000;">值接收者</span>来实现接口的话，那么那个类型的值和指针都能够实现相对应的接口。接下来对上面的例子稍微改变即可看到指针作为接收者的不同。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

//定义了一个接口
type notifier interface {
	notify()
}

//定义一个用户类型
type user struct {
	name string
	age  int
}

//使用指针接收者实现方法
func (u *user) notify() {
	fmt.Printf("notify()执行了--&gt;%s-%d\n", u.name, u.age)
}

func main() {
	user := user{"lisi", 20}
	var n notifier
	//需要传递user的地址，否者报错
	n = &amp;user
	n.notify()
}
-----------
notify()执行了--&gt;lisi-20</pre>

<hr />

&nbsp;
<h2>多态</h2>
我们看下接口的多态案例，新增一个admin类型。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

//定义了一个接口
type notifier interface {
	notify()
}

//user
type user struct {
	name string
	age  int
}

//admin
type admin struct {
	name string
	age  int
}

//使用指针接收者实现方法,接收者为user
func (u *user) notify() {
	fmt.Printf("user--&gt;%s-%d\n", u.name, u.age)
}

//使用指针接收者实现方法,接受为为admin
func (a *admin) notify() {
	fmt.Printf("admin--&gt;%s-%d\n", a.name, a.age)
}

//接收实现notifier接口的值，通知
func show(n notifier) {
	n.notify()
}
func main() {
	user := user{"user", 20}
	show(&amp;user)

	admin := admin{"admin", 22}
	show(&amp;admin)
}
---------
user--&gt;user-20
admin--&gt;admin-22</pre>
可以看到，user、admin两个实体类型都可以实现notify接口。

&nbsp;
<h2>类型的嵌入</h2>
嵌入是将已经有的类型直接声明到新的结构体类型当中，被嵌入的类型被称为新的外部类型的内部类型。对外部类型来说，内部类型总是存在的，所以没有指定内部类型对应的字段名，我们还可以直接使用内部类型的类型名来访问。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

//user
type user struct {
	name string
	age  int
}

//admin
type admin struct {
	user	//嵌入类型
	email string
}

//使用指针接收者实现方法
func (u *user) notify() {
	fmt.Printf("user--&gt;%s-%d\n", u.name, u.age)
}

func main() {
	//创建admin用户
	admin := admin{
		user: user{
			name: "xiaoxin",
			age:  20,
		},
		email: "123456@qq.com",
	}
	//两种访问访问都可以
	admin.user.notify()
	
	admin.notify()
}
----------
user--&gt;xiaoxin-20
user--&gt;xiaoxin-20</pre>
&nbsp;
<h4>看看接口是否如此</h4>
我们能看到下方案例，admin类型并没有实现该接口，通过user也是可以访问的。那么由于内部类型的提升，内部类型的接口会自动提升到外部类型。意味着由于内部类型的实现，外部类型同样可以实现该接口。
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

type notifier interface {
	notify()
}

//user
type user struct {
	name string
	age  int
}

//admin
type admin struct {
	user
	email string
}

//使用指针接收者实现方法 user
func (u *user) notify() {
	fmt.Printf("user--&gt;%s-%d\n", u.name, u.age)
}

func main() {
	admin := admin{
		user: user{
			name: "xiaoxin",
			age:  20,
		},
		email: "123456@qq.com",
	}
	show(&amp;admin)
}
func show(n notifier) {
	n.notify()
}
-----------
user--&gt;xiaoxin-20</pre>
&nbsp;

但是如果外部类型实现了notify方法，内部类型的实现将不会被提升。但是内部类型的值还一直存在，仍然可以直接访问方法。

<hr />

&nbsp;
<h2>标识符</h2>
<strong>当一个标识符的名称是以小写字母开头时，那么这个标识符是未公开的，包外不可访问；</strong>

<strong>当一个标识符的名称是以大写字母开头时，那么这个标识符就是公开的，包外可以访问。</strong>
