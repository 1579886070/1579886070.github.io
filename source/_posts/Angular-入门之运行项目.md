---
title: '[Angular]入门之运行项目'
date: 2020-02-04 18:59:16
categories: Web
tags: Angular
---

## 1、安装node.js

## 2、安装cnpm
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 3、安装angular
```
cnpm install -g @angular/cli
```
### 查看是否安装成功
```
C:\Users\Administrator>ng v

     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/


Angular CLI: 8.3.24
Node: 10.16.3
OS: win32 x64
Angular:
...

Package                      Version
------------------------------------------------------
@angular-devkit/architect    0.803.24
@angular-devkit/core         8.3.24
@angular-devkit/schematics   8.3.24
@schematics/angular          8.3.24
@schematics/update           0.803.24
rxjs                         6.5.4
```

## 4、创建安装angular项目
打开命令提示符窗口
```
ng new demo
```

## 5、运行项目
进入到文件夹下
```
cd demo
```
运行
```
ng serve --open
```

## 6、访问
 http://localhost:4200/

---
##  跳过npm安装
```
ng new demo2 --skip-install
```
```
cnpm install
```

