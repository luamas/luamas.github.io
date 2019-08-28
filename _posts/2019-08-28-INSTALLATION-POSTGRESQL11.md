---
title: Centos安装postgresql11最新版
layout: post
date: '2019-08-28 12:00:00'
categories: license-server
tags:  postgresql11 postgresql
author: luamas
original: true
---

* content
{:toc}


一、下载rpm包

地址：https://yum.postgresql.org/repopackages.php

这里我的是centos7所以我选择的

https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

### 安装
```bash
#安装到yum仓库
sudo yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
#查看postgresql版本
sudo yum list | grep postgresql
#这里我们选择11版本
sudo yum install -y postgresql11-contrib postgresql11-server
```

### 初始化数据库
```bash
sudo /usr/pgsql-11/bin/postgresql-11-setup initdb
```

### 启动并设置开机启动
```bash
sudo systemctl start postgresql-11
sudo systemctl enable postgresql-11
```

### 登录postgresql并设置密码

postgresql在安装时默认添加用户postgres
```bash
sudo su - postgres
psql
#将密码暂时设置为123456
ALTER USER postgres WITH PASSWORD '123456';
#输入exit退出到postgres用户
exit
```

### 修改pg_hba.conf和postgresql.conf支持远程登录

注意这里使用postgres执行
```bash
vi /var/lib/pgsql/11/data/pg_hba.conf
```

此处将三行带有replication注释掉，并在行尾添加如下（METHOD使用trust为可信任方法）

`host     all             all             0.0.0.0/0               trust`

```bash
vi /var/lib/pgsql/11/data/postgresql.conf
```

`增加如下listen_addresses = '*'`

退出postgres用户，重启服务
```bash
exit
sudo systemctl restart postgresql-11
```

### 至此数据库即可远程访问

# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

