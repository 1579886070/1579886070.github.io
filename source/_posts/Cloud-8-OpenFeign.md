---
title: '[Cloud]8-OpenFeign'
date: 2020-03-22 19:01:51
categories: JAVA
tags: SpringCloud
---

# OpenFeign
## 是什么？
>Feign是一个声明式WebService客户端。使用Feign能让编写Web Service客户端更加简单。
它的使用方法是定义一个服务接口然后在上面添加注解即可。

github: [点击访问](https://github.com/spring-cloud/spring-cloud-openfeign)

## 能干嘛？
>可以在使编写Java Http客户端变得更容易。
>
>前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方法。


## 编码
### 新建cloud-comsumer-fegin-order80
pom新增
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```
### yml
```
server:
  port: 80

eureka:
  client:
    register-with-eureka: false   #是否将自己注册到注册中心,集群必须设置为true配合ribbon
    service-url:
      #defaultZone: http://localhost:7001/eureka
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/  #集群版
```

### 主启动类
@EnableFeignClients
```
@SpringBootApplication
@EnableFeignClients
public class OrderFeignMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderFeignMain80.class, args);
    }
}
```

### 业务类编写
**定义一个feign接口**
```
@Configuration
@FeignClient(value = "CLOUD-PAYMENT-SERVICE")
public interface PaymentFeignService {

    @GetMapping(value = "/payment/get/{id}")
    CommonResult<Payment> getPaymentById(@PathVariable("id") Long id);
}
```
**controller**
```
@RestController
@Slf4j
public class OrderFeignController {

    @Resource
    private PaymentFeignService paymentFeignService;

    @GetMapping(value = "/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
        return paymentFeignService.getPaymentById(id);
    }
}
```

### 测试
访问 http://127.0.0.1/consumer/payment/get/1 即可发现跟前面写的Ribbon+RestTemplate效果一致。

---
## OpenFeign超时控制
cloud-provider-payment8001服务新增接口，同时cloud-comsumer-fegin-order80服务的PaymentFeignService接口和controller同样添加调用。
```
    @GetMapping("/payment/feign/timeout")
    public String paymentFeignTimeOut(){
        //测试OpenFeign超时
        try {
            TimeUnit.SECONDS.sleep(3);
        }catch (InterruptedException e){
            e.printStackTrace();
        }
        return serverPort;
    }
```
 ## 测试
访问http://127.0.0.1:8001/payment/feign/timeout -正常

访问http://127.0.0.1/consumer/payment/feign/timeout -超时
```
connect timed out executing GET http://CLOUD-PAYMENT-SERVICE/payment/feign/timeout
feign.RetryableException: connect timed out executing GET http://CLOUD-PAYMENT-SERVICE/payment/feign/timeout
	at feign.FeignException.errorExecuting(FeignException.java:213)
```

### 超时解决配置
```
#设置feign客户端超时时间
ribbon:
  #指的是建立连接后从服务器读取到可用资源所用的时间
  ReadTimeout: 5000
  #指的是建立连接所用的时间，适用于网络状况正常的情况下，两端连接所用的时间
  ConnectTimeout: 5000
```
---
## OpenFeign日志增强
### 日志级别
>NONE：默认的，不显示任何日志；
>
>BASIC：仅记录请求方法、URL、响应状态码及执行时间；
>
>HEADERS：除了BASIC中定义的信息之外，还有请求和响应的头信息；
>
>FULL：除了HEADERS中定义的信息之外，还有请求和响的正文及元数据。

### 添加配置类
```
@Configuration
public class FeignConfig {

    @Bean
    Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```

### yml
```
logging:
  level:
    #以什么级别监控哪个接口
    xyz.zx21.springcloud.service.PaymentFeignService: debug
```

### 测试
访问 http://127.0.0.1/consumer/payment/get/1

控制台输出日志
```
2020-03-22 19:46:33.369 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] ---> GET http://CLOUD-PAYMENT-SERVICE/payment/get/1 HTTP/1.1
2020-03-22 19:46:33.370 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] ---> END HTTP (0-byte body)
2020-03-22 19:46:33.376 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] <--- HTTP/1.1 200 (6ms)
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] connection: keep-alive
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] content-type: application/json
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] date: Sun, 22 Mar 2020 11:46:33 GMT
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] keep-alive: timeout=60
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] transfer-encoding: chunked
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] 
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] {"code":200,"message":"查询成功,serverPort: 8001","data":{"id":1,"serial":"小信"}}
2020-03-22 19:46:33.377 DEBUG 3496 --- [p-nio-80-exec-2] x.z.s.service.PaymentFeignService        : [PaymentFeignService#getPaymentById] <--- END HTTP (88-byte body)
```