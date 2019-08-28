---
title: 安装sonarqube7.9.1
layout: post
date: '2018-09-29 12:00:00'
categories: centos
tags: sonarqube7 sonarqube sonar postgresql
author: luamas
original: true
---

* content
{:toc}


### 准备工作
#### 安装openjdk11(sonar7.9.1需要jdk11)
```bash
sudo yum search java-11
sudo yum install java-11-openjdk-devel
```

#### 创建数据库([查看postgresql11安装](http://blog.luamas.com/2019/08/28/INSTALLATION-POSTGRESQL11/))
```bash
#在postgresql11所在服务器执行
sudo su - postgres
psql
create user sonar with password 'sonar';
create database sonar;
create database sonar owner sonar;
grant all on database sonar to sonar;
create schema my_schema;
exit
exit
```



### 创建sonarqube用户
```bash
useradd sonarqube
```

### 优化系统参数
```bash
sudo sysctl -w vm.max_map_count=262144
sudo sysctl -w fs.file-max=65536
ulimit -u 4096 sonarqube
ulimit -n 65536 sonarqube
```



### 下载安装包并安装
```bash
#安装unzip,wget,vim，如果确认已安装不执行也可以
sudo yum install -y unzip wget vim 
#如果是复制过来的文件最好重新给用户目录授权下
sudo chown -R sonarqube:sonarqube /home/sonarqube/
#切换到sonarqube用户
sudo su - sonarqube
#解压sonarqube
unzip sonarqube-7.9.1.zip
```

### 更改配置文件
```bash
#这里依然在sonarqube用户下修改
vim sonarqube-7.9.1/conf/sonar.properties
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://127.0.0.1/sonar
```

### 添加系统服务
```bash
#退出sonarqube用户
exit
sudo ln -s /home/sonarqube/sonarqube-7.9.1/bin/linux-x86-64/sonar.sh  /usr/bin/sonar
sudo vim /etc/init.d/sonar
```


以下是/etc/init.d/sonar内容

```bash 
#!/bin/sh
#
# rc file for SonarQube
#
# chkconfig: 345 96 10
# description: SonarQube system (www.sonarsource.org)
#
### BEGIN INIT INFO
# Provides: sonar
# Required-Start: $network
# Required-Stop: $network
# Default-Start: 3 4 5
# Default-Stop: 0 1 2 6
# Short-Description: SonarQube system (www.sonarsource.org)
# Description: SonarQube system (www.sonarsource.org)
### END INIT INFO
 
su sonarqube -c "/usr/bin/sonar $*"
```

````bash
sudo chmod 755 /etc/init.d/sonar
sudo chkconfig --add sonar
sudo service sonar start
````


如果发现有报错，请到logs查看具体错误。


### 拓展中文插件
[sonar中文插件](https://github.com/SonarQubeCommunity/sonar-l10n-zh)
点击releases下载，我这里下载的sonar-l10n-zh-plugin-1.29.jar
````bash
sudo su - sonarqube
wget https://github.com/SonarQubeCommunity/sonar-l10n-zh/releases/download/sonar-l10n-zh-plugin-1.29/sonar-l10n-zh-plugin-1.29.jar -P sonarqube-7.9.1/extensions/plugins
exit
sudo service sonar restart
````


默认用户名:admin
默认密码:admin

# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

