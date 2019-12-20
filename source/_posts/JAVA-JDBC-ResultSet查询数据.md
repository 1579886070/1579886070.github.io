---
title: '[JAVA]JDBC-ResultSet查询数据'
date: 2019-10-17 19:35:34
categories: JAVA
tags: java
---

executeQuery 执行SQL语句

```
public class JdbcDemo {
    public static void main(String[] args) {
        Connection connection = null;
        Statement Statement = null;
        try {
//注册驱动
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        try {
//获取连接
            connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/test?characterEncoding=UTF-8",
                    "root", "1579886070");
            Statement = connection.createStatement();
            String sql = "select *from student";
            //返回结果集
            ResultSet resultSet = Statement.executeQuery(sql);
//遍历集合
            while (resultSet.next()){
                //可以用字段名
                int id = resultSet.getInt("id");
                //可以用字段的位置
                String name = resultSet.getString(2);
                int age = resultSet.getInt("age");
                System.out.println("id:"+id+",name:"+name+",age:"+age);
            }
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
}
```
&nbsp;

遍历时可以使用字段名或者字段的顺序。
