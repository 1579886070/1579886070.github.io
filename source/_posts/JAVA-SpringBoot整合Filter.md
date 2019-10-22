---
title: '[JAVA]SpringBoot整合Filter'
date: 2019-10-22 22:35:41
categories: JAVA
tags: java
---

同样有注解扫描与方法注册两种方式。<!--more-->

## 1.注解扫描

编写filter类
<pre class="lang:java decode:true ">package xyz.xioaxin12.springboot.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter(filterName = "MyFilter",urlPatterns = "/hello")
public class MyFilter implements Filter {
    public void destroy() {

    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        System.out.println("进入doFilter...");
        chain.doFilter(req, resp);
        System.out.println("退出doFilter...");
    }

    public void init(FilterConfig config) throws ServletException {

    }

}
</pre>
&nbsp;

拦截MyServlet
<pre class="lang:java decode:true">package xyz.xioaxin12.springboot.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(name = "MyServlet",urlPatterns = "/hello")
public class MyServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       doGet(request,response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MyServlet...");
    }
}
</pre>
&nbsp;

启动类
<pre class="lang:java decode:true ">package xyz.xioaxin12.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;

@SpringBootApplication
@ServletComponentScan
public class SpringBoot02FilterApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot02FilterApplication.class, args);
    }

}

</pre>
&nbsp;

启动运行

![](http://image.xiaoxinyes.club/2019-01-06_144153.png)

* * *

&nbsp;

## 2.方法注册

1).删除原filter类上的注解。

2).将原Servlet类修改成方法注册形式。

3).修改启动类，如下
<pre class="lang:java decode:true ">package xyz.xioaxin12.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import xyz.xioaxin12.springboot.filter.MyFilter;
import xyz.xioaxin12.springboot.servlet.MyServlet;

@SpringBootApplication
public class SpringBoot02FilterApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot02FilterApplication.class, args);
    }

    @Bean
    public ServletRegistrationBean getregistrationBean(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new MyServlet());
        bean.addUrlMappings("/hello");
        return bean;
    }

    @Bean
    public FilterRegistrationBean getfilterRegistrationBean(){
        FilterRegistrationBean bean = new FilterRegistrationBean(new MyFilter());
        bean.addUrlPatterns("/hello");
        return bean;
    }

}

</pre>
&nbsp;

还可以拦截多个请求：
<pre class="lang:default decode:true ">@WebFilter(filterName = "MyFilter",urlPatterns = {"*.do","*.jsp","/hello"})</pre>
&nbsp;
