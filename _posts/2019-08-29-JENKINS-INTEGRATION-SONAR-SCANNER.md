---
title: jenkins+sonarscanner
layout: post
date: '2018-09-29 16:30:00'
categories: centos
tags: jenkins jenkins2 sonar-scanner
author: luamas
original: true
---

* content
{:toc}


### 生成SonarQube中的token
`配置`->`权限`->`用户`，admin用户点击令牌列的更新令牌按钮，随便起一个名字比如jenkins，生成，记住这个令牌，下面会在jenkins的SonarQube Scanner插件中使用

### 下载sonar-scanner
```bash
#将sonar-scanner安装到/usr/local
cd /usr/local
sudo wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.0.0.1744-linux.zip
sudo unzip sonar-scanner-cli-4.0.0.1744-linux.zip
```
修改`/usr/local/sonar-scanner-4.0.0.1744-linux/bin/sonar-scanner`并加入`JAVA_HOME = /usr/lib/jvm/java-11`

### jenkins安装sonar-scanner插件
`系统管理`->`插件管理`->`可选插件`,搜索`SonarQube Scanner`点击直接安装并勾选`安装完成后重启Jenkins(空闲时)`

安装完重启后接下来来配置`SonarQube Scanner`的路径和SonarQube服务器的令牌

`系统管理`->`全局工具配置`->`SonarQube Scanner`->`新增 SonarQube Scanner`,Name填写为SonarScanner，
取消默认安装，SONAR_RUNNER_HOME填写/usr/local/sonar-scanner-4.0.0.1744-linux点击保存

`系统管理`->`系统设置`->`SonarQube servers`->`Add SonarQube`,Name填写为SonarQubeServer，
Server URL填写sonar服务器地址即可，Server authentication token，凭证类型为`Secret text`,Secret为上面第一步的token令牌，描述填写sonar，点击保存


### 为任务增加构建扫描

创建任务（具体查看前面章节）点击Execute SonarQube Scanner，Analysis properties属性如下（sonar.projectKey和sonar.projectName保持一致且在sonar下唯一）
```
sonar.projectKey=test
sonar.projectName=test
sonar.sources=.
sonar.java.binaries=.
sonar.sourceEncoding=UTF-8
sonar.language=java
```

点击保存，然后执行此任务即可完成集成


### 在sonar服务器新建项目名为test的名称

# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

