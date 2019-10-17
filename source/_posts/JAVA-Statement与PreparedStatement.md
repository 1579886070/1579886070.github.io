---
title: '[JAVA]Statement与PreparedStatement'
date: 2019-10-16 18:43:08
categories: JAVA
tags: java
---

Statement和PreparedStatement都是用来执行sql语句的,那我们在使用的时候选择谁呢？

### Statement

<pre class="lang:java decode:true ">public class JdbcDemo {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        try {
            connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/test?characterEncoding=UTF-8",
                    "root", "1579886070");
            statement = connection.createStatement();
            String sql = "insert into student values(null,"+"'小信'"+","+"18)";
            statement.execute(sql);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        finally {
            if(statement != null){
                try {
                    statement.close();
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
}
</pre>

* * *

&nbsp;

### PreparedStatement

<pre class="lang:java decode:true ">public class JdbcDemo {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        try {
            connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/test?characterEncoding=UTF-8",
                    "root", "1579886070");
            String sql = "insert into student values(null,?,?)";
            preparedStatement = connection.prepareStatement(sql);

            //设置参数值
            preparedStatement.setString(1,"小天");
            preparedStatement.setInt(2,20);
            //执行
            preparedStatement.execute();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        finally {
            if(preparedStatement != null){
                try {
                    preparedStatement.close();
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

&nbsp;

以上分别用Statement和PreparedStatement实现数据库插入数据的案例。

对比明显总结：

*   <span style="font-size: 12pt;">**Statement使用繁琐的字符串拼接，不但不易阅读，当字段比较多时容易出错。可读性与维护性都不好;**</span>
*   <span style="font-size: 12pt;">**PreparedStatement使用设置参数的形式，简单易读，不易出错。**</span>
其他优点：

*   **PreparedStatement执行效率高于Statement，假设一次要插入10条数据，那么Statement需要执行10次，把10次全部传输到数据库端。**
*   **PreparedStatement防止Sql注入。**
