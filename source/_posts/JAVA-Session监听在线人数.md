---
title: '[JAVA]Session监听在线人数'
date: 2019-10-21 19:31:03
categories: JAVA
tags: java
---

统计网站在线人数，我们可以判断是否存在新的session被创建，创建则+1，销毁则-1。<!--more-->

首先建立监听类实现HttpSessionListener。监听session的创建和销毁。
sessionCreated--创建
sessionDestroyed--销毁
<pre class="lang:java decode:true ">package xyz.xioaxin12.listener;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class MyListener implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext context = se.getSession().getServletContext();
        Integer num = (Integer) context.getAttribute("num");
        if(num == null){
            context.setAttribute("num",1);
        }else {
            context.setAttribute("num",++num);
        }
        System.out.println(num);
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext context = se.getSession().getServletContext();
        Integer num = (Integer) context.getAttribute("num");
        if(num == null){
            context.setAttribute("num",1);
        }else {
            context.setAttribute("num",--num);
        }
        System.out.println(num);
    }
}</pre>

* * *

&nbsp;

web.xml 配置监听器
<pre class="lang:default decode:true ">  &lt;listener&gt;
    &lt;listener-class&gt;xyz.xioaxin12.listener.MyListener&lt;/listener-class&gt;
  &lt;/listener&gt;</pre>

* * *

&nbsp;

jsp El 接受数据
**在线人数:${num}**

&nbsp;

如图

[![](http://image.xiaoxinyes.club/2018-12-7.gif)](http://image.xiaoxinyes.club/2018-12-7.gif)

* * *

&nbsp;

session过期默认时间为30分钟。否则需要额外的操作，如点击按钮调用方法清除session等等。
