---

title: frp安装在阿里云ECS

date: 2024-9-03 19:07:05

tags: "笔记"

thumbnail: "https://picx.zhimg.com/70/v2-11cd1f933bf5711475c7e2d1b714dd2c_1440w.avis?source=172ae18b&biz_tag=Post"

cover:"https://picx.zhimg.com/70/v2-11cd1f933bf5711475c7e2d1b714dd2c_1440w.avis?source=172ae18b&biz_tag=Post"

---



frp安装在阿里云ECS-Alpine

![](https://raw.githubusercontent.com/gyc-12/images/master/8759aeff4c94442da3a74a36a544a13b.png)

要让frps在Alpine系统上在后台运行并开机自动启动,你可以按照以下步骤操作:

1. 创建一个systemd服务文件:

```bash
doas vi /etc/init.d/frps
```

1. 在文件中添加以下内容:

```shell
#!/sbin/openrc-run

name="frps"
description="frp server daemon"
command="/path/of/frps"
command_args="-c /path/of/frps.toml"
pidfile="/run/${RC_SVCNAME}.pid"
command_background="yes"
start_stop_daemon_args="--make-pidfile --user nobody"

depend() {
    need net
    after firewall
}
```

请确保将`/path/to/frps`和`/path/to/frps.toml`替换为实际的路径。

1. 给服务文件添加执行权限:

```bash
doas chmod +x /etc/init.d/frps
```

1. 添加服务到开机启动:

```bash
doas rc-update add frps default
```

1. 启动服务:

```bash
doas rc-service frps start
```

现在,frps将在后台运行,并在系统启动时自动启动。你可以使用以下命令来管理服务:

+ 启动: `doas rc-service frps start`
+ 停止: `doas rc-service frps stop`
+ 重启: `doas rc-service frps restart`
+ 查看状态: `doas rc-service frps status`

这样设置后,frps就会在后台运行,并且在系统重启后自动启动。



## 我的配置文件
```bash
bindPort = 7000

# 配置 frp dashboard
webServer.addr = "0.0.0.0"
webServer.port = 7500
webServer.user = "admin"
webServer.password = "pass"

# 配置 token 认证，frpc 客户端也需指定一样的token
auth.method = "token"
auth.token = "pass"
```

### frpc.toml由openwrt的frp的web应用很容易操作
![](https://raw.githubusercontent.com/gyc-12/images/master/f1e45f0f622c9b7ed400aac19649e64a.png)

## <font style="color:rgb(36, 41, 46);">服务端 - frps</font>
### <font style="color:rgb(36, 41, 46);">1. 下载程序</font>
<font style="color:rgb(36, 41, 46);">首先到 frp 的 releases 页面下载最新版的对应 VPS 的处理器架构的压缩包  
</font>[<font style="color:rgb(36, 41, 46);">https://github.com/fatedier/frp/releases</font>](https://github.com/fatedier/frp/releases)

<font style="color:rgb(36, 41, 46);">如何知道 VPS 的处理器架构？在 VPS 上运行这个命令：</font>

```shell
arch
```

<font style="color:rgb(36, 41, 46);">如果输出</font>`<font style="color:rgb(199, 37, 78);">x86_64</font>`<font style="color:rgb(36, 41, 46);">则需要下载带</font>`<font style="color:rgb(199, 37, 78);">linux_amd64</font>`<font style="color:rgb(36, 41, 46);">的那个压缩包；  
</font><font style="color:rgb(36, 41, 46);">如果输出的是其他的，则在文件列表中找 linux 的对应架构的压缩包</font>

<font style="color:rgb(36, 41, 46);">以</font>`<font style="color:rgb(199, 37, 78);">x86_64</font>`<font style="color:rgb(36, 41, 46);">架构举例（目前大多数都应该是这个架构），本文撰写时 frp 最新版是</font>`<font style="color:rgb(199, 37, 78);">v0.18.0</font>`

```bash

cd /root
# 下载
wget --no-check-certificate https://github.com/fatedier/frp/releases/download/v0.18.0/frp_0.18.0_linux_amd64.tar.gz
# 解压
tar -xzvf frp_0.18.0_linux_amd64.tar.gz
# 文件夹名改成 frp，不然目录太长了不方便
mv frp_0.18.0_linux_amd64 frp
cd frp
# 确保 frps 程序具有可执行权限
chmod +x frps
```

<font style="color:rgb(36, 41, 46);">然后试着运行一下</font>`<font style="color:rgb(199, 37, 78);">frps</font>`<font style="color:rgb(36, 41, 46);">，看看是否能正常运行</font>

```bash

./frps --help
```

<font style="color:rgb(36, 41, 46);">正常情况下会输出一串帮助信息，那么就说明你下载了正确架构的版本</font>

<font style="color:rgb(36, 41, 46);">如果提示</font>`<font style="color:rgb(199, 37, 78);">-bash: ./frps: cannot execute binary file: Exec format error</font>`<font style="color:rgb(36, 41, 46);">就说明你下错版本了</font>

### <font style="color:rgb(36, 41, 46);">2. 配置程序</font>
<font style="color:rgb(36, 41, 46);">参考以下配置说明来书写配置文件</font>`<font style="color:rgb(199, 37, 78);">frps.ini</font>`<font style="color:rgb(36, 41, 46);">，你可以先在电脑上打一份草稿  
</font><font style="color:rgb(36, 41, 46);">此处只解释说明一些必要和常用的配置，如需研究完整配置说明请看目录下的</font>`<font style="color:rgb(199, 37, 78);">frps_full.ini</font>`<font style="color:rgb(36, 41, 46);">，以及参考</font>[<font style="color:rgb(36, 41, 46);">frp中文说明</font>](https://github.com/fatedier/frp/blob/master/README_zh.md)

```bash

# 下面这句开头必须要有，表示配置的开始
[common]

# frp 服务端端口（必须）
bind_port = 7000

# frp 服务端密码（必须）
token = 12345678

# 认证超时时间，由于时间戳会被用于加密认证，防止报文劫持后被他人利用
# 因此服务端与客户端所在机器的时间差不能超过这个时间（秒）
# 默认为900秒，即15分钟，如果设置成0就不会对报文时间戳进行超时验证
authentication_timeout = 900

# 仪表盘端口，只有设置了才能使用仪表盘（即后台）
dashboard_port = 7500

# 仪表盘访问的用户名密码，如果不设置，则默认都是 admin
dashboard_user = admin
dashboard_pwd = admin

# 如果你想要用 frp 穿透访问内网中的网站（例如路由器设置页面）
# 则必须要设置以下两个监听端口，不设置则不会开启这项功能
vhost_http_port = 10080
vhost_https_port = 10443

# 此设置需要配合客户端设置，仅在穿透到内网中的 http 或 https 时有用（可选）
# 假设此项设置为 example.com，客户端配置 http 时将 subdomain 设置为 test，
# 则你将 test.example.com 解析到服务端后，可以使用此域名来访问客户端对应的 http
subdomain_host = example.com
```

<font style="color:rgb(36, 41, 46);">然后把你的准备好的配置文件内容写入</font>`<font style="color:rgb(199, 37, 78);">frps.ini</font>`

```bash

echo "[common]
bind_port = 7000
token = 12345678
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin
vhost_http_port = 10080
vhost_https_port = 10443
subdomain_host = example.com" > frps.ini
```

<font style="color:rgb(36, 41, 46);">试着启动一下</font>`<font style="color:rgb(199, 37, 78);">frps</font>`

```bash

# 使用 -c 参数指定配置文件
./frps -c frps.ini
```

<font style="color:rgb(36, 41, 46);">如果没有出现错误提示就说明配置没有问题，可以正常使用</font>

<font style="color:rgb(36, 41, 46);">接着按下</font>`<font style="color:rgb(199, 37, 78);">Ctrl + C</font>`<font style="color:rgb(36, 41, 46);">终止程序运行</font>

### <font style="color:rgb(36, 41, 46);">3. 使 frps 在后台持续运行</font>
#### <font style="color:rgb(36, 41, 46);">启动</font>
<font style="color:rgb(36, 41, 46);">直接使用前面的命令行来运行是不行的，因为在关掉 ssh 窗口后程序</font>`<font style="color:rgb(199, 37, 78);">frps</font>`<font style="color:rgb(36, 41, 46);">就会停止运行，因此要使用</font>`<font style="color:rgb(199, 37, 78);">nohup [command] &</font>`<font style="color:rgb(36, 41, 46);">这种操作来使其在后台运行</font>

```bash

nohup /root/frp/frps -c /root/frp/frps.ini &
```

<font style="color:rgb(36, 41, 46);">并且程序的所有输出（日志）会被写入</font>`<font style="color:rgb(199, 37, 78);">nohup.out</font>`<font style="color:rgb(36, 41, 46);">文件中，你可以使用</font>`<font style="color:rgb(199, 37, 78);">cat</font>`<font style="color:rgb(36, 41, 46);">命令查看其内容</font>

#### <font style="color:rgb(36, 41, 46);">停止</font>
<font style="color:rgb(36, 41, 46);">想停止的话，结束</font>`<font style="color:rgb(199, 37, 78);">frps</font>`<font style="color:rgb(36, 41, 46);">即可</font>

```bash

pkill frps
```

#### <font style="color:rgb(36, 41, 46);">重启</font>
<font style="color:rgb(36, 41, 46);">那就先停止再启动嘛23333</font>

#### <font style="color:rgb(36, 41, 46);">加入开机自启</font>
<font style="color:rgb(36, 41, 46);">编辑</font>`<font style="color:rgb(199, 37, 78);">/etc/rc.local</font>`<font style="color:rgb(36, 41, 46);">文件，将启动那句命令加到</font>`<font style="color:rgb(199, 37, 78);">exit 0</font>`<font style="color:rgb(36, 41, 46);">语句之前（如果有）</font>

## <font style="color:rgb(36, 41, 46);">客户端 - frpc</font>
<font style="color:rgb(36, 41, 46);">如需研究完整配置说明请看目录下的</font>`<font style="color:rgb(199, 37, 78);">frpc_full.ini</font>`<font style="color:rgb(36, 41, 46);">，以及参考</font>[<font style="color:rgb(36, 41, 46);">frp中文说明</font>](https://github.com/fatedier/frp/blob/master/README_zh.md)

### <font style="color:rgb(36, 41, 46);">书写配置</font>
#### <font style="color:rgb(36, 41, 46);">基本配置（必须）</font>
```bash

# 下面这句开头必须要有，表示配置的开始
[common]
# frp 服务端地址，可以填ip或者域名
server_addr = 0.0.0.0
# frp 服务端端口，即填写服务端配置中的 bind_port
server_port = 7000
# 填写 frp 服务端密码
token = 12345678
```

#### <font style="color:rgb(36, 41, 46);">TCP/UDP</font>
<font style="color:rgb(36, 41, 46);">这里以转发 ssh 为例</font>

```bash

# 自定义一个配置名称，格式为“[名称]”，放在开头
[ssh]
# 连接类型，填 tcp 或 udp
type = tcp

# 本地ip，填你需要转发到的目的ip
# 如果是转发到frp客户端所在本机（比如路由器）则填 127.0.0.1
# 否则填对应机器的内网ip
local_ip = 127.0.0.1
# 需要转发到的端口，比如 ssh 端口是 22
local_port = 22

# 是否加密客户端与服务端之间的通信，默认是 false
use_encryption = false
# 是否压缩客户端与服务端之间的通信，默认是 false
# 压缩可以节省流量，但需要消耗 CPU 资源
# 加密自然也会消耗 CPU 资源，但是不大
use_compression = false

# frp 服务端的远程监听端口，即你访问服务端的 remote_port 就相当于访问
# 客户端的 local_port，如果填0则会随机分配一个端口
remote_port = 6001
```

#### <font style="color:rgb(36, 41, 46);">HTTP(S)</font>
<font style="color:rgb(36, 41, 46);">以转发路由器设置页面为例</font>

```bash

# 自定义一个配置名称，格式为“[名称]”，放在开头
[router-web]
# 连接类型，填 http 或 https
type = http

local_ip = 127.0.0.1
local_port = 80

# http 可以考虑加密和压缩一下
use_encryption = true
use_compression = true

# 自定义访问网站的用户名和密码，如果不定义的话谁都可以访问，会不安全
# 有些路由器如果从内部访问web是不需要用户名密码的，因此需要在这里加一层密码保护
# 如果你发现不加这个密码保护，路由器配置页面原本的用户认证能正常生效的话，可以不加
http_user = admin
http_pwd = admin

# 还记得我们在服务端配置的 subdomain_host = example.com 吗
# 假设这里我们填 web01，那么你将 web01.example.com 解析到服务端ip后
# 你就可以使用 域名:端口 来访问你的 http 了
# 这个域名的作用是用来区分不同的 http，因为你可以配置多个这样的配置
subdomain = web01

# 自定义域名，这个不同于 subdomain，你可以设置与 subdomain_host 无关的其他域名
# subdomain 与 custom_domains 中至少有一个必须要设置
custom_domains = web02.yourdomain.com

# 匹配路径，可以设置多个，用逗号分隔，比如你设置 locations 为以下这个，
# 那么所有 http://xxx/abc 和 http://xxx/def 都会被转发到 http://xxx/
# 如果不需要这个功能可以不写这项，就直接该怎么访问就怎么访问
locations = /abc,/def

# 重写 host header，相当于反向代理中的“发送域名”
# 如果设置了，转发 http 时，请求中的 host 会被替换成这个
# 一般情况下不需要用到这个，可以不写这项
host_header_rewrite = dev.yourdomain.com
```

#### <font style="color:rgb(36, 41, 46);">TCP/UDP 范围转发</font>
```bash

# 自定义一个配置名称，格式为“[range:名称]”，放在开头
[range:multi-port]

type = tcp
local_ip = 127.0.0.1
use_encryption = false
use_compression = false

# 本地端口和远程端口可以指定多个范围，如下格式，且范围之间必须一一对应
local_port = 6010-6020,6022,6024-6028
remote_port = 16010-16020,16022,16024-16028
```

### <font style="color:rgb(36, 41, 46);">合并配置</font>
<font style="color:rgb(36, 41, 46);">将你决定要使用的配置合起来，然后填到路由器的 frp 配置脚本中</font>

<font style="color:rgb(36, 41, 46);">如果你是在 linux 上运行，则写入到</font>`<font style="color:rgb(199, 37, 78);">frpc.ini</font>`<font style="color:rgb(36, 41, 46);">中，然后仿照运行服务端的方式来运行客户端</font>

<font style="color:rgb(36, 41, 46);">比如将以上示例配置合并之后，看起来应该是这个样子</font>

```bash

[common]
server_addr = 0.0.0.0
server_port = 7000
token = 12345678

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
use_encryption = false
use_compression = false
remote_port = 6001

[router-web]
type = http
local_ip = 127.0.0.1
local_port = 80
use_encryption = true
use_compression = true
http_user = admin
http_pwd = admin
subdomain = web01
custom_domains = web02.yourdomain.com
locations = /abc,/def
host_header_rewrite = dev.yourdomain.com

[range:multi-port]
type = tcp
local_ip = 127.0.0.1
use_encryption = false
use_compression = false
local_port = 6010-6020,6022,6024-6028
remote_port = 16010-16020,16022,16024-16028
```

## <font style="color:rgb(36, 41, 46);">事后</font>
<font style="color:rgb(36, 41, 46);">登录服务端的 dashboard，看看是否连接成功，测试各项转发是否可用</font>

<font style="color:rgb(36, 41, 46);">如果出现无法使用的情况，按照基本思路排查问题：</font>

1. <font style="color:rgb(36, 41, 46);">看服务端和客户端日志是否有报错</font>
2. <font style="color:rgb(36, 41, 46);">客户端与服务端连接正常，就检查是否是服务端的防火墙或者安全组是否没开放使用到的端口</font>
3. <font style="color:rgb(36, 41, 46);">如果客户端是在路由器上，想转发至内网设备上的某个端口或服务，请检查目标设备的防火墙是否开放所需端口，且建议为该内网设备分配一个静态ip</font>

