---
title: '[JAVA]SpringBoot整合thymeleaf'
date: 2019-10-24 00:55:08
categories: JAVA
tags: java
---

本篇将通过java实例，详细举出thymeleaf常用语法。

pom添加thymeleaf依赖
```
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

html导入头标签，这样编写的时候会有相关提示。
```
<html xmlns:th="http://www.thymeleaf.org">
```

* * *

&nbsp;

&nbsp;

好了，接下来我们通过代码来看效果吧!

## 一.创建Student对象

这是为了后面演示thymeleaf中迭代遍历的语法。
```
package xyz.xioaxin12.springboot.student;

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
```
&nbsp;

## 二.编写Controller

该controller存储了一些字符串、日期、list、map等数据。
```
package xyz.xioaxin12.springboot.controller;

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
        model.addAttribute("msg","thymeleay<hr/>！SHOW");
        //演示日期
        model.addAttribute("date",new Date());
        //演示条件判断
        model.addAttribute("age",20);
        //演示迭代遍历list
        List<Student> list = new ArrayList<>();
        list.add(new Student(1,"王五"));
        list.add(new Student(2,"赵六"));
        list.add(new Student(3,"李四"));
        model.addAttribute("list",list);
        //演示迭代遍历map
        Map<String,Student> map = new HashMap<>();
        map.put("s1",new Student(1,"芳芳"));
        map.put("s2",new Student(2,"娇娇"));
        map.put("s3",new Student(3,"美美"));
        model.addAttribute("map",map);
        return "index";
    }
```


### 三.编写视图

通过上面的controller，发现返回到index页面。

代码看上去或许比较乱，演示thymeleaf中对文本、字符串、日期、条件判断、迭代遍历的语法。
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
 
<head>
    <meta charset="UTF-8">
    <title>文本+字符串+日期+条件+遍历</title>
</head>
<body>
<h2>1.文本操作</h2>
<br/>
 
普通字符串：<span th:text="${msg}"></span><hr/>
可转义字符：<span th:utext="${msg}"></span><hr/>
显示input标签的value中：<input type="text" th:value="${msg}"/><hr/>
<br/>
 
<h2>2.字符串操作</h2>
判断字符串是否为空：<span th:text="${#strings.isEmpty(msg)}" ></span><hr/>
判断字符串是否包含指定字串：<span th:text="${#strings.contains(msg,'me')}"></span><hr/>
判断是否是指定的子串开头：<span th:text="${#strings.startsWith(msg,'thy')}"></span><hr/>
判断是否是指定的子串结尾：<span th:text="${#strings.endsWith(msg,'SHOW')}"></span><hr/>
返回该字符串的长度：<span th:text="${#strings.length(msg)}"></span><hr/>
查找子串下标位置：<span th:text="${#strings.indexOf(msg,'hy')}"></span><hr/>
截取子串：<span th:text="${#strings.substring(msg,5,10)}"></span><hr/>
字符串转大写：<span th:text="${#strings.toUpperCase(msg)}"></span><hr/>
字符串转小写：<span th:text="${#strings.toLowerCase(msg)}"></span><hr/>
<br/>
 
<h2>3.日期操作</h2>
系统默认格式日期：<span th:text="${#dates.format(date)}"></span><hr/>
自定义日期格式：<span th:text="${#dates.format(date,'yyy-mm-dd')}"></span><hr/>
分别取年月日：
<select>
    <option th:text="${#dates.year(date)}"></option>
    <option th:text="${#dates.month(date)}"></option>
    <option th:text="${#dates.day(date)}"></option>
</select>
<hr/><br/>
 
<h2>4.条件判断</h2>
if演示：
<span th:if="${age}>30">超过30岁呢</span>
<span th:if="${age}<30">没有30岁呢</span><hr/>
<div th:switch="${age}">
    swith演示：
    <span th:case="10">10岁</span>
    <span th:case="20">20岁</span>
</div><hr/>
<br/>
 
<h2>5.遍历迭代</h2>
list方式一：
<table border="1">
    <tr>
        <th>ID</th>
        <th>姓名</th>
    </tr>
    <tr th:each="u : ${list}">
        <td th:text="${u.id}"></td>
        <td th:text="${u.name}"></td>
    </tr>
</table><hr/>
list方式二，可以取index,size,count等信息
<table border="1">
    <tr>
        <th>ID</th>
        <th>姓名</th>
        <th>Index</th>
    </tr>
    <tr th:each="u,var : ${list}">
        <td th:text="${u.id}"></td>
        <td th:text="${u.name}"></td>
        <td th:text="${var.index}"></td>
    </tr>
</table><hr/>
map迭代:
<table border="1">
    <tr>
        <th>ID</th>
        <th>姓名</th>
    </tr>
    <tr th:each="maps : ${map}">
      <td th:each="entry : ${maps}" th:text="${entry.value.id}"></td>
      <td th:each="entry : ${maps}" th:text="${entry.value.name}"></td>
    </tr>
</table>
</body>
</html>
```

## 四.启动类
```
package xyz.xioaxin12.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBoot05ThymeleafApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot05ThymeleafApplication.class, args);
    }

}
```

&nbsp;

## 五.浏览器访问路径

效果如下

[![](http://image.xiaoxinyes.club/2019-01-10_103057.png)](http://image.xiaoxinyes.club/2019-01-10_103057.png)

* * *

&nbsp;

接下来在原controller类中添加一个方法，用于演示thymeleaf中的其他语法。
```

1
2
3
4
5
6
7
8
9
@RequestMapping("/show2")
public String show2(Model model, HttpServletRequest request){
    request.setAttribute("req","HttpServletRequest");
 
    request.getSession().setAttribute("sess","HttpSession");
 
    request.getSession().getServletContext().setAttribute("app","Application");
    return "show2";
}
```

返回到show.html页面

该页面举出了thymeleaf对域对象、路径等语法
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>域对象+路径+图片</title>
</head>
<body>
HttpServletRequest：<span th:text="${#httpServletRequest.getAttribute('req')}"></span><hr/>
HttpSession：<span th:text="${session.sess}"></span><hr/>
Application：<span th:text="${application.app}"></span><hr/>

绝对路径：<a th:href="@{http://www.xioaxin12.xyz}">爱生活爱技术</a><hr/>
相对路径：<a th:href="@{/show}">相对于项目上下文路径</a><hr/>
相对路径：<a th:href="@{~/show}">相对于服务器上的根路径</a><hr/>
相对路径：<a th:href="@{/show(id=1,name=zhagnsan)}">传参</a><hr/>

图片：<img th:src="@{https://www.baidu.com/img/baidu_jgylogo3.gif}" th:alt="百度"/><hr/>
th:attr设置属性值：<img th:attr="src=@{https://www.baidu.com/img/baidu_jgylogo3.gif},alt=@{百度},title=@{百度}"/>

</body>
</html>
```

浏览器访问该路径

[![](http://image.xiaoxinyes.club/2019-01-10_103722.png)](http://image.xiaoxinyes.club/2019-01-10_103722.png)

* * *

&nbsp;

&nbsp;

关于上面的thymeleaf语法，我弄了一张图，详细如下图。

[![](http://image.xiaoxinyes.club/thymeleaf.png)](http://image.xiaoxinyes.club/thymeleaf.png)

