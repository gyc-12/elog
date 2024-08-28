---
title: openwrt 部署 Alist
date: 2024-8-24 19:07:05
tags: "笔记"
thumbnail: "https://alist.nn.ci/logo.svg"
---
openwrt中使用alist，设备为斐讯的 n1盒子。

![image.png](https://raw.githubusercontent.com/gyc-12/images/master/7f9af8dadf496381100382f599e1fe51.png)
```shell
#下载对应架构的安装包
wget https://github.com/alist-org/alist/releases/download/beta/alist-linux-musl-arm64.tar.gz
# 解压
tar -zxvf alist-linux-musl-arm64.tar.gz
# 更改权限
chmod +x alist
# 启动测试,第一次启动会打印出 admin 的初始密码。端口默认 5244，尝试登录
./alist server
# 设置开机启动，粘贴一下内容
vi /etc/init.d/alist

#!/bin/sh /etc/rc.common
START=99
start() {
cd /root/alist #alist的路径
./alist server #启动alist命令
}
stop() {
killall alist
}
# 赋予权限并开启脚本
chmod +x /etc/init.d/alist

/etc/init.d/alist enable



```
