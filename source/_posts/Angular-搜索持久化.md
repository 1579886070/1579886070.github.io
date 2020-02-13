---
title: '[Angular]搜索持久化'
date: 2020-02-13 16:32:24
categories: Web
tags: Angular
---

类似与淘宝/京东等，搜索关键字，保留搜索历史记录。

## 新建search组件
```
ng g component components/search
```

## 新建服务
可以用于公用，我理解为java中的类一样。
```
ng g service services/storage
```

## 编写页面
search.component.html
```
<div class="search">
    <input type="text" [(ngModel)]="keyWord"><button (click)="doSearch()">搜索</button>
    <hr>
    搜索记录
    <hr>
    <ul>
        <li *ngFor="let item of historyList;let key=index;">
            {{item}}---
            <button (click)="deleteSearch(key)">X</button>
            
        </li>
    </ul>
</div>
```
search.component.css
```
.search {
  width: 400px;
  margin: 20px auto;
}

button {
  height: 32px;
  width: 80px;
}

input {
  margin-bottom: 20px;
  width: 300px;
  height: 30px;
}

ul {
  list-style-type: none;
}

```

## 根模块引入服务
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
//引入表单相关模块
import { FormsModule } from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { FormComponent } from './components/form/form.component';
import { SearchComponent } from './components/search/search.component';

//引入服务
import { StorageService } from './services/storage.service';

@NgModule({
  declarations: [
    AppComponent,
    FormComponent,
    SearchComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
  ],
  //引入服务
  providers: [StorageService],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

## 服务模块中的方法
storage.service.ts
```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class StorageService {

  constructor() { }

  set(key:string,value:any){
    localStorage.setItem(key,JSON.stringify(value));
  }

  get(key:string) {
   return JSON.parse(localStorage.getItem(key));

  }

  remove(key:string){
    localStorage.removeItem(key);
  }
}

```

## search组件中调用
* 注意同样需要引入服务才可以使用

```
import { Component, OnInit } from '@angular/core';

//引入服务
import { StorageService } from '../../services/storage.service';

@Component({
  selector: 'app-search',
  templateUrl: './search.component.html',
  styleUrls: ['./search.component.css']
})
export class SearchComponent implements OnInit {

  public keyWord: string;
  public historyList: any[] = [];

  //构造函数注入服务
  constructor(public storage: StorageService) {
  }

  ngOnInit() {
    //刷新页面持久化
    var historyList: any = this.storage.get('historyList');
    if (historyList) {
      this.historyList = this.storage.get('historyList');
    }
  }


  doSearch() {
    //避免重复数据
    if (this.historyList.indexOf(this.keyWord) == -1) {
      this.historyList.push(this.keyWord);
      this.storage.set('historyList', this.historyList);
    }
    this.keyWord = '';
  }

  deleteSearch(key: number) {
    this.historyList.splice(key, 1);
    this.storage.set('historyList', this.historyList);
  }
}

```

