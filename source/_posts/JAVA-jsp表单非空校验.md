---
title: '[JAVA]jsp表单非空校验'
date: 2019-10-16 18:40:49
categories: JAVA
tags: java
---

弹窗式提示，在点击提交后，为空则弹出提示框。<!--more-->

### jsp表单

<pre class="lang:default decode:true ">&lt;body&gt;
&lt;h1&gt;提交&lt;/h1&gt;
&lt;form class="form" action="/aa" method="post"&gt;
    &lt;input type="text" name="name" id="name" placeholder="用户名" onblur="validateNonEmpty(this)" /&gt;
    &lt;input type="password" name="password" id="password" placeholder="密码"/&gt;
    &lt;button type="submit" name="regist" onclick="return valid()" &gt;注册&lt;/button&gt;
&lt;/form&gt;
&lt;/body&gt;</pre>
&nbsp;

### js代码

<pre class="lang:js decode:true ">&lt;script type="text/javascript"&gt;
    function valid() {
        var name = document.getElementById("name").value;
        var pas = document.getElementById("password").value;

        if(name=="" || name==null){
            alert("用户名不能为空！");
            return false;
        }
        else if(pas=="" || pas==null){
            alert("密码不能为空！");
            return false;
        }
        else{
            return true;
        }
    }
&lt;/script&gt;</pre>
&nbsp;

### 效果

![](http://image.xiaoxinyes.club/8_15_1.gif)
