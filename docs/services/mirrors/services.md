# 镜像服务

## 首页生成

镜像站主页是静态的，由jekyll生成。

crontab 会定时运行该脚本，生成首页和 [mirrorz 项目](https://mirrorz.org/)需要的数据。

在首页展示的「获取安装镜像」、「获取开源软件」、「反向代理列表」分别由 config 内配置指定，「文件列表」内容则会从[同步程序 tunasync](https://github.com/ustclug/yuki) 的 api 中获取。

## HTTP 服务

Mirrors 使用 nginx 提供 HTTP 服务。

## Rsync 服务

TODO

## 反向代理服务

TODO
