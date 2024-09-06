

# 教程
<font style="color:rgb(17, 17, 17);">V</font><font style="color:rgb(17, 17, 17);">nStat 是一款适用于服务器和路由器的免费开源应用程序。它是一个基于控制台的网络流量监控器。它保留所选接口的 5 分钟间隔、每小时、每天、每月和每年网络流量的日志。让我们看看如何在 Alpine Linux 服务器上安装 vnStat 以密切关注带宽使用情况。  
  
</font><font style="color:rgb(17, 17, 17);">打开终端应用程序，然后使用 ssh 命令登录，在 Alpine Linux 服务器上安装 vnStat：</font>  
`<font style="color:rgb(153, 153, 153);">$ </font><font style="color:rgb(17, 17, 17);">ssh ec2-user@alpine-ec2-server  
</font><font style="color:rgb(153, 153, 153);"># My Linode server</font><font style="color:rgb(17, 17, 17);">  
</font><font style="color:rgb(153, 153, 153);">$ </font><font style="color:rgb(17, 17, 17);">ssh root@linode-server-vm1</font>`  


<font style="color:rgb(17, 17, 17);">我们这里有两个部分：</font>

<font style="color:rgb(204, 204, 204);background-color:rgb(247, 247, 247);"></font>

1. **<font style="color:rgb(17, 17, 17);">vnstatd</font>**<font style="color:rgb(17, 17, 17);"> </font><font style="color:rgb(17, 17, 17);">– vnStat 的基于守护进程的数据库更新</font>
2. **<font style="color:rgb(17, 17, 17);">vnstat</font>**<font style="color:rgb(17, 17, 17);"> </font><font style="color:rgb(17, 17, 17);">– 基于控制台的网络流量监控器，用于查询由 vnstatd 创建的数据库</font>

## <font style="color:rgb(17, 17, 17);">在 Alpine Linux 上安装 vnstat</font>
<font style="color:rgb(17, 17, 17);">确保已启用社区存储库。编辑</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">/etc/apk/repositories</font>](https://www.cyberciti.biz/faq/alpine-linux-etc-apk-repositories-repositories-file/)<font style="color:rgb(17, 17, 17);">：  
  
</font><font style="color:rgb(17, 17, 17);">以下是 Alpine Linux 版本 3.17 的外观：</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">vim /etc/apk/repositories</font>`

```plain
http://dl-cdn.alpinelinux.org/alpine/v3.17/main
http://dl-cdn.alpinelinux.org/alpine/v3.17/community
```

### <font style="color:rgb(17, 17, 17);">更新 repo 数据库</font>
<font style="color:rgb(17, 17, 17);">以下</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">apk 命令</font>](https://www.cyberciti.biz/faq/10-alpine-linux-apk-command-examples/)<font style="color:rgb(17, 17, 17);">强制更新所有已配置的软件包存储库中的索引，运行：  
  
</font><font style="color:rgb(17, 17, 17);">将任何待处理的升级应用于已安装的软件包，以从已配置的软件包存储库中获得的最新版本，输入：</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">apk update</font>`<font style="color:rgb(17, 17, 17);">  
</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">apk upgrade</font>`

### <font style="color:rgb(17, 17, 17);">搜索 vnstat 软件包</font>
<font style="color:rgb(17, 17, 17);">让我们看看您的 Alpine Linux 盒子上可用的 vnstat 软件包：  
  
</font><font style="color:rgb(153, 153, 153);">列表：</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">apk search vnstat</font>`

```plain
vnstat-openrc-2.10-r0
vnstat-2.10-r0
vnstat-doc-2.10-r0
```

### <font style="color:rgb(17, 17, 17);">安装</font>
<font style="color:rgb(17, 17, 17);">运行以下</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">apk 命令</font>](https://www.cyberciti.biz/faq/10-alpine-linux-apk-command-examples/)<font style="color:rgb(17, 17, 17);">：  
  
