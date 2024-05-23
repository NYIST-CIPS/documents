---
icon: fontawesome/solid/certificate
---

# 服务总览

!!! warning "注意"

    CIPS 的主页上还有一份[《网络服务列表》 :octicons-link-external-16:](https://cips.nyist.edu.cn/wiki/cips/services/)，如果有服务状态改变，记得同步更新主页上的列表。

## Mirror 镜像站

服务器：

- 主：[kruby](mirrors/kruby/index.md)
    - 主服务器存储部分仓库，对外提供 HTTP(S), git 与 Rsync 服务。
        - HTTP(S) 对应 nginx，Rsync 对应 rsync-proxy，两者均支持反向代理（因此可以将仓库放在别的机器上，同时在 nuclear 向外提供对应的服务）。
        - 接入了包括教育网等在内的多个 ISP。
        - kruby 存储的内容是 nuclear 的子集。其搭载 ~~nvme全闪~~ 储存提供更良好的使用体验。
    - 状态页面通过隧道与 nuclear 暴露 tunasync 公开 API。
- 副：[nuclear](mirrors/nuclear/index.md)
    - nuclear 储存了除 pypi 外的所有仓库。
- 副：[mirror3](mirrors/3/index.md)
    - mirror3 存储的内容是 pypi 。


## [GitLab](gitlab.md) {#gitlab}

主服务器：`git.nyist.edu.cn`，校内 Only 

### 主页 {#homepage}

pull 的 github pages


### [Grafana](../infrastructure/monitor.md)

监控站点

## [PXE](pxe/index.md)

网络启动服务，负责为全校机器提供接入网络即可安装系统的功能。

## 其他 {#others}

此处所列出的“服务”均托管在我们自己的服务器资源，包括 DNS 。
