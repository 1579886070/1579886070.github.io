---
title: '[JAVA]MVC请求映射'
date: 2019-11-10 20:50:49
categories: JAVA
tags: java
---

## 一、@RequestMapping方式
以前我们controller写请求路劲都是如下格式,指定每个http方法，如get、post等。
```
@RequestMapping(value = "v1/get",method = RequestMethod.GET)
```

点击方法，进入源代码中查看一下，可以看到方法包含如下多种：
![源码](http://image.xiaoxinyes.club/2019-11-10_205659.png)

---
## 二、更简洁的方式
很多以前的老项目的格式都是上面的那种，而现在新增了@GetMapping，@PostMapping，@PutMapping，@DeleteMapping，@PatchMapping。

```
    @GetMapping("get")
    public String get(){
        return "get请求!";
    }
    @PostMapping("post")
    public String post(){
        return "post请求!";
    }
    @DeleteMapping("delete")
    public String delete(){
        return "delete请求!";
    }
    @PutMapping("put")
    public String put(){
        return "put请求!";
    }
```
点击任意一个注解进入源码里面，可以看到组合注解。只是简化了，而依旧调用的是RequestMethod。
![源码](http://image.xiaoxinyes.club/2019-11-10_210259.png)

---
## 三、基于restful接口开发
Get:对应查询
post：对应新增
put:对应更新
delete:对应删除