---
title: '[JAVA]Mybatis-Mapper动态代理'
date: 2019-10-21 18:54:33
categories: JAVA
tags: java
---

基于[<span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">[Mybatis增删改查]</span></span>]这篇文章基础上进行更改，详细步骤请先了解后在看本文。<!--more-->

## 1，新建一个工具类MyBatisUtils.java用于获取sqlSessionFactory对象

<pre class="lang:java decode:true">package xyz.xioaxin12.utils;
/**
 * @author 小信
 */
public class MyBatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    public static SqlSession getSqlsession(){
        try {
            InputStream inputStream = Resources.getResourceAsStream("mybatis.xml");
            if(sqlSessionFactory == null){
                sqlSessionFactory =  new SqlSessionFactoryBuilder().build(inputStream);
            }
            return sqlSessionFactory.openSession();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;

    }
}
</pre>

* * *

&nbsp;

## 2，删除StudentDaoImpl的接口实现类。

&nbsp;

## 3，修改测试类

<pre class="lang:java decode:true">package xyz.xioaxin12.test;

/**
 * @author 小信
 */
public class TestMybatis {
    private static StudentDao dao;

    private static SqlSession sqlSession;
    static{
        //获取配置接口
        sqlSession = MyBatisUtils.getSqlsession();
        dao = sqlSession.getMapper(StudentDao.class);
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
        sqlSession.commit();
        closeSqlsession();
    }

    public static void deleteStu(){
        dao.deleteStudent(1);
        sqlSession.commit();
        closeSqlsession();
    }

    public static void updateStu(){
        Student student = new Student("阿六",20);
        student.setId(2);
        dao.updateStudent(student);
        sqlSession.commit();
        closeSqlsession();
    }

    public static void selectStu(){
        List&lt;Student&gt; students = dao.selectStudent();
        for (Student student: students) {
            System.out.println(student.getId()+"-"+student.getName()+"-"+student.getAge());
        }
        closeSqlsession();

    }

    /**
     * 关闭资源
     */
    public static void closeSqlsession(){
        if(sqlSession!=null){
            sqlSession.close();
        }
    }
}
</pre>

* * *

&nbsp;

&nbsp;

## 注意

*   **mapper映射文件namespace为接口的全类名；**
*   **接口中的每个方法名分别与mapper映射文件每个处理数据的id一致。**
