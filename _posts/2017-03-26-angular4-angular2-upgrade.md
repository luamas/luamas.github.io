---
title: Angular2升级到Angular4
layout: post
date: '2017-03-26 10:00:00'
categories: angular2
tags: angular2,angular4
author: luamas
original: true
---

* content
{:toc}

{% raw %}

angular4终于在两天前发布了正式版本,那么怎么升级呢?其实angular2和angular4之间属于平滑过渡,并不像1和2之间颠覆性的重写代码.






1. 使用npm-check方案升级
使用如下命令检查,并按下空格来选择要升级的包
```
npm-check -u
```


2. 官方推荐
    1. 升级cli

    ```
    npm uninstall -g @angular/cli
    npm cache clean
    npm install -g @angular/cli@latest

    rm -rf node_modules dist # use rmdir /S/Q node_modules dist in Windows Command Prompt; use rm -r -fo node_modules,dist in Windows PowerShell
    npm install --save-dev @angular/cli@latest
    ```
    2. 升级包

    ```
    // linux/mac
    npm install @angular/{common,compiler,compiler-cli,core,forms,http,platform-browser,platform-browser-dynamic,platform-server,router,animations}@latest typescript@latest --save
    // Windows
    npm install @angular/common@latest @angular/compiler@latest @angular/compiler-cli@latest @angular/core@latest @angular/forms@latest @angular/http@latest @angular/platform-browser@latest @angular/platform-browser-dynamic@latest @angular/platform-server@latest @angular/router@latest @angular/animations@latest typescript@latest --save
    ```

    3. 更换一些其他包

    ```
    npm install zone.js@0.8.4 --save
    ```

    3. 执行安装命令

    ```
    npm install
    ```


{% endraw %}