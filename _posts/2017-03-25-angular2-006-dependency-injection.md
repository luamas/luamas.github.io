---
title: Angular2学习笔记(六) Angular2 依赖注入
layout: post
date: '2017-03-25 00:00:00'
categories: angular2
tags: angular2 angular-cli di
author: luamas
original: true
---

* content
{:toc}

{% raw %}

在java的世界中我们有spring的`DI`神器,而在angular的世界中我们同样有`DI`,由于这里文章篇幅偏大,只捡主要的讲解,其他的信息可参考最后给出的链接


### 什么是装饰器,装饰器都有什么

* 装饰器
java里面叫注解,angular官方称其为装饰器.其最后会生成json文件的元数据文件.

* 装饰器类型





|装饰器|作用|重要|
|:-:|:-:|:-:|
|[`Component`](https://angular.cn/docs/ts/latest/api/core/index/Component-decorator.html)|标记类作为组件并收集组件配置元数据(继承Directive)|*****|
|[`Directive`](https://angular.cn/docs/ts/latest/api/core/index/Directive-decorator.html)|标记类作为指令并收集组件配置元数据| ***** |
|[`Injectable`](https://angular.cn/docs/ts/latest/api/core/index/Injectable-decorator.html)|标记元数据并可以使用`Injector`注入器注入| ***** |
|[`Inject`](https://angular.cn/docs/ts/latest/api/core/index/Inject-decorator.html)|指定依赖关系的参数装饰器(一般用来注入被标记`Injectable`的类)| **** |
|[`Optional`](https://angular.cn/docs/ts/latest/api/core/index/Optional-decorator.html)|将依赖项标记为可选的参数元数据. 如果没有找到依赖关系,注射器将提供null| **** |
|[`ContentChild`](https://angular.cn/docs/ts/latest/api/core/index/ContentChild-decorator.html)|配置一个内容查询| *** |
|[`ViewChild`](https://angular.cn/docs/ts/latest/api/core/index/ViewChild-decorator.html)|配置一个视图查询| *** |
|[`ViewChildren`](https://angular.cn/docs/ts/latest/api/core/index/ViewChildren-decorator.html)|配置多个视图查询(返回`QueryList`类型)| *** |
|[`ContentChildren`](https://angular.cn/docs/ts/latest/api/core/index/ContentChildren-decorator.html)|配置多个个内容查询(返回`QueryList`类型)| *** |
|[`Host`](https://angular.cn/docs/ts/latest/api/core/index/Host-decorator.html)|按照依赖关系来检索| ** |
|[`Self`](https://angular.cn/docs/ts/latest/api/core/index/Self-decorator.html)|指定注射器只能从本身检索依赖关系| ** |
|[`SkipSelf`](https://angular.cn/docs/ts/latest/api/core/index/SkipSelf-decorator.html)|指定注射器只能从父类检索依赖关系| ** |
|[`Type`](https://angular.cn/docs/ts/latest/api/core/index/Type-decorator.html)|调用ES7装饰| * |



java的世界大家都叫他注解,其实就是带着@符号的类(暂时这么理解就好)


### 依赖注入的好处
有了依赖注入我们可以随时更改注入类,比如我们定义一个日志接口Logger,其实现类有`Logger1,Logger2,Logger3....`,等一系列实现,假如我们要没有依赖注入,那么我们要创建n多次实例,有了依赖注入,只需要更改下就可以了.

#### useClass属性
useClasss,要注入的实际子类

```
providers:[{ provide: Logger, useClass: Logger1 }]
```

**注意千万别使用接口注入,例如useClass放一个接口,那么angular会报错的,因为在JavaScript世界里面没有接口的概念**

#### useExisting属性
useExisting,使用已经注册的类型注入到这里(即别名),下面示例意思是将Logger1起个叫Logger2的别名

```
providers:[{ provide: Logger2, useExisting: Logger1 }]
```

#### useValue属性
useValue,参数

#### useFactory属性
useFactory,使用工厂注入

```ts
//采用的lambda表达式语法
let testServiceFactory = (logger: Logger) => {
  return new TestService(logger);
};

providers:[{ provide: TestService , useFactory: testServiceFactory, deps: [Logger] }]
```

#### deps属性
deps属性是提供商令牌(其实就是一个类的标示)数组.


### 手动创建一个基于接口的注入类
解决方案是定义和使用 [OpaqueToken](https://angular.cn/docs/ts/latest/api/core/index/OpaqueToken-class.html)（不透明的令牌）。定义方式类似于这样：

```ts
import { OpaqueToken } from '@angular/core';
export let APP_CONFIG = new OpaqueToken('app.config');
```

使用这个OpaqueToken对象注册依赖的提供商：

```ts
providers: [{ provide: APP_CONFIG, useValue: HERO_DI_CONFIG }]
```

现在，在@Inject装饰器的帮助下，这个配置对象可以注入到任何需要它的构造函数中：

```ts
constructor(@Inject(APP_CONFIG) config: AppConfig) {
  this.title = config.title;
}
```


 - 虽然ConfigAppConfig接口在依赖注入过程中没有任何作用，但它为该类中的配置对象提供了强类型信息。

或者在 ngModule 中提供并注入这个配置对象，如AppModule。

```ts
providers: [
  UserService,
  { provide: APP_CONFIG, useValue: HERO_DI_CONFIG }
]
```


### 总结
关于Inject和Optional的说明,这里没有说明,具体的可以的可以参照上面表格去查看官方教程.

### 其他资料

[中文官方文档](https://angular.cn/docs/ts/latest/guide/dependency-injection.html)

[英文官方文档](https://angular.io/docs/ts/latest/guide/dependency-injection.html)

<br>


[查看源代码](https://github.com/luamas/angular2-sample)

[演示地址](http://blog.luamas.com/angular2-sample)

{% endraw %}