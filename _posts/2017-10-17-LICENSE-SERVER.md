---
title: idea ando jrebel license-server
layout: post
date: '2017-10-17 00:00:00'
categories: license-server
tags: jrebel idea jetbrains license-server mybatis-plugins-server
author: luamas
original: true
---

* content
{:toc}

这个license-server之前已经搞完很久了，一直没大面积上传，版本号也没有固定，每次都是直接覆盖，只是在docker里面做了镜像，如果使用docker可以直接访问以下地址

[https://hub.docker.com/r/luamas/license-server/](https://hub.docker.com/r/luamas/license-server/)

文件下载地址(有广告，有下载不了的请自行登梯子)
#[https://uploadocean.com/w2xtc2qfgyd9](https://uploadocean.com/w2xtc2qfgyd9)

下载程序后可以使用以下命令查看可选参数（mac是darwin）

`./license-server_darwin_amd64 -h`

-host string

        bind on host (default "0.0.0.0")
        
  -port int
  
        port number (default 22508)
        
  -prolongationPeriod int
  
        prolong ation period (default 607875500)
  -u string
  
        If you want to use a local user name, fill in empty such, -u empty (default "luamas")

内部可以激活jetbrains系列和jrebel的license-server方式激活

至于idea的mybatis插件需要用nginx做反向代理到域名www.codesmagic.com，并且使用https方式(这里需要对服务器有一定的了解)
做好之后将自己服务器ip地址做host,如：`127.0.0.1  www.codesmagic.com

jrebel支持离线激活和在线切换，之前有看到lanyus的是没有这个功能的，所以自己写了一个出来。


如有侵权请联系删除，谢谢！

