---
title: Angular2学习笔记(二) angular2结构
layout: post
date: '2017-03-20 00:00:00'
categories: angular2
tags: angular2 angular-cli
author: luamas
original: true
---

* content
{:toc}


Angular2结构图

![](http://luamas.github.io/images/2017/03/20/angular2_structure.png)



### 八大模块

模块 (module)

组件 (component)

模板 (template)

元数据 (metadata)

数据绑定 (data binding)

指令 (directive)

服务 (service)

依赖注入 (dependency injection)


### 模块
Angular 应用是模块化的，并且 Angular 有自己的模块系统，它被称为 Angular 模块或 NgModules。

每个 Angular 应用至少有一个模块（根模块），习惯上命名为AppModule。

根模块在一些小型应用中可能是唯一的模块，大多数应用会有很多特性模块，每个模块都是一个内聚的代码块专注于某个应用领域、工作流或紧密相关的功能。

Angular 模块（无论是根模块还是特性模块）都是一个带有@NgModule装饰器的类。.l-sub-section

装饰器是用来修饰 JavaScript 类的函数。 Angular 有很多装饰器，它们负责把元数据附加到类上，以了解那些类的设计意图以及它们应如何工作。
NgModule是一个装饰器函数，它接收一个用来描述模块属性的元数据对象。其中最重要的属性是

```
// 导入Angular的系统模块库
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
// 导入自定义组件
import { AppComponent } from './app.component';

@NgModule({
  // 声明本模块中拥有的视图类。 Angular 有三种视图类：组件、指令和管道
  declarations: [
    AppComponent
  ],
  // declarations 的子集，可用于其它模块的组件模板。
  exports: [],
  // 本模块声明的组件模板需要的类所在的其它模块
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  // 服务的创建者，并加入到全局服务列表中，可用于应用任何部分
  providers: [],
  // 指定应用的主视图（称为根组件），它是所有其它视图的宿主。只有根模块才能设置bootstrap属性
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### 组件
组件负责控制屏幕上的一小块区域，我们称之为视图。

利用angular-cli创建一个HeroListComponent组件

```shell
ng g component HeroListComponent
```

```ts
import { Component } from '@angular/core';

//Component装饰器
@Component({
  // 选择器,页面中的标签
  selector: 'app-root',
  // 指定模板
  templateUrl: './app.component.html',
  // 指定样式
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  // 标题属性
  title = 'app works!';
}
```

### 模板
我们通过组件的自带的模板来定义组件视图。模板以 HTML 形式存在，告诉 Angular 如何渲染组件。

多数情况下，模板看起来很像标准 HTML，当然也有一点不同的地方。

```html
<h1>
  { {title}}
</h1>
```
### 元数据(装饰器)

元数据告诉 Angular 如何处理一个类.在TypeScript 中，我们用装饰器 (decorator) 来附加元数据.
@Component装饰器能接受一个配置对象， Angular 会基于这些信息创建和展示组件及其视图。

@Component的配置项包括：
moduleId: 为与模块相关的 URL（例如templateUrl）提供基地址。

selector： CSS 选择器，它告诉 Angular 在父级 HTML 中查找<hero-list>标签，创建并插入该组件。 例如，如果应用的 HTML 包含<hero-list></hero-list>， Angular 就会把HeroListComponent的一个实例插入到这个标签中。

templateUrl：组件 HTML 模板的模块相对地址，如前所示。

providers:  组件所需服务的依赖注入提供商数组。

@Component里面的元数据会告诉 Angular 从哪里获取你为组件指定的主要的构建块。

模板、元数据和组件共同描绘出这个视图。

其它元数据装饰器用类似的方式来指导 Angular 的行为。 例如@Injectable、@Input和@Output等是一些最常用的装饰器。

### 数据绑定

![](http://luamas.github.io/images/2017/03/20/data_binding.png)

数据绑定的语法有四种形式。每种形式都有一个方向 —— 绑定到 DOM 、绑定自 DOM 以及双向绑定。

### 指令
Angular 模板是动态的。当 Angular 渲染它们时，它会根据指令提供的操作对 DOM 进行转换。

组件是一个带模板的指令；@Component装饰器实际上就是一个@Directive装饰器，只是扩展了一些面向模板的特性。
还有两种其它类型的指令：结构型指令和属性 (attribute) 型指令。

它们往往像属性 (attribute) 一样出现在元素标签中， 偶尔会以名字的形式出现，但多数时候还是作为赋值目标或绑定目标出现。

结构型指令通过在 DOM 中添加、移除和替换元素来修改布局。

### 服务
服务是一个广义范畴，包括：值、函数，或应用所需的特性。

几乎任何东西都可以是一个服务。 典型的服务是一个类，具有专注的、明确的用途。它应该做一件特定的事情，并把它做好。

例如：
日志服务
数据服务
消息总线
税款计算器
应用程序配置
服务没有什么特别属于 Angular 的特性。 Angular 对于服务也没有什么定义。 它甚至都没有定义服务的基类，也没有地方注册一个服务。

即便如此，服务仍然是任何 Angular 应用的基础。组件就是最大的服务消费者。

### 依赖注入(DI,IOC)
这个要是学过java的,基本都很熟悉这个概念
“依赖注入”是提供类的新实例的一种方式，还负责处理好类所需的全部依赖。大多数依赖都是服务。 Angular 使用依赖注入来提供新组件以及组件所需的服务。
当 Angular 创建组件时，会首先为组件所需的服务请求一个注入器 (injector)。

注入器维护了一个服务实例的容器，存放着以前创建的实例。 如果所请求的服务实例不在容器中，注入器就会创建一个服务实例，并且添加到容器中，然后把这个服务返回给 Angular。 当所有请求的服务都被解析完并返回时，Angular 会以这些服务为参数去调用组件的构造函数。 这就是依赖注入 。


### 注意
代码中出现\{\{}}的都会变成`{ {}}`,