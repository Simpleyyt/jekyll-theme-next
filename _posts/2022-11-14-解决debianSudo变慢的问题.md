---
title: 解决Debian/Ubuntu/Deepin在sudo时变慢等待
description: 在我升级使用Deepin20.7的时候突然发现使用sudo命令总是需要等待个十秒钟，特别影响使用体验，于是经过查询发现使用sudo命令时，会首先通过网络查找sudoer名单，然后才是本地。10秒钟恰恰是DNS超时时间。解决方法也很简单，只需要通过修改hosts将域名直接指向localhost即可
categories:
 - linux
tags:
 - linux
 - solution
 - debian
keywords:
  - sudo
  - sudoer
  - delay
  - slow
  - debian
  - ubuntu
  - deepin
  - upgrade
  - DNS
  - localhost
  - 10
  - minute
  - network
date: 2022-11-14 00:00:00
updated: 2022-11-14 00:00:00
---

## 概述

在我升级使用Deepin20.7的时候突然发现使用sudo命令总是需要等待个十秒钟，特别影响使用体验，于是经过查询发现使用sudo命令时，会首先通过网络查找sudoer名单，然后才是本地。10秒钟恰恰是DNS超时时间。解决方法也很简单，只需要通过修改hosts将域名直接指向localhost即可。

## 解决步骤

在控制台输入

```shell
hostname
```

输出主机名。然后使用

```shell
sudo vi /etc/hosts
```

在hosts文件中添加一行(注意使用`Tab`实现分割)：

```text
127.0.0.1 {YOUR HOSTNAME} {YOUR HOSTNAME}.localdomain
```

例如我的`hostname`是`zhengqiaowang`，那么就可以在文件末尾追加

```text
127.0.0.1 zhengqiaowang zhengqiaowang.localdomain
```

保存后即可解决。
