---
title: '[JAVA]SpringBoot整合Jsp'
date: 2019-10-24 00:41:35
categories: JAVA
tags: java
---

springboot并不推荐使用jsp，不推荐!<!--more-->

### 1.首先idea快速构建一个springboot项目。

我们需要手动添加WEB-INF这些文件夹。

[![](http://image.xiaoxinyes.club/2019-01-07_162623.png)](http://image.xiaoxinyes.club/2019-01-07_162623.png)

* * *

&nbsp;

### 2.配置视图解析

<span style="font-size: 10pt;">application.properties</span>
<pre class="lang:default decode:true ">spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp

server.port=8081</pre>

* * *

&nbsp;

### 3.编写一个实体类，用户传数据。

<pre class="lang:java decode:true " title="Student.java">package xyz.xioaxin12.springboot.student;

public class Student {
    private Integer id;
    private String name;
    private int age;

    public Student() {
    }

    public Student(Integer id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age='" + age + '\'' +
                '}';
    }
}
</pre>

* * *

&nbsp;

### 4.controller视图层

注意注解是@Controller,可以直接返回一个视图，由于已经设置了视图的"前后缀"，return便是返回到WEB-INF/jsp/index.jsp页面。
<pre class="lang:java decode:true " title="JspContreller.java">package xyz.xioaxin12.springboot.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import xyz.xioaxin12.springboot.student.Student;

import java.util.ArrayList;
import java.util.List;

@Controller
public class JspContreller {

    @RequestMapping("/show")
    public String Show(Model model){
        List&lt;Student&gt; students = new ArrayList&lt;&gt;();
        students.add(new Student(1,"小信",33));
        students.add(new Student(2,"小田",25));
        students.add(new Student(3,"小夏",19));
        students.add(new Student(4,"小方",23));

        model.addAttribute("student",students);
        return "index";
    }

}
</pre>

* * *

&nbsp;

### 5.jsp页面的编写

<pre class="lang:default decode:true" title="index.jsp">&lt;%@ page contentType="text/html;charset=UTF-8" language="java" %&gt;
&lt;%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %&gt;
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;Title&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;table border="1" width="600px" style="margin: auto;text-align: center"&gt;
        &lt;tr&gt;
            &lt;th&gt;id&lt;/th&gt;
            &lt;th&gt;姓名&lt;/th&gt;
            &lt;th&gt;年龄&lt;/th&gt;
        &lt;/tr&gt;
        &lt;c:forEach items="${student}" var="s"&gt;
            &lt;tr&gt;
                &lt;td&gt;${s.id}&lt;/td&gt;
                &lt;td&gt;${s.name}&lt;/td&gt;
                &lt;td&gt;${s.age}&lt;/td&gt;
            &lt;/tr&gt;
        &lt;/c:forEach&gt;
    &lt;/table&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>
&nbsp;

用到了jsp中的<span style="color: #ff0000;">jstl</span>，所以我们需要提前在pom.xml中引入下列依赖。
<pre class="lang:default decode:true " title="pom.xml">        &lt;!-- jstl --&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;javax.servlet&lt;/groupId&gt;
            &lt;artifactId&gt;jstl&lt;/artifactId&gt;
        &lt;/dependency&gt;

        &lt;!-- jasper --&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.apache.tomcat.embed&lt;/groupId&gt;
            &lt;artifactId&gt;tomcat-embed-jasper&lt;/artifactId&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;</pre>

* * *

&nbsp;

### 6.查看

浏览器地址：http://localhost:8081/show

[![](http://image.xiaoxinyes.club/2019-01-07_193614.png)](http://image.xiaoxinyes.club/2019-01-07_193614.png)

&nbsp;

<span style="color: #ff0000;">补充</span>：关于webapp文件的生成。idea在构建springboot项目中，是没有这个文件的，我们可以按照下图直接生成。

[![](http://image.xiaoxinyes.club/2019-01-07_194025.png)](http://image.xiaoxinyes.club/2019-01-07_194025.png)
