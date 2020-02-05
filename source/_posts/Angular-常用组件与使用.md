---
title: '[Angular]常用组件与使用'
date: 2020-02-05 20:00:23
categories: Web
tags: Angular
---

记录一下笔记

html代码放在xxx.component.html中

组件代码放在xxx.component.ts中

css放在xxx.component.css中

---
## angular绑定数据
```
<h1>angular模板绑定数据</h1>
<h3>{{title}}</h3>
<h3>{{msg}}</h3>
<h3>{{username}}</h3>
<h3>{{userage}}</h3>
<h3>{{userinfo.age}}</h3>
<h3>{{message}}</h3>
```
```
  public title='我是一个新闻组件';

  msg='我是一个新闻组件的msg';

  public username:string = "我是一个姓名";

  public userage:any=111;

  public userinfo:object={
    age:11
  }

  public message:string = '这是消息';
```
---
## angular绑定属性
```
<h1>angular模板绑定属性</h1>
<div title="我是一个属性">
    鼠标瞄上去看一下
</div>

<div [title]="msg">
    鼠标再瞄上去看一下
</div>
```
```
public msg='我是一个属性';
```
---

## angular绑定html
```
<h1>angular模板绑定html</h1>
<div>
    {{content}}
</div>
<br>
<span [innerHTML]='content' class="red"></span>
```
```
 public content:string="<h2>我是一个html标签</h2>"
```
```
.red{
    color:red;
}
```
---

## angular做简单的运算、
```
<h1>angular做简单的运算</h1>
<h3>1+2={{1+2}}</h3>
```
---

## angular数据循环
```
<ul>
    <li *ngFor="let item of arr">
        {{item}}
    </li>
</ul>

<ol>
    <li *ngFor="let item of list">
        {{item}}
    </li>
</ol>

<ul>
    <li *ngFor="let item of userlist">
        {{item.username}}---{{item.userage}}
    </li>
</ul>

<ul>
    <li *ngFor="let item of cars">
        <h3>{{item.brand}}</h3>
        <ol>
            <li *ngFor="let car of item.list">
                {{car.title}}---{{car.price}}
            </li>
        </ol>
    </li>
</ul>
```
```
//循环
  public arr=['abc','123','哈哈'];

  public list:any=['zxc','321','嘻嘻'];

  public lists:Array<string>=['qwe','456','呵呵'];

  public userlist=[{
    username:'张三',
    userage:20
  },{
    username:'李四',
    userage:22
  },{
    username:'王五',
    userage:25
  }];


  public cars:any[]=[
    {
    brand:'宝马',
    list:[
      {
        title:'宝马01',
        price:'30万'
      },
      {
        title:'宝马02',
        price:'40万' 
      }
    ]
  },
  {
    brand:'奔驰',
    list:[
      {
        title:'奔驰01',
        price:'30万'
      },
      {
        title:'奔驰02',
        price:'40万' 
      }
    ]
  }]
```
---

## angular引入图片
```
<h1>angular引入图片</h1>
<img src="assets/images/100.jpg" alt="图片">
<br>
<img [src]="picUrl">
```
```
  public picUrl: string = "https://www.baidu.com/img/pc_1c6e30772d5e4103103bd460913332f9.png";
```
---

## angular显示数组索引
```
<h1>angular显示数组索引</h1>
<ul>
    <li *ngFor="let item of arr;let key=index" [ngClass]="{'red': key==0,'orange':key==2}">
        {{item.name}}---{{key}}
    </li>
</ul>
```
```
  public arr: any[] = [{
    name: '张三',
  }, {
    name: '王五',
  }, {
    name: '赵六'
  }]
```
---

## angular条件判断
```
<div *ngIf="flag">
    输出true
</div>
<div *ngIf="!flag">
    输出false
</div>

<div *ngIf="false;else other">
    输出false
</div>
<ng-template #other>other content...</ng-template>

<div [ngSwitch]="404">
    <div *ngSwitchCase="200">成功</div>
    <div *ngSwitchCase="404">找不到</div>
    <div *ngSwitchCase="500">内部服务错误</div>
    <div *ngSwitchDefault>其他错误</div>
</div>
```
```
  public flag: boolean = true;
```
---

## angular-ngStyle
```
<h1>angular-ngStyle</h1>
<p style="color: red;">我是一个标签</p>
<p [ngStyle]="{color: 'blue'}">我也是一个标签</p>
```
---

## angular-管道
```
<h1>angular-管道</h1>
{{today|date:'yyyy-MM-dd HH:mm:ss'}}
```
```
public today: any = new Date();
```
---

## angular-点击事件
```
<h1>angular-点击事件</h1>
<button (click)="run()">点击</button>
```
```
 run(){
    alert("触发点击事件")
  }
```
---

## angular-表单事件
```
<h1>angular-表单事件</h1>
<input type="text" (keyup)="keyup($event)"/>
```
```
  keyup(e){
    if(e.keyCode==13){
      console.log("按了一下回车");
    }else{
      //e.target 获取dom对象
      console.log(e.target.value);
    }
  }
```

