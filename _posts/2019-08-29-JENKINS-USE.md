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
一个标识这里我起名为jenkins

在jenkins所在服务器输入ssh-keygen生成ssh私钥和公钥


从jenkins拿到ssh key的私钥`cat ~/.ssh/id_rsa`，选择Enter directly-> Add粘贴即可，Passphrase值在你生成ssh key的时候会让输入，一般留空即可


在git网页上面增加部署公钥即可

在jenkins页面保存即可，手动点击立即构建即可触发下拉代码





# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

