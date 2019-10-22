---
title: '[JAVA]SpringBoot整合Listener'
date: 2019-10-22 22:39:00
categories: JAVA
tags: java
---

还是两种方式，注解与方法注册。<!--more-->

## 1.注解形式

很简单，添加@WebListener()即可。
<pre class="lang:java decode:true">package xyz.xioaxin12.springboot.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener()
public class MyListener implements ServletContextListener{
    @Override
    public void contextDestroyed(ServletContextEvent sce) {

    }

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("listener...init......");
    }
}
</pre>
&nbsp;

启动类

添加@ServletComponentScan就行了。
<pre class="lang:java decode:true">package xyz.xioaxin12.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;
import org.springframework.boot.web.servlet.ServletListenerRegistrationBean;
import org.springframework.context.annotation.Bean;
import xyz.xioaxin12.springboot.listener.MyListener;

@SpringBootApplication
@ServletComponentScan
public class SpringBoot02FilterApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot02FilterApplication.class, args);
    }
}

</pre>
这样在项目启动的时候就会打印listener...init......

* * *

&nbsp;

## 2.方法注册

1).删除MyListener上的注解。

2).修改启动类。

在原启动类上添加如下代码即可!
<pre class="lang:java decode:true">@Bean                                                                                                                      
public ServletListenerRegistrationBean&lt;MyListener&gt; getListenerServletListenerRegistrationBean(){                           
    ServletListenerRegistrationBean&lt;MyListener&gt; bean = new ServletListenerRegistrationBean&lt;MyListener&gt;(new MyListener());  
   return bean;                                                                                                            
}</pre>
&nbsp;
