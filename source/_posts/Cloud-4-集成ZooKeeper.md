---
title: '[Cloud]4-集成ZooKeeper'
date: 2020-03-21 11:08:48
categories: JAVA
tags: SpringCloud
---

**本机与虚拟机测试，注意：保证本机与虚拟机能互相ping通**

# zookeepper命令
```
1. 启动ZK服务:       zkServer.sh start
2. 查看ZK服务状态:   zkServer.sh status
3. 停止ZK服务:       zkServer.sh stop
4. 重启ZK服务:       zkServer.sh restart
5. ZK服务连接:       zkCli.sh
```

---
# 新建服务提供者
## cloud-provider-payment8004
引入zookeeper相关的依赖
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

    <artifactId>cloud-provider-payment8004</artifactId>
    <dependencies>
        <dependency>
            <artifactId>cloud-api-commons</artifactId>
            <groupId>xyz.zx21.springcloud</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <!--   引入zookeeper客户端     -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.zookeeper</groupId>
                    <artifactId>zookeeper</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.10</version>
        </dependency>
    </dependencies>
</project>
```

## application.yml
```
server:
  port: 8004

# 服务别名---注册zookeeper到注册中心名称
spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 192.168.8.208:2181
      max-retries: 10
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://127.0.0.1:3306/db2019?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456
```

## controller测试
```
@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/zk")
    public String paymentzk() {
        return "springcloud with zookeeper: " + serverPort + "\t" + UUID.randomUUID().toString();
    }
}
```

## 主启动类
```
/**
 * @author Administrator
 * @date 2020/3/19 14:10
 *
 * //@EnableDiscoveryClient该注解用于向使用consul或 zookeeper作为法册中心时注册服务
 */
@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8004 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8004.class, args);
    }
}
```

## 测试
启动zookeeper，运行微服务。
```
[zk: localhost:2181(CONNECTED) 25] ls /                                                                     
[services, zookeeper]
[zk: localhost:2181(CONNECTED) 26] ls /services                                                             
[cloud-provider-payment]
[zk: localhost:2181(CONNECTED) 27] ls /services/cloud-provider-payment
[c0ca3c06-7384-42a1-bfa2-63ed8e6f3a1e]
[zk: localhost:2181(CONNECTED) 28] 
[zk: localhost:2181(CONNECTED) 28] get /services/cloud-provider-payment/c0ca3c06-7384-42a1-bfa2-63ed8e6f3a1e
{"name":"cloud-provider-payment","id":"c0ca3c06-7384-42a1-bfa2-63ed8e6f3a1e","address":"DESKTOP-T08AAHT","port":8004,"sslPort":null,"payload":{"@class":"org.springframework.cloud.zookeeper.discovery.ZookeeperInstance","id":"application-1","name":"cloud-provider-payment","metadata":{}},"registrationTimeUTC":1584766408452,"serviceType":"DYNAMIC","uriSpec":{"parts":[{"value":"scheme","variable":true},{"value":"://","variable":false},{"value":"address","variable":true},{"value":":","variable":false},{"value":"port","variable":true}]}}
cZxid = 0x36
ctime = Sat Mar 21 20:47:00 CST 2020
mZxid = 0x36
mtime = Sat Mar 21 20:47:00 CST 2020
pZxid = 0x36
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x170fd2061fe0002
dataLength = 536
numChildren = 0
```

---
# 新建消费者cloud-consumerzk-order80

## application.yml
```
server:
  port: 80

spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: 192.168.8.208:2181
      max-retries: 10
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://127.0.0.1:3306/db2019?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456
```

## 业务类
```
@Configuration
public class ApplicationContextConfig {
    /**
     * //appLicationcontext.xml <bean id=""class="">
     *
     * 使用@LoapBalanced注解赋于RestTemplate负载均衡的能力
     */
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```
```
@RestController
@Slf4j
public class OrderZKController {
    private static final String INVOKE_URL = "http://cloud-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/zk")
    public String paymentInfo(){
        String result = restTemplate.getForObject(INVOKE_URL + "/payment/zk", String.class);
        return result;
    }
}
```

## zookeeper查看
```
[zk: localhost:2181(CONNECTED) 4] ls /services
[cloud-provider-payment, cloud-consumer-order]

```

启动后访问http://127.0.0.1/consumer/payment/zk