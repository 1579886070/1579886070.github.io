---
title: '[JAVA]SpringBoot整合Servlet'
date: 2019-10-22 22:34:48
categories: JAVA
tags: java
---

Spring Boot整合Servlet有两种方式。<!--more-->

1.通过注解扫描.

2.通过编写一个方法.

&nbsp;

## 1.注解扫描

以前我们在web写select，需要在web.xml配置&lt;servlet&gt;&lt;/servlet&gt;和&lt;servlet-mapping&gt;&lt;/servlet-mapping&gt;等信息。那么使用springboot不必了。

<span style="font-size: 10pt;">AnnotationServlet.java</span>

这里我们设置的路径是/hello
<pre class="lang:java decode:true" title="AnnotationServlet">package xyz.xioaxin12.springboot01servlet.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name="AnnotationServlet",urlPatterns = "/hello")
public class AnnotationServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");
        resp.getWriter().append("这是springboot整合servlet注解的方式!");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
</pre>
&nbsp;

<span style="font-size: 10pt;">SpringBoot01ServletApplication.java</span>

启动类，这里能看到加了@ServletComponentScan注解，它会自动注册带有@WebServlet的注解。
<pre class="lang:default decode:true">package xyz.xioaxin12.springboot01servlet;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;

@SpringBootApplication
@ServletComponentScan
public class SpringBoot01ServletApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot01ServletApplication.class, args);
    }

}

</pre>
&nbsp;

由于我的8080被占用了，我在application.properties修改成8081端口。
<pre class="lang:default decode:true ">server.port=8081</pre>
&nbsp;

启动访问localhost:8081/hello

[![](http://image.xiaoxinyes.club/2019-01-06_112731.png)](http://image.xiaoxinyes.club/2019-01-06_112731.png)

* * *

&nbsp;

&nbsp;

## 2.通过编写一个方法

为了测试明显，不在原类上修改，重写一个servlet与启动类。

<span style="font-size: 10pt;">DeployServlet.java </span>

去除了注解。
<pre class="lang:java decode:true">package xyz.xioaxin12.springboot01servlet.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class DeployServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");
        resp.getWriter().append("这是springboot整合servlet另一种方式!");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
</pre>
&nbsp;

<span style="font-size: 10pt;">SpringBoot01ServletApplication2.java</span>

一定要添加@Bean
<pre class="lang:java decode:true " title="SpringBoot01ServletApplication2 ">package xyz.xioaxin12.springboot01servlet;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import xyz.xioaxin12.springboot01servlet.servlet.DeployServlet;

@SpringBootApplication
public class SpringBoot01ServletApplication2 {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot01ServletApplication2.class, args);
    }

    @Bean
    public ServletRegistrationBean getRegistrationBean(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new DeployServlet());
        bean.addUrlMappings("/hello2");
        return bean;
    }

}

</pre>
&nbsp;

启动访问localhost:8081/hello2

[![](http://image.xiaoxinyes.club/2019-01-06_113424.png)](http://image.xiaoxinyes.club/2019-01-06_113424.png)
