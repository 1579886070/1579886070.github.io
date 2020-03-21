---
title: '[Cloud]3-集成Eureka'
date: 2020-03-20 13:46:09
categories: JAVA
tags: SpringCloud
---

# EurekaServer服务端安装
新建一个EurekaServer服务,名称：cloud-eureka-server7001 

## 主要依赖
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

## application.yml
```
server:
  port: 7001
eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
  client:
    register-with-eureka: false   #false表示不向注册中心注册自己
    fetch-registry: false   #false表示自己端就是注册中心
    #false表示自己端魔是注册中心，我的钢责就是维护服务实，并不需要去检索服务
    service-url:
      #设置与Eureka Server交互的地址查询服务和注般服务部需要依藏这个她址。
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka
```

## 主启动类
```
package xyz.zx21.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

/**
 * @author Administrator
 * @date 2020/3/20 13:56
 */
@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7001.class, args);
    }
}

```

# cloud-provider-payment8001入驻Eureka

## 修改pom
新增依赖
```
<!--   引入eureka客户端     -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

## 启动类添加注解
`@EnableEurekaClient`

## 修改application.yml
新增
```
eureka:
  client:
    register-with-eureka: true   #是否将自己注册到注册中心,集群必须设置为true配合ribbon
    fetch-registry: true    #是否从服务端抓取已有的注册信息
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    service-url:
      defaultZone: http://localhost:7001/eureka
```

---
# cloud-comsumer-order80入驻Euraka
**与上面的同理，添加依赖，改yml和主启动类**

application.yml
```
server:
  port: 80

spring:
  application:
    name: cloud-order-service

eureka:
  client:
    register-with-eureka: true   #是否将自己注册到注册中心,集群必须设置为true配合ribbon
    fetch-registry: true    #是否从服务端抓取已有的注册信息
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    service-url:
      defaultZone: http://localhost:7001/eureka
```

---
# 测试
分别启动eureka和80与8001服务，访问http://localhost:7001/

结果如下已经注册进来了
```
Application	AMIs	Availability Zones	Status
CLOUD-ORDER-SERVICE	n/a (1)	(1)	UP (1) - DESKTOP-T08AAHT:cloud-order-service:80
CLOUD-PAYMENT-SERVICE	n/a (1)	(1)	UP (1) - DESKTOP-T08AAHT:cloud-payment-service:8001
```

---
# Eureka集群原理说明

**互相注册，相互守望**

```
1 先启动eureka注册中心
2 启动服务提供者payment支付服务
3 支付服务启动后会把自身信息（比如服务地址以别名方式注册进eureka）
4 消费者order服务在需要调用接口时，使用服务别名去注册中心获取实际的RPC远程调用地址
5 消费者获得调用地址后，底层实际是利用HttpClient技术实现远程调用
6 消费者获得服务地址后会统存在本地jvm内存中，默认每间隔30秒更新一次服务调用地址
```

---
# Eureka集群环境构建
## 新建cloud-eureka-server7002
## 修改hosts文件
```
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
```
EurekaServer7001 的yml
```
server:
  port: 7001
eureka:
  instance:
#    hostname: localhost #eureka服务端的实例名称
    hostname: eureka7001.com #eureka服务端的实例名称
  client:
    register-with-eureka: false   #false表示不向注册中心注册自己
    fetch-registry: false   #false表示自己端就是注册中心
    #false表示自己端魔是注册中心，我的钢责就是维护服务实，并不需要去检索服务
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务部需要依赖这个她址。
#      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka #单例
      defaultZone: http://eureka7002.com:7002/eureka/
```

EurekaServer7002 的yml
```
server:
  port: 7002
eureka:
  instance:
#    hostname: localhost #eureka服务端的实例名称
    hostname: eureka7002.com #eureka服务端的实例名称
  client:
    register-with-eureka: false   #false表示不向注册中心注册自己
    fetch-registry: false   #false表示自己端就是注册中心
    #false表示自己端魔是注册中心，我的钢责就是维护服务实，并不需要去检索服务
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务部需要依赖这个她址。
#      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka #单例
      defaultZone: http://eureka7001.com:7001/eureka/
```

## 测试
访问http://eureka7001.com:7001/ 和 http://eureka7002.com:7002/

能看到7001指向7002，7002指向7001即可。
```
DS Replicas
eureka7001.com
```

---
# 其他服务注册到Eureka集群
## 修改yml即可
```
eureka:
  client:
    register-with-eureka: true   #是否将自己注册到注册中心,集群必须设置为true配合ribbon
    fetch-registry: true    #是否从服务端抓取已有的注册信息
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    service-url:
      #defaultZone: http://localhost:7001/eureka
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/  #集群版
```

## 测试
### 启动步骤
```
1 先要启动EurekaServer，7001/7002服务
2 再要启动服务提供者provider，8001
3 再要启动消费者，80
4 http://localhost/consumer/payment/get/1
```

---
# 支付微服务集群配置
## 新建cloud-provider-payment8002
copycloud-provider-payment8001的代码即可，修改端口。

**为了区分方便，修改controller,用于显示端口**
```
@RestController
@Slf4j
public class PaymentController {
    @Resource
    private PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @PostMapping(value = "/payment/create")
    public CommonResult create(@RequestBody Payment payment) {
        int result = paymentService.create(payment);
        log.info("插入结果:" + result);
        if (result > 0) {
            return new CommonResult(200, "插入成功,serverPort: " + serverPort, result);
        } else {
            return new CommonResult(444, "插入失败", null);
        }
    }

