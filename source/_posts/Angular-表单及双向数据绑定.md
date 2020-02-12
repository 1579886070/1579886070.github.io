---
title: '[Angular]表单及双向数据绑定'
date: 2020-02-12 20:43:47
categories: Web
tags: Angular
---

## 引入表单相关模块
引入表单相关模块，才可以使用双向数据绑定。

app.module.ts
```
//引入表单相关模块
import { FormsModule } from '@angular/forms';
```

## 新建一个form组件
```
ng g component components/form
```

## 编写页面
form.component.html
```
<h2>表单演示</h2>

<div class="form">
    <ul>
        <li>
            姓名：<input type="text" [(ngModel)]="formInfo.inputValue" />
        </li>

        <li>
            性别：
            <input type="radio" value="1" name="sex" id="sex1" [(ngModel)]="formInfo.sex"><label for="sex1"> 男</label>
            <input type="radio" value="2" name="sex" id="sex2" [(ngModel)]="formInfo.sex"><label for="sex2"> 女</label>
        </li>

        <li>
            城市：
            <select [(ngModel)]="formInfo.defaultCity">
                <option *ngFor="let item of formInfo.city" [value]="item">{{item}}</option>
            </select>
        </li>

        <li>
            爱好：
            <span *ngFor="let item of formInfo.hobby;let key=index;">
                <input type="checkbox" [id]="'check'+key" [(ngModel)]="item.cheked"/><label [for]="'check'+key"> {{item.title}}</label>
            </span>

        </li>

        <li>
            <textarea name="mark" id="" cols="30" rows="10" [(ngModel)]="formInfo.mark"></textarea>
        </li>
    </ul>

    <button (click)="dosubmit()">获取内容</button>

    <br />

    <pre>
        {{formInfo|json}}
    </pre>
</div>
```

## css样式
form.component.css
```
h2{
    text-align: center;
}

.form{
    width: 400px;
    margin: auto;
    border: 2px solid #000;
}

button{
    width: 100px;
    float: right;
}

ul{
    list-style-type: none;
}

li{
    line-height: 50px;
}

```

## 模块代码
form.component.ts
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-form',
  templateUrl: './form.component.html',
  styleUrls: ['./form.component.css']
})
export class FormComponent implements OnInit {
  public citys: any[] = ['长沙', '杭州', '苏州'];

  public formInfo: any = {
    inputValue: '',
    sex: '1',
    city: this.citys,
    defaultCity:this.citys[0],

    hobby:[{
      title:'画画',
      cheked:false
    },{
      title:'弹吉他',
      cheked:false
    },{
      title:'运动',
      cheked:false
    },],

    mark:''
  }

  constructor() { }

  ngOnInit() {
  }

  dosubmit() {
      console.log(this.formInfo);
  }
}

```
