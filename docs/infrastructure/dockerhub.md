# Docker Hub

## 地址

<https://docker.mirror.nyist.edu.cn/>

## 说明

Docker Hub 镜像缓存

## 使用说明

!!! warning

    由于访问原始站点的网络带宽等条件的限制，导致 Docker Hub
    的镜像缓存处于基本不可用的状态，因此镜像站的容器镜像服务仅限校内使用。

    对于从校外的访问：

    -   Docker Hub 会返回 403；

    本文档结尾提供了搭建本地镜像缓存的方式，以供参考。

!!! warning

    2020 年 11 月后，Docker Hub 新增了 [访问速率限制](https://docs.docker.com/docker-hub/download-rate-limit/)，这可能导致在校内使用 Docker Hub 镜像缓存时出现间歇性的问题。

### Linux

对于使用 systemd 的系统（Ubuntu 16.04+、Debian 8+、CentOS 7），
在配置文件 `/etc/docker/daemon.json` 中加入：

    {
      "registry-mirrors": ["https://docker.mirror.nyist.edu.cn/"]
    }

重新启动 dockerd：

    sudo systemctl restart docker

### macOS

旧版本：

1. 打开 "Docker.app"
2. 进入偏好设置页面（快捷键 `⌘,`）
3. 打开 "Daemon" 选项卡
4. 在 "Registry mirrors" 中添加 `https://docker.mirror.nyist.edu.cn/`
5. 点击下方的 "Apply & Restart" 按钮

新版本：

1. 打开 "Docker.app"
2. 进入偏好设置页面（快捷键 `⌘,`）
3. 打开 "Docker Engine" 选项卡
4. 参考 Linux 中 "使用 systemd 系统" 的配置，在 JSON 配置中添加
    `"registry-mirrors"` 一项。

### Windows

旧版本：

在系统右下角托盘 Docker 图标内右键菜单选择 `Settings`
，打开配置窗口后左侧导航菜单选择 `Daemon`。在 `Registry mirrors`
一栏中填写地址 `https://docker.mirror.nyist.edu.cn/`，之后点击 Apply
保存后 Docker 就会重启并应用配置的镜像地址了。

新版本：

在系统右下角托盘 Docker 图标内右键菜单选择 `Settings`，
打开配置窗口后左侧导航菜单选择 `Docker Engine`。参考 Linux 中 "使用 systemd 系统" 的配置，
在 JSON 配置中添加 `"registry-mirrors"` 一项，之后点击 "Apply & Restart" 保存并重启 Docker 即可。

### 检查 Docker Hub 是否生效

在命令行执行 `docker info`，如果从结果中看到了如下内容，说明配置成功。

    Registry Mirrors:
        https://docker.mirror.nyist.edu.cn/

### 如何搭建本地镜像缓存？

由于镜像站目前暂不为校外提供容器镜像缓存服务，如果需要自行搭建本地镜像缓存，可以参考以下的方式：

Redis 容器：

    docker rm -f redis
    docker run \
        --name=redis \
        -itd \
        --net=docker-registry \
        --restart=always \
        redis \
        redis-server --maxmemory 512m

镜像缓存容器：

    docker rm -f dockerhub-mirror
    docker run -itd \
        --name dockerhub-mirror \
        --restart=always \
        --net=docker-registry \
        -v /srv/docker/dockerhub/data:/var/lib/registry \
        -v /srv/docker/dockerhub/config.yml:/etc/docker/registry/config.yml:ro \
        -p 127.0.0.1:5000:5000/tcp \
        --log-driver=journald \
        --log-opt tag="dockerd-dockerhub" \
        registry:2.8.2

`/srv/docker/dockerhub/config.yml` 的参考内容：

```yaml
version: 0.1
log:
    level: error
storage:
    delete:
        enabled: true
    cache:
        blobdescriptor: redis
    filesystem:
        rootdirectory: /var/lib/registry
    maintenance:
        uploadpurging:
            enabled: false
http:
    addr: :5000
    debug:
        addr: localhost:5001
    headers:
        X-Content-Type-Options: [nosniff]
notifications:
    endpoints:
        - name: local-5003
            url: http://localhost:5003/callback
            headers:
                Authorization: [Bearer <an example token>]
            timeout: 1s
            threshold: 10
            backoff: 1s
            disabled: true
        - name: local-8083
            url: http://localhost:8083/callback
            timeout: 1s
            threshold: 10
            backoff: 1s
            disabled: true
health:
    storagedriver:
        enabled: true
        interval: 10s
        threshold: 3

# 部分上游需要该选项，诸如 quay
# 详情参 https://github.com/distribution/distribution/issues/2367#issuecomment-454805687
# compatibility:
#     schema1:
#         enabled: true

proxy:
    remoteurl: https://registry-1.docker.io
    # 更换为上游的地址。

redis:
    addr: redis:6379
```

## 相关链接

Docker 主页

:   <https://www.docker.com>

Docker Hub

:   <https://hub.docker.com>