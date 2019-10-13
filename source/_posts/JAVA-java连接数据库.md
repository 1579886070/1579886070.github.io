---
title: '[JAVA]java连接数据库'
date: 2019-10-13 22:53:53
categories: JAVA
tags: java
---

Driver 是一个接口：数据库厂商必须提供实现得接口。能从其中获取数据库连接。可以通过Driver 的实现类对象获取数据库连接。

1，加入mysql驱动
1),解压mysql-connector-java-5.1.44
2),在当前项目下新建lib目录
3),把mysql-connector-java-5.1.44-bin.jar 复制到lib目录下
4),右键build-path，add to buildpath 加入到类路径下

**说明：博主用的mysql连接的，默认端口为3306，我修改过：为3360。**
<pre class="lang:java decode:true">public void testDriver() throws SQLException {
		//1,创建一个Driver实现类的对象
		Driver driver = new com.mysql.jdbc.Driver();

		//2,准备连接数据库的基本信息:url,user,password
		String url = "jdbc:mysql://127.0.0.1:3360/test";
		Properties info = new Properties();	
		info.put("user", "root");
		info.put("password", "1579886070");

		//3,调用Driver接口的 connect(url,info) 获取数据库连接
		Connection connection = driver.connect(url, info);
		System.out.println(connection);
	}</pre>
&nbsp;

* * *

**另一种方式，用读取配置文件的方式**
** 好处：这样提高了复用性，如要将MySql换成Oracle，只需要修改配置文件中的信息即可**

首先建立一个配置文件为：jdbc.properties

[![爱生活爱语录](http://image.xiaoxinyes.club/java_4_25_1.png)](http://image.xiaoxinyes.club/java_4_25_1.png)

然后使用类加载器加载bin目录 (类路径下的)文件
<pre class="lang:java decode:true ">public void testGetConnection2() throws Exception{
	//1.读取jdbc.properties
	Properties properties = new Properties();
	InputStream inStream = ReviewTest.class.getClassLoader().getResourceAsStream("jdbc.properties");

	properties.load(inStream);

	//2.获取连接的4个字符串
	String user = properties.getProperty("user");
	String password = properties.getProperty("password");
	String jdbcUrl = properties.getProperty("jdbcUrl");
	String driverClass = properties.getProperty("driverClass");

	//3.加载驱动		
	Class.forName(driverClass);

	//4.调用获取
	Connection connection = DriverManager.getConnection(jdbcUrl, user, password);
	System.out.println(connection);
}</pre>

* * *

**方法说明：**

*   <span style="font-size: 10pt;">getClassLoader()是取得该Class对象的类装载器;</span>
*   <span style="font-size: 10pt;">getResourceAsStream(“database.properties”) 调用类加载器的方法加载 资源，返回的是字节流。使用Properties类是为了可以从.properties属性文件对应的文件输入流中，加载属性列表到Properties类对象，然后通过getProperty方法用指定的键在此属性列表中搜索属性.</span>
*   <span style="font-size: 10pt;">getProperty ( String key)，用指定的键在此属性列表中搜索属性。也就是通过参数 key ，得到 key 所对应的 value。</span>
*   <span style="font-size: 10pt;">load ( InputStream inStream)，从输入流中读取属性列表（键和元素对）。</span>
