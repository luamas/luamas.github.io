---
title: Angular2学习笔记(八) Angular2 模块
layout: post
date: '2017-03-26 10:00:00'
categories: angular2
tags: angular2
author: luamas
original: true
---

* content
{:toc}

{% raw %}


前几节一直都把所有用到的类放在了`AppModule`,那么这个到底是什么呢?其实它是angular2的根模块.
当然我们可以给他起别的名字,但是为了规范我们都将模块起名为`AppModule`.




### 根模块(`AppModule`)


>imports — 导入模块依赖项
 declarations — 声明组件,告诉Angular哪个组件属于AppModule,只有声明了在模板中才可以使用
 bootstrap — 根组件，Angular创建它并插入index.html宿主页面。

`@NgModule`装饰器将`AppModule`标记为Angular模块类(也叫`NgModule`类)。 `@NgModule`接受一个元数据对象，告诉Angular如何编译和启动应用。

* 为什么它是根模块

实际上用cli(angular-cli以后在angular中讲解都叫cli)生成的项目都会有一个main.ts,它是脚本的主入口点,就像我们的c,java,c# 的main的概念一样

```ts
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule);
```

在`.angular-cli.json`中定义了如下内容

```
// 主页
"index": "index.html",
// 主入口点
"main": "main.ts",
```


### NgModule的属性

先来看下NgModule的几个属性


----

| 名称 | 说明 |
|:-:| :-: |
| providers | 对前模块及导入当前模块的模块中的组件、指令、管道、服务等,提供注入(如构造方法注入) |
| declarations | 声明从属于当前模块的组件、指令和管道,组件 |
| imports | 导入到当前模块中的所有模块。被导入模块的declarations也同样对当前模块有效,本当前module的组件中被使用,并不会导出 |
| exports | 导出当前模块的可见的组件、指令、管道,控制内部成员暴露给外部使用 |
| entryComponents | 不会在模板中被引用到的组件。这个属性一般情况下只有ng自己使用，一般是bootstrap组件或者路由组件，ng会自动把bootstrap、路由组件放入其中。 除非不通过路由动态将component加入到dom中，否则不会用到这个属 |
| bootstrap | 一个数组，包括由当前模块引导时应该引导的组件 |


### 模块的分割

按照如下我们将会分割成三个模块


1. 共享(Shared)模块
我们添加SharedModule来存放这些公共组件、指令和管道，并且共享给那些需要它们的模块。

>创建src/app/shared目录,并创建SharedModule模型
创建SharedModule类来管理这些共享的服务(例如http请求服务),管道(例如自定义boolean类型转换),等等,将其导入SharedModule

2. 核心(Core)模块

这里一般我们放了一些全局模块,比如用户页面设置,用户的本地化,页面的主题,等等.

3. 布局(Layout)模块

这里一般我们放了footer,header,offsidebar,sidebar等等一些布局相关的


4. 布局(Route)模块

这里我们放了一些页面相关的主逻辑,比如各个模块,其中内部会分一些小模块


**注意模块和模块之间是相隔离的千万不要在子模块中引用多次同一个模块,这样会造成脏数据,相当于多个实例了.实例之间数据就不同了**



### CommonModule和BrowserModule

当我们在子模块中用到BrowserModule的内容一定要使用CommonModule模块来代替,CommonModule具有通用的指令和组件.


### 用forRoot配置核心服务

加入我们需要某个子模块作为AppModule的核心模块怎么办呢?那么我们就要为自己的模块书写forRoot方法


```
static forRoot(config: UserServiceConfig): ModuleWithProviders {
  return {
    ngModule: CoreModule,
    providers: [
      {provide: UserServiceConfig, useValue: config }
    ]
  };
}
```

```ts
static forRoot(): ModuleWithProviders {
  return {
    ngModule: CoreModule
  };
}
```

调用

```
imports: [
    BrowserModule,
    CoreModule.forRoot({userName: 'Miss Marple'})
  ],
```

后面我们会讲到路由器,其实路由器就是这么实现的包括,以后的ngx-translate都是这么实现的.


### 改造项目

为了以后,我们现在来改造下之前的项目

1. 执行下面命令
```
ng g m routes
```

上面会创建一个routes文件夹

2. 将input和myform拖进routes文件夹,记住一定要用ide拖拽,否则不会改变依赖的目录关系

3. 执行下面两条命令

```
ng g m routes/input
ng g m routes/myform
```


具体代码修改如下

input.module.ts

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import {ClickMeComponent} from './click-me/click-me.component';
import {KeyupComponent} from './keyup/keyup.component';
import {LoopBackComponent} from './loop-back/loop-back.component';
import {Keyup2Component} from './keyup2/keyup2.component';
import {Keyup3Component} from './keyup3/keyup3.component';
import {Keyup4Component} from './keyup4/keyup4.component';
import {MySkillComponent} from './my-skill/my-skill.component';
import {FormsModule} from '@angular/forms';

@NgModule({
  imports: [
    CommonModule,
    FormsModule
  ],
  declarations: [
    ClickMeComponent,
    KeyupComponent,
    LoopBackComponent,
    Keyup2Component,
    Keyup3Component,
    Keyup4Component,
    MySkillComponent
  ]
})
export class InputModule { }
```

myform.module.ts

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import {MyForm1Component} from './my-form1/my-form1.component';
import {FormsModule} from '@angular/forms';

@NgModule({
  imports: [
    CommonModule,
    FormsModule
  ],
  declarations: [MyForm1Component]
})
export class MyformModule { }
```

routes.module.ts

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import {InputModule} from './input/input.module';
import {MyformModule} from './myform/myform.module';

@NgModule({
  imports: [
    CommonModule,
    InputModule,
    MyformModule
  ],
  declarations: []
})
export class RoutesModule { }
```

app.module.ts

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';
import { ClickMeComponent } from './routes/input/click-me/click-me.component';
import { KeyupComponent } from './routes/input/keyup/keyup.component';
import { LoopBackComponent } from './routes/input/loop-back/loop-back.component';
import { Keyup2Component } from './routes/input/keyup2/keyup2.component';
import { Keyup3Component } from './routes/input/keyup3/keyup3.component';
import { Keyup4Component } from './routes/input/keyup4/keyup4.component';
import { MySkillComponent } from './routes/input/my-skill/my-skill.component';
import { MyForm1Component } from './routes/myform/my-form1/my-form1.component';
import {RoutesModule} from "./routes/routes.module";

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RoutesModule
  ],
  providers: [],
  bootstrap: [AppComponent, ClickMeComponent, KeyupComponent, LoopBackComponent, Keyup2Component, Keyup3Component, Keyup4Component,
    MySkillComponent, MyForm1Component]
})
export class AppModule { }
```

注意上面的子模型一定要declarations组件,否则外部无法使用

### 其他资料

[根模块中文官方文档](https://angular.cn/docs/ts/latest/guide/appmodule.html)

[根模块中文官方文档](https://angular.io/docs/ts/latest/guide/appmodule.html)

[模块中文官方文档](https://angular.cn/docs/ts/latest/guide/ngmodule.html)

[模块中文官方文档](https://angular.io/docs/ts/latest/guide/ngmodule.html)

<br>

[查看源代码](https://github.com/luamas/angular2-sample/tree/v2)

[演示地址](http://blog.luamas.com/angular2-sample)

{% endraw %}