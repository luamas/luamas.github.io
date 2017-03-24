---
title: Jekyll 搭建静态博客
layout: post
date: '2017-03-20 00:00:00'
categories: jekyll
tags: jekyll gem gitpage
author: luamas
original: true
---

* content
{:toc}

一直都想搭建一个长久可以使用的博客,之前自己使用WordPress搭建过,由于服务器经常会变动,后来自己的博客也就搁置了,所以自己研究了下基于git page的静态博客,在此感谢[GyH](http://https://github.com/Gaohaoyang) ,这里使用了其主体模板,有改变.
自己采用mac系统搭建



## 安装过程

brew需要翻墙,要么自己改源,具体谷歌或者百度

### 安装Ruby

```shell
brew install ruby
```

### 用RubyGems安装Jekyll

```shell
gem install jekyll
```

### 创建博客

```shell
jekyll new luamas.github.com
```
将主体替换到此文件夹,然后定制化自己的页面,并修改_config.yml配置文件的一些配置

### 启动
```shell
jekyll serve #或者jekyll s
```
### 利用jekyll-admin插件写文章
访问 http://127.0.0.1:4000/admin 即可写文章
上面需要自己在Gemfile中添加如下依赖,并注释掉minima(默认主题)依赖,
```shell
gem 'jekyll-admin', group: :jekyll_plugins
```
commit所有文件到本地git到仓库,然后push至luamas.github.com仓库

绑定自己的域名
增加CNAME文件,并将自己要绑定域名写入CNAME文件中,然后去域名中心用cname方式解析