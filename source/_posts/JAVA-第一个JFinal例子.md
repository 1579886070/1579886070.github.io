---
title: '[JAVA]第一个JFinal例子'
date: 2019-10-21 19:28:54
categories: JAVA
tags: java
---

通过IDEA+Maven下搭建。<!--more-->

首先创建一个maven项目。

![爱生活爱技术](http://image.xiaoxinyes.club/2018-12-02_162449.png)

* * *

&nbsp;

下一步下一步，环境搭建好之后，开始添加maven依赖。
<pre class="lang:default decode:true">&lt;dependency&gt;
    &lt;groupId&gt;com.jfinal&lt;/groupId&gt;
    &lt;artifactId&gt;jfinal-undertow&lt;/artifactId&gt;
    &lt;version&gt;1.1&lt;/version&gt;
&lt;/dependency&gt;</pre>

* * *

&nbsp;

创建DemoConfig类继承JFinalConfig。
<pre class="lang:default decode:true" title="DemoConfig">package xyz.xioaxin12.demo;

import com.jfinal.config.*;
import com.jfinal.server.undertow.UndertowServer;
import com.jfinal.template.Engine;
import xyz.xioaxin12.controller.HelloController;

public class DemoConfig extends JFinalConfig {
    public static void main(String[] args) {
        UndertowServer.start(DemoConfig.class,80,true);
    }
    @Override
    public void configConstant(Constants me) {
        me.setDevMode(true);
    }

    @Override
    public void configRoute(Routes me) {
        me.add("/hello",HelloController.class);
    }

    @Override
    public void configEngine(Engine me) {

    }

    @Override
    public void configPlugin(Plugins me) {

    }

    @Override
    public void configInterceptor(Interceptors me) {

    }

    @Override
    public void configHandler(Handlers me) {

    }
}
</pre>
&nbsp;

HelloController类
<pre class="lang:default decode:true " title="HelloController ">package xyz.xioaxin12.controller;

import com.jfinal.core.Controller;

public class HelloController extends Controller {
    public void index(){
        renderText("hello JFinal!");
    }
}
</pre>
&nbsp;

启动访问localhost/hello即可，如下：

[![](http://image.xiaoxinyes.club/2018-12-02_165651.png)](http://image.xiaoxinyes.club/2018-12-02_165651.png)

通过阅读官方文档，HelloController类可以使用注解，如下：
<pre class="lang:default decode:true ">public class HelloController extends Controller {
    @ActionKey("/test")
    public void index(){
        renderText("hello JFinal!");
    }
}</pre>
&nbsp;

访问的路径则是localhost/test，但是发现配置了注解，configRoute下的映射路径即失效了，但是又不能不写，所以感觉注解并没有多大的用处。
<pre class="lang:default decode:true ">public void configRoute(Routes me) {
    me.add("/hello",HelloController.class);
}</pre>
&nbsp;

发现该框架配置起来的确很简便。

JFinal官网：[http://www.jfinal.com/](http://www.jfinal.com/)
