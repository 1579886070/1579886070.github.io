---
title: '[Cloud]5-集成Consul'
date: 2020-03-21 17:53:36
categories: JAVA
tags: SpringCloud
---

# Consul
## 安装与启动
下载地址：[点击](https://www.consul.io/downloads.html)

我用windows测试的，解压有一个exe程序， cmd

启动：**consul agent -dev**

访问：**http://127.0.0.1:8500/ui/dc1/services**

---
# 编码
## 服务提供者
新增cloud-provider-payment8006

### pom
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
```

### application.yml
```
server:
  port: 8006

# 服务别名---consul注册中心名称
spring:
  application:
    name: consul-provider-payment
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

### controller
```
@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/consul")
    public String paymentzk() {
        return "springcloud with consul: " + serverPort + "\t" + UUID.randomUUID().toString();
    }
}
```
---

## 服务消费者
新增cloud-consumerconsul-order80

### pom
跟提供者服务一致

### application.yml
```
server:
  port: 80

# 服务别名---consul注册中心名称
spring:
  application:
    name: consul-consumer-order
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

### controller
```
@RestController
@Slf4j
public class OrderConsulController {
    private static final String INVOKE_URL = "http://consul-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/consul")
    public String paymentInfo(){
        String result = restTemplate.getForObject(INVOKE_URL + "/payment/consul", String.class);
        return result;
    }
}
```

### 测试
访问http://127.0.0.1/consumer/payment/consul 可以调用成功！