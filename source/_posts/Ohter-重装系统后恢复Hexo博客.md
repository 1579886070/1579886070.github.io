---
title: '[Ohter]重装系统后恢复Hexo博客'
date: 2019-12-20 11:15:25
categories: Ohter
tags: github
---

电脑加了个固态，重装了下，所以有些资料需要迁移一下。

## git和nodo安装
先安装git和nodo.js,git生成密钥添加到自己的git账户
 
 ---
## 处理备份的blog文件
删除不必要的文件，保留如下：
![](http://image.xiaoxinyes.club/201912202.png)

---
## 关联
```
git init
git remote add origin git@github.com:XXX/XXX.github.io.git
```
![](http://image.xiaoxinyes.club/201912201.png)

---
## 安装hexo
```
npm install hexo --save
```

## 安装依赖
```
npm install  
```

## 其他
```
hexo g
hexo s
hexo d
```