---
title: Angular2学习笔记(七) Angular2 模板语法
layout: post
date: '2017-03-25 10:00:00'
categories: angular2
tags: angular2
author: luamas
original: true
---

* content
{:toc}

{% raw %}


### 什么是模板语法
比如我们之前用过的在template里面出现的`*ngFor`,`[(ngModel)]`等一系列用angular来解析的html属性和元素的东东都是模板语法


### 插值表达式
所谓的插值表达式其实就是`{{}}`这种表达式,`{{表达式}}`,内部的表达式是可以进行基础运算的,比如加减法

```html
<h3>
  { {title}}
  <img src="{ {heroImageUrl}}" style="height:30px">
</h3>
```



### 绑定语法
先给出表格

| 数据方向 | 语法 | 绑定类型 |
| :-: | :-: | :-: |
| 单向从数据源到视图目标 | `{ {expression}}`<br>`[target]="expression"`<br>`bind-target="expression"` | 插值表达式,Property,Attribute,类<br>样式(注意区分Property,Attribute) |
| 单向从视图目标到数据源 | `(target)="statement"`<br>`on-target="statement"` | 事件 |
| 双向 | `[(target)]="expression"`<br> `bindon-target="expression"` | 双向 |



<br>
> 由于 HTML `attribute` 和 DOM `property` 在中文中都被翻译成了“属性”,无法区分,而接下来的部分重点是对它们进行比较。
我们无法改变历史，因此，在本章的翻译中，保留了它们的英文形式，不加翻译，以免混淆。 本章中，如果提到“属性”的地方，一定是指 property，因为在 Angular 中，实际上很少涉及 attribute。

除了插值表达式之外的绑定类型，在等号左边是目标名， 无论是包在括号中`[]`、`()`还是用前缀形式`bind-`、`on-`、`bindon-`.

* 绑定目标

| 绑定类型 | 目标 | 范例 |
| :-: | :-: | :-: |
| Property | 元素的 property<br> 组件的 property<br> 指令的 property | `<img [src] = "heroImageUrl">`<br>`<hero-detail [hero]="currentHero"></hero-detail>`<br>`<div [ngClass] = "{selected: isSelected}"></div>` |
| 事件 |元素的事件<br>组件的事件<br>指令的事件<br>|`<button (click) = "onSave()">Save</button>`<br>`<hero-detail (deleteRequest)="deleteHero()"></hero-detail>`<br>`<div (myClick)="clicked=$event">click me</div>` |
| 双向 | 事件与 property | `<input [(ngModel)]="heroName">` |
| Attribute | attribute（例外情况)  | `<button [attr.aria-label]="help">help</button>` |
| CSS 类 | `class` property | `<div [class.special]="isSpecial">Special</div>` |
| 样式 | `style` property | `<button [style.color] = "isSpecial ? 'red' : 'green'">` |



* 别忘了方括号

方括号告诉 Angular 要计算模板表达式。 如果忘了加方括号，Angular 会把这个表达式当做字符串常量看待，并用该字符串来初始化目标属性。 它不会计算这个字符串。

我们经常这样在标准HTML中用这种方式初始化`attribute`，这种方式也可以用在初始化指令和组件的属性。 下面这个例子把HeroDetailComponent的`prefix`属性初始化为固定的字符串，而不是模板表达式。

```html
<!-- ERROR: HeroDetailComponent.hero expects a
     Hero object, not the string "currentHero" -->
  <hero-detail hero="currentHero"></hero-detail>
```

* 一次性字符串初始化

1. 目标属性接受字符串值。

2. 字符串是个固定值，可以直接合并到模块中。

3. 这个初始值永不改变。

```html
<hero-detail prefix="You are my" [hero]="currentHero"></hero-detail>
```

作为对比，`[hero]`绑定是组件的`currentHero`属性的活绑定，它会一直随着更新。

* 属性绑定还是插值表达式？

我们通常得在插值表达式和属性绑定之间做出选择。 下列这几对绑定做的事情完全相同：

```html
<p><img src="{{heroImageUrl}}"> is the <i>interpolated</i> image.</p>
<p><img [src]="heroImageUrl"> is the <i>property bound</i> image.</p>

<p><span>"{{title}}" is the <i>interpolated</i> title.</span></p>
<p>"<span [innerHTML]="title"></span>" is the <i>property bound</i> title.</p>
```

* 内容安全

```ts
evilTitle = 'Template <script>alert("evil never sleeps")</script>Syntax';
```

