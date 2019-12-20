---
title: '[JAVA]Mybatis增删改查'
date: 2019-10-21 18:51:44
categories: JAVA
tags: java
---

建立test数据库，student_数据表，含有id，name，age字段。<!--more-->

## sql

```
CREATE TABLE `student_`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `age` int(11) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
)
```

* * *

&nbsp;

## 定义实体类Student.java

```
package xyz.xioaxin12.bean;

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
```

* * *

&nbsp;

## 定义StudentDao接口，含增删改查方法

```
package xyz.xioaxin12.dao;

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
```

* * *

&nbsp;

## StudentDao的实现类StudentDaoImpl

```
package xyz.xioaxin12.dao;

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
```

* * *

&nbsp;

## 映射文件mapper.xml,放在dao包下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="xyz.xioaxin12.dao.StudentDao">
    <insert id="insertStudent" parameterType="Student">
        insert into student_(name,age) values (#{name},#{age})
    </insert>
    <delete id="deleteStudent">
        delete from student_ where id=#{id}
    </delete>
    <update id="updateStudent">
        update student_ set name=#{name},age=#{age} where id=#{id}
    </update>
    <select id="selectStudentId" resultType="Student">
        select id,name,age from student_ where id=#{id}
    </select>
    <select id="selectStudent" resultType="Student">
        select * from student_
    </select>
</mapper>
```
**<span style="font-size: 10pt;">id跟StudentDaoImpl中的参数一致</span>**

* * *

&nbsp;

## 主配置文件mybatis.xml

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="xyz.xioaxin12.dao.StudentDao">
    <insert id="insertStudent" parameterType="Student">
        insert into student_(name,age) values (#{name},#{age})
    </insert>
    <delete id="deleteStudent">
        delete from student_ where id=#{id}
    </delete>
    <update id="updateStudent">
        update student_ set name=#{name},age=#{age} where id=#{id}
    </update>
    <select id="selectStudentId" resultType="Student">
        select id,name,age from student_ where id=#{id}
    </select>
    <select id="selectStudent" resultType="Student">
        select * from student_
    </select>
</mapper>
```
**<span style="font-size: 10pt;">&lt;package name="xyz.xioaxin12.bean"/&gt; ：定义别名，该包中的实体类简单类名作为别名</span>**

**<span style="font-size: 10pt;">&lt;environments&gt;...&lt;/environments&gt;：运行环境，提供驱动，账号，密码，数据库信息。</span>**

**<span style="font-size: 10pt;">&lt;mapper resource="xyz/xioaxin12/dao/mapper.xml"/&gt;： 映射到mapper.xml文件</span>**

* * *

&nbsp;

## logo4j.properties日志文件

```
log4j.logger.xyz.xioaxin12.dao.StudentDao=DEBUG, stdout  

log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
log4j.appender.stdout.layout.ConversionPattern=%d [%-5p] %c - %m%n
#show sql
log4j.logger.java.sql.ResultSet=INFO   
log4j.logger.org.apache=INFO   
log4j.logger.java.sql.Connection=DEBUG   
log4j.logger.java.sql.Statement=DEBUG   
log4j.logger.java.sql.PreparedStatement=DEBUG
```

* * *

&nbsp;

## 测试类TestMybaties.java

```
package xyz.xioaxin12.test;

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
```


&nbsp;

<span style="font-size: 12pt;">_本案例用了dao接口的实现类。可以删除以mapper动态代理的方式完成，无需实现dao接口。_</span>
