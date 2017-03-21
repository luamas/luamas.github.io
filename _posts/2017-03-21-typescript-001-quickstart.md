---
title: TypeScript学习笔记(一) 快速上手
layout: post
date: '2017-03-21 00:00:00'
categories: typescript
tags: typescript
---

* content
{:toc}

### 安装TypeScript

```
npm install -g typescript
```



### 创建第一个TypeScript文件

+ 无类型参数

在编辑器中创建greeter.ts文件，并输入以下JavaScript代码：

```
function greeter(person :any) {
    return "Hello, " + person;
}
var user = "Jane User";
document.body.innerHTML = greeter(user);
```

编译文件

```
tsc greeter.ts
```

编译后
```
function greeter(person) {
    return "Hello, " + person;
}
var user = "Jane User";
document.body.innerHTML = greeter(user);
```

+ 有类型参数

现在我们对person增加类型

```
function greeter(person: string) {
    return "Hello, " + person;
}
var user = [0, 1, 2];
document.body.innerHTML = greeter(user);
```


对其编译,发现会出现类型不正确

```
greeter.ts(17,35): error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```

### 接口

TypeScript创建一个接口,它有两个属性,其类型为string类型,具体看文件interface-sample.ts

```
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = { firstName: "Jane", lastName: "User" };
document.body.innerHTML = greeter(user);
```

编译后文件

```
function greeter(person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}
var user = { firstName: "Jane", lastName: "User" };
document.body.innerHTML = greeter(user);
```

### 类

TypeScript是面向对象语言,同样也具有class概念,文件查看

```
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = new Student("Jane", "M.", "User");
document.body.innerHTML = greeter(user);
```

编译后

```
var Student = (function () {
    function Student(firstName, middleInitial, lastName) {
        this.firstName = firstName;
        this.middleInitial = middleInitial;
        this.lastName = lastName;
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
    return Student;
}());
function greeter(person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}
var user = new Student("Jane", "M.", "User");
document.body.innerHTML = greeter(user);
```

创建greeter.html页面,代码如下
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>TypeScript Greeter</title>
</head>
<body>
<script src="class-sample.js"></script>
</body>
</html>
```

打开浏览后则会出现 Hello, Jane User

[源码地址](https://github.com/luamas/typescript-sample)

