---
title: centos7内核升级
layout: post
date: '2017-07-19 00:00:00'
categories: MongoDB
tags: centos7 内核 docker
author: luamas
original: true
---

* content
{:toc}

今天在docker下运行gitlab-runner的集成环境的时候总是报错，换了n种方案依旧不可以。后来怀疑是内核问题，果断升级了下
下面是升级记录




查看下初始内核`uname -r`

3.10.0-327.4.5.el7.x86_64

好，开始升级

在yum的ELRepo源中，有mainline、long-term这2个内核版本，long-term更稳定，会长期更新，所以选择这个版本

```
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-lt -y
#然后修改默认启动方式,修改GRUB_DEFAULT为0
vim /etc/default/grub
grub2-mkconfig -o /boot/grub2/grub.cfg
reboot
```

最后恢复GRUB_DEFAULT的值，这里不恢复也可以
```
# GRUB_DEFAULT=saved
vim /etc/default/grub
reboot
```



查看下内核`uname -r`

4.4.77-1.el7.elrepo.x86_64


至此结束


