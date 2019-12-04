---
title: '[MySql]关于查询的一次异常排查'
date: 2019-12-05 00:20:04
categories: Sql
tags: mysql
---

最近因为一个java项目，其中查询一组数据响应十分慢。后面发现对一张表有40万条左右的数据，单表每次查询速度1S以上。事有蹊跷，开始研究!

## 数据40万条

```
SELECT count(*) FROM `lz_commodity_info`

--
条数 350000万左右
```
---
## 一、SQL查询测试
### 执行sql查看执行时间
```
SELECT * FROM `lz_commodity_info` where commodity_id=544844787596
```

** 响应信息如下,发现竟然查询需要1秒以上。虽然不是主键id,但是也不应该这么慢。**
```
SELECT * FROM `lz_commodity_info` where commodity_id=544844787596
OK
时间: 1.13s
```
---
### 添加索引
查看没有添加索引，先加个索引看看。
```
alter table `lz_commodity_info` add INDEX(commodity_id)
```
### 再次查询
** 响应信息如下,竟然还是这么慢，事情没我想得那么简单。**
```
SELECT * FROM `lz_commodity_info` where commodity_id=544844787596
OK
时间: 1.107s
```
---
### 查看sql
** 而后查看sql发现，定义的类型是varchar。那么尝试修改下sql语句，加上单引号查询试试。**

`` `commodity_id` varchar(50) ``
```
SELECT * FROM `lz_commodity_info` where commodity_id='544844787596'
```
** 响应信息如下，发现从蜗牛直接到火箭了，直接起飞，那么导致慢的原因也浮现出来了。 **
```
SELECT * FROM `lz_commodity_info` where commodity_id='544844787596'
OK
时间: 0.002s
```
---

## 二、疑问开始
### 删除索引再测试
```
alter table `lz_commodity_info` drop INDEX `commodity_id`
```
### 再次查看速度
** 发现速度竟然基本差不多，都长达1s以上了 **
```
时间: 1.099s
------------
时间: 1.077s
------------
```
---
### explain测试
sql前+explain 查询sql的执行情况。
- 1、没加索引
不管是加上单引号还是不加。返回如下：

![查询结果](http://image.xiaoxinyes.club/20191205000538.png)

---
- 2、加上索引
不加单引号，返回结果跟上面的一样。

加单引号，可以看到这才走了索引。返回结果如下：
![查询结果](http://image.xiaoxinyes.club/20191205001000.png)

## 三、总结
>1、查询时添加索引可以极大的优化查询速度；
>
>2、定义的是varchar类型的字段，添加索引，查询切记要加上单引号；



