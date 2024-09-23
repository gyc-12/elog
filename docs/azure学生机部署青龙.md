---
title: azure学生机部署青龙
urlname: owyfttbwv9momh6y
date: 2024-08-29T19:07:05.000Z
updated: '2024-09-23 11:54:39'
author: gaoyanchen
description: '---title: azure学生机部署青龙date: 2024-8-29 19:07:05tags: "笔记"thumbnail: "https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/acom_social_icon_...'
tags: 笔记
thumbnail: 'https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/acom_social_icon_azure'
---
## <font style="color:rgb(33, 53, 71);">安装</font>
<font style="color:rgb(33, 53, 71);">建议使用纯净系统安装，避免系统原有数据丢失，需要自己安装 node/npm/python3/pip3</font>

```bash
# 提权
su
# Debian/Ubuntu
curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install nodejs -y
# Centos
curl --silent --location https://rpm.nodesource.com/setup_20.x | sudo bash
```

```bash
npm install -g node-pre-gyp pnpm
npm install -g @whyour/qinglong

qinglong
# 根据提示增加环境变量 QL_DIR 和 QL_DATA_DIR
export QL_DIR=""
export QL_DATA_DIR=""
# 再次执行
qinglong
```

## 安全组开放5700端口
![](https://raw.githubusercontent.com/gyc-12/images/master/a19846699b120158bcde2f3774213464.png)

## 访问ip:5700
