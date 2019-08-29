---
title: jenkins+git使用
layout: post
date: '2018-09-29 14:00:00'
categories: centos
tags: jenkins jenkins2
author: luamas
original: true
---

* content
{:toc}


### 在jenkins创建一个自由风格的软件项目test
在常规里面选择JDK为你项目的jdk版本

源码管理选择git，在Repository URL输入git地址，
在Credentials点击添加凭据类型我们选择`SSH Username with private key`，Username随意填写，这里只是
一个标识这里我起名为jenkins（需要在git网页上面添加部署公钥）

在jenkins所在服务器输入ssh-keygen生成ssh私钥和公钥


从jenkins拿到ssh key的私钥`cat ~/.ssh/id_rsa`，选择Enter directly-> Add粘贴即可，Passphrase值在你生成ssh key的时候会让输入，一般留空即可

Branch Specifier 我们来选择分支默认`*/master`,master分支


在构建位置增加gradle脚本，因为我的项目是基于gradle的项目。Tasks，填写`build -x test`，忽略测试并构建

点击保存

在手动点击立即构建即可触发下拉代码并开始构建


在jenkins插件管理—>可选插件搜索gitlab关键字，安装GitLab API ，Gitlab Authentication plugin，Gitlab Hook Plugin，GitLab Plugin。


勾选构建触发器的Build when a change is pushed to GitLab. GitLab webhook选项
将url复制到gitlab的webhook即可触发自动化

系统管理->Gitlab->Enable authentication for '/project' end-point取消勾选点击保存

在项目中触发提交事件即可触发







# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