</font><font style="color:rgb(17, 17, 17);">我们还可以在</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">Alpine Linux 中为 vnstat 添加/安装手册页</font>](https://www.cyberciti.biz/faq/howto-adding-installing-man-pages-in-alpine-linux/)<font style="color:rgb(17, 17, 17);">：</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">apk add vnstat</font>`<font style="color:rgb(17, 17, 17);">  
</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">apk add vnstat-doc</font>`<font style="color:rgb(17, 17, 17);">  
</font>![](https://raw.githubusercontent.com/gyc-12/images/master/efd3c9061e14e4f96ecd59041fa22e49.png)

### <font style="color:rgb(17, 17, 17);">配置</font>
<font style="color:rgb(17, 17, 17);">使用 vim 命令或您选择的任何其他编辑器进行编辑：  
  
</font>[<font style="color:rgb(17, 17, 17);">想找出 Linux 上的接口名称</font>](https://www.cyberciti.biz/faq/linux-list-network-interfaces-names-command/)<font style="color:rgb(17, 17, 17);">吗？使用</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">ip 命令</font>](https://www.cyberciti.biz/faq/linux-ip-command-examples-usage-syntax/)<font style="color:rgb(17, 17, 17);">，如下所示：  
  
</font><font style="color:rgb(17, 17, 17);">以下输出表明我有环回 （lo0） 和以太网 （eth0） NIC：</font>**<font style="color:rgb(78, 78, 78);background-color:rgb(247, 247, 247);">/etc/vnstat.conf</font>**`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">vi /etc/vnstat.conf</font>``<font style="color:rgb(17, 17, 17);">ip link show</font>`

```plain
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether c4:54:44:1f:f7:35 brd ff:ff:ff:ff:ff:ff
```

<font style="color:rgb(17, 17, 17);">First we need to set default network interface name. You can leave empty for automatic selection but I recommend that you set up your main Eth name here:  
  
</font><font style="color:rgb(17, 17, 17);">Set date output formats:</font>`<font style="color:rgb(17, 17, 17);">Interface "eth0"</font>`

```plain
DayFormat    "%Y-%m-%d"
MonthFormat  "%Y-%m"
TopFormat    "%Y-%m-%d"
```

<font style="color:rgb(17, 17, 17);">Set IEC standard prefixes when traffic is shown. In other words, display traffic in KiB/MiB/GiB format:  
  
</font>[<font style="color:rgb(17, 17, 17);">Save and close the file</font>](https://www.cyberciti.biz/faq/linux-unix-vim-save-and-quit-command/)<font style="color:rgb(17, 17, 17);">.</font>`<font style="color:rgb(17, 17, 17);">UnitMode 0</font>`

### <font style="color:rgb(17, 17, 17);">Enable vnstatd service on Alpine Linux</font>
<font style="color:rgb(17, 17, 17);">Use the rc-update command as follows to enable vnstatd service at boot time:</font><font style="color:rgb(17, 17, 17);">  
</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">rc-update add vnstatd</font>`

### <font style="color:rgb(17, 17, 17);">如何启动/停止/重新启动 vnstatd 服务</font>
<font style="color:rgb(17, 17, 17);">用于控制 vnStat 的</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">service 命令</font>](https://bash.cyberciti.biz/guide/Service_command)<font style="color:rgb(17, 17, 17);">的语法如下：</font><font style="color:rgb(17, 17, 17);">  
</font>`<font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">service vnstatd start  
</font><font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">service vnstatd stop  
</font><font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">service vnstatd restart  
</font><font style="color:red;">#</font><font style="color:red;"> </font><font style="color:rgb(17, 17, 17);">service vnstatd status</font>`<font style="color:rgb(17, 17, 17);">  
</font>![](https://raw.githubusercontent.com/gyc-12/images/master/1abd9c85ae04d20c3473cf8824315dc3.png)

## <font style="color:rgb(17, 17, 17);">查看统计信息</font>
<font style="color:rgb(17, 17, 17);background-color:rgb(204, 241, 255);">查看带宽统计信息不需要 root 访问权限。</font>

<font style="color:rgb(17, 17, 17);">打开终端，然后运行：  
  
</font><font style="color:rgb(17, 17, 17);">以下是</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">Alpine Linux 上 eth0 和 Wireguard wg0 统计数据的</font>](https://www.cyberciti.biz/faq/how-to-set-up-wireguard-vpn-server-on-alpine-linux/)<font style="color:rgb(17, 17, 17);">统计数据：</font>`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat</font>`

```plain
rx      /      tx      /     total    /   estimated
 eth0:
       2021-02    120.62 GiB  /  122.09 GiB  /  242.71 GiB
       2021-03      5.45 GiB  /    5.48 GiB  /   10.93 GiB  /   68.22 GiB
     2021-03-04   917.71 MiB  /  920.74 MiB  /    1.80 GiB
     2021-03-05     1.08 GiB  /    1.09 GiB  /    2.17 GiB  /    2.24 GiB
 
 wg0:
       2021-02      5.32 GiB  /  114.57 GiB  /  119.88 GiB
       2021-03    308.75 MiB  /    5.09 GiB  /    5.39 GiB  /   33.62 GiB
     2021-03-04    64.83 MiB  /  840.80 MiB  /  905.63 MiB
     2021-03-05    64.92 MiB  /    1.00 GiB  /    1.07 GiB  /    1.10 GiB
```

<font style="color:rgb(17, 17, 17);">我们可以将统计信息限制为 wg0 接口，如下所示：  
  
</font><font style="color:rgb(17, 17, 17);">我们还可以显示接口 eth0、eth1 和 eth2 合并的流量摘要。  
  
</font><font style="color:rgb(17, 17, 17);">请注意，跳过后，vnstat 将使用 /etc/vnstat.conf 文件中设置的默认接口。让我们找出每月的带宽使用情况，运行：</font>`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i {name}  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i wg0  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i tun0  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i wan1  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i lan</font>``<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i eth0+eth1+eth3</font>`<font style="color:rgb(78, 78, 78);background-color:rgb(247, 247, 247);">-i</font><font style="color:rgb(17, 17, 17);">  
</font>`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -h  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i wg0 -h</font>`

```plain
eth0  /  hourly
 
         hour        rx      |     tx      |    total    |   avg. rate
     ------------------------+-------------+-------------+---------------
     2021-03-05
         00:00    215.45 KiB |   86.55 KiB |  301.99 KiB |      687 bit/s
         01:00    193.94 KiB |   32.62 KiB |  226.56 KiB |      515 bit/s
         02:00    118.09 MiB |  119.53 MiB |  237.62 MiB |  553.70 kbit/s
         03:00    195.14 KiB |   32.43 KiB |  227.57 KiB |      517 bit/s
         04:00    218.15 KiB |   90.95 KiB |  309.10 KiB |      703 bit/s
         05:00    198.29 KiB |   32.77 KiB |  231.07 KiB |      525 bit/s
         06:00    116.24 MiB |  117.24 MiB |  233.48 MiB |  544.05 kbit/s
         07:00    199.29 KiB |   34.15 KiB |  233.44 KiB |      531 bit/s
         08:00    220.96 KiB |   88.86 KiB |  309.83 KiB |      705 bit/s
         09:00    199.49 KiB |   31.86 KiB |  231.34 KiB |      526 bit/s
         10:00    296.73 MiB |  298.69 MiB |  595.42 MiB |    1.39 Mbit/s
         11:00    275.92 KiB |   33.13 KiB |  309.04 KiB |      703 bit/s
         12:00    242.26 KiB |  108.64 KiB |  350.90 KiB |      798 bit/s
         13:00    198.14 KiB |   31.68 KiB |  229.82 KiB |      522 bit/s
         14:00    114.90 MiB |  116.25 MiB |  231.15 MiB |  538.61 kbit/s
         15:00    193.16 KiB |   28.96 KiB |  222.13 KiB |      505 bit/s
         16:00     74.23 MiB |   75.48 MiB |  149.71 MiB |  348.84 kbit/s
         17:00    146.12 MiB |  145.49 MiB |  291.62 MiB |  679.51 kbit/s
         18:00    116.93 MiB |  118.25 MiB |  235.18 MiB |  548.02 kbit/s
         19:00    192.78 KiB |   28.61 KiB |  221.39 KiB |      503 bit/s
         20:00    226.13 KiB |   85.76 KiB |  311.89 KiB |      709 bit/s
         21:00    202.47 KiB |   32.92 KiB |  235.38 KiB |      535 bit/s
         22:00    116.09 MiB |  117.42 MiB |  233.51 MiB |  544.11 kbit/s
         23:00      2.89 MiB |    2.91 MiB |    5.80 MiB |   65.73 kbit/s
     ------------------------+-------------+-------------+---------------
```

### <font style="color:rgb(17, 17, 17);">显示 5 分钟带宽统计信息</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -5  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -5 -i wg0  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -5 [limit] -i wg0  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -5 10 -i wg0</font>`

### <font style="color:rgb(17, 17, 17);">查看每小时统计图</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -hg</font>`

```plain
eth0                                                                     23:12 
  ^                                 t                                           
  |                                rt                                           
  |                                rt                                           
  |                                rt                                           
  |                                rt                                           
  |                                rt                                           
  |         t                      rt                   rt                      
  |        rt          rt          rt          rt       rt rt          rt       
  |        rt          rt          rt          rt    rt rt rt          rt       
  |        rt          rt          rt          rt    rt rt rt          rt       
 -+---------------------------------------------------------------------------> 
  |  00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 21 22 23    
 
 h  rx (MiB)   tx (MiB)  ][  h  rx (MiB)   tx (MiB)  ][  h  rx (MiB)   tx (MiB) 
00        0.2        0.1 ][ 08        0.2        0.1 ][ 16       74.2       75.5
01        0.2        0.0 ][ 09        0.2        0.0 ][ 17      146.1      145.5
02      118.1      119.5 ][ 10      296.7      298.7 ][ 18      116.9      118.3
03        0.2        0.0 ][ 11        0.3        0.0 ][ 19        0.2        0.0
04        0.2        0.1 ][ 12        0.2        0.1 ][ 20        0.2        0.1
05        0.2        0.0 ][ 13        0.2        0.0 ][ 21        0.2        0.0
06      116.2      117.2 ][ 14      114.9      116.2 ][ 22      116.1      117.4
07        0.2        0.0 ][ 15        0.2        0.0 ][ 23        2.9        2.9
```

### <font style="color:rgb(17, 17, 17);">想要查看每日统计数据？</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -d</font>`

```plain
eth0  /  daily
 
          day        rx      |     tx      |    total    |   avg. rate
     ------------------------+-------------+-------------+---------------
     2021-02-04     2.09 GiB |    2.09 GiB |    4.18 GiB |  415.43 kbit/s
     2021-02-05     1.59 GiB |    1.60 GiB |    3.19 GiB |  317.47 kbit/s
     2021-02-06     1.18 GiB |    1.19 GiB |    2.38 GiB |  236.16 kbit/s
     2021-02-07    21.58 GiB |   22.02 GiB |   43.60 GiB |    4.33 Mbit/s
     2021-02-08     6.15 GiB |    6.35 GiB |   12.50 GiB |    1.24 Mbit/s
     2021-02-09     1.30 GiB |    1.31 GiB |    2.61 GiB |  259.23 kbit/s
     2021-02-10     6.21 GiB |    6.31 GiB |   12.52 GiB |    1.24 Mbit/s
     2021-02-11     1.14 GiB |    1.15 GiB |    2.30 GiB |  228.22 kbit/s
     2021-02-12     1.29 GiB |    1.30 GiB |    2.59 GiB |  257.14 kbit/s
     2021-02-13     1.35 GiB |    1.36 GiB |    2.71 GiB |  269.69 kbit/s
     2021-02-14    18.77 GiB |   18.96 GiB |   37.73 GiB |    3.75 Mbit/s
     2021-02-15     2.00 GiB |    2.02 GiB |    4.01 GiB |  398.97 kbit/s
     2021-02-16     1.08 GiB |    1.09 GiB |    2.17 GiB |  215.83 kbit/s
     2021-02-17   868.25 MiB |  871.92 MiB |    1.70 GiB |  168.95 kbit/s
     2021-02-18     1.86 GiB |    1.87 GiB |    3.73 GiB |  370.69 kbit/s
     2021-02-19     1.14 GiB |    1.14 GiB |    2.28 GiB |  226.84 kbit/s
     2021-02-20     1.15 GiB |    1.14 GiB |    2.29 GiB |  227.70 kbit/s
     2021-02-21    18.12 GiB |   18.22 GiB |   36.34 GiB |    3.61 Mbit/s
     2021-02-22     1.03 GiB |    1.04 GiB |    2.07 GiB |  205.38 kbit/s
     2021-02-23     1.79 GiB |    1.79 GiB |    3.58 GiB |  355.92 kbit/s
     2021-02-24     2.09 GiB |    2.09 GiB |    4.18 GiB |  415.16 kbit/s
     2021-02-25     1.22 GiB |    1.24 GiB |    2.46 GiB |  244.27 kbit/s
     2021-02-26   971.47 MiB |  977.38 MiB |    1.90 GiB |  189.22 kbit/s
     2021-02-27   829.45 MiB |  833.88 MiB |    1.62 GiB |  161.49 kbit/s
     2021-02-28    17.20 GiB |   17.45 GiB |   34.66 GiB |    3.45 Mbit/s
     2021-03-01     1.66 GiB |    1.69 GiB |    3.35 GiB |  333.08 kbit/s
     2021-03-02   908.53 MiB |  910.37 MiB |    1.78 GiB |  176.60 kbit/s
     2021-03-03   941.24 MiB |  946.52 MiB |    1.84 GiB |  183.28 kbit/s
     2021-03-04   917.71 MiB |  920.74 MiB |    1.80 GiB |  178.50 kbit/s
     2021-03-05     1.08 GiB |    1.09 GiB |    2.17 GiB |  222.65 kbit/s
     ------------------------+-------------+-------------+---------------
     estimated      1.12 GiB |    1.12 GiB |    2.24 GiB |
```

### <font style="color:rgb(17, 17, 17);">查看每月统计数据</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -m</font>`

```plain
eth0  /  monthly
 
        month        rx      |     tx      |    total    |   avg. rate
     ------------------------+-------------+-------------+---------------
       2020-09     41.57 MiB |    4.64 MiB |   46.21 MiB |      149 bit/s
       2020-10     99.96 GiB |  100.55 GiB |  200.51 GiB |  643.05 kbit/s
       2020-11    114.78 GiB |  116.36 GiB |  231.14 GiB |  766.00 kbit/s
       2020-12    102.51 GiB |  103.67 GiB |  206.18 GiB |  661.25 kbit/s
       2021-01    272.13 GiB |  275.48 GiB |  547.61 GiB |    1.76 Mbit/s
       2021-02    120.62 GiB |  122.09 GiB |  242.71 GiB |  861.79 kbit/s
       2021-03      5.45 GiB |    5.48 GiB |   10.93 GiB |  218.80 kbit/s
     ------------------------+-------------+-------------+---------------
     estimated     33.99 GiB |   34.23 GiB |   68.22 GiB |
```

### <font style="color:rgb(17, 17, 17);">想要查看年度数据</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -y</font>`

```plain
eth0  /  yearly
 
         year        rx      |     tx      |    total    |   avg. rate
     ------------------------+-------------+-------------+---------------
          2020    317.29 GiB |  320.58 GiB |  637.87 GiB |  173.27 kbit/s
          2021    398.19 GiB |  403.06 GiB |  801.25 GiB |    1.25 Mbit/s
     ------------------------+-------------+-------------+---------------
     estimated      2.22 TiB |    2.25 TiB |    4.46 TiB |
```

### <font style="color:rgb(17, 17, 17);">获取热门日期列表</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -t</font>`

```plain
eth0  /  top 10
 
    #      day          rx      |     tx      |    total    |   avg. rate
   -----------------------------+-------------+-------------+---------------
    1   2021-01-08    77.41 GiB |   78.92 GiB |  156.33 GiB |   15.54 Mbit/s
    2   2021-01-27    52.90 GiB |   53.38 GiB |  106.28 GiB |   10.57 Mbit/s
    3   2021-01-03    49.36 GiB |   50.03 GiB |   99.39 GiB |    9.88 Mbit/s
    4   2021-01-05    30.69 GiB |   31.18 GiB |   61.87 GiB |    6.15 Mbit/s
    5   2020-10-11    23.98 GiB |   24.31 GiB |   48.29 GiB |    4.80 Mbit/s
    6   2020-10-25    23.22 GiB |   23.60 GiB |   46.82 GiB |    4.65 Mbit/s
    7   2021-02-07    21.58 GiB |   22.02 GiB |   43.60 GiB |    4.33 Mbit/s
    8   2021-02-14    18.77 GiB |   18.96 GiB |   37.73 GiB |    3.75 Mbit/s
    9   2020-11-15    18.43 GiB |   18.73 GiB |   37.16 GiB |    3.69 Mbit/s
   10   2020-12-13    18.15 GiB |   18.26 GiB |   36.41 GiB |    3.62 Mbit/s
   -----------------------------+-------------+-------------+---------------
```

### <font style="color:rgb(17, 17, 17);">如何以 json 和 xml 格式显示数据库</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat --json  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat --xml</font>`

### <font style="color:rgb(17, 17, 17);">想要实时查看传输速率？</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -l</font>`

### <font style="color:rgb(17, 17, 17);">如何向数据库添加新接口</font>
<font style="color:rgb(17, 17, 17);">我们可以为指定的接口创建数据库条目，如下所示。例如，添加一个名为 tun0 的新接口：</font><font style="color:rgb(78, 78, 78);background-color:rgb(247, 247, 247);">-i</font><font style="color:rgb(17, 17, 17);">  
</font>`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i tun0 --add</font>`

### <font style="color:rgb(17, 17, 17);">如何删除接口 eth1 的数据库条目并停止监控</font>
`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">vnstat -i eth1 --remove</font>`<font style="color:rgb(17, 17, 17);">  
</font><font style="color:rgb(17, 17, 17);">获得帮助也很容易。尝试</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">man 命令</font>](https://bash.cyberciti.biz/guide/Man_command)<font style="color:rgb(17, 17, 17);">或</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">help 命令</font>](https://bash.cyberciti.biz/guide/Help_command)<font style="color:rgb(17, 17, 17);">，如下所示。例如：</font><font style="color:rgb(17, 17, 17);">  
</font>`<font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">man vnstat  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">man vnstat.conf  
</font><font style="color:rgb(153, 153, 153);">$</font><font style="color:rgb(153, 153, 153);"> </font><font style="color:rgb(17, 17, 17);">man vnstatd</font>`

## <font style="color:rgb(17, 17, 17);">总结</font>
<font style="color:rgb(17, 17, 17);">您学习了如何在 Alpine Linux 上安装 vnstat。vnstat 命令的目的是提供一个接口，用于查询存储在数据库中的流量信息。而守护程序 vnstatd 负责数据检索、缓存和存储。尽管守护进程始终作为服务运行，但它大部分时间都在数据更新之间休眠。请参阅 vnstat</font><font style="color:rgb(17, 17, 17);"> </font>[<font style="color:rgb(17, 17, 17);">项目主页</font>](https://humdi.net/vnstat/)<font style="color:rgb(17, 17, 17);">。</font><font style="color:rgb(17, 17, 17);">  
</font>

  




