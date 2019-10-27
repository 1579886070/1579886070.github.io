---
title: '[ERROR]SpringBoot+Mybatis一个错误'
date: 2019-10-27 09:37:46
categories: ERROR
tags: error
---

### 异常信息如下
```
java.sql.SQLNonTransientConnectionException: CLIENT_PLUGIN_AUTH is required
```
image
![](http://image.xiaoxinyes.club/Snipaste_2019-02-13_19-50-35.png)

---
### 解决方法
降低mysql-connector-java的版本即可，我自己试了下5.x的似乎都没问题。

#### 详细步骤
将pom.xml中的jdbc数据库驱动修改下。
原
```
 <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>provided</scope>
</dependency>
```
修改为
```
 <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.25</version>
</dependency>
```

#### 关于application.properties配置修改
原来(mysql6版本以上)
```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
修改为(mysql5)
```
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```
