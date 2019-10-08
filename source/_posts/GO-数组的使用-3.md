---
title: '[GO]数组的使用-3'
date: 2019-10-08 22:41:46
categories: Go
tags: Golang
---

数组是一个长度确定的数据类型，可以理解为一排排格子放置着物品，可以是不同类型的物品。本节讲解数组的基本使用<!--more-->（因为数组长度不可变的关系，Golang中提供了“动态数组”--切片。使用更广，将在以后说明）

&nbsp;
<h2>数组的定义与初始化</h2>
一但声明，该数组的类型与长度不能改变了，当没手动赋初值的时候，会自动将每个元素的值初始化为0。结果在注释中!
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

func main() {
	var arr [5]int
	fmt.Println(arr)

//{0 0 0 0 0}
}
</pre>
&nbsp;

下面是几种不同的定义形式
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	var arr = [5]int{1, 2, 3, 4, 5}
	var arr2 [5]int = [5]int{1, 2, 3, 4, 5}
	arr3 := [5]int{1, 2, 3, 4, 5}
	arr4 := [...]int{1, 2, 3, 4, 5}

	fmt.Println("arr: ", arr)
	fmt.Println("arr2: ", arr2)
	fmt.Println("arr3: ", arr3)
	fmt.Println("arr4: ", arr4)
/*
arr:  [1 2 3 4 5]
arr2:  [1 2 3 4 5]
arr3:  [1 2 3 4 5]
arr4:  [1 2 3 4 5]
*/
}</pre>

<hr />

&nbsp;
<h2>声明数组时同时指定特定的元素</h2>
数组下标都是从0开始的。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	arr := [5]int{1: 22, 4: 99}
	fmt.Println(arr)
	
//[0 22 0 0 99]
}
</pre>

<hr />

&nbsp;
<h2>数组的赋值</h2>
按照对应的下标，即可修改数组元素的值。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	arr := [5]int{1, 2, 3, 4, 5}
	arr[0] = 66
	arr[3] = 99
	fmt.Println(arr)

//[66 2 3 99 5]
}
</pre>
&nbsp;

同时可以将同类型同长度的数组赋值给另一个数组

我们这里能看出来，对arr的修改并不影响arr2，golang中传递方式为值传递，相当于copy了一份。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	arr := [5]string{"hehe", "haha", "xixi", "huhu", "pupu"}
	var arr2 [5]string
	arr2 = arr
	arr[0] = "pipi"
	fmt.Println(arr)
	fmt.Println(arr2)

/*
[pipi haha xixi huhu pupu]
[hehe haha xixi huhu pupu]
*/
}
</pre>

<hr />

&nbsp;
<h2>数组的遍历</h2>
我们平时通过下标遍历的方式。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	arr := [5]string{"hehe", "haha", "xixi", "huhu", "pupu"}

	for i := 0; i &lt; len(arr); i++ {
		fmt.Println(arr[i])
	}

/*
hehe
haha
xixi
huhu
pupu
*/
}</pre>
&nbsp;

这里还可以使用range关键字来遍历，这种方式可以将下标与对应的值全取出来，下面是取值的几种用法。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	arr := [...]string{"hehe", "haha", "xixi"}
	//取下标
	for a := range arr {
		fmt.Print(a)
	}
	fmt.Println()

	//取下标和值
	fmt.Println()
	for a := range arr {
		fmt.Println(a, arr[a])
	}

	//取下标和值
	fmt.Println()
	for a, v := range arr {
		fmt.Println(a, v)
	}

	//取值
	fmt.Println()
	for _, v := range arr {
		fmt.Println(v)
	}
	
/*
012

0 hehe
1 haha
2 xixi

0 hehe
1 haha
2 xixi

hehe
haha
xixi
*/
}</pre>
&nbsp;

<hr />

<h2>多维数组</h2>
多维的定义与初始化。他们之间许多能理解为父子的对应关系。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	var arr [3][2]int
	fmt.Println(arr)

	arr2 := [2][2]int{{3, 4}, {6, 9}}
	fmt.Println(arr2)

	arr3 := [3][2]int{1: {3, 6}, 2: {9, 11}}
	fmt.Println(arr3)

	arr4 := [3][2]int{1: {0: 33}, 2: {1: 55}}
	fmt.Println(arr4)
	
/*
[[0 0] [0 0] [0 0]]
[[3 4] [6 9]]
[[0 0] [3 6] [9 11]]
[[0 0] [33 0] [0 55]]
*/
}
</pre>
&nbsp;
<h2>二维数组的赋值</h2>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	var arr [2][3]int

	arr[0][0] = 1
	arr[0][1] = 3
	arr[1][1] = 9
	fmt.Println(arr)

/*
[[1 3 0] [0 9 0]]
*/
}
</pre>
&nbsp;
