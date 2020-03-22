---
title: '[Cloud]7-Ribbon相关'
date: 2020-03-22 13:02:23
categories: JAVA
tags: SringCloud
---

# 介绍
## 是什么？
>Spring Cloud Ribbon是基于Netflix Ribbon实现的一套客户端负载均衡的工具。
简单的说，Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出Load Balancer（简称LB）后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。我们很容易使用Ribbon实现自定义的负载均衡算法。

**负载均衡+ResTemplate调用**

## IRule
特定算法：
```
com.netflix.loadbalancer.RoundRobinRule -轮询
com.netflix.loadbalancer.RandomRule-随机
com.netflix.loadbalance.RetryRule-先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试
WeightedResponseTimeRule-对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择
BestAvailableRule-会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务
AvailabilityFilteringRule-先过滤掉故障实例，再选择并发较小的实例
ZoneAvoidanceRule-默认规则，复合判断server所在区域的性能和server的可用性选择服务器
```
---
# Ribbon负载
## 负载规则替换
注意：这个自定义配置类不能放在@ComponentScan所扫描的当前包下以及子包下，否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享，达不到特殊化定制的目的了。

### 新建MySelfRule类
```
@Configuration
public class MySelfRule {
    @Bean
    public IRule myRule() {
        //定义为随机
        return new RandomRule();    
    }
}
```
### 修改主启动类
添加注解@RibbonClient
```
@SpringBootApplication
@EnableEurekaClient
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class, args);
    }
}
```
### 测试
访问http://127.0.0.1/consumer/payment/getForEntity/1 ,这个时候轮询变成了随机。

## 负载轮询算法
```
负载轮询算法：rest接口第几次请求数%服务器集群总数量=实际调用服务器位置下标，每次服务重启动后rest接口计数从1开始。
```
### 源码
RoundRobinRule
```
    public Server choose(ILoadBalancer lb, Object key) {
        if (lb == null) {
            log.warn("no load balancer");
            return null;
        }

        Server server = null;
        int count = 0;
        while (server == null && count++ < 10) {
            List<Server> reachableServers = lb.getReachableServers();
            List<Server> allServers = lb.getAllServers();
            int upCount = reachableServers.size();
            int serverCount = allServers.size();

            if ((upCount == 0) || (serverCount == 0)) {
                log.warn("No up servers available from load balancer: " + lb);
                return null;
            }

            int nextServerIndex = incrementAndGetModulo(serverCount);
            server = allServers.get(nextServerIndex);

            if (server == null) {
                /* Transient. */
                Thread.yield();
                continue;
            }

            if (server.isAlive() && (server.isReadyToServe())) {
                return (server);
            }

            // Next.
            server = null;
        }

        if (count >= 10) {
            log.warn("No available alive servers after 10 tries from load balancer: "
                    + lb);
        }
        return server;
    }
```

## 手写轮询算法
### 8001/8002
controller添加如下,方便测试。
```
    @GetMapping(value = "/payment/lb")
    public String getPaymentLB(){
        return serverPort;
    }
```
### 注释掉消费端的@LoadBalanced
测试自己写的负载均衡是否生效。

### 定义接口LoadBalance
```
public interface LoadBalance {
    ServiceInstance instance(List<ServiceInstance> serviceInstances);
}
```
### 接口实现类
```
@Component
public class MyLB implements LoadBalance {

    private AtomicInteger atomicInteger = new AtomicInteger(0);

    public final int getAndIncrement() {
        int current;
        int next;
        do {
            current = this.atomicInteger.get();
            next = current >= 2147483647 ? 0 : current + 1;
        } while (!this.atomicInteger.compareAndSet(current, next)); //CAS
        System.out.println("*****第几次访问，次数next: " + next);
        return next;
    }

    @Override
    public ServiceInstance instance(List<ServiceInstance> serviceInstances) {
        int index = getAndIncrement() % serviceInstances.size();

        return serviceInstances.get(index);
    }
}
```

### 修改controller
```
    @Resource
    private RestTemplate restTemplate;

    @Resource
    private LoadBalance loadBalance;

    @Resource
    private DiscoveryClient discoveryClient;

    @GetMapping("/consumer/payment/lb")
    public String getPaymentLB() {
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        if (instances == null || instances.size() <= 0) {
            return null;
        }
        ServiceInstance serviceInstance = loadBalance.instance(instances);
        URI uri = serviceInstance.getUri();

        return restTemplate.getForObject(uri + "/payment/lb", String.class);
    }
```

### 测试
访问http://localhost//consumer/payment/lb ，发现每次访问端口都在变化。