---
title: '[JAVA]缓存之ConcurrentMapCacheManager'
date: 2019-11-27 22:04:23
categories: JAVA
tags: java
---

我用mybatis+springboot默认的ConcurrentMapCacheManager缓存演示。maven依赖导入mybatis和web相关即可。

## 一、配置文件

这里显示打印sql,是方便查看调用方法时是不是走缓存拿去数据，如果是的则不会打印sql日志。
```
spring:
  datasource:
    password: 1579886070
    username: root
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&useSSL=false

mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #打印sql
```

---
## 二、sql
```
CREATE TABLE `student` (
  `id` int(11) NOT NULL,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

---
## 三、service和实现类
### service
```
package xyz.zx21.demo.service;

import xyz.zx21.demo.vo.Student;

import java.util.List;

public interface StudentService {
    Student add(Student student);

    boolean deleteById(int id);

    Student selectById(Student student);
}
```
### Impl

**@CachePut:新增或者将更新的数据存到缓存，名称为student,key为student的id。**

**@CacheEvic:删除key为id的缓存**

**@Cacheable:将student的id作为Key缓存到student中**

没有指定key的话，方法参数作为key存储到缓存中。
```
package xyz.zx21.demo.service.impl;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.CachePut;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;
import xyz.zx21.demo.mapper.StudentMapper;
import xyz.zx21.demo.service.StudentService;
import xyz.zx21.demo.vo.Student;

import java.util.List;

@Service
public class StudentServiceImlp implements StudentService {

    @Autowired
    private StudentMapper studentMapper;

    @Override
    @CachePut(value = "student", key = "#student.id")
    public Student add(Student student) {
        if(studentMapper.insertStudent(student)>0){
            return student;
        }
        return null;
    }

    @Override
    @CacheEvict(value = "student")
    public boolean deleteById(int id) {

        return studentMapper.deleteStudent(id);
    }
    
    @Override
    @Cacheable(value = "student", key = "#student.id")
    public Student selectById(Student student) {
        return studentMapper.selectById(student);
    }
}
```

## 四、controller
```
package xyz.zx21.demo.controller;

import org.apache.ibatis.annotations.Param;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.CachePut;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import xyz.zx21.demo.service.StudentService;
import xyz.zx21.demo.vo.Student;
import xyz.zx21.demo.vo.StudentVO;

@RestController
@RequestMapping
public class StudentController {
    @Autowired
    private StudentVO studentVO;

    @Autowired
    private StudentService studentService;

    /**
     * 添加数据
     *
     * @return
     */
    @GetMapping("addStu")
    public Object insertStudent(Student student) {
        return studentService.add(student);
    }

    /**
     * 根据id删除数据
     *
     * @param id
     * @return
     */
    @GetMapping("delete")
    public Object delete(@Param("id") int id) {
        return studentService.deleteById(id);
    }

    /**
     * 根据id查找
     *
     * @param student
     * @return
     */
    @GetMapping("selectOne")
    public Object selectOne(Student student) {
        return studentService.selectById(student);
    }
}
```

## 五、启动类
注意加上@EnableCaching注解，开启缓存支持。
```
package xyz.zx21.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cache.annotation.EnableCaching;

@SpringBootApplication
@EnableCaching
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(DemoApplication.class);
        app.run(args);
    }

}
```

---
## 演示
### 1、添加数据存入缓存
请求：
`http://127.0.0.1:8080/addStu?id=3&name=小信&age=23`

返回：
```
{
    "id": 3,
    "name": "小信",
    "age": 23
}
```

idea日志：
```
JDBC Connection [HikariProxyConnection@1383776946 wrapping com.mysql.jdbc.JDBC4Connection@59329e8b] will not be managed by Spring
==>  Preparing: insert into student(id,name,age) values (?,?,?) 
==> Parameters: 3(Integer), 小信(String), 23(Integer)
<==    Updates: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@668ffe2e]
```

### 2、根据id去缓存查数据
请求：
`http://127.0.0.1:8080/selectOne?id=3`

返回：
```
{
    "id": 3,
    "name": "小信",
    "age": 23
}
```

idea日志：
```
2019-11-27 22:45:23.211 DEBUG 10040 --- [nio-8080-exec-8] o.s.web.servlet.DispatcherServlet        : GET "/selectOne?id=3", parameters={masked}
2019-11-27 22:45:23.211 DEBUG 10040 --- [nio-8080-exec-8] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped to xyz.zx21.demo.controller.StudentController#selectOne(Student)
2019-11-27 22:45:23.213 DEBUG 10040 --- [nio-8080-exec-8] m.m.a.RequestResponseBodyMethodProcessor : Using 'application/json;q=0.8', given [text/html, application/xhtml+xml, image/webp, image/apng, application/signed-exchange;v=b3, application/xml;q=0.9, */*;q=0.8] and supported [application/json, application/*+json, application/json, application/*+json]
2019-11-27 22:45:23.213 DEBUG 10040 --- [nio-8080-exec-8] m.m.a.RequestResponseBodyMethodProcessor : Writing [Student{id=3, name='小信', age=23}]
2019-11-27 22:45:23.214 DEBUG 10040 --- [nio-8080-exec-8] o.s.web.servlet.DispatcherServlet        : Completed 200 OK
```

> 可以看到它并没有去打印sql语句，直接从缓存取的数据

- 如果查询数据，指定为其他id，第一查询会走数据库，sql会打印到控制台，第二次再同样的请求则直接走缓存。