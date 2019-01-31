# 二进制包安装

我们可以到 Prometheus 二进制安装包[下载页面](https://prometheus.io/download/)，根据自己的操作系统选择下载对应的安装包。下面我们将以 ubuntu server 作为演示。

## 环境准备

* linux amd64 (ubuntu server)
* prometheus 1.6.2

## 下载 Prometheus Server

创建下载目录,以便安装过后清理掉

```
mkdir ~/Download
cd ~/Download
```

使用 wget 下载 Prometheus 的安装包

```
wget https://github.com/prometheus/prometheus/releases/download/v1.6.2/prometheus-1.6.2.linux-amd64.tar.gz
```

创建 Prometheus 目录，用于存放所有 Prometheus 相关的运行服务

```
mkdir ~/Prometheus
cd ~/Prometheus
```

使用 tar 解压缩 prometheus-1.6.2.linux-amd64.tar.gz

```
tar -xvzf ~/Download/prometheus-1.6.2.linux-amd64.tar.gz
cd prometheus-1.6.2.linux-amd64
```

当解压缩成功后，可以运行 version 检查运行环境是否正常

```
./prometheus version
```

如果你看到类似输出，表示你已安装成功:

```
prometheus, version 1.6.2 (branch: master, revision: xxxx)
  build user:       xxxx
  build date:       xxxx
  go version:       go1.8.1
```

## 启动 Prometheus Server

```
./prometheus
```

如果 prometheus 正常启动，你将看到如下信息：

```
INFO[0000] Starting prometheus (version=1.6.2, branch=master, revision=b38e977fd8cc2a0d13f47e7f0e17b82d1a908a9a)  source=main.go:88
INFO[0000] Build context (go=go1.8.1, user=root@c99d9d650cf4, date=20170511-13:03:00)  source=main.go:89
INFO[0000] Loading configuration file prometheus.yml     source=main.go:251
INFO[0000] Loading series map and head chunks...         source=storage.go:421
INFO[0000] 0 series loaded.                              source=storage.go:432
INFO[0000] Starting target manager...                    source=targetmanager.go:61
INFO[0000] Listening on :9090                            source=web.go:259
```

通过启动日志，可以看到 Prometheus Server 默认端口是 9090。

当 Prometheus 启动后，你可以通过浏览器来访问  `http://IP:9090`，将看到如下页面

![prometheus-graph.png](/images/install/prometheus-graph.png)

在默认配置中，我们已经添加了 Prometheus Server 的监控，所以我们现在可以使用 `PromQL` （Prometheus Query Language）来查看，比如：

![prometheus-console.png](/images/install/prometheus-console.png)

## 总结

1. 可以看出 Prometheus 二进制安装非常方便，没有依赖，自带查询 web 界面。
2. 在生产环境中，我们可以将 Prometheus 添加到 init 配置里，或者使用 supervisord 作为服务自启动。