Angular数据绑定对HTML标签有过滤。 在显示它们之前，它对内容先进行过滤。 不管是插值表达式还是属性绑定，都不会允许带有`script`标签的`HTML`赋值到浏览器中

```html
// 将会过转义script标签内的内容
<p><span>"{{evilTitle}}" is the <i>interpolated</i> evil title.</span></p>
// 将会过滤掉script标签内的内容
<p>"<span [innerHTML]="evilTitle"></span>" is the <i>property bound</i> evil title.</p>
```

插值表达式处理`script`标签与属性绑定有所不同，插值则会转义输出,而属性绑定则会直接过滤掉不输出.


### 使用`EventEmitter`实现自定义事件

* 模板内容

```html
<div>
  <img src="{{heroImageUrl}}">
  <span [style.text-decoration]="lineThrough">
    {{prefix}} {{hero?.fullName}}
  </span>
  <button (click)="delete()">Delete</button>
</div>
```

* 组件内容

```ts
deleteRequest = new EventEmitter<Hero>();

delete() {
  this.deleteRequest.emit(this.hero);
}
```

组件定义了`deleteRequest`属性，它是`EventEmitter`实例。 当用户点击删除时，组件会调用`delete()`方法，让`EventEmitter`发出一个`Hero`对象

```html
<hero-detail (deleteRequest)="deleteHero($event)" [hero]="currentHero"></hero-detail>
```



### 使用`NgModel`进行双向数据绑定

```
<input [(ngModel)]="currentHero.firstName">
```

* `[(ngModel)]`内幕

```html
<input [value]="currentHero.firstName" (input)="currentHero.firstName=$event.target.value" >
```

```html
<input [ngModel]="currentHero.firstName" (ngModelChange)="currentHero.firstName=$event">
```

`ngModel`数据属性设置元素的`value`属性，`ngModelChange`事件属性监听元素`value`的变化

### 内置指令

下面的`click`就是内置指令

```
<button (click)="onSave()">Save</button>
```


### `NgClass`

经常用动态添加或删除`CSS`类的方式来控制元素如何显示。 通过绑定到`NgClass`，可以同时添加或移除多个类。

```
<div [class.special]="isSpecial">The class binding is special</div>
```

上面是一个类元素的使用,下面来看下多个类的情况

```ts
currentClasses: {};
setCurrentClasses() {
  // CSS classes: added/removed per current state of component properties
  this.currentClasses =  {
    saveable: this.canSave,
    modified: !this.isUnchanged,
    special:  this.isSpecial
  };
}
```

```html
<div [ngClass]="currentClasses">This div is initially saveable, unchanged, and special</div>
```

# `NgStyle`

根据组件的状态动态设置内联样式。 `NgStyle`绑定可以同时设置多个内联样式。

```html
<div [style.font-size]="isSpecial ? 'x-large' : 'smaller'" >
  This div is x-large.
</div>
```

同样样式也可以多个同时赋值

```ts
currentStyles: {};
setCurrentStyles() {
  this.currentStyles = {
    // CSS styles: set per current state of component properties
    'font-style':  this.canSave      ? 'italic' : 'normal',
    'font-weight': !this.isUnchanged ? 'bold'   : 'normal',
    'font-size':   this.isSpecial    ? '24px'   : '12px'
  };
}
```

```html
<div [ngStyle]="currentStyles">
  This div is initially italic, normal weight, and extra large (24px).
</div>
```

### `NgIf`

