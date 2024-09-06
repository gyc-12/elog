转自[白嫖微软云（一）：申请Azure学生订阅 - 阿雷的小窝 (leiting2020.top)](https://blog.leiting2020.top/index.php/%e6%8a%80%e6%9c%af%e9%9a%8f%e7%ac%94/vps/631/) 。

首先要编译一个freebsd能用的override

## <font style="color:rgb(34, 34, 34);">步骤如下：</font>
<font style="color:rgb(34, 34, 34);">直接贴ChatGPT的回答：</font>

1. **<font style="color:rgb(34, 34, 34);">设置环境变量</font>**<font style="color:rgb(34, 34, 34);">： 在编译之前，需要设置相应的环境变量来指定目标操作系统和架构。假设你的项目的主文件是</font><font style="color:rgb(34, 34, 34);"> </font>`main.go`<font style="color:rgb(34, 34, 34);">，并且你当前在项目的根目录下。</font>

```plain
export GOOS=freebsd
export GOARCH=amd64
```

<font style="color:rgb(34, 34, 34);">如果你的项目使用的是其他架构，可以调整</font><font style="color:rgb(34, 34, 34);"> </font>`GOARCH`<font style="color:rgb(34, 34, 34);"> </font><font style="color:rgb(34, 34, 34);">变量。例如，对于 32 位系统，你可以将</font><font style="color:rgb(34, 34, 34);"> </font>`GOARCH`<font style="color:rgb(34, 34, 34);"> </font><font style="color:rgb(34, 34, 34);">设置为</font><font style="color:rgb(34, 34, 34);"> </font>`386`<font style="color:rgb(34, 34, 34);">。  
</font><font style="color:rgb(34, 34, 34);">2.</font><font style="color:rgb(34, 34, 34);"> </font>**<font style="color:rgb(34, 34, 34);">编译项目</font>**<font style="color:rgb(34, 34, 34);">： 使用</font><font style="color:rgb(34, 34, 34);"> </font>`go build`<font style="color:rgb(34, 34, 34);"> </font><font style="color:rgb(34, 34, 34);">命令编译项目。你可以指定输出文件名，例如</font><font style="color:rgb(34, 34, 34);"> </font>`override`<font style="color:rgb(34, 34, 34);">。</font>

```plain
go build -o override
```

<font style="color:rgb(34, 34, 34);">这会生成一个名为</font><font style="color:rgb(34, 34, 34);"> </font>`override`<font style="color:rgb(34, 34, 34);"> </font><font style="color:rgb(34, 34, 34);">的二进制文件，它可以在 FreeBSD 上运行。</font>

## <font style="color:rgb(34, 34, 34);">运行</font>
<font style="color:rgb(34, 34, 34);">申请开放一个随机端口，然后写到config.json中类似"bind": “0.0.0.0:12345”,  
</font><font style="color:rgb(34, 34, 34);">上传编译好的override和config.json然后pm2 start override ;即可。</font>

<font style="color:rgb(34, 34, 34);"></font>

![](https://raw.githubusercontent.com/gyc-12/images/master/e2836d5f2703d2fcb059a4071c0c4b5e.png)

## <font style="color:rgb(34, 34, 34);">绑定域名</font>
<font style="color:rgb(34, 34, 34);">绑定一个域名poxy。映射到override的端口。这里可以是serv00自带的域名,也可以绑定自己的域名</font>

![](https://raw.githubusercontent.com/gyc-12/images/master/1583d3a66215ecba7f64c3f1ef646286.png)

<font style="color:rgb(34, 34, 34);">仅供参考。</font>

