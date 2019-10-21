---
title: '[JAVA]Jsp通过JSTL遍历数据'
date: 2019-10-21 18:57:05
categories: JAVA
tags: java
---

视图层获取到数据库的中数据后，存入List集合中，通过JSTL的c:foErch遍历。<!--more-->

&nbsp;
<pre class="lang:java decode:true" title="View">@RequestMapping("/selectAll.do")
public ModelAndView doSelectAll(){
    List&lt;Student&gt; student = service.findAllStudents();
    ModelAndView view = new ModelAndView();
    view.addObject("student",student);
    view.setViewName("/main.jsp");
    return view;
}</pre>
&nbsp;

### main.jsp

<pre class="lang:default decode:true ">&lt;%@ page contentType="text/html; charset=UTF-8" language="java" %&gt;
&lt;%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%&gt;
&lt;html&gt;
&lt;title&gt;查询&lt;/title&gt;
&lt;body&gt;
&lt;h2 align="center"&gt;查询成功&lt;/h2&gt;
&lt;table width="100%" border=1&gt;
    &lt;c:forEach items="${student}" var="c"&gt;
        &lt;tr&gt;
            &lt;td&gt;${c.id }&lt;/td&gt;
            &lt;td&gt;${c.name }&lt;/td&gt;
            &lt;td&gt;${c.age }&lt;/td&gt;
        &lt;/tr&gt;
    &lt;/c:forEach&gt;

&lt;/table&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>
&nbsp;

### 遍历结果

![](http://image.xiaoxinyes.club/2018-09-07_1.png "爱生活爱技术")