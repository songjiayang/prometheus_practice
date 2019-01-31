# Docker 安装

首先确保你已安装了最新版本的 Docker, 如果没有安装请点击[这里](https://docs.docker.com/engine/installation/)。

下面我将以 Mac 版本的 Docker 作为演示。

## 安装

Docker 镜像地址 [Quay.io](https://quay.io/repository/prometheus/prometheus)

执行命令安装:

```
$ docker run --name prometheus -d -p 127.0.0.1:9090:9090 quay.io/prometheus/prometheus
```

如果安装成功你可以访问 `127.0.0.1:9090` 查看到该页面:

![prometheus-graph.png](/images/install/prometheus-graph.png)

## Docker 管理 prometheus

运行 docker ps 查看所有服务:

```
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS              PORTS                      NAMES
e9ebc2435387        quay.io/prometheus/prometheus   "/bin/prometheus -..."   26 minutes ago      Up 26 minutes       127.0.0.1:9090->9090/tcp   prometheus
```

运行 `docker start prometheus` 启动服务

运行 `docker stats prometheus` 查看 prometheus 状态

运行 `docker stop prometheus` 停止服务
