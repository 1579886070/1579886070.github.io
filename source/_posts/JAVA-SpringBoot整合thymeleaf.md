---
title: '[JAVA]SpringBoot整合thymeleaf'
date: 2019-10-24 00:55:08
categories: JAVA
tags: java
---

本篇将通过java实例，详细举出thymeleaf常用语法。<!--more-->

pom添加thymeleaf依赖
<pre class="lang:default decode:true " title="pom.xml">&lt;dependency&gt;
      &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
      &lt;artifactId&gt;spring-boot-starter-thymeleaf&lt;/artifactId&gt;
&lt;/dependency&gt;</pre>
&nbsp;

html导入头标签，这样编写的时候会有相关提示。
<pre class="lang:default decode:true ">&lt;html xmlns:th="http://www.thymeleaf.org"&gt;</pre>

* * *

&nbsp;

&nbsp;

好了，接下来我们通过代码来看效果吧!

## 一.创建Student对象

这是为了后面演示thymeleaf中迭代遍历的语法。
<pre class="lang:java decode:true " title="Student.java">package xyz.xioaxin12.springboot.student;

public class Student {
    private Integer id;
    private String name;

    public Student() {
    }

    public Student(Integer id, String name) {
        this.id = id;
        this.name = name;
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

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
</pre>
&nbsp;

## 二.编写Controller

该controller存储了一些字符串、日期、list、map等数据。
<pre class="lang:java decode:true" title="showStudent.java">package xyz.xioaxin12.springboot.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import xyz.xioaxin12.springboot.student.Student;

import javax.servlet.http.HttpServletRequest;
import java.util.*;

@Controller
public class SutdentController {

    @RequestMapping("/show")
    public String showStudent(Model model){
        //演示文本
        model.addAttribute("msg","thymeleay&lt;hr/&gt;！SHOW");
        //演示日期
        model.addAttribute("date",new Date());
        //演示条件判断
        model.addAttribute("age",20);
        //演示迭代遍历list
        List&lt;Student&gt; list = new ArrayList&lt;&gt;();
        list.add(new Student(1,"王五"));
        list.add(new Student(2,"赵六"));
        list.add(new Student(3,"李四"));
        model.addAttribute("list",list);
        //演示迭代遍历map
        Map&lt;String,Student&gt; map = new HashMap&lt;&gt;();
        map.put("s1",new Student(1,"芳芳"));
        map.put("s2",new Student(2,"娇娇"));
        map.put("s3",new Student(3,"美美"));
        model.addAttribute("map",map);
        return "index";
    }</pre>
&nbsp;

### 三.编写视图

通过上面的controller，发现返回到index页面。

代码看上去或许比较乱，演示thymeleaf中对文本、字符串、日期、条件判断、迭代遍历的语法。
```
<pre class="lang:default decode:true " title="index.html">&lt;!DOCTYPE html&gt;
&lt;html xmlns:th="http://www.thymeleaf.org"&gt;

&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;title&gt;文本+字符串+日期+条件+遍历&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h2&gt;1.文本操作&lt;/h2&gt;
&lt;br/&gt;

普通字符串：&lt;span th:text="${msg}"&gt;&lt;/span&gt;&lt;hr/&gt;
可转义字符：&lt;span th:utext="${msg}"&gt;&lt;/span&gt;&lt;hr/&gt;
显示input标签的value中：&lt;input type="text" th:value="${msg}"/&gt;&lt;hr/&gt;
&lt;br/&gt;

&lt;h2&gt;2.字符串操作&lt;/h2&gt;
判断字符串是否为空：&lt;span th:text="${#strings.isEmpty(msg)}" &gt;&lt;/span&gt;&lt;hr/&gt;
判断字符串是否包含指定字串：&lt;span th:text="${#strings.contains(msg,'me')}"&gt;&lt;/span&gt;&lt;hr/&gt;
判断是否是指定的子串开头：&lt;span th:text="${#strings.startsWith(msg,'thy')}"&gt;&lt;/span&gt;&lt;hr/&gt;
判断是否是指定的子串结尾：&lt;span th:text="${#strings.endsWith(msg,'SHOW')}"&gt;&lt;/span&gt;&lt;hr/&gt;
返回该字符串的长度：&lt;span th:text="${#strings.length(msg)}"&gt;&lt;/span&gt;&lt;hr/&gt;
查找子串下标位置：&lt;span th:text="${#strings.indexOf(msg,'hy')}"&gt;&lt;/span&gt;&lt;hr/&gt;
截取子串：&lt;span th:text="${#strings.substring(msg,5,10)}"&gt;&lt;/span&gt;&lt;hr/&gt;
字符串转大写：&lt;span th:text="${#strings.toUpperCase(msg)}"&gt;&lt;/span&gt;&lt;hr/&gt;
字符串转小写：&lt;span th:text="${#strings.toLowerCase(msg)}"&gt;&lt;/span&gt;&lt;hr/&gt;
&lt;br/&gt;

&lt;h2&gt;3.日期操作&lt;/h2&gt;
系统默认格式日期：&lt;span th:text="${#dates.format(date)}"&gt;&lt;/span&gt;&lt;hr/&gt;
自定义日期格式：&lt;span th:text="${#dates.format(date,'yyy-mm-dd')}"&gt;&lt;/span&gt;&lt;hr/&gt;
分别取年月日：
&lt;select&gt;
    &lt;option th:text="${#dates.year(date)}"&gt;&lt;/option&gt;
    &lt;option th:text="${#dates.month(date)}"&gt;&lt;/option&gt;
    &lt;option th:text="${#dates.day(date)}"&gt;&lt;/option&gt;
&lt;/select&gt;
&lt;hr/&gt;&lt;br/&gt;

&lt;h2&gt;4.条件判断&lt;/h2&gt;
if演示：
&lt;span th:if="${age}&gt;30"&gt;超过30岁呢&lt;/span&gt;
&lt;span th:if="${age}&lt;30"&gt;没有30岁呢&lt;/span&gt;&lt;hr/&gt;
&lt;div th:switch="${age}"&gt;
    swith演示：
    &lt;span th:case="10"&gt;10岁&lt;/span&gt;
    &lt;span th:case="20"&gt;20岁&lt;/span&gt;
&lt;/div&gt;&lt;hr/&gt;
&lt;br/&gt;

&lt;h2&gt;5.遍历迭代&lt;/h2&gt;
list方式一：
&lt;table border="1"&gt;
    &lt;tr&gt;
        &lt;th&gt;ID&lt;/th&gt;
        &lt;th&gt;姓名&lt;/th&gt;
    &lt;/tr&gt;
    &lt;tr th:each="u : ${list}"&gt;
        &lt;td th:text="${u.id}"&gt;&lt;/td&gt;
        &lt;td th:text="${u.name}"&gt;&lt;/td&gt;
    &lt;/tr&gt;
&lt;/table&gt;&lt;hr/&gt;
list方式二，可以取index,size,count等信息
&lt;table border="1"&gt;
    &lt;tr&gt;
        &lt;th&gt;ID&lt;/th&gt;
        &lt;th&gt;姓名&lt;/th&gt;
        &lt;th&gt;Index&lt;/th&gt;
    &lt;/tr&gt;
    &lt;tr th:each="u,var : ${list}"&gt;
        &lt;td th:text="${u.id}"&gt;&lt;/td&gt;
        &lt;td th:text="${u.name}"&gt;&lt;/td&gt;
        &lt;td th:text="${var.index}"&gt;&lt;/td&gt;
    &lt;/tr&gt;
&lt;/table&gt;&lt;hr/&gt;
map迭代:
&lt;table border="1"&gt;
    &lt;tr&gt;
        &lt;th&gt;ID&lt;/th&gt;
        &lt;th&gt;姓名&lt;/th&gt;
    &lt;/tr&gt;
    &lt;tr th:each="maps : ${map}"&gt;
      &lt;td th:each="entry : ${maps}" th:text="${entry.value.id}"&gt;&lt;/td&gt;
      &lt;td th:each="entry : ${maps}" th:text="${entry.value.name}"&gt;&lt;/td&gt;
    &lt;/tr&gt;
&lt;/table&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
&nbsp;
```
## 四.启动类

<pre class="lang:java decode:true ">package xyz.xioaxin12.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBoot05ThymeleafApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot05ThymeleafApplication.class, args);
    }

}

