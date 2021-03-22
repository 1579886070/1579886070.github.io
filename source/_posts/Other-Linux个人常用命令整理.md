---
title: '[Other]Linux个人常用命令整理'
date: 2020-03-29 13:04:27
categories: Other
tags: linux
---

## 1、查看整体机器性能
	
   top
    
		--1.1 cpu
        
		--1.2 mem
        
		--1.3 id = idle 空闲率(越大越好)
        
		--1.4 load average 系统负载率(后面三个值分别是1分钟5分钟15分钟的平均负载
							如果这三个值相加除三乘百分之百高于60说明系统负担重，
							高于80说明要gg了)

## 2、低配版查看整机性能

uptime
		
        --19:21:23 up 17 min,  2 users,  load average: 0.00, 0.00, 0.00

## 3、内存

free
		
        --free -m

## 4、硬盘

df
		
        --df -h

## 5、cpu

vmstat(包含但不限于)
```
--[root@contOS-1 ~]# vmstat -n 2 3
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 904864  27448  39636    0    0    50     4   22   28  0  1 99  0  0	
 0  0      0 904848  27448  39632    0    0     0     0   14   13  0  0 100  0  0	
 0  0      0 904848  27448  39632    0    0     0     0   15   13  0  0 100  0  0	

```
		 
## 6、磁盘(io高很大可能写了大sql)

iostat
		
        --iostat -xdk 2 3
			--r/s     w/s 每s读写

--------------------以上源于周阳哥讲解，我总结的笔记

---

其他

## 1、获取线程总数

    pstree -p 22704|wc -l

## 2、linux相关的参数

    ulimit -a

## 3、Logstash
    
    systemctl start|restart|stop elasticsearch

## 4、nginx日志情况

    grep '"responsetime":2.' /usr/local/nginx/logs/access.log

## 5、java堆栈分析

    进程十六进制值
    printf "%x\n" 22414

    jstack -l PID > jstack.log

>dump 文件里，值得关注的线程状态有：
>
>死锁，Deadlock（重点关注）
>
>执行中，Runnable 
>
>等待资源，Waiting on condition（重点关注）
>
>等待获取监视器，Waiting on monitor entry（重点关注）
>
>暂停，Suspended
>
>对象等待中，Object.wait() 或 TIMED_WAITING
>
>阻塞，Blocked（重点关注） 
>
>停止，Parked

