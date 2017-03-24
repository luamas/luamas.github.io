---
title: Angular2学习笔记(一) angular-cli创建项目
layout: post
date: '2017-03-20 00:00:00'
categories: angular2
tags: angular2 angular-cli
author: luamas
original: true
---

* content
{:toc}


### 首要工具安装 [nodejs](https://nodejs.org/en/)和npm(安装好nodejs后会自带)
更改npm为国内镜像(国外比较慢)
```
npm config set registry https://registry.npm.taobao.org 
npm info underscore （如果上面配置正确这个命令会有字符串response）
```



### 安装angular-cli工具
```
npm install -g @angular/cli
```

### 创建项目
这里可能会比较慢,稍等下
```
ng new my-app
```
### 启动项目
```
cd my-app
ng serve
```
打开地址 http://localhost:4200/ 即可看到项目

### 文件结构如下
![](http://luamas.github.io/images/2017/03/20/file_structure.png)

### 内部文件说明

app/app.component.{ts,html,css,spec.ts}	
使用HTML模板、CSS样式和单元测试定义AppComponent组件。 它是根组件，随着应用的成长它会成为一棵组件树的根节点。

app/app.module.ts	
定义AppModule，这个根模块会告诉Angular如何组装该应用。 目前，它只声明了AppComponent。 稍后它还会声明更多组件。

assets/*	
这个文件夹下你可以放图片等任何东西，在构建应用时，它们全都会拷贝到发布包中。

environments/*	
这个文件夹中包括为各个目标环境准备的文件，它们导出了一些应用中要用到的配置变量。 这些文件会在构建应用时被替换。 比如你可能在产品环境中使用不同的API端点地址，或使用不同的统计Token参数。 甚至使用一些模拟服务。 所有这些，CLI都替你考虑到了。
favicon.ico	
每个网站都希望自己在书签栏中能好看一点。 请把它换成你自己的图标。

index.html	
这是别人访问你的网站是看到的主页面的HTML文件。 大多数情况下你都不用编辑它。 在构建应用时，CLI会自动把所有js和css文件添加进去，所以你不必在这里手动添加任何 <script> 或 <link> 标签。

main.ts	
这是应用的主要入口点。 使用JIT compiler编译器编译本应用，并启动，使其运行在浏览器中。 你还可以使用AOT compiler编译器，而不用修改任何代码 —— 只要给ng build 或 ng serve 传入 --aot 参数就可以了。

polyfills.ts	
不同的浏览器对Web标准的支持程度也不同。 填充库（polyfill）能帮我们把这些不同点进行标准化。 你只要使用core-js 和 zone.js通常就够了，不过你也可以查看浏览器支持指南以了解更多信息。

styles.css	
这里是你的全局样式。 大多数情况下，你会希望在组件中使用局部样式，以利于维护，不过那些会影响你整个应用的样式你还是需要集中存放在这里。

test.ts	
这是单元测试的主要入口点。 它有一些你不熟悉的自定义配置，不过你并不需要编辑这里的任何东西。

tsconfig.json	
TypeScript编译器的配置文件。


### 外部文件说明

e2e/*	
在e2e/下是端到端（End-to-End）测试。 它们不在src/下，是因为端到端测试实际上和应用是相互独立的，它只适用于测试你的应用而已。 这也就是为什么它会拥有自己的tsconfig.json。

node_modules/...	
Node.js创建了这个文件夹，并且把package.json中列举的所有第三方模块都放在其中。

.editorconfig	
给你的编辑器看的一个简单配置文件，它用来确保参与你项目的每个人都具有基本的编辑器配置。 大多数的编辑器都支持.editorconfig文件，详情参见 http://editorconfig.org 。

.gitignore	
一个Git的配置文件，用来确保某些自动生成的文件不会被提交到源码控制系统中。

angular-cli.json	
Angular-CLI的配置。 在这个文件中，你可以设置一系列默认值，还能配置当构件项目时应该排除哪些文件。 要了解更多，请参阅Angular-CLI的官方文档。

karma.conf.js	
给Karma的单元测试配置，当运行ng test时会用到它。

package.json	
npm配置文件，其中列出了项目使用到的第三方依赖包。 你还可以在这里添加自己的自定义脚本。

protractor.conf.js	
给Protractor使用的端到端测试配置文件，当运行ng e2e的时候会用到它。

README.md	
项目的基础文档，预先写入了CLI命令的信息。 别忘了用项目文档改进它，以便每个查看此仓库的人都能据此构建出你的应用。

tslint.json	
给TSLint和Codelyzer用的配置信息，当运行ng lint时会用到。 Lint功能可以帮你保持代码风格的统一。