通过绑定NgIf指令到真值表达式，可以把元素子树(元素及其子元素添加到DOM上

```html
<div *ngIf="currentHero">Hello, {{currentHero.firstName}}</div>
```

**注意:别忘了ngIf前面的星号(`*`)。 更多信息，见 `*` 与 `<template>`。**


其实现方式与hidden并不相同

当隐藏子树时，它仍然留在DOM中。 子树中的组件及其状态仍然保留着。 即使对于不可见属性，Angular也会继续检查变更。 子树可能占用相当可观的内存和运算资源

当`NgIf`为`false`时，Angular从DOM中物理地移除了这个元素子树。 它销毁了子树中的组件及其状态，也潜在释放了可观的资源，最终让用户体验到更好的性能

**显示/隐藏技术用在小型元素树上可能还不错。 但在隐藏大树时我们得小心；NgIf可能是更安全的选择。但要记住：永远得先测量，再下结论**

### `NgSwitch`

当需要从一组可能的元素树中根据条件显示一个时，我们就把它绑定到`NgSwitch`。 Angular将只把选中的元素树放进DOM中.

```html
<span [ngSwitch]="toeChoice">
  <span *ngSwitchCase="'Eenie'">Eenie</span>
  <span *ngSwitchCase="'Meanie'">Meanie</span>
  <span *ngSwitchCase="'Miney'">Miney</span>
  <span *ngSwitchCase="'Moe'">Moe</span>
  <span *ngSwitchDefault>other</span>
</span>
```

1. `ngSwitch`：绑定到返回开关值的表达式
2. `ngSwitchCase`：绑定到返回匹配值的表达式
3. `ngSwitchDefault`：用于标记出默认元素的`attribute`

**注意:不要在ngSwitch的前面加星号 (*)，而应该用属性绑定。要把星号 (*) 放在ngSwitchCase和ngSwitchDefault的前面**

### `NgFor`

`NgFor`是一个重复器指令 —— 自定义数据显示的一种方式

```html
<div *ngFor="let hero of heroes">{{hero.fullName}}</div>
```

也可以把`NgFor`应用在一个组件元素上，就下例这样：

```html
<hero-detail *ngFor="let hero of heroes" [hero]="hero"></hero-detail>
```

**注意:不要忘了`ngFor`前面的星号**

* 索引

ngFor指令支持可选的index，它在迭代过程中会从 0 增长到“数组的长度”。 可以通过模板输入变量来捕获这个 index，并在模板中使用。


```html
<div *ngFor="let hero of heroes; let i=index">{{i + 1}} - {{hero.fullName}}</div>
```

* 追踪函数(`trackBy`)

`ngFor`指令有时候会性能较差，特别是在大型列表中。 对一个条目的一点改动、移除或添加，都会导致级联的DOM操作。
这时就可以使用trackBy来操作,它可以更改列表中的部分元素

如果给它一个追踪函数，Angular就可以避免这种折腾。 追踪函数告诉Angular：我们知道两个具有相同`hero.id`的对象其实是同一个英雄。 下面就是这样一个函数：

```ts
trackByHeroes(index: number, hero: Hero) { return hero.id; }
```

绑定一个追踪函数

```ts
<div *ngFor="let hero of heroes; trackBy:trackByHeroes">({{hero.id}}) {{hero.fullName}}</div>
```

追踪函数不会使所有DOM改变。 如果同一个对象的属性变化了，Angular就必须更新全部DOM元素。 但是如果这个属性没有变化,Angular就能留下这些DOM元素。列表界面就会更加平滑，提供更好的响应。

### `*`与`<template>`

上面看到了`NgFor`、`NgIf`和`NgSwitch`这些内置指令时，我们使用了一种特殊的语法：指令名称前面有一个星号 (`*`)。

`*`是一种语法糖，它让那些需要借助模板来修改 HTML布局的指令更易于读写。 `NgFor`、`NgIf`和`NgSwitch`都会添加或移除元素子树，这些元素子树被包裹在`<template>`标签中。

```
<hero-detail *ngIf="currentHero" [hero]="currentHero"></hero-detail>
```

下面我们分两步展开这个语法糖

1. 第一步展开 `*ngIf="currentHero"`为`template="ngIf:currentHero"`,把`ngIf`（没有*前缀）和它的内容传给表达式，再赋值给`template`指令
```
<hero-detail template="ngIf:currentHero" [hero]="currentHero"></hero-detail>
```

2. 最后一步，是把 HTML 包裹进<template>标签和[ngIf]属性绑定中


```html
<template [ngIf]="currentHero">
  <hero-detail [hero]="currentHero"></hero-detail>
</template>
```

**注意:不要误写为`ngIf="currentHero"`! 这种语法会把一个字符串"currentHero"赋值给`ngIf`。 在JavaScript中，非空的字符串是真值，所以`ngIf`总会是`true`，而Angular将永远显示hero-detail标签！**

其他同理


### 模板引用变量

模板引用变量是模板中对DOM元素或指令的引用。

它能在原生DOM元素中使用，也能用于Angular组件 —— 实际上，它可以和任何自定义Web组件协同工作

```html
<!-- phone refers to the input element; pass its `value` to an event handler -->
<input #phone placeholder="phone number">
<button (click)="callPhone(phone.value)">Call</button>

<!-- fax refers to the input element; pass its `value` to an event handler -->
<input ref-fax placeholder="fax number">
<button (click)="callFax(fax.value)">Fax</button>
```

`phone`的井号(`#`)前缀表示定义了一个`phone`变量。

> 如果看着`#`不舒服，我们可以使用`ref-`前缀。 例如，既能用`#phone`，也能用`ref-phone`来定义`phone`变量。

### `NgForm`和模板引用变量

```html
<form (ngSubmit)="onSubmit(theForm)" #theForm="ngForm">
  <div class="form-group">
    <label for="name">Name</label>
    <input class="form-control" name="name" required [(ngModel)]="currentHero.firstName">
  </div>
  <button type="submit" [disabled]="!theForm.form.valid">Submit</button>
</form>
```

`theForm`是个`ngForm`，对Angular内置指令`NgForm`的引用。 它包装了原生的[HTMLFormElement](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFormElement)并赋予它更多超能力，比如跟踪用户输入的有效性

通过检查`theForm.form.valid`来禁用提交按钮

具体请查看[NgForm](https://angular.cn/docs/ts/latest/api/forms/index/NgForm-directive.html)


### 输入与输出属性

绑定目标是在等号左侧，源则在等号右侧.

绑定的目标是绑定符：`[]`、`()`或`[()]`中的属性或事件名， 源则是引号`" "` 中的部分或插值符号`{{}}`中的部分

源指令中的每个成员都会自动绑定并可用。 不需要特殊配置，就能在模板表达式或语句中访问指令的成员。

访问目标指令中的成员则受到限制。 只能绑定到那些显式标记为输入或输出的属性。

### 声明输入和输出属性

目标属性必须被显式的标记为输入或输出。

```ts
@Input()  hero: Hero;
@Output() deleteRequest = new EventEmitter<Hero>();
```

>还可以在指令元数据的inputs或outputs数组中标记出这些成员
```ts
@Component({
      inputs: ['hero'],
      outputs: ['deleteRequest'],
    })
```
既可以通过装饰器，也可以通过元数据数组来指定输入/输出属性。**但不要同时用！**


* 指定别名
```ts
@Output('myClick') clicks = new EventEmitter<string>(); //  @Output(alias) propertyName
```

>也可在`inputs`和`outputs`数组中为属性指定别名。 可以写一个冒号`:`分隔的字符串，左侧是指令中的属性名，右侧则是公开的别名
```ts
@Directive({
  outputs: ['clicks:myClick']  // propertyName:alias
})
```


一般我们定义事件都是使用输出而且需要配合使用EventEmitter<Object>,其实一个泛型,而属性绑定则使用分为两步:
比如变量`a`, `(aChange)="数据源"` `[a]="数据源"`,从而:

```ts
@Output aChange;
@Input a;
```

> 解析:
左侧是模板的**输入源组件**,右侧则是**使用模板的组件**的属性,这个不要搞混了,只要记住这是两个组件是在互相通信就理解了


### 模板表达式操作符

模板表达式语言使用了`JavaScript`语法的子集，并补充了几个用于特定场景的特殊操作符。 下面介绍其中的两个：管道和安全导航操作符。

* 管道操作符`|`

在绑定之前，表达式的结果可能需要一些转换。例如，可能希望把数字显示成金额、强制文本变成大写，或者过滤列表以及进行排序。

```html
<div>Title through uppercase pipe: {{title | uppercase}}</div>
```

串联管道

```html
<!-- Pipe chaining: convert title to uppercase, then to lowercase -->
<div>
  Title through a pipe chain:
  {{title | uppercase | lowercase}}
</div>
```

带参管道

```html
<div>Birthdate: {{currentHero?.birthdate | date:'longDate'}}</div>
```

json管道

```html
<div>{{currentHero | json}}</div>
```

更多管道请查看<https://angular.cn/docs/ts/latest/api/#!?apiFilter=pipe&type=pipe>

* 安全导航操作符`?.`和空属性路径

Angular的安全导航操作符`?.`是一种流畅而便利的方式，用来保护出现在属性路径中`null`和`undefined`值。 下例中，当`currentHero`为空时，保护视图渲染器，让它免于失败

```
The current hero's name is {{currentHero?.firstName}}
```

这样我们就不会让程序中出现异常的情况,从而省去了一大堆的判断语句

### 其他资料

[中文官方文档](https://angular.cn/docs/ts/latest/guide/template-syntax.html)

[英文官方文档](https://angular.io/docs/ts/latest/guide/template-syntax.html)



{% endraw %}