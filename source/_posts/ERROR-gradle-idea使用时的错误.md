---
title: '[ERROR]gradle+idea使用时的错误'
date: 2019-10-27 09:26:34
categories: ERROR
tags: error
---

gradle类似于maven，今天(以前的文章，不是当天哦)我下载配置完后发现了许多问题，导致纠结了许久。

下载官网：http://services.gradle.org/distributions/

我去下载了最新的版本，解压后配置环境变量。于是开始在idea上创建项目，无奈怎样都有问题，显示无法构建，这个问题真是搞了许久，检查环境变量，idea搭建过程是否有误等等因素。一直没看出来…..
`
1
Failed to notify build listener`

无奈之下，重新下载了另一个版本，再次测试，同样的结果，我都开始怀疑是不是idea版本的原因了~

***
 
先去忙其他事情…

临近晚上…

---

想着不解决心里不舒服，于是把原来下载的删去，又去下载一个低版本的，不过这次下载的是4.0版本(发现在结合spring boot有问题后修改成4.6)的。系统环境配置依旧。idea开启!

![](http://image.xiaoxinyes.club/2019-01-13_190710.png)

---
选择自动导入，和本地已有的gradle路径。
![](http://image.xiaoxinyes.club/2019-01-13_191241.png)

---
刷新，目录如下。
![](http://image.xiaoxinyes.club/2019-01-13_191534.png)

---
发现用了该版本，没有报错了，这里暂时不纠结确实是否版本的原因，有时间我下次再下载一个试试。

能够发现，没有呈现src目录，这里有两个方法解决。

---
**方法一：**

file–>settings
![](http://image.xiaoxinyes.club/2019-01-13_192126.png)

**方法二：**

build.gradle添加下列代码
```
task "create-dirs" << {
    sourceSets*.java.srcDirs*.each {
        it.mkdirs()
 
    }
    sourceSets*.resources.srcDirs*.each{
        it.midirs()
    }
}
```
