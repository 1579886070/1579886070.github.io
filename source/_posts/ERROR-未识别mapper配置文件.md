---
title: '[ERROR]未识别mapper配置文件'
date: 2019-10-27 08:58:34
categories: ERROR
tags: error
---

学习就是从错误中不断吸取知识!在springboot+mybatis整合中，发现一个异常。

## 如下
```
context with path [] threw exception [Request processing failed; nested exception is org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): xyz.xioaxin12.mapper.StudentMapper.insetStudent] with root cause
org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): xyz.xioaxin12.mapper.StudentMapper.insetStudent
```
![](http://image.xiaoxinyes.club/2019-01-12_142810.png)
<br/>
发现并没有调用到mapper.xml文件，而我是将mapper.xml和接口写在同包下。
![](http://image.xiaoxinyes.club/2019-01-12_143343.png)
<br/>
发现别名，路径等信息无误之后，我注意到classes文件，果然编译后的xml并那个不存在。
![](http://image.xiaoxinyes.club/2019-01-12_143633.png)
<br/>
## 解决方法
pom.xml中加入以下代码,这样不论是java下还是resourecs下编译后都会加载进去。
```
resources>
        <resource>
            <directory>src/main/resources</directory>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
</resources>
```

若放在指定的resources文件下，需要指定mapper的位置。
```
mybatis.mapper-locations=classpath:mapper/*.xml
```