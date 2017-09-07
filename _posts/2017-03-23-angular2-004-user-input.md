---
title: Angular2学习笔记(四) Angular2 用户输入
layout: post
date: '2017-03-23 00:00:00'
categories: angular2 typescript
tags: angular2 angular-cli typescript
author: luamas
original: true
---

* content
{:toc}

用户输入触发 DOM 事件。我们通过事件绑定来监听它们，把更新过的数据导入回我们的组件和 model。

### 输入事件绑定

要绑定 DOM 事件，只要把 DOM 事件的名字包裹在圆括号中，然后用放在引号中的模板语句对它赋值就可以了
打开我们之前的my-app应用并在命令行,创建一个component

```shell
ng g c input/clickMe
```




之后控制台会打印以下信息

```
installing component
  create src/app/input/click-me/click-me.component.css
  create src/app/input/click-me/click-me.component.html
  create src/app/input/click-me/click-me.component.spec.ts
  create src/app/input/click-me/click-me.component.ts
  update src/app/app.module.ts
```

g代表generate,c代表component这个是cli的简写命令

修改click-me.component.html模板

```html
<button (click)="onClickMe()">点击我!</button>
{{clickMessage}}
```

修改组件click-me.component.ts文件为以下内容

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-click-me',
  templateUrl: './click-me.component.html',
  styleUrls: ['./click-me.component.css']
})
export class ClickMeComponent {
  clickMessage;
  onClickMe() {
    this.clickMessage = '我被点击了!!';
  }
}
```

在首页html中使用其组件

```html
<h1>================第一课内容================</h1>
  <app-root></app-root>

  <h1>================第三课内容================</h1>
  <h2>1.我被点击了</h2>
  <app-click-me></app-click-me>
```

最后我们千万别忘了在app.module.ts的bootstrap中加入组件,否则组件是不会显示的,好,ng s启动就可以看到页面了


### 通过$event对象取得用户输入

DOM 事件可以携带可能对组件有用的信息。 本节将展示如何绑定输入框的keyup事件，在每个敲击键盘时获取用户输入。

下面的代码进行监听keyup事件，并将整个事件对象$event传递给组件的事件处理器。

同样,还是继续创建一个component

```shell
ng g c input/keyup
```

修改模板文件keyup.component.html

```html
<input (keyup)="onKey($event)">
{{values}}
```

修改组件文件keyup.component.ts
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-keyup',
  templateUrl: './keyup.component.html',
  styleUrls: ['./keyup.component.css']
})
export class KeyupComponent {
  values = '';

  // 这个方法不安全,因为没有给出事件类型,而且不是所有的事件都会有event.target.value,所以会采用下面方法
  // onKey(event: any) { // 松散类型
  //   // event.target.value获取值
  //   // event.key 这个返回的是建名,例如:d | s | f | s | d | Shift | CapsLock | Enter | | Enter | Shift | ArrowLeft | ArrowRight | ArrowUp |
  //   this.values += event.target.value + ' | ';
  // }

  //
  onKey(event: KeyboardEvent) {
    // 这里这个语法前面学过的类型断言,其实就是将event.target类型转换为HTMLInputElement类型
    this.values += (<HTMLInputElement> event.target).value + ' | ';
  }
}
```

上面给出了两个方法,其中下面这个是给出具体的键盘事件类型

index.html增加以下内容,这些内容在以后章节中就不在重复说明了.

```html
<h2>2.通过$event对象取得用户输入</h2>
<app-keyup></app-keyup>
```

app.module.ts增加以下内容,这些内容在以后章节中就不在重复说明了.

```
// 这里增加了KeyupComponent的启动项,如果不增加,首页的标签是不起作用的
bootstrap: [AppComponent, ClickMeComponent, KeyupComponent]
```

* **上面演示了使用$event对象来获取dom的事件信息,其实Angular并不建议这么做这样做会将整个的dom事件全部暴露给用户,下面就来看一下官方推荐的方法,从一个模板引用变量中获得用户输入**

### 从一个模板引用变量中获得用户输入

