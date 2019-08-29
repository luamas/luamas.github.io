---
title: Centos7安装jenkins2
layout: post
date: '2018-09-29 12:00:00'
categories: centos
tags: jenkins jenkins2
author: luamas
original: true
---

* content
{:toc}


### 准备工作
#### 安装openjdk8(2.164以后版本支持jdk11，由于有些插件的兼容性问题，所以暂时选择jdk8版本)
```bash
sudo yum search java-1.8.0
sudo yum install java-1.8.0-openjdk-devel
```

#### 添加jenkins源
```bash
#长期支持版本
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
#非长期支持版本
#sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
#sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
```





#### 安装jenkins
```bash
sudo yum install -y jenkins
```
```bash
sudo systemctl enable jenkins
```

说明：安装好jenkins会默认创建一个jenkins用户，如果我们要修改jenkins数据一定要使用jenkins用户来修改



### 修改jdk版本设置
```bash
sudo vi /etc/sysconfig/jenkins
#修改JENKINS_JAVA_CMD为java8的java命令
JENKINS_JAVA_CMD="/usr/lib/jvm/java-1.8.0/bin/java"
#重启
sudo systemctl restart jenkins
```

### 进入系统并设置

在浏览器输入http://ip:8080即可访问
```bash
#查看初始临时密码
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

点击继续即可，然后继续安装推荐插件，安装好后会要求创建用户，这里最好新建用户,继续会要求填写网站域名，没有特殊情况继续就可以,完成后最好重启一次jenkins


至此，jenkins安装完毕

接下来会介绍jenkins的设置以及简单集成

# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

