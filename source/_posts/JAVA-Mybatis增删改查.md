---
title: '[JAVA]Mybatis增删改查'
date: 2019-10-21 18:51:44
categories: JAVA
tags: java
---

建立test数据库，student_数据表，含有id，name，age字段。<!--more-->

## sql

<pre class="lang:default decode:true">CREATE TABLE `student_`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `age` int(11) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
)</pre>

* * *

&nbsp;

## 定义实体类Student.java

<pre class="lang:java decode:true">package xyz.xioaxin12.bean;

/**
 * @author 小信
 */
public class Student {
    private Integer id;
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
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

## 定义StudentDao接口，含增删改查方法

<pre class="lang:java decode:true">package xyz.xioaxin12.dao;

import xyz.xioaxin12.bean.Student;

/**
 * @author 小信
 */
public interface StudentDao {

    void insertStudent(Student student);

    void deleteStudent(int id);

    void updateStudent(Student student);

    List&lt;Student&gt; selectStudent();
}
</pre>

* * *

&nbsp;

## StudentDao的实现类StudentDaoImpl

<pre class="lang:java decode:true">package xyz.xioaxin12.dao;

/**
 * @author 小信
 */
public class StudentDaoImpl implements StudentDao {

    private SqlSession sqlSession;

    @Override
    public void insertStudent(Student student) {
        try {
            InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            sqlSession=sqlSessionFactory.openSession();
            sqlSession.insert("insertStudent",student);
            sqlSession.commit();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(sqlSession != null){
                sqlSession.close();
            }
        }

    }

    @Override
    public void deleteStudent(int id) {
        try {
            InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            sqlSession=sqlSessionFactory.openSession();
            sqlSession.insert("deleteStudent",id);
            sqlSession.commit();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(sqlSession != null){
                sqlSession.close();
            }
        }
    }

    @Override
    public void updateStudent(Student student) {
        try {
            InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            sqlSession=sqlSessionFactory.openSession();
            sqlSession.update("updateStudent",student);
            sqlSession.commit();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(sqlSession != null){
                sqlSession.close();
            }
        }
    }

    @Override
    public List&lt;Student&gt; selectStudent() {
        List&lt;Student&gt; students = null;
        try {
            InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            sqlSession=sqlSessionFactory.openSession();
            students = sqlSession.selectList("selectStudent");
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(sqlSession != null){
                sqlSession.close();
            }
        }
        return students;
    }
}
</pre>

* * *

&nbsp;

## 映射文件mapper.xml,放在dao包下

<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;
&lt;mapper namespace="xyz.xioaxin12.dao.StudentDao"&gt;
    &lt;insert id="insertStudent" parameterType="Student"&gt;
        insert into student_(name,age) values (#{name},#{age})
    &lt;/insert&gt;
    &lt;delete id="deleteStudent"&gt;
        delete from student_ where id=#{id}
    &lt;/delete&gt;
    &lt;update id="updateStudent"&gt;
        update student_ set name=#{name},age=#{age} where id=#{id}
    &lt;/update&gt;
    &lt;select id="selectStudentId" resultType="Student"&gt;
        select id,name,age from student_ where id=#{id}
    &lt;/select&gt;
    &lt;select id="selectStudent" resultType="Student"&gt;
        select * from student_
    &lt;/select&gt;
&lt;/mapper&gt;</pre>
**<span style="font-size: 10pt;">id跟StudentDaoImpl中的参数一致</span>**

* * *

&nbsp;

## 主配置文件mybatis.xml

<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;
&lt;configuration&gt;
    &lt;!--定义类型别名--&gt;
    &lt;typeAliases&gt;
        &lt;package name="xyz.xioaxin12.bean"/&gt;
    &lt;/typeAliases&gt;

    &lt;!--配置运行环境--&gt;
    &lt;environments default="test"&gt;
        &lt;environment id="test"&gt;
            &lt;transactionManager type="JDBC"&gt;&lt;/transactionManager&gt;
            &lt;dataSource type="POOLED"&gt;
                &lt;property name="driver" value="com.mysql.jdbc.Driver"/&gt;
                &lt;property name="url" value="jdbc:mysql://localhost:3306/test?characterEncoding=UTF-8"/&gt;
                &lt;property name="username" value="root"/&gt;
                &lt;property name="password" value="1579886070"/&gt;
            &lt;/dataSource&gt;
        &lt;/environment&gt;
    &lt;/environments&gt;
    &lt;mappers&gt;
        &lt;mapper resource="xyz/xioaxin12/dao/mapper.xml"/&gt;
    &lt;/mappers&gt;
&lt;/configuration&gt;</pre>
**<span style="font-size: 10pt;">&lt;package name="xyz.xioaxin12.bean"/&gt; ：定义别名，该包中的实体类简单类名作为别名</span>**

**<span style="font-size: 10pt;">&lt;environments&gt;...&lt;/environments&gt;：运行环境，提供驱动，账号，密码，数据库信息。</span>**

**<span style="font-size: 10pt;">&lt;mapper resource="xyz/xioaxin12/dao/mapper.xml"/&gt;： 映射到mapper.xml文件</span>**

* * *

&nbsp;

## logo4j.properties日志文件

<pre class="lang:default decode:true">log4j.logger.xyz.xioaxin12.dao.StudentDao=DEBUG, stdout  

log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
log4j.appender.stdout.layout.ConversionPattern=%d [%-5p] %c - %m%n
#show sql
log4j.logger.java.sql.ResultSet=INFO   
log4j.logger.org.apache=INFO   
log4j.logger.java.sql.Connection=DEBUG   
log4j.logger.java.sql.Statement=DEBUG   
log4j.logger.java.sql.PreparedStatement=DEBUG</pre>

* * *

&nbsp;

## 测试类TestMybaties.java

<pre class="lang:default decode:true ">package xyz.xioaxin12.test;

import xyz.xioaxin12.bean.Student;
import xyz.xioaxin12.dao.StudentDao;
import xyz.xioaxin12.dao.StudentDaoImpl;

/**
 * @author 小信
 */
public class TestMybatis {
    private static StudentDao dao;
    static{
        dao = new StudentDaoImpl();
    }
    public static void main(String[] args) {
        //增
        addStu();
        //删
//        deleteStu();
        //改
//        updateStu();
        //查
//       selectStu();
    }
    public static void addStu(){
        Student student = new Student("小信",19);
        dao.insertStudent(student);
    }

    public static void deleteStu(){
        dao.deleteStudent(1);
    }

    public static void updateStu(){
        Student student = new Student("阿六",20);
        student.setId(2);
        dao.updateStudent(student);
    }

    public static void selectStu(){
        List&lt;Student&gt; students = dao.selectStudent();
        for (Student student: students
             ) {
            System.out.println(student.getId()+"-"+student.getName()+"-"+student.getAge());
        }

    }
}
</pre>
&nbsp;

&nbsp;

&nbsp;

&nbsp;

<span style="font-size: 12pt;">_本案例用了dao接口的实现类。可以删除以mapper动态代理的方式完成，无需实现dao接口。_</span>
