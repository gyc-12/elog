---
title: cloudflared
date: 2024-8-24 19:07:05
tags: "笔记"
thumbnail: "https://avatars.githubusercontent.com/u/74996459?v=4"
---
openwrt中使用cloudflared，查看已经存在的服务。已经查看日志

## 命令

```shell
ls /etc/cloudflared/

# 手动运行查看日志
/usr/bin/cloudflared --protocol tcp tunnel run --token /usr/bin/cloudflared --protocol tcp tunnel run --token eyJhIjoiZmRjMGU3Yjg2NDFlZDl


# 如果你想完全重新配置，可以先卸载现有服务
cloudflared service uninstall

#然后重新安装服务，使用token
cloudflared service install eyJhIjoiZmRjMGU3Yjg2NDF

```

