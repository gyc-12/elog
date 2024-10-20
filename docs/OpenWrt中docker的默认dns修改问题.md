---
title: OpenWrt中docker的默认dns修改问题
urlname: xoe4063ux4tpunze
date: 2024-10-14T19:07:05.000Z
updated: '2024-10-20 17:21:55'
author: gaoyanchen
description: '---title: OpenWrt中docker的默认dns修改问题date: 2024-10-14 19:07:05tags: "笔记"thumbnail: "https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/acom...'
tags: 笔记
thumbnail: 'https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/acom_social_icon_azure'
---
<font style="color:rgb(38, 38, 38);"></font>

`<font style="color:rgb(38, 38, 38);">/etc/init.d/dockerd</font>`<font style="color:rgb(38, 38, 38);"> 脚本设置使用 </font>`<font style="color:rgb(38, 38, 38);">/tmp/dockerd/daemon.json</font>`<font style="color:rgb(38, 38, 38);"> 作为 Docker 的配置文件，而脚本启动过程中</font>`<font style="color:rgb(38, 38, 38);">process_config()</font>`<font style="color:rgb(38, 38, 38);"> 函数负责生成 Docker 配置。它从 </font>`<font style="color:rgb(38, 38, 38);">/etc/config/dockerd</font>`<font style="color:rgb(38, 38, 38);"> 读取设置，然后创建 JSON 格式的配置文件。可以使用uci修改也可以直接修改</font>`<font style="color:rgb(38, 38, 38);">/etc/config/dockerd</font>`<font style="color:rgb(38, 38, 38);">文件</font>

<font style="color:rgb(38, 38, 38);">参考：</font>

```shell
config globals 'globals'
    option log_level 'warn'
    option auto_start '1'
    option data_root '/mnt/sda4/docker/'
    option bip '172.31.0.1/24'
    option iptables 'true'
    list registry_mirrors 'https://hub-mirror.c.163.com'
    list dns '114.114.114.114'
```

1. <font style="color:rgb(38, 38, 38);">检查 UCI 配置：</font>

```bash

uci show dockerd
```

2. <font style="color:rgb(38, 38, 38);">如果需要添加或修改设置（例如 DNS），可以使用 UCI 命令：</font>

```bash

uci add_list dockerd.globals.dns='114.114.114.114'
uci commit dockerd
```

3. <font style="color:rgb(38, 38, 38);">重启 Docker 服务以应用新的配置：</font>

```bash

/etc/init.d/dockerd restart
```

4. <font style="color:rgb(38, 38, 38);">检查新生成的配置文件：</font>

```bash

cat /tmp/dockerd/daemon.json
```

5. <font style="color:rgb(38, 38, 38);">验证 Docker 是否使用了新的配置：</font>

```bash
docker info
docker run --rm alpine cat /etc/resolv.conf
```

