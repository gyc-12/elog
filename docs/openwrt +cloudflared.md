```java
ls /etc/cloudflared/

# 手动运行查看日志
/usr/bin/cloudflared --protocol tcp tunnel run --token /usr/bin/cloudflared --protocol tcp tunnel run --token eyJhIjoiZmRjMGU3Yjg2NDFlZDl


# 如果你想完全重新配置，可以先卸载现有服务
cloudflared service uninstall

#然后重新安装服务，使用token
cloudflared service install eyJhIjoiZmRjMGU3Yjg2NDF

```

