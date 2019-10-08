---
title: '[GO]何为映射-5'
date: 2019-10-08 22:44:16
categories: Go
tags: Golang
---

映射是一种数据结构，用来存储无序的键值对，键值对那么最方便的便是检索数据。<!--more-->
<h2>映射的创建</h2>
<h4>1.通过make函数</h4>
创建一个映射，键和值的类型都是int。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	dict := make(map[int]int)
	fmt.Println(dict, len((dict)))

}
--------
map[] 0</pre>
&nbsp;
<h4>2.通过映射字面量创建</h4>
通过这种方式，映射的初始长度会根据你初始的指定的键值对的数量确定。另外注意逗号！
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

func main() {
	dict := map[string]int{
		"apple":  5,
		"banana": 2,
		"peach":  8, //注意这里的逗号，不然会报错
	}
	fmt.Println(dict, len((dict)))

	dict2 := map[int]int{1: 2, 2: 3}
	fmt.Println(dict2, len((dict2)))

}
---------
map[apple:5 banana:2 peach:8] 3
map[2:3 1:2] 2</pre>
&nbsp;
<h4>3.键值的类型</h4>
映射的键可以是任何值。可以用==运算符作比较的。但是切片，函数等不能作为key，如下。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	dict := map[[]string]int{}

	fmt.Println(dict, len((dict)))

}
---------------
invalid map key type []string</pre>
&nbsp;

但是能作为值value。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	dict := map[string][]int{}

	fmt.Println(dict, len((dict)))

}
</pre>

<hr />

&nbsp;
<h2>映射的使用</h2>
<h4>1.赋值</h4>
通过指定映射的键来对值进行修改。这里我们能看到，如果没有的话会自动添加，已经存在的key会进行修改。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	dict := map[string]int{
		"apple":  5,
		"banana": 2,
		"peach":  8,
	}
	dict["apple"] = 9

	//如果没有会自动添加
	dict["app"] = 10
	fmt.Println(dict, len((dict)))

}
---------------
map[apple:9 banana:2 peach:8 app:10] 4</pre>
&nbsp;
<h4>2.判断是否存在当前键</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	dict := map[string]int{
		"apple":  5,
		"banana": 2,
		"peach":  8,
	}

	value, exists := dict["apple"]
	if exists {
		fmt.Println(value)
	} else {
		fmt.Println("不存在")
	}

}
-----------
5</pre>
&nbsp;
<h4>3.遍历映射</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	dict := map[string]int{
		"apple":  5,
		"banana": 2,
		"peach":  8,
	}
	for key, value := range dict {
		fmt.Printf("key:%s,value:%d\n", key, value)
	}

}
--------------
key:apple,value:5
key:banana,value:2
key:peach,value:8</pre>
&nbsp;
<h4>4.映射的删除</h4>
如果要把一个键值对从映射中删去，那么使用内置函数delete。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	dict := map[string]int{
		"apple":  5,
		"banana": 2,
		"peach":  8,
	}
	fmt.Println(dict, len(dict))
	delete(dict, "banana")

	fmt.Println(dict, len(dict))

}
-----------
map[apple:5 banana:2 peach:8] 3
map[apple:5 peach:8] 2</pre>
&nbsp;

&nbsp;
