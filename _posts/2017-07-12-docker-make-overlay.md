---
title: gitlab runner问题
layout: post
date: '2017-07-12 00:00:00'
categories: MongoDB
tags: docker gitlab gitlab-runner
author: luamas
original: true
---

* content
{:toc}

{% raw %}

最近所有项目中均采用docker服务，在搞gitlab-runner的时候，出现了以下错误
```
ERROR: for gitlab-runner  Cannot create container for service gitlab-runner: mkdir /var/lib/docker/overlay/5c686d8eee2998532ee440eb76d3cd54c2e2e02d82fc1d19869ffca419ec5920-init/merged/dev/shm: invalid argument
ERROR: Encountered errors while bringing up the project.
```




这个经过多方查找主要原因在于系统无法支持ext4文件系统造成，在docker的配置文件/etc/docker/daemon.json中添加如下配置即可

```
{ "storage-driver": "devicemapper" }
```




{% endraw %}