    @GetMapping(value = "/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") Long id) {
        Payment paymentById = paymentService.getPaymentById(id);
        log.info("查询结果:" + paymentById);
        if (paymentById != null) {
            return new CommonResult(200, "查询成功,serverPort: "+ serverPort, paymentById);
        } else {
            return new CommonResult(444, "没有对应记录，查询id:" + id, null);
        }
    }
}

```

---
# cloud-comsumer-order80修改
```
@RestController
@Slf4j
public class OrderController {

//    public static final String PAYMENT_URL = "http://localhost:8001";
    public static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";

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

## 添加@LoadBalanced
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

---
## actuator信息完善
## 服务名称修改
```
  instance:
    instance-id:  payment8001
```
## ip信息显示
```
  instance:
    instance-id:  payment8001
    prefer-ip-address: true #访问路径可以显示IP地址
```

---
# 服务发现Discovery
**对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息**
 ## 修改cloud-provider-payment8001
 ### controller
 ```
 @RestController
@Slf4j
public class PaymentController {
    @Resource
    private PaymentService paymentService;

    @Resource
    private DiscoveryClient discoveryClient;


    @Value("${server.port}")
    private String serverPort;

    @PostMapping(value = "/payment/create")
    public CommonResult create(@RequestBody Payment payment) {
        int result = paymentService.create(payment);
        log.info("插入结果:" + result);
        if (result > 0) {
            return new CommonResult(200, "插入成功,serverPort: " + serverPort, result);
        } else {
            return new CommonResult(444, "插入失败", null);
        }
    }

    @GetMapping(value = "/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") Long id) {
        Payment paymentById = paymentService.getPaymentById(id);
        log.info("查询结果:" + paymentById);
        if (paymentById != null) {
            return new CommonResult(200, "查询成功,serverPort: "+ serverPort, paymentById);
        } else {
            return new CommonResult(444, "没有对应记录，查询id:" + id, null);
        }
    }

    @GetMapping(value = "/payment/discovery")
    public Object discovery(){
        List<String> services = discoveryClient.getServices();
        for (String element : services) {
            log.info("****element: " +element);
        }
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance instance:instances) {
            log.info(instance.getServiceId()+"\t"+instance.getHost()+"\t"+instance.getPort()+"\t"+instance.getUri());
        }
        return this.discoveryClient;
    }
}
 ```
 
 ### 主启动类
 新增@EnableDiscoveryClient注解
 
 ## 测试
 访问 http://localhost:8001/payment/discovery
 
 响应

 ```
 {
    "services": [
        "cloud-payment-service"
    ],
    "order": 0
}
 ```
 log日志
 ```
2020-03-20 16:24:03.078  INFO 11660 --- [nio-8001-exec-9] x.z.s.controller.PaymentController       : ****element: cloud-payment-service
2020-03-20 16:24:03.078  INFO 11660 --- [nio-8001-exec-9] x.z.s.controller.PaymentController       : CLOUD-PAYMENT-SERVICE	192.168.8.122	8002	http://192.168.8.122:8002
2020-03-20 16:24:03.078  INFO 11660 --- [nio-8001-exec-9] x.z.s.controller.PaymentController       : CLOUD-PAYMENT-SERVICE	192.168.8.122	8001	http://192.168.8.122:8001
 ```
 
 ---
 # Eureka自我保护机制
 ## 自我保护机制理论知识
 自我保护机制：默认情况下EurekaClient定时向EurekaServer端发送心跳包如果Eureka在server端在一定时间内（默认90秒）没有收到EurekaClient发送心跳包，便会直接从服务注册列表中剔除该服务，但是在短时间（90秒中）内丢失了大量的服务实例心跳，这时候EurekaServer会开启自我保护机制，不会剔除该服务（该现象可能出现在如果网络不通但是EurekaClient为出现右机，此时如果换做别的注册中心如果一定时间内没有收到心跳会将剔除该服务，这样就出现了严重失误，因为客户端还能正常发送心就，只是网络延迟问题，而保护机制是为了解决此问题而产生的。
 
 ## 禁用自我保护机制
 ### 修改EurekaServer
 yml
 ```
 eureka:
  instance:
#    hostname: localhost #eureka服务端的实例名称
    hostname: eureka7001.com #eureka服务端的实例名称
  client:
    register-with-eureka: false   #false表示不向注册中心注册自己
    fetch-registry: false   #false表示自己端就是注册中心
    #false表示自己端魔是注册中心，我的钢责就是维护服务实，并不需要去检索服务
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务部需要依赖这个她址。
#      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka #单例
      #defaultZone: http://eureka7002.com:7002/eureka/
      defaultZone: http://eureka7001.com:7001/eureka/ #单机
  server:
    enable-self-preservation: false #禁用自我保护模式，保证不可用服务被及时剔除
    eviction-interval-timer-in-ms: 2000 #2秒
 ```
 
 ### 修改Client
 yml
 ```
eureka:
  client:
    register-with-eureka: true   #是否将自己注册到注册中心,集群必须设置为true配合ribbon
    fetch-registry: true    #是否从服务端抓取已有的注册信息
    #是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    service-url:
      defaultZone: http://localhost:7001/eureka
      #defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/  #集群版
  instance:
    instance-id:  payment8001
    prefer-ip-address: true #访问路径可以显示IP地址
    lease-renewal-interval-in-seconds: 1  #向服务端发送心跳的时间间隔，单位为秒（默认是30秒）
    lease-expiration-duration-in-seconds: 2 #收到最后一次心跳后等待时间上限，单位为秒（默认是90秒），超时将剔除
 ```
 
 ### 现象
 当关闭8001服务时，eureka会很快删除