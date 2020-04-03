---
title: jenkins系统工具设置
layout: post
date: '2018-08-29 13:00:00'
categories: centos
tags: jenkins jenkins2
author: luamas
original: true
---

* content
{:toc}


### 全局安装git，jdk8和jdk11
```bash
sudo yum install -y java-1.8.0-openjdk-devel java-11-openjdk-devel git unzip
```




#### 下载所需工具
```bash
#将工具下载到`/usr/local`目录
cd /usr/local
#下载安装maven
sudo wget http://mirror.cc.columbia.edu/pub/software/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz
sudo tar zxvf apache-maven-3.6.1-bin.tar.gz
#下载安装gradle
sudo wget https://downloads.gradle-dn.com/distributions/gradle-5.6.1-all.zip
sudo unzip gradle-5.6.1-all.zip
#下载安装ant
sudo wget http://mirror.bit.edu.cn/apache//ant/binaries/apache-ant-1.10.6-bin.tar.gz
sudo tar zxvf apache-ant-1.10.6-bin.tar.gz
```


docker 安装在以后的文章会有讲到，这里暂时先预留

### jenkins页面配置

打开：系统管理->全局工具配置

#### JDK配置

我们先加JDK8，点击新增JDK，并取消掉自动安装，别名`JDK8`，JAVA_HOME`/usr/lib/jvm/java-1.8.0`

同理JDK11一样，别名`JDK11`，JAVA_HOME`/usr/lib/jvm/java-11`

应用即可保存

#### Git配置

这个没什么可说的，我们如果系统有安装Git工具，默认就可以了

#### Gradle配置

点击新增JDK，并取消掉自动安装，name`Gradle56`,GRADLE_HOME`/usr/local/gradle-5.6.1`

点击应用

#### Ant配置

点击新增JDK，并取消掉自动安装，name`Ant`,GRADLE_HOME`/usr/local/apache-ant-1.10.6`

点击应用

#### Maven配置

点击新增JDK，并取消掉自动安装，name`M3`,GRADLE_HOME`/usr/local/apache-maven-3.6.1`

点击应用


至此环境配置完毕

# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

