---
title: Ionic3学习笔记(一) Ionic3 快速创建项目
layout: post
date: '2017-05-10 00:00:00'
categories: Ionic
tags: Ionic
author: luamas
original: true
---

* content
{:toc}

{% raw %}


采用cli方式安装,安装之前需要自行安装nodejs,以及ios和android开发环境

安装Ionic和Cordova
```bash
npm install -g cordova ionic
```




在当前目录下创建应用并启动
```bash
ionic start ionic3-sample tabs
```

上面的tab是钟模型,你也可以使用blank(空项目),sidemenu(侧边栏).



进入目录并启动app
```bash
cd ionic3-sample
ionic serve
```

打开<http://localhost:8100/>


项目整体结构其实就是angular 2+版本的目录结构,具体可以查看angular2系列笔记

在android模拟器中运行
```bash
ionic cordova run android --emulator
# 也可以使用以下命令,选择android
ionic cordova run --emulator
```

在ios模拟器中运行
```bash
ionic cordova run ios --emulator
```

在android设备中运行
```bash
ionic cordova run android --device
```

在ios设备中运行,需要先生成出源码在用xcode去打包,并且需要签名,这里暂时不去过多学习.


ionic cordova run的命令集合


    --livereload, -l ......... Live reload app dev files from the device

    --address ................ Use specific address (livereload req.) (default: 0.0.0.0)

    --consolelogs, -c ........ Print app console logs to Ionic CLI

    --serverlogs, -s ......... Print dev server logs to Ionic CLI

    --port, -p ............... Dev server HTTP port (default: 8100)

    --livereload-port, -r .... Live Reload port (default: 35729)

    --prod ................... Create a prod build with app-scripts

    --list ................... List all available Cordova run targets

    --debug .................. Create a Cordova debug build

    --release ................ Create a Cordova release build

    --device ................. Deploy Cordova build to a device

    --emulator ............... Deploy Cordova build to an emulator

    --target ................. Deploy Cordova build to a device (use --list to see all)

    --buildConfig ............ Use the specified Cordova build configuration


[查看源代码](https://github.com/luamas/ionic3-sample/tree/l1)

{% endraw %}

