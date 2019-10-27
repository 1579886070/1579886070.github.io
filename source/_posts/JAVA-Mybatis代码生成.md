---
title: '[JAVA]Mybatis代码生成'
date: 2019-10-27 09:43:26
categories: JAVA
tags: java
---

generatorConfig.xml

### 首先在pom.xml中添加插件依赖
```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        <!-- mybatis generator 自动生成代码插件 -->
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.1</version>
            <configuration>
                <configurationFile>${basedir}/src/main/resources/generator/generatorConfig.xml</configurationFile>
                <overwrite>true</overwrite>
                <verbose>true</verbose>
            </configuration>
        </plugin>

    </plugins>
</build>
```

### 新建generatorConfig.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!-- 数据库驱动:选择你的本地硬盘上面的数据库驱动包-->
    <classPathEntry  location="E:\SSH_SSM\mysql-connector-java-5.1.44\mysql-connector-java-5.1.44\mysql-connector-java-5.1.44-bin.jar"/>
    <context id="DB2Tables"  targetRuntime="MyBatis3">
        <commentGenerator>
            <property name="suppressDate" value="true"/>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="false"/>
        </commentGenerator>
        <!--数据库连接驱动类,URL，用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://127.0.0.1/blogs" userId="root" password="密码">
        </jdbcConnection>
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>
        <!-- 生成(实体)模型的包名和位置-->
        <javaModelGenerator targetPackage="xyz.xioaxin12.blogs.entity" targetProject="E:\Idea Project\blogs\src\main\java">
            <property name="enableSubPackages" value="true"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <!-- 生成XML映射文件的包名和位置-->
        <sqlMapGenerator targetPackage="resources.mapper" targetProject="E:\Idea Project\blogs\src\main">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>
        <!-- 生成DAO接口的包名和位置-->
        <javaClientGenerator type="XMLMAPPER" targetPackage="xyz.xioaxin12.blogs.dao" targetProject="E:\Idea Project\blogs\src\main\java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>
        <!-- 要生成的表 tableName是数据库中的表名或视图名 domainObjectName是实体类名-->
        <table tableName="t_blog" domainObjectName="Blog" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>        <table tableName="t_blog" domainObjectName="Blog" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>
        <table tableName="t_blogger" domainObjectName="Blogger" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>        <table tableName="t_blog" domainObjectName="Blog" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>
        <table tableName="t_blogtype" domainObjectName="BlogType" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>        <table tableName="t_blog" domainObjectName="Blog" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>
        <table tableName="t_comment" domainObjectName="Comment" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>        <table tableName="t_blog" domainObjectName="Blog" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>
        <table tableName="t_link" domainObjectName="Link" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>        <table tableName="t_blog" domainObjectName="Blog" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>
    </context>
</generatorConfiguration>
```
配置信息，包的所在位置等自行更改。

---
idea执行步骤，run–>Edit Configurations–>添加maven–>mybatis-generator:generate -e

![](http://image.xiaoxinyes.club/2019-02-13_15-18-24.png)

![](http://image.xiaoxinyes.club/2019-02-13_15-23-07.png)

最后生成即可
![](http://image.xiaoxinyes.club/Snipaste_2019-02-13_15-25-55.png)
