---
title: 记录一次mongo引发cpu使用率99%的问题
layout: post
date: '2017-06-26 10:00:00'
categories: MongoDB
tags: MongoDB 索引
author: luamas
original: true
---

* content
{:toc}

有个应用程序使用的是mongo数据库，其中有个excel的导出功能，因为是小应用，基本属于全库的导出。数据量也就是几万条，外加统计。优化前cpu总是99%


这个原因基本是由于索引引起的问题的，所以增加响应的查询索引即可


由于mongo每天会产生很大的Cache，导致内存一直在消耗中，所以建议增加计划任务去清理Cache。

```
sysctl -w vm.drop_caches=1
```

以下是每天00:00的计划任务

```
00 0 * * * root  sysctl -w vm.drop_caches=1
```