</pre>
&nbsp;

## 五.浏览器访问路径

效果如下

[![](http://image.xiaoxinyes.club/2019-01-10_103057.png)](http://image.xiaoxinyes.club/2019-01-10_103057.png)

* * *

&nbsp;

接下来在原controller类中添加一个方法，用于演示thymeleaf中的其他语法。
<pre class="lang:java decode:true ">@RequestMapping("/show2")
public String show2(Model model, HttpServletRequest request){
    request.setAttribute("req","HttpServletRequest");

    request.getSession().setAttribute("sess","HttpSession");

    request.getSession().getServletContext().setAttribute("app","Application");
    return "show2";
}</pre>
&nbsp;

返回到show.html页面

该页面举出了thymeleaf对域对象、路径等语法
```
<pre class="lang:default decode:true" title="show2.html">&lt;!DOCTYPE html&gt;
&lt;html xmlns:th="http://www.thymeleaf.org"&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;title&gt;域对象+路径+图片&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
HttpServletRequest：&lt;span th:text="${#httpServletRequest.getAttribute('req')}"&gt;&lt;/span&gt;&lt;hr/&gt;
HttpSession：&lt;span th:text="${session.sess}"&gt;&lt;/span&gt;&lt;hr/&gt;
Application：&lt;span th:text="${application.app}"&gt;&lt;/span&gt;&lt;hr/&gt;

绝对路径：&lt;a th:href="@{http://www.xioaxin12.xyz}"&gt;爱生活爱技术&lt;/a&gt;&lt;hr/&gt;
相对路径：&lt;a th:href="@{/show}"&gt;相对于项目上下文路径&lt;/a&gt;&lt;hr/&gt;
相对路径：&lt;a th:href="@{~/show}"&gt;相对于服务器上的根路径&lt;/a&gt;&lt;hr/&gt;
相对路径：&lt;a th:href="@{/show(id=1,name=zhagnsan)}"&gt;传参&lt;/a&gt;&lt;hr/&gt;

图片：&lt;img th:src="@{https://www.baidu.com/img/baidu_jgylogo3.gif}" th:alt="百度"/&gt;&lt;hr/&gt;
th:attr设置属性值：&lt;img th:attr="src=@{https://www.baidu.com/img/baidu_jgylogo3.gif},alt=@{百度},title=@{百度}"/&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
&nbsp;
```
浏览器访问该路径

[![](http://image.xiaoxinyes.club/2019-01-10_103722.png)](http://image.xiaoxinyes.club/2019-01-10_103722.png)

* * *

&nbsp;

&nbsp;

关于上面的thymeleaf语法，我弄了一张图，详细如下图。

[![](http://image.xiaoxinyes.club/thymeleaf.png)](http://image.xiaoxinyes.club/thymeleaf.png)

