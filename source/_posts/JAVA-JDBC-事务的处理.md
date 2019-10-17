---
title: '[JAVA]JDBC-事务的处理'
date: 2019-10-17 19:39:51
categories: JAVA
tags: java
---

假设我现在有1000元，借给王五500，同时赵六还钱给我500，那么现在我所拥有的钱还是1000。若是没有事务会出现怎样的情况呢？假设在赵六还钱时出现了错误，那么我借出了500，而未收到500，现在自己只有500了。
<pre class="lang:java decode:true">public class JdbcDemo {
    public static void main(String[] args) {
        Connection connection = null;
        Statement Statement = null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        try {
            connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/test?characterEncoding=UTF-8",
                    "root", "1579886070");
            //手动提交
            connection.setAutoCommit(false);
            Statement = connection.createStatement();
            String sql1 = "update moneys set money=money-500 where name='小信'";
            Statement.execute(sql1);
            String sql2 = "update moneys set money=money+500 where name='小信'";
            Statement.execute(sql2);
            //提交事务
            connection.commit();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        finally {
            if(Statement != null){
                try {
                    Statement.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if(connection != null){
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}</pre>
&nbsp;

connection.setAutoCommit(false);关闭自动提交，开启事务。
connection.commit();手动提交。

这样的话，多个数据操作时，成功一起成功，有任意一条数据操作失败便不会提交。

假如第二条SQL语句update moneys set money=money+500 where name='小信',其中的单词写错，没使用事务的话，就只会执行第一条sql语句，造成非期待的结果。
