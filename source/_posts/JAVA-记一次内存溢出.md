---
title: '[JAVA]记一次内存溢出'
date: 2019-12-21 15:03:40
categories: JAVA
tags: java
---

项目部署到服务器后，第二天发现异常，记录下排查过程。

## 异常发现
带有权限的账号，突然ssh连接不上服务器了，项目也出现访问不了的情况。后面尝试用root账号，意外的可以登录上。
```
top
jps
```
发现CPU没啥明显的异常，项目进程也没有挂。于是进入项目，查看日志，发现日志中存在异常。
```
2019-12-15 11:36:58.200 default [nioEventLoopGroup-4-2] ERROR o.r.client.handler.CommandsQueue - Exception occured. Channel: [id: 0x2f80895c, L:/172.16.149.12:60435 - R:172.16.149.13/172.16.149.13:6379]
java.lang.OutOfMemoryError: unable to create new native thread
	at java.lang.Thread.start0(Native Method)
	at java.lang.Thread.start(Thread.java:717)
	at io.netty.util.concurrent.ThreadPerTaskExecutor.execute(ThreadPerTaskExecutor.java:33)
	at io.netty.util.internal.ThreadExecutorMap$1.execute(ThreadExecutorMap.java:57)
	at io.netty.util.concurrent.SingleThreadEventExecutor.doStartThread(SingleThreadEventExecutor.java:895)
	at io.netty.util.concurrent.SingleThreadEventExecutor.startThread(SingleThreadEventExecutor.java:866)
	at io.netty.util.concurrent.SingleThreadEventExecutor.execute(SingleThreadEventExecutor.java:759)
	at io.netty.util.concurrent.AbstractScheduledEventExecutor.schedule(AbstractScheduledEventExecutor.java:232)
	at io.netty.util.concurrent.AbstractScheduledEventExecutor.schedule(AbstractScheduledEventExecutor.java:155)
	at io.netty.util.concurrent.AbstractEventExecutorGroup.schedule(AbstractEventExecutorGroup.java:50)
	at org.redisson.client.RedisConnection.async(RedisConnection.java:205)
	at org.redisson.client.RedisConnection.async(RedisConnection.java:191)
	at org.redisson.client.RedisConnection.async(RedisConnection.java:183)
	at org.redisson.client.handler.BaseConnectionHandler.channelActive(BaseConnectionHandler.java:68)
```
**竟然内存溢出了**,开始了一段分析过程。

---
报错为无法创建线程`unable to create new native thread`。使用命令查看下是否有问题。
```
pstree -a    //查看所有进程相关的线程
pstree -p PID|wc -l    //查看指定进程的线程数
```
发现一个项目竟然创建了接近3000线程。能大致推出这是代码有问题，线程未释放导致溢出，但是为什么服务器有权限的账号在发生异常时登录不上了，而root可以呢？

