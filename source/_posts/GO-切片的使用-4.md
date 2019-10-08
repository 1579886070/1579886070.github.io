---
title: '[GO]切片的使用-4'
date: 2019-10-08 22:42:53
categories: Go
tags: Golang
---

切片可以理解为动态的数组，可以按需求自动增长或者是缩小，无疑比数组更加灵活多变，首先看下图。<!--more-->

<img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2018-12-19_110758.png" alt="爱生活爱技术" width="697" height="329" />

上图中，有3个字段，分别是指向底层数组的指针、切片访问的元素的长度和切片允许增长的元素个数(即容量)。如果当后面的表示0的格子满了，底层的数组就会更换。

&nbsp;
<h2>切片的创建</h2>
<h4>1.使用内置的make函数创建</h4>
使用make时，需要指定切片的类型、长度与容量大小。
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

func main() {
	//长度和容量都为5
	s1 := make([]int, 5)
	fmt.Println(s1)
	//长度为3，容量为5
	s2 := make([]int, 3, 5)
	fmt.Println(s2)

}
--------
[0 0 0 0 0]
[0 0 0]</pre>
&nbsp;
<h3>2.容量小于长度将会报错。</h3>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	//长度和容量都为5
	s1 := make([]int, 5, 3)
	fmt.Println(s1)
	//长度为3，容量为5

}
----------
 len larger than cap in make([]int)</pre>
&nbsp;
<h3>3.通过初始化赋值的方式创建切片</h3>
这种方式创建，初始的长度与容量会根据初始化提供的元素个数确定，即时满容的。内置函数 len() 和 cap() 可以直接获得切片的长度和容量属性。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	s1 := []string{"haha", "hehe", "oo"}
	fmt.Println(s1, len(s1), cap(s1))
}
-----------
[haha hehe oo] 3 3</pre>
&nbsp;
<h4>4.使用索引声明切片</h4>
01，02字符串分别初始化第4和3个元素。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	s1 := []string{4: "01", 3: "02"}
	fmt.Println(s1, len(s1), cap(s1))
}
---------
[   02 01] 5 5</pre>

<hr />

&nbsp;
<h2>nil与空切片</h2>
在声明时不做任何的初始化，即创建一个nil切片。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {
	//	nil切片
	var s1 []int
	fmt.Println(s1, len(s1), cap(s1))
	//空切片
	var s2 []int = make([]int, 0)
	fmt.Println(s2, len(s2), cap(s2))
	//空切片
	s3 := []int{}
	fmt.Println(s3, len(s3), cap(s3))
}
---------
[] 0 0
[] 0 0
[] 0 0</pre>

<hr />

&nbsp;
<h2>切片的使用</h2>
<h4>1.赋值</h4>
如同对数组的索引指向的元素赋值一样。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {

	s1 := []int{1, 2, 3, 4, 5}
	s1[2] = 11
	s1[3] = 99
	fmt.Println(s1, len(s1), cap(s1))

}
------------
[1 2 11 99 5] 5 5</pre>
&nbsp;
<h4>2.使用切片来创建切片</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {

	s1 := []int{1, 2, 3, 4, 5}
	s2 := s1[1:3]
	fmt.Println(s1, len(s1), cap(s1))
	fmt.Println(s2, len(s2), cap(s2))
}
--------
[1 2 3 4 5] 5 5
[2 3] 2 4</pre>
&nbsp;

当我们有了两个切片，实际上它们共享了同一段底层数组，如图。对于s1我们能知道它的全部容量，但是之后s2就是中间到切片末尾的长度。

<a href="http://image.xiaoxinyes.club/2018-12-19_115237.png"><img class="aligncenter size-medium" src="http://image.xiaoxinyes.club/2018-12-19_115237.png" alt="爱生活爱技术" width="738" height="416" /></a>

起始值和结束值都是可选的。
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {

	s1 := []int{1, 2, 3, 4, 5}
	s2 := s1[1:]
	s3 := s1[:3]
	s4 := s1[:]
	fmt.Println(s1, len(s1), cap(s1))
	fmt.Println(s2, len(s2), cap(s2))
	fmt.Println(s3, len(s3), cap(s3))
	fmt.Println(s4, len(s4), cap(s4))
}
----------
[1 2 3 4 5] 5 5
[2 3 4 5] 4 4
[1 2 3] 3 5
[1 2 3 4 5] 5 5</pre>
&nbsp;
<h4>3.长度和容量的计算</h4>
对于底层数组容量是k的切片s1[i:j]来说
<p style="padding-left: 30px;">长度：j-i</p>
<p style="padding-left: 30px;">容量：k-i</p>
举例：容量是5的切片s1[1:3]
<p style="padding-left: 30px;">长度：3-1=2</p>
<p style="padding-left: 30px;">容量：5-1=4</p>


<hr />

&nbsp;
<h2>切片增长</h2>
<h4>1.append向切片增加元素</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {

	s1 := []int{1, 2, 3, 4, 5}
	//长度为2，容量为4
	s2 := s1[1:3]
	s2 = append(s2, 7)
	fmt.Println(s2, len(s2), cap(s2))
}
//[2 3 7] 3 4</pre>
&nbsp;
<h4>2.切片追加</h4>
前面说了切片是动态的，可以进行增长。若底层数组扩容了，append操作完成后，会拥有一个全新的底层数组。
<pre class="lang:default decode:true">package main

import (
	"fmt"
)

func main() {

	s1 := []int{1, 2, 3, 4, 5}
	fmt.Println(s1, len(s1), cap(s1))
	s2 := append(s1, 6)
	fmt.Println(s2, len(s2), cap(s2))
	s3 := append(s2, 8)
	fmt.Println(s3, len(s3), cap(s3))
}
-----------
[1 2 3 4 5] 5 5
[1 2 3 4 5 6] 6 10
[1 2 3 4 5 6 8] 7 10</pre>
&nbsp;

在切片的容量小于1000个元素时，总会成倍的增加容量。一旦超过1000，容量每次增长25%。

&nbsp;
<h4>3.使用3个索引创建切片</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {

	s1 := []int{1, 2, 3, 4, 5}
	s2 := s1[2:3:4]
	fmt.Println(s1, len(s1), cap(s1))
	fmt.Println(s2, len(s2), cap(s2))
}
-----------
[1 2 3 4 5] 5 5
[3] 1 2</pre>
&nbsp;

我们可以用之前定义的公式，对于s1[2:3:4]就是s1[i:j:k]
<p style="padding-left: 30px;">长度：j-i=3-2-1</p>
<p style="padding-left: 30px;">容量：k-i=4-2=2</p>


<hr />

&nbsp;
<h2>切片的遍历</h2>
<h4>1.for的形式</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {

	s1 := []int{10, 20, 30, 40, 50}
	for i := 0; i &lt; len(s1); i++ {
		fmt.Println(i, s1[i])
	}
}
--------------
0 10
1 20
2 30
3 40
4 50</pre>
&nbsp;
<h4>2.range形式</h4>
<pre class="lang:default decode:true ">package main

import (
	"fmt"
)

func main() {

	s1 := []int{10, 20, 30, 40, 50}
	for i, v := range s1 {
		fmt.Println(i, v)
	}
}
-----------
0 10
1 20
2 30
3 40
4 50</pre>
&nbsp;
