---
title: license-server 1.0.3发布
layout: post
date: '2018-06-12 12:00:00'
categories: license-server
tags: jrebel idea jetbrains license-server mybatis-plugins-server ldapkeygen MyBatisCodeHelper-Pro android
author: luamas
original: true
---

* content
{:toc}

查看1.0.0版本
[http://blog.luamas.com/2017/10/24/LICENSE-SERVER-v1.0.0/](http://blog.luamas.com/2017/10/24/LICENSE-SERVER-v1.0.0/)
查看1.0.1版本
[http://blog.luamas.com/2017/10/24/LICENSE-SERVER-v1.0.1/](http://blog.luamas.com/2017/10/24/LICENSE-SERVER-v1.0.1/)
查看1.0.2版本
[http://blog.luamas.com/2018/04/28/LICENSE-SERVER-v1.0.2/](http://blog.luamas.com/2018/04/28/LICENSE-SERVER-v1.0.2/)







文件下载地址(有的广告，有下载不了的请自行登梯子)


#### uploadocean:

[https://uploadocean.com/3jc2oha5uj20](https://uploadocean.com/3jc2oha5uj20)


#### MEGA

[https://mega.nz/#!17o3xD6J!Iw2R35WtGO8LdZi4z_yDEfKfKT7A2DEkdwxAsmMA6P8](https://mega.nz/#!17o3xD6J!Iw2R35WtGO8LdZi4z_yDEfKfKT7A2DEkdwxAsmMA6P8)


#### 百度网盘：

链接:[https://pan.baidu.com/s/1Uz8zKdYg75ZjgGHWizr9dw](https://pan.baidu.com/s/1Uz8zKdYg75ZjgGHWizr9dw) 密码:3dnt


43df249c779d829c370fc47e1fb3dfb9  license-server(v1.0.3).tar.gz

默认端口号为22508

## idea和jetbrain系列产品的license-server方式注册

## jrebel和zeroturnaround系列产品的license-server方式注册

在界面中点击Tools->Jrebel For Android->I have a license,选择到Connect to License Server, Email 中随意填写一个邮箱,在Group URL中填写http://x.x.x.x:22508/luamas
(x.x.x.x是域名地址，如果是本地则可以使用127.0.0.1,22508是端口号，默认为22508),勾选 I agree with terms & conditions of the License Agreement, 点击 Activate JRebel for Android 即可激活.

## LDAPSoft系列软件的注册

*增加对ldap的支持，具体查看*

[http://blog.luamas.com/2017/08/09/LDAPSoft-LDAP-Admin-Tool-key/](http://blog.luamas.com/2017/08/09/LDAPSoft-LDAP-Admin-Tool-key/)

路径为/ldap，t=i，其中i为产品类型

###注册机支持以下产品的注册

0.LDAP Admin Tool(Professional Edition)([点击获取激活码](http://ldap.luamas.com/ldap?t=0))

1.LDAP Admin Tool(Standard Edition)([点击获取激活码](http://ldap.luamas.com/ldap?t=1))

2.AD Admin & Reporting Tool([点击获取激活码](http://ldap.luamas.com/ldap?t=2))

3.LDAP Plus AD HelpDesk Professional Tool([点击获取激活码](http://ldap.luamas.com/ldap?t=3))

4.LDAP Admin & Reporting Tool([点击获取激活码](http://ldap.luamas.com/ldap?t=4))

5.AD Admin Tool([点击获取激活码](http://ldap.luamas.com/ldap?t=5))

## Mybatis Plugin 2.x

mybatis插件需要用nginx做反向代理到域名www.codesmagic.com，并且使用https方式(这里需要对服务器有一定的了解)

做好之后将自己服务器ip地址做host,如：`127.0.0.1  www.codesmagic.com

## MyBatisCodeHelperPro

增加idea的mybatis插件MyBatisCodeHelperPro的支持，具体路径/codehelp查看使用说明


# 帮助信息(./license-server_darwin_amd64 -h)

  -host string
  
        bind on host (default "0.0.0.0")
        
  -port int
  
        port number (default 22508)
        
  -prolongationPeriod int
  
        prolong ation period (default 607875500)
        
  -u string
  
        If you want to use a local user name, fill in empty such, -u empty (default "luamas")
        
  -v    
  
        show version


1.0.2新增功能
更新编译版本，增加Android手机端支持

1.0.3更改
更新编译器版本，进行优化


# docker服务(作废，已被和谐)

[https://hub.docker.com/r/luamas/license-server/](https://hub.docker.com/r/luamas/license-server/)

Dockerfile如下：


```
FROM scratch
ADD license-server/license-server_linux_amd64 /license-server
ENTRYPOINT ["/license-server"]

```

构建,将Dockerfile放在license-server的同级目录

```
docker build --rm=true -t luamas/jetbrains-license-server:latest --file Dockerfile .
```

运行

```
docker run --restart=always \
-p 22508:22508 \
--name license-server -d luamas/jetbrains-license-server
```

# Android支持 1.0.2版本

#### uploadocean:

[https://uploadocean.com/jt4dn8odk8b6](https://uploadocean.com/jt4dn8odk8b6)


#### MEGA

[https://mega.nz/#!tiA0DbCT!x9MfaVucdRn0fQXr-mTLVr3MsqFjGo0ZbZQjjkeawOM](https://mega.nz/#!tiA0DbCT!x9MfaVucdRn0fQXr-mTLVr3MsqFjGo0ZbZQjjkeawOM)


#### 百度网盘：

链接:[https://pan.baidu.com/s/1pWoM1u3ftNCgCHml0sUeeA](https://pan.baidu.com/s/1pWoM1u3ftNCgCHml0sUeeA) 密码:khj9


##### 如有愿意开发ios端的朋友，可以联系我要.framework库文件

# 来杯咖啡

AliPay

![Alipay](http://blog.luamas.com/images/aliPay.jpg)

WXPay

![WechatPay](http://blog.luamas.com/images/wechatPay.jpg)



如有侵权请联系删除，谢谢！