还有另一种获取用户数据的方式：使用 Angular 的模板引用变量。 这些变量提供了从模块中直接访问元素的能力。 在标识符前加上井号 (#) 就能声明一个模板引用变量。


老步骤创建一个component

```shell
ng g c input/loopBack
```

生成完成后先做下每次必要工作,修改app.module.ts和index.html上面说过的

loop-back.component.html代码

```html
<input #box (keyup)="0">
{{box.value}}
```

这个模板引用变量名叫box，在`<input>`元素声明，它引用`<input>`元素本身。 代码使用box获得输入元素的value值，并通过插值表达式把它显示.

这个模板完全是完全自包含的。它没有绑定到组件，组件也没做任何事情.

keyup事件绑定到了数字0，这是可能是最短的模板语句。 这个语句没有什么特殊意义

从模板变量获得输入框比通过$event对象更加简单。 下面的代码重写了之前keyup示例，它使用变量来获得用户输入

```shell
ng g c input/keyup2
```

修改app.module.ts和index.html

keyup2.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-keyup2',
  templateUrl: './keyup2.component.html',
  styleUrls: ['./keyup2.component.css']
})
export class Keyup2Component {
  values = '';
  onKey(value: string) {
    this.values += value + ' | ';
  }
}
```

keyup2.component.html

```html
<input #box (keyup)="onKey(box.value)">
{{values}}
```

### 通过按键事件(key.enter)过滤

(keyup)事件处理器监听每一次按键。 有时只在意回车键，因为它标志着用户结束输入。 解决这个问题的一种方法是检查每个$event.keyCode，只有键值是回车键时才采取行动。

更简单的方法是：绑定到 Angular 的keyup.enter 模拟事件。 然后，只有当用户敲回车键时，Angular 才会调用事件处理器。

```shell
ng g c input/keyup3
```

修改app.module.ts和index.html

keyup3.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-keyup3',
  templateUrl: './keyup3.component.html',
  styleUrls: ['./keyup3.component.css']
})
export class Keyup3Component {
  value = '';
  onEnter(value: string) { this.value = value; }
}
```

keyup3.component.html

```html
<<input #box (keyup.enter)="onEnter(box.value)">
{{value}}
```

上面只有我们按下enter后抬起才会触发事件


### 通过失去焦点事件(blur)过滤


```shell
ng g c input/keyup4
```

修改app.module.ts和index.html

keyup4.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-keyup4',
  templateUrl: './keyup4.component.html',
  styleUrls: ['./keyup4.component.css']
})
export class Keyup4Component {
  value = '';
  update(value: string) { this.value = value; }
}
```

keyup4.component.html

```html
<input #box
       (keyup.enter)="update(box.value)"
       (blur)="update(box.value)">
{{value}}

```

上面只有我们的输入框失去焦点时触发事件


### 集成数据展示

前面一节我们学习了数据展示,下面我们就使用键盘输入和数据展示写一个示例


```shell
ng g c input/MySkill
```

修改app.module.ts和index.html

my-skill.component.ts

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-my-skill',
  templateUrl: './my-skill.component.html',
  styleUrls: ['./my-skill.component.css']
})
export class MySkillComponent {
  title = '请输入你的拥有的技能:';
  skills = ['JAVA', 'PHP'];
  /**
   * 增加技能的方法
   * @param newSkill
   */
  addSkill(newSkill: string) {
    if (newSkill) {
      this.skills.push(newSkill);
    }
  }
}
```

my-skill.component.html

```html
<p>{{title}}</p>
<input #newSkill
       (keyup.enter)="addSkill(newSkill.value)"
       (blur)="addSkill(newSkill.value); newSkill.value='' ">
<button (click)="addSkill(newSkill.value)">增加</button>
<ul><li *ngFor="let skill of skills">{{skill}}</li></ul>
```



#### 注意
我们用cli创建组件的的时候一定要用驼峰命名,这样ng-cli将用 - 使其分割,开始的首字母不区分大小写比如clickMe这个c,大小写无所谓,因为cli会自己处理


[查看源代码](https://github.com/luamas/angular2-sample/tree/v1)

[演示地址](http://blog.luamas.com/angular2-sample)

