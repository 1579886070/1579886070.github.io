---
title: '[JAVA]Cookie常用API'
date: 2019-10-14 19:17:53
categories: JAVA
tags: java
---

Cookie：访问Servlet，记录访问的信息，关闭浏览器后再次访问，前面的记录的信息自动销毁了。

**会话级别**：（<span style="color: #ff0000;">默认级别</span>）浏览器访问某个站点，到关闭这个浏览器的整个过程，未一次会话。只要关闭了浏览器，cookie也将销毁。
**持久级别**：将cookie保存到硬盘上。下次再次打开浏览器，cookie不会消失。

&nbsp;

## 创建cookie

<span style="color: #ff0000;">**Cookie cookie = new Cookie(String cookieName,String cookieValue);**</span>

举例

<span style="color: #ff0000;">**Cookie cookie = new Cookie("name","xiaoxin");**</span>

&nbsp;


## 常用的方法：

*   设置Cookie在客户端的持久化时间,单位:秒。时间设置0,代表删除cookie
<span style="color: #ff0000;">**cookie.setMaxAge(60);**</span>
&nbsp;

*   设置Cookie的携带路径,访问这个路径时才携带cookie
**<span style="color: #ff0000;">cookie.setPath(request.getContextPath()+"/setcookie");</span>**
&nbsp;

*   向客户端发送cookie
<span style="color: #ff0000;">**response.addCookie(cookie);**</span>
&nbsp;

&nbsp;

## 注意：

*   若不设置持久化时间，cookie默认存储在浏览器的内存当中，当退出浏览器，cookie信息销毁。
*   手动删除cookie，覆盖同名同路径，将持久化时间设置成0即可。
