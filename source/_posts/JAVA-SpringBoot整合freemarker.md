---
title: '[JAVA]SpringBoot整合freemarker'
date: 2019-10-24 00:43:38
categories: JAVA
tags: java
---

FreeMarker是一款模板引擎： 即一种基于模板和要改变的数据<!--more-->， 并用来生成输出文本（HTML网页、电子邮件、配置文件、源代码等）的通用工具。 它不是面向最终用户的，而是一个Java类库，是一款程序员可以嵌入他们所开发产品的组件。--百度百科

&nbsp;

首先pom.xml导入freemarker依赖
<pre class="lang:default decode:true " title="pom.xml">        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
            &lt;artifactId&gt;spring-boot-starter-freemarker&lt;/artifactId&gt;
        &lt;/dependency&gt;</pre>
&nbsp;

跟<span style="text-decoration: underline;">[Spring Boot整合Jsp](http://www.xioaxin12.xyz/1226.html)</span>该篇文章类似，一个实体类用户返回结果，controller和启动类。

<span style="font-size: 10pt;">Student.java</span>
<pre class="lang:java decode:true " title="Student.java">package xyz.xioaxin12.springboot.controller.student;

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
}
</pre>
&nbsp;

<span style="font-size: 10pt;">StudentController .java </span>
<pre class="lang:java decode:true" title="StudentController ">package xyz.xioaxin12.springboot.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import xyz.xioaxin12.springboot.controller.student.Student;

import java.util.ArrayList;
import java.util.List;

@Controller
public class StudentController {
    @RequestMapping("/show")
    public String showStudent(Model model){
        List&lt;Student&gt; list = new ArrayList&lt;&gt;();
        list.add(new Student(1,"小信",22));
        list.add(new Student(2,"天天",25));
        list.add(new Student(1,"赵六",20));
        model.addAttribute("list",list);
        return "show";
    }
}
</pre>
&nbsp;

<span style="font-size: 10pt;">启动类</span>
<pre class="lang:java decode:true ">package xyz.xioaxin12.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @author 爱生活爱技术
 */
@SpringBootApplication
public class SpringBoot04FreemarkerApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot04FreemarkerApplication.class, args);
    }

}

</pre>
&nbsp;

最后编写视图

<span style="color: #ff0000;">注意：模板形式的文件必须放在templates下，外界不可直接访问的！</span>
<pre class="lang:default decode:true " title="show.ftl">&lt;html&gt;
    &lt;head&gt;
        &lt;title&gt;freemarker&lt;/title&gt;
        &lt;meta charset="utf-8"/&gt;
    &lt;/head&gt;

    &lt;body&gt;
        &lt;table border="2" align="center" width="50%"&gt;
            &lt;tr&gt;
                &lt;th&gt;ID&lt;/th&gt;
                &lt;th&gt;姓名&lt;/th&gt;
                &lt;th&gt;年龄&lt;/th&gt;
            &lt;/tr&gt;

            &lt;#list list as s&gt;
                &lt;tr&gt;
                    &lt;td&gt;${s.id}&lt;/td&gt;
                    &lt;td&gt;${s.name}&lt;/td&gt;
                    &lt;td&gt;${s.age}&lt;/td&gt;
                &lt;/tr&gt;
            &lt;/#list&gt;
        &lt;/table&gt;

    &lt;/body&gt;
&lt;/html&gt;</pre>
&nbsp;

[![](http://image.xiaoxinyes.club/2019-01-08_120014.png)](http://image.xiaoxinyes.club/2019-01-08_120014.png)

* * *

freemarker官方在线手册：[点击](http://freemarker.foofun.cn/ref_directive_switch.html#ref.directive.case)
