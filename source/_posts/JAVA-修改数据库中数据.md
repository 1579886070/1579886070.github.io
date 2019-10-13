---
title: '[JAVA]修改数据库中数据'
date: 2019-10-13 22:56:07
categories: JAVA
tags: java
---

首先，创建一个名为test的数据库，库中创建一个student的表。将修改表中NAME字段中的"Tom"。

**添加数据**

[![爱生活爱语录](http://image.xiaoxinyes.club/4_26_1.png)](http://image.xiaoxinyes.club/4_26_1.png)

* * *

&nbsp;

**我将连接数据库，调用数据库和关闭资源分了三个方法，
**
<pre class="lang:java decode:true">/*
* 连接数据库方法
 * */
public Connection getCconnection() throws Exception {
	//数据信息
	String user = "root";
	String password = "1579886070";
	String jdbcUrl = "jdbc:mysql://127.0.0.1:3360/test";
	String driverClass = "com.mysql.jdbc.Driver";

	//加载驱动
	Class.forName(driverClass);
	Connection connection = DriverManager.getConnection(jdbcUrl, user, password);
	return connection;
}</span></pre>

* * *

&nbsp;

**然后我们建立主方法，在其中调用**
<pre class="lang:java decode:true">
public void testUpdate(){
	Connection connection = null;
	Statement statement = null;
	try {
		//连接数据库
		connection = getCconnection();	
		//调用 Connection 对象的 createStatement() 方法获取Statement 对象
		statement = connection.createStatement();			
		//准备SQL语句
		String sql = "UPDATE student SET name = 'XiaoXin' WHERE id = 1";			
		//4.发送SQL语句：调用Statement 对象的 executeUpdate(sql)方法
		statement.executeUpdate(sql);		
	} catch (Exception e) {
		e.printStackTrace();
	}finally {
		//调用关闭资源方法
		getRelease(null, statement, connection);
	}	
}</span></pre>

* * *

&nbsp;

**在finally块中调用关闭资源的方法**
<pre class="lang:java decode:true">/*
* 关闭资源方法
* */
public void getRelease(ResultSet resultSet,Statement statement,Connection connection){
	if(resultSet != null){
		try {
			resultSet.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
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
}</span></pre>

* * *

&nbsp;

**运行，执行完后查看数据表**

[![爱生活爱语录](http://image.xiaoxinyes.club/4_26_2.png)](http://image.xiaoxinyes.club/4_26_2.png)

好啦，将原本NAME中的"Tom"修改为"XiaoXin"了。
