---
title: '[ERROR]Mybaits传递多个参数'
date: 2019-10-27 09:34:00
categories: ERROR
tags: error
---

### 异常信息如下：
```
2019-02-13 19:33:46.931 ERROR 1992 --- [p-nio-80-exec-3] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.binding.BindingException: Parameter 'username' not found. Available parameters are [arg1, arg0, param1, param2]] with root cause

org.apache.ibatis.binding.BindingException: Parameter 'username' not found. Available parameters are [arg1, arg0, param1, param2]
```

image
![](http://image.xiaoxinyes.club/Snipaste_2019-02-13_19-40-48.png)

---
### 原因在于用mybatis查询时，传递了两个参数。
```
Blogger selectByNameAndPassword(String username, String password);
```

而这样的话匹配不到，用注解@Param即可。
```
Blogger selectByNameAndPassword(@Param("username")String username, @Param("password") String password)
```

