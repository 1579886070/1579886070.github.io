---
title: '[Cloud]6-三个注册中心异同点'
date: 2020-03-21 22:37:12
categories: JAVA
tags: SpringCloud
---

# 异同点
|组件名|语言|CAP|服务健康检查|对外暴露接口|
|:-----  |:-----|:-----
|Eureka |Java|AP|可配支持|HTTP|
|Consul |Go|CP|支持|HTTP/DNS|
|ZooKeeper |java|CP|支持|客户端


# CAP
CAP理论关注粒度是数据，而不是整体系统设计的策略

>**C:Consistency（强一致性）**

>**A:Availability（可用性）**

>**P:Partition tolerance（分区容错性）**

最多只能同时较好的满足两个。
CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，因此，根据 CAP原理将NoSQL数据库分成了满足CA原则、满足CP原则和满足AP原则三大类： 

CA-单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。

CP-满足一致性，分区容忍性的系统，通常性能不是特别高。

AP-满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。
