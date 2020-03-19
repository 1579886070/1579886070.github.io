---
title: '[Cloud]2-支付模块与工程重构'
date: 2020-03-19 16:29:40
categories: JAVA
tags: SpringCloud
---

**创建消费者模块cloud-comsumer-order80调用上篇文章的支付模块cloud-provider-payment8001。**

# 构建步骤
```
1、建module
2、改pom
3、写yml
4、主启动
5、业务类
```


# 编码
## application.yml
```
server:
  port: 80
```

## 主启动类
```
package xyz.zx21.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @author Administrator
 * @date 2020/3/19 15:53
 */
@SpringBootApplication
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class, args);
    }
}
```

## 业务类
**注入RestTemplate，文档地址**：[点击访问](https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html)
```
package xyz.zx21.springcloud.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * @author Administrator
 * @date 2020/3/19 16:39
 */
@Configuration
public class ApplicationContextConfig {
    /**
     * //appLicationcontext.xml <bean id=""class="">
     *
     */
    @Bean
    public RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```

## controller
```
package xyz.zx21.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;
import xyz.zx21.springcloud.entities.CommonResult;
import xyz.zx21.springcloud.entities.Payment;

import javax.annotation.Resource;

/**
 * @author Administrator
 * @date 2020/3/19 16:37
 */
@RestController
@Slf4j
public class OrderController {

    public static final String PAYMENT_URL = "http://localhost:8001";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment> create(Payment payment) {
        return restTemplate.postForObject(PAYMENT_URL + "/payment/create", payment, CommonResult.class);
    }

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id) {
        return restTemplate.getForObject(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
    }
}

```


# 测试
启动PaymentMain8001和OrderMain80，测试OrderMain80调用PaymentMain8001服务。

---

# 工程重构
**将多余的代码，单独放到一个模块，提供给各个服务，避免冗余，比如实体类**

## 新建cloud-api-commons模块
pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud2020</artifactId>
        <groupId>xyz.zx21.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-api-commons</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <!--        当optional为true则说明该依赖禁止依赖传递    <optional>true</optional>-->
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.1.1</version>
        </dependency>
    </dependencies>

</project>
```

## 将其他服务的实体类拷贝
## maven clean install 安装
## 删除其他模块下的实体包
## 新增依赖
```
<dependency>
    <artifactId>cloud-api-commons</artifactId>
    <groupId>xyz.zx21.springcloud</groupId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

---

# TIPS
关于调用/consumer/payment/create接口，插入数据为null的情况，需要在cloud-provider-payment8001服务对应的controller添加@RequestBody注解
```
@PostMapping(value = "/payment/create")
public CommonResult create(@RequestBody Payment payment) {...}
```