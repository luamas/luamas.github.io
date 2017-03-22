---
title: Angular2学习笔记(三) Angular2 数据显示
layout: post
date: '2017-03-22 00:00:00'
categories: angular2 typescript
tags: angular2 angular-cli typescript
---

* content
{:toc}

前几节是Angular2和TypeScript分开学的,以后准备边学Angular2遍学TypeScript,只有边学边用,才会更深刻的理解TypeScript,好,下面直奔主题

### 创建应用

前面我们已经介绍了如何安装nodejs和angular-cli,下面我们默认都已经安装了angular-cli,首先来创建一个名为my-app的应用

```
ng new my-app
```

等待几分钟后会创建好应用,现在我们将对应用进行自定义的修改以下文件

+ app.component.html(模板文件)

```
<h1>{{title}}</h1>
<h2>介绍: {{introduction}}</h2>
<h2>我的技能:</h2>
方法一展示
<ul>
  <!--这里采用TypeScript语法遍历skills-->
  <li *ngFor="let skill of skills1">
    id: {{ skill.id }} ,名称: {{ skill.name }}
  </li>
</ul>
方法二展示
<ul class="show2">
  <!--这里采用TypeScript语法遍历skills-->
  <li *ngFor="let skill of skills2">
    id: {{ skill.id }} ,名称: {{ skill.name }}
  </li>
</ul>

<p *ngIf="skills1.length >= 3">哇,好厉害哦,你有三种以上技能!</p>
<p *ngIf="skills1.length < 3">您不到三种技能,加油哦!</p>
```

上面的{{title}}和{{introduction}},title和introduction为组件的变量,其实就是输出变量,当然是动态的了(当变量变化自动更新页面),
*ngFor="let skill of skills1"代表循环变量skills1,*ngIf="skills1.length >= 3"同理是判断语句

+ 增加skill.ts文件

```
export class Skill {
  id: number;
  name: string;
  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}
```

这里出现了export关键词,只有export了,其他地方才可以import,constructor是构造方法,其实两个参数.

+ app.component.ts文件如下

```
import { Component } from '@angular/core';
import { Skill } from './skill';
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
// export为导出类(如果想外部引用必须要求可导出,否则外部不可以导入),typescript语法
export class AppComponent {
  title = 'Angular2笔记';
  introduction = '我是一个Angular2新手';
  skills1: Skill[] = [
    {id: 1, name: 'JAVA'},
    {id: 2, name: 'PHP'},
    {id: 3, name: 'Python'},
    {id: 4, name: 'JavaScript'},
    {id: 5, name: 'Others'}];

  // 必须要求有对应的构造方法
  skills2 = [
    new Skill(1, 'JAVA'),
    new Skill(2, 'PHP'),
    new Skill(3, 'Python'),
    new Skill(4, 'JavaScript'),
    new Skill(5, 'Others'),
  ];
}
```
上面我们初始化了四个变量,其中title和introduction为字符串类型,skills1和skills2为数组类型,skills1: Skill\[\]可以加类型也可以不加类型,
不加类型代表松散类型对象,skills2同样也可以规定类型.new Skill代表创建对象并给定参数,上面已经给出具体构造方法.

+ app.component.css 我们为其增加模板样式

```
h1 {color: red}
h2 {font-size: large;}
.show2 {
  list-style-type: none ;
}
.show2 li {
  color: green;
}
```

+ app.component.spec.ts为集成测试文件,暂时不用管它,只要不执行test就不会出错,或者我们删除相关的对


### 我们在命令行执行ng s 就可以运行项目

项目运行时会出现Loading...英文字母,因为angular2渲染需要时间,所以会闪动,我们可以将其修改成自己喜欢的一些动画之类的来提高体验效果,其在index.html文件中



#### 注意
我们要使用import的时候一定要export一下,否则无法使用

[查看源代码](https://github.com/luamas/angular2-sample)