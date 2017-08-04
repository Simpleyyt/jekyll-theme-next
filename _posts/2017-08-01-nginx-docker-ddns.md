---
layout: post
title: "Docker Nginx 动态域名自动刷新"
date: 2017-08-01 23:57:06 
description: "Docker Nginx 动态域名自动刷新"
tag: nginx
---

# Docker Nginx 动态域名自动刷新
nginx代理动态域名时，当域名改变，nginx不会自动更新ip。
在网上找到了两个脚本，修改下可以直接使用。
感谢[fullbug](http://blog.csdn.net/fullbug/article/details/54175987)提供的脚本。

## 适用于docker-nginx
### getip.sh
```shell
#!/bin/sh
if [ $# -lt 1 ]; then
    echo $0 need a parameter
    exit 0
fi

ADDR=$1
TMPSTR=`ping ${ADDR} -s 1 -c 1 | grep ${ADDR} | head -n 1`
echo ${TMPSTR} | cut -d'(' -f 2 | cut -d')' -f1
```
### reload_nginx.sh
```shell
#!/bin/bash
echo '...begin...'
if [ $# -lt 1 ]; then
    echo $0 need a host parameter
    exit 0
fi
if [ ! -n "$2" ] ;then
   sleeptime=10
else
   sleeptime=$2
fi

echo '...refreshtime='${sleeptime}'s'
host=$1
ipfile="ip.ini"

while [ true ]; do

  runlogfile="run."`date "+%Y-%m-%d"`".log"
  reloadlogfile="reload."`date "+%Y-%m-%d"`".log"
  echo `date`'...read ip.ini...'>>"$runlogfile" >&1
  if [ ! -f "$ipfile" ]; then
    #touch "$ipfile"
    sh getip.sh "$host" > "$ipfile"
  fi

  oldIpAddress=`cat ip.ini |head -n 1`
  curIpAddress=`sh getip.sh "$host"`
  echo `date`'...oldIpAddress='${oldIpAddress} >>"$runlogfile"
  echo `date`'...curIpAddress='${curIpAddress} >>"$runlogfile"

  if [ "$oldIpAddress" != "$curIpAddress" ];then
     echo '..oldIpAddress:'${oldIpAddress}'!=curIpAddress:'${curIpAddress}'.......' >>"$runlogfile"
     docker restart nginx
     echo '...docker restart nginx....' >>"$runlogfile"
     sh getip.sh "$host" > "$ipfile"
     echo `date`'...ipchanged..oldIpAddress:'${oldIpAddress}'!=curIpAddress:'${curIpAddress}'..docker restart nginx' >>"$reloadlogfile"
  fi
 
  /bin/sleep "$sleeptime"
done

echo '...end .....'
```

## 使用方法
```shell
nohup reload_nginx.sh 域名 刷新时间 > /dev/null &
```
> nohup reload_nginx.sh www.baidu.com 30 > /dev/null &
