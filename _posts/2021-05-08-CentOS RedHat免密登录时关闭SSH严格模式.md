---
title: CentOS RedHat免密登录时关闭SSH严格模式
description: 在CentOS RedHat免密登录时关闭SSH严格模式，实现免密登陆。在使用VSCode的RemoteSSH连接远程RedHat服务器的时候发现总是链接失败，直接使用ssh可以免密登录。经过检查发现服务器默认设置了严格模式。
categories:
 - linux
tags:
 - solution
 - linux
 - ssh
 - redhat/centos
keywords:
  - linux
  - centos
  - redhat
  - 免密登陆
  - ssh
  - strictmode
---

## 前言

在使用VSCode的RemoteSSH连接远程RedHat服务器的时候发现总是链接失败，直接使用

```bash
ssh root@xxxx
```

这种形式可以免密登录。经过检查发现服务器默认设置了严格模式。

## 操作步骤

修改`/etc/ssh/sshd_config`文件，将`StrictMode`从`yes`设置为`no`：

```bash
StrictModes yes
```

改为

```bash
StrictModes no
```

保存后，重启ssh服务：

```bash
service sshd restart
```

## 原因分析

这里原因是启动了严格模式，有关Openssl的StrictModes，找到相关资料说明如下：
> **StrictModes**
> OpenSSH enables the StrictModes option by default. StrictModes disable the use of public and private key authentication if the required files are not properly protected to protect public key files when security is too relaxed.
> **Troubleshoot SSH**
> When you troubleshoot collective security problems where the controller cannot start or stop a member, or you cannot view or edit configuration files by using the Admin Center, if you disable StrictModes and the problem is solved, then the ownership, group, or world access to ssh directories or files that are related to public and private key authentication are incorrect. Correct the access and reenable StrictModes.

可见严格模式下，会禁用公钥和私钥认证，但本机使用命令行为什么能访问这个存疑。
