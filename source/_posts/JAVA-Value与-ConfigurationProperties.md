---
title: '[JAVA]@Value与@ConfigurationProperties'
date: 2019-10-29 00:49:59
categories: JAVA
tags: java
---

有时重要的信息需要写在配置文件中，如application.yml或是如application.preperties，那么可以通过@Value或是@ConfigurationProperties获取到配置文件中的值。

---
## 使用
### 一、@Value
#### yml配置
```
person:
  name: 小信
  age: 20
```

#### controller
```
@RestController
@RequestMapping
public class PersonController {

    @Value("${person.name}")
    private String name;
    @Value("${person.age}")
    private Integer age;

    @GetMapping("getInfo")
    public String Info() {
        return name + ":" + age;
    }
}

```
#### 访问地址
http://127.0.0.1:8080/getInfo

响应 `小信:20`

---
*使用该方式可以发现，若配置多个，那么都需要使用@Value注入多次，显得较为繁琐*

---
### 二、@ConfigurationProperties
将配置文件中得属性与一个Bean相关联，实现类型安全的配置。

#### yml配置
```
student:
  name: 小信
  email: xiaoxin1218@qq.com
```

#### Bean实体
```
@ConfigurationProperties(prefix = "student")
public class StudentVO {
    private String name;
    private String email;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```
**通过prefix指定配置文件的前缀**

#### controller
```
@RestController
@RequestMapping
public class StudentController {
    @Autowired
    private StudentVO studentVO;

    @GetMapping("getStu")
    public String studentInfo() {
        return studentVO.getName() + "::" + studentVO.getEmail();
    }
}

```
**@Autowired可直接注入**
#### 访问地址
http://127.0.0.1:8080/getStu
响应 `小信::xiaoxin1218@qq.com`


