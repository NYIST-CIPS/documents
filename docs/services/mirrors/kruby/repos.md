# Repositories

## 新建仓库

TODO ~~Palve写不动了~~

### 修改 rsync-proxy 与 rsyncd 配置

[rsync-proxy](https://github.com/ustclug/rsync-proxy) 为近年来由 USTCLUG 自行编写的 rsync 反向代理服务。

确认后重载 rsync-proxy:

```console
systemctl reload rsync-proxy
```

Rsyncd 不需要重载：每个有效连接会启动新进程，而新进程会重新读取配置。
