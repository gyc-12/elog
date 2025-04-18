---
title: Alpine 系统部署 Sub-Store和 alist 教程
urlname: izaugv6r676hs04o
date: 2025-04-11T19:07:05.000Z
updated: '2025-04-18 15:26:21'
author: gaoyanchen
description: '---title: Alpine 系统部署 Sub-Store和 alist 教程date: 2025-4-11 19:07:05tags: "笔记"thumbnail: "https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcor...'
tags: 笔记
thumbnail: 'https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/acom_social_icon_azure'
---
# Alpine 系统部署 Sub-Store和 alist 教程
## 准备工作
### 安装必要软件包
```bash
sudo apk add node
```

### 系统基础配置
1. 创建 sudo 命令软连接

```bash
doas ln -s $(which doas) /usr/local/bin/sudo
```

2. 修改 root 密码

```bash
sudo passwd root
```

3. 配置 SSH 允许 root 登录  
在 `/etc/ssh/sshd_config.d/50-cloud-init.conf` 中添加：

```plain
PasswordAuthentication yes
PermitRootLogin yes
```

4. 调整时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

> 更多 Alpine 系统配置可参考：[https://linux.do/t/topic/197976](https://linux.do/t/topic/197976)
>

## 下载安装文件
1. 前端文件：从 [Sub-Store-Front-End](https://github.com/sub-store-org/Sub-Store-Front-End/releases) 下载 `dist.zip`
2. 后端文件：从 [Sub-Store](https://github.com/sub-store-org/Sub-Store/releases) 下载 `sub-store.min.js`

## 部署步骤
### 目录结构
```plain
/root/sub-store/
├── frontend/dist/  # 前端文件解压到此
└── sub-store.bundle.js  # 后端文件
```

### 创建服务启动脚本
在 `/etc/init.d/` 目录下创建名为 `substore` 的文件：

```bash
touch /etc/init.d/substore
chmod +x /etc/init.d/substore
```

编辑内容如下：

```bash
#!/sbin/openrc-run

name="Sub-Store"
description="Sub-Store Backend Service"
command="/usr/bin/node"
command_args="/root/sub-store/sub-store.bundle.js"
command_user="root"
pidfile="/var/run/sub-store.pid"

# 环境变量配置
export SUB_STORE_FRONTEND_BACKEND_PATH="/2cXaAxRGfddmGz2yx1wA"
export SUB_STORE_BACKEND_CRON="0 0 * * *"
# 你自己的目录
export SUB_STORE_FRONTEND_PATH="/root/sub-store/frontend/dist"
export SUB_STORE_FRONTEND_HOST="0.0.0.0"
export SUB_STORE_FRONTEND_PORT="3001"
# 你自己的目录
export SUB_STORE_DATA_BASE_PATH="/root/sub-store"
export SUB_STORE_BACKEND_API_HOST="127.0.0.1"
export SUB_STORE_BACKEND_API_PORT="3000"

depend() {
    need net
    after firewall
}

start_pre() {
    # 资源限制配置
    ulimit -n 32767
    echo "[$(date)] 启动检测：Node版本 $(node -v)" > /var/log/sub-store.log
}

start() {
    ebegin "Starting Sub-Store"
    start-stop-daemon --start \
        --user "$command_user" \
        --exec "$command" \
        --pidfile "$pidfile" \
        --make-pidfile \
        --background \
        -- \
        $command_args >> /var/log/sub-store.log 2>&1
    eend $?
}

stop() {
    ebegin "Stopping Sub-Store"
    start-stop-daemon --stop \
        --pidfile "$pidfile" \
        --exec "$command"
    eend $?
    rm -f "$pidfile"
}
```

> **安全提示**：请将 `SUB_STORE_FRONTEND_BACKEND_PATH` 的值 `/2cXaAxRGfddmGz2yx1wA` 修改为其他复杂字符串，这是 API 验证所必需的。不要更改后端 API 监听地址，保持 `SUB_STORE_BACKEND_API_HOST=127.0.0.1` ，尤其是监听的3000端口不要开公网，可避免 API 暴露在公网。
>

### 启动服务
```bash
chmod +x /etc/init.d/substore
rc-update add substore default  # 加入开机启动
rc-service substore start       # 立即启动服务
```

## 访问方式
### 直接 IP 访问
```plain
http://服务器IP:3001?api=http://服务器IP:3001/路径标识符
```

> **说明**：将"服务器IP"替换为您的实际IP地址，"路径标识符"替换为您在配置文件中设置的 `SUB_STORE_FRONTEND_BACKEND_PATH` 值（默认为 `2cXaAxRGfddmGz2yx1wA`）。
>

### 配置域名和 SSL
如需使用域名并配置 SSL，可参考：[https://surge.tel/08/2930/](https://surge.tel/08/2930/)

---

## AList 部署教程
> 主要用AList 提供的WebDAV 服务，可用于备份软件配置，特别适合流量有限的服务器。
>

## 下载安装
1. 从 [AList Releases](https://github.com/AlistGo/alist/releases) 下载 `alist-linux-musl-amd64.tar.gz`
2. 解压并配置

```bash
# 解压下载的文件
tar -zxvf alist-linux-musl-amd64.tar.gz

# 授予程序执行权限
chmod +x alist

# 测试运行程序
./alist server
```

## 管理员配置
### 修改管理员密码
```bash
# v3.25.0 以下版本
./alist admin

# v3.25.0 及以上版本
# 随机生成密码
./alist admin random
# 或设置自定义密码
./alist admin set 您的新密码
```

## 创建服务启动脚本
在 `/etc/init.d/` 目录下创建名为 `alist` 的文件：

```bash
touch /etc/init.d/alist
chmod +x /etc/init.d/alist
```

编辑内容如下：

```bash
#!/sbin/openrc-run

name="alist"
description="AList WebDAV Service"
# 替换为您的 AList 安装目录
command="/home/alpine/alist/alist"
command_args="server"
pidfile="/run/${name}.pid"

depend() {
    need net
}

start_pre() {
    # 替换为您的 AList 安装目录
    if [ ! -d "/home/alpine/alist" ]; then
        eerror "AList 目录不存在!"
        return 1
    fi
}

start() {
    ebegin "Starting AList"
    # 替换为您的 AList 安装目录
    cd "/home/alpine/alist" && nohup ./alist server > /var/log/alist.log 2>&1 &
    echo $! > ${pidfile}
    eend $?
}

stop() {
    ebegin "Stopping AList"
    start-stop-daemon --stop --pidfile "$pidfile"
    eend $?
    rm -f "$pidfile"
}
```

> **注意**：请将脚本中的 `/home/alpine/alist` 路径替换为您实际的 AList 安装目录
>

### 启动服务
```bash
chmod +x /etc/init.d/alist
rc-update add alist default  # 加入开机启动
rc-service alist start       # 立即启动服务
```

## 访问方式
AList 默认监听 5244 端口，可通过以下地址访问：

```plain
http://服务器IP:5244
```

登录后可在"管理" 中配置 WebDAV 功能。官方文档见

```plain
https://alist.nn.ci/zh/guide/webdav.html
```

