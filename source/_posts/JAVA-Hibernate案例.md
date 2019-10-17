---
title: '[JAVA]Hibernate案例'
date: 2019-10-17 19:41:58
categories: JAVA
tags: java
---

Hibernate是操作数据库的框架，实现了对JDBC的封装。

## 第一步

建立test数据库，准备表为student_，字段为id,name,age,结构如下
<pre class="lang:default decode:true">DROP TABLE IF EXISTS `student_`;
CREATE TABLE `student_`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `age` int(11) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
)
</pre>

* * *

&nbsp;

## 第二步，创建java项目，导入jar包。

[![](http://image.xiaoxinyes.club/2018-09-02_1.png)](http://image.xiaoxinyes.club/2018-09-02_1.png)

* * *

&nbsp;

## 第三步

实体类
<pre class="lang:java decode:true ">package xyz.xioaxin12.bean;

/**
 * @author 小信
 */
public class Student {
    private int id;
    private String name;
    private int age;

    public Student(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public Student() {

    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
</pre>

* * *

&nbsp;

## 第四步

实体类同包下建立Student.hbm.xml映射文件
<pre class="lang:default decode:true ">&lt;?xml version="1.0"?&gt;
&lt;!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd"&gt;
&lt;hibernate-mapping package="xyz.xioaxin12.bean"&gt;
    &lt;class name="Student" table="student_"&gt;
        &lt;id name="id" column="id"&gt;
            &lt;generator class="native"&gt;&lt;/generator&gt;
        &lt;/id&gt;
        &lt;property name="name"/&gt;
        &lt;property name="age"/&gt;
    &lt;/class&gt;
&lt;/hibernate-mapping&gt;</pre>
&nbsp;

<span style="font-size: 10pt; color: #ff0000;">Student.hbm.xml文件首字母大写，与类名一致。</span>

<span style="font-size: 10pt; color: #ff0000;">&lt;class name="Student" table="student_"&gt; ：类名对应表名。</span>

* * *

&nbsp;

## 第五步

src目录下建立hibernate.cfg.xml用于配置数据
<pre class="lang:default decode:true ">&lt;?xml version='1.0' encoding='utf-8'?&gt;
&lt;!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd"&gt;
&lt;!--数据库配置--&gt;
&lt;hibernate-configuration&gt;

    &lt;session-factory&gt;
        &lt;!-- Database connection settings --&gt;
        &lt;property name="connection.driver_class"&gt;com.mysql.jdbc.Driver&lt;/property&gt;
        &lt;property name="connection.url"&gt;jdbc:mysql://localhost:3306/test?characterEncoding=UTF-8&lt;/property&gt;
        &lt;property name="connection.username"&gt;root&lt;/property&gt;
        &lt;property name="connection.password"&gt;1579886070&lt;/property&gt;
        &lt;!-- SQL dialect --&gt;
        &lt;property name="dialect"&gt;org.hibernate.dialect.MySQLDialect&lt;/property&gt;
        &lt;!--Hibernate事务管理方式，即每个线程一个事务--&gt;
        &lt;property name="current_session_context_class"&gt;thread&lt;/property&gt;
        &lt;!--这表示是否在控制台显示执行的sql语句--&gt;
        &lt;property name="show_sql"&gt;true&lt;/property&gt;
        &lt;!--自动更新数据库的表结构--&gt;
        &lt;property name="hbm2ddl.auto"&gt;update&lt;/property&gt;
        &lt;mapping resource="xyz/xioaxin12/bean/Student.hbm.xml" /&gt;
    &lt;/session-factory&gt;

&lt;/hibernate-configuration&gt;</pre>
前面包括加载驱动，账号密码等。其他含义看注释。

* * *

&nbsp;

## 第六步

测试类TestStudentHibernate
<pre class="lang:java decode:true">package xyz.xioaxin12.test;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
import org.hibernate.classic.Session;
import xyz.xioaxin12.bean.Student;

/**
 * @author 小信
 */
public class TestStudentHibernate {
    public static void main(String[] args) {
        SessionFactory sessionFactory = new Configuration().configure().buildSessionFactory();
        Session session = sessionFactory.openSession();

        session.beginTransaction();
        Student student = new Student();
        student.setName("小信");
        student.setAge(19);

        session.save(student);
        session.getTransaction().commit();

        session.close();
        sessionFactory.close();
    }
}
</pre>
**实现步骤**：

<span style="color: #ff0000;">1，获取SessionFactory ;</span>
<span style="color: #ff0000;">2，通过SessionFactory获取Session;</span>
<span style="color: #ff0000;">3，开启事务;</span>
<span style="color: #ff0000;">4，保存到数据库;</span>
<span style="color: #ff0000;">5，提交;</span>
<span style="color: #ff0000;">6，关闭Session和SessionFactory</span>

* * *

&nbsp;

## 第七步

查看数据表

[![](http://image.xiaoxinyes.club/2018-09-02_2.png)](http://image.xiaoxinyes.club/2018-09-02_2.png)
