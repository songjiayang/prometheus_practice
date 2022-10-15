# Docker 安装

首先确保你已安装了最新版本的 Docker, 如果没有安装请点击[这里](https://docs.docker.com/engine/installation/)。

下面我将以 Mac 版本的 Docker 作为演示。

## 安装

Docker 镜像地址 [Quay.io](https://quay.io/repository/prometheus/prometheus)

执行命令安装:

```
$ docker run --name prometheus -d -p 9090:9090 quay.io/prometheus/prometheus:v2.39.1
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

## Docker 镜像配置参数

通过 Prometheus 的 [Dockerfile](https://github.com/prometheus/prometheus/blob/main/Dockerfile)可以发现，其运行命令为 /bin/prometheus，并包含了4个默认启动参数：

```yaml
ENTRYPOINT [ "/bin/prometheus" ]
CMD        [ "--config.file=/etc/prometheus/prometheus.yml", \
             "--storage.tsdb.path=/prometheus", \
             "--web.console.libraries=/usr/share/prometheus/console_libraries", \
             "--web.console.templates=/usr/share/prometheus/consoles" ]
```

所以，当我们要修改配置 Prometheus 的启动参数，直接覆盖掉默认的即可，例如：

- 修改配置文件和存储目录

```
docker run --name prometheus
    -v=$(pwd)/examples/prometheus.yml:/etc/prometheus/prometheus.yml \
    -v=$(pwd)/examples/tsdb:/prometheus \
    -d -p 9090:9090 \
    quay.io/prometheus/prometheus:v2.39.1
```

此时在 `$(pwd)/examples` 目录下将生产一个 tsdb 目录，存储 Promtheus 时序数据。

- 开启 Lifecycle API

更新启动命令，添加 `--web.enable-lifecycle` 命令参数：

```
docker run --name prometheus \
    -v=$(pwd)/examples/prometheus.yml:/etc/prometheus/prometheus.yml \
    -v=$(pwd)/examples/tsdb:/prometheus \
    -d -p 9090:9090 \
    quay.io/prometheus/prometheus:v2.39.1 \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/prometheus \
    --web.console.libraries=/usr/share/prometheus/console_libraries \
    --web.console.templates=/usr/share/prometheus/consoles \
    --web.enable-lifecycle
```

当重新执行 Docker 命令后并更新 `examples/prometheus.yml` 后，可以通过 `curl -X POST http://localhost:9090/-/reload` 动态加载配置文件。