### 账号登录不上解决方法
把服务kill之后，发现用权限的账户可以登录了，原因在于最初时用该账户执行的项目，那么也就是说跟权限配置有关了。用ulimit -a查看下，发现max user processes只有**4096**,这样一算的话启动那些java服务加起来创建的线程数正好就超过这个数了，提高该配置之后，未发现之前出现的权限用户登录不上的情况。
```
ulimit -a

---
core file size          (blocks, -c) unlimited
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 63459
max locked memory       (kbytes, -l) 32000
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1000000
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 8192000
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 4096
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```
---
### 堆栈分析
为了排查错误，项目开着，发现线程一直在慢慢累加，最终到了10000多，不再继续，但也没有释放。linux中**jstack**分析下线程堆栈情况。
```
jstack -l PID > jstack.log
```
日志
```
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.212-b10 mixed mode):

"nioEventLoopGroup-1240-13" #13017 prio=10 os_prio=0 tid=0x00007f1278f46000 nid=0x374 runnable [0x00007f1264b4a000]
   java.lang.Thread.State: RUNNABLE
	at sun.nio.ch.EPollArrayWrapper.epollWait(Native Method)
	at sun.nio.ch.EPollArrayWrapper.poll(EPollArrayWrapper.java:269)
	at sun.nio.ch.EPollSelectorImpl.doSelect(EPollSelectorImpl.java:93)
	at sun.nio.ch.SelectorImpl.lockAndDoSelect(SelectorImpl.java:86)
	- locked <0x0000000768f9f6d8> (a io.netty.channel.nio.SelectedSelectionKeySet)
	- locked <0x0000000768f9f6c8> (a java.util.Collections$UnmodifiableSet)
	- locked <0x0000000768f9f6f0> (a sun.nio.ch.EPollSelectorImpl)
	at sun.nio.ch.SelectorImpl.select(SelectorImpl.java:97)
	at io.netty.channel.nio.SelectedSelectionKeySetSelector.select(SelectedSelectionKeySetSelector.java:62)
	at io.netty.channel.nio.NioEventLoop.select(NioEventLoop.java:791)
	at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:439)
	at io.netty.util.concurrent.SingleThreadEventExecutor$5.run(SingleThreadEventExecutor.java:906)
	at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
	at java.lang.Thread.run(Thread.java:748)

   Locked ownable synchronizers:
	- None

"nioEventLoopGroup-1240-16" #13016 prio=10 os_prio=0 tid=0x00007f15700d4800 nid=0x373 runnable [0x00007f1264c4b000]
   java.lang.Thread.State: RUNNABLE
	at sun.nio.ch.EPollArrayWrapper.epollWait(Native Method)
	at sun.nio.ch.EPollArrayWrapper.poll(EPollArrayWrapper.java:269)
	at sun.nio.ch.EPollSelectorImpl.doSelect(EPollSelectorImpl.java:93)
	at sun.nio.ch.SelectorImpl.lockAndDoSelect(SelectorImpl.java:86)
	- locked <0x0000000768fa15e8> (a io.netty.channel.nio.SelectedSelectionKeySet)
	- locked <0x0000000768fa15d8> (a java.util.Collections$UnmodifiableSet)
	- locked <0x0000000768fa1600> (a sun.nio.ch.EPollSelectorImpl)
	at sun.nio.ch.SelectorImpl.select(SelectorImpl.java:97)
	at io.netty.channel.nio.SelectedSelectionKeySetSelector.select(SelectedSelectionKeySetSelector.java:62)
	at io.netty.channel.nio.NioEventLoop.select(NioEventLoop.java:791)
	at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:439)
	at io.netty.util.concurrent.SingleThreadEventExecutor$5.run(SingleThreadEventExecutor.java:906)
	at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
	at java.lang.Thread.run(Thread.java:748)

   Locked ownable synchronizers:
	- None

"nioEventLoopGroup-1240-14" #13015 prio=10 os_prio=0 tid=0x00007f1578006800 nid=0x362 runnable [0x00007f1264d4c000]
   java.lang.Thread.State: RUNNABLE
	at sun.nio.ch.EPollArrayWrapper.epollWait(Native Method)
	at sun.nio.ch.EPollArrayWrapper.poll(EPollArrayWrapper.java:269)
	at sun.nio.ch.EPollSelectorImpl.doSelect(EPollSelectorImpl.java:93)
	at sun.nio.ch.SelectorImpl.lockAndDoSelect(SelectorImpl.java:86)
	- locked <0x0000000768f9d840> (a io.netty.channel.nio.SelectedSelectionKeySet)
	- locked <0x0000000768f9d830> (a java.util.Collections$UnmodifiableSet)
	- locked <0x0000000768f9d858> (a sun.nio.ch.EPollSelectorImpl)
	at sun.nio.ch.SelectorImpl.select(SelectorImpl.java:97)
	at io.netty.channel.nio.SelectedSelectionKeySetSelector.select(SelectedSelectionKeySetSelector.java:62)
	at io.netty.channel.nio.NioEventLoop.select(NioEventLoop.java:791)
	at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:439)
	at io.netty.util.concurrent.SingleThreadEventExecutor$5.run(SingleThreadEventExecutor.java:906)
	at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
	at java.lang.Thread.run(Thread.java:748)

   Locked ownable synchronizers:
	- None
```
以上，能看到nioEventLoopGroup一直处于运行状态，为了更加直观的查看，我本地运行，使用jdk自带的jvm分析工具(在jdk安装目录，bin目录下的jvisualvm.exe),，发现随着时间的推移，nioEventLoopGroup创建数量越来越多。
![](http://image.xiaoxinyes.club/2019122101.png)

### 代码追踪
查询代码后，发现有个定时任务中循环创建了redisson，而每次创建琐时都会new NioEventLoopGroup();正是因为这个每次创建后一直处于执行中，JVM垃圾回收并不会处理，所以溢出了。

### 解决
我的处理方法
1、初始化之创建一个线程池。
```
private static NioEventLoopGroup nioEventLoopGroup;

private static NioEventLoopGroup getNioEventLoopGroup() {
        if (nioEventLoopGroup == null) {
            nioEventLoopGroup = new NioEventLoopGroup();
        }
        return nioEventLoopGroup;
    }
```

2、或者释放
```
nioEventLoopGroup.shutdownGracefully();
```