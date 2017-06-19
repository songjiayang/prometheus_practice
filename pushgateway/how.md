# Pushgateway 安装和使用

## 安装

### 二进制安装

你可以到[下载页面](https://github.com/prometheus/pushgateway/releases/tag/v0.4.0),选择对应的版本，
以 v0.4.0 为例子,

* 使用 wget 下载 pushgateway

```
cd ~/Download
https://github.com/prometheus/pushgateway/releases/download/v0.4.0/pushgateway-0.4.0.linux-amd64.tar.gz
```

* 使用 tar 解压缩 pushgateway-0.4.0.linux-amd64.tar.gz

```
cd ~/Prometheus
tar -xvzf ~/Download/pushgateway-0.4.0.linux-amd64.tar.gz
cd pushgateway-0.4.0.linux-amd64
```

* 启动 pushgateway

我们可以使用 ./pushgateway -h 查看运行选项，./pushgateway 运行 pushgateway, 如果看到类似输出，表示启动成功。

```
INFO[0000] Starting pushgateway (version=0.4.0, branch=master, revision=6ceb4a19fa85ac2d6c2d386c144566fb1ede1f6c)  source=main.go:57
INFO[0000] Build context (go=go1.8.3, user=root@87741d1b66a9, date=20170609-12:26:14)  source=main.go:58
INFO[0000] Listening on :9091.                           source=main.go:102
```

### Docker 安装

我们可以使用 [prom/pushgateway](https://registry.hub.docker.com/u/prom/pushgateway/) 的 Docker 镜像，

```
docker pull prom/pushgateway

docker run -d -p 9091:9091 prom/pushgateway
```

## 数据管理

正常情况我们会使用 Client SDK 推送数据到 pushgateway, 但是我们还可以通过 API 来管理, 例如：

* 向 `{job="some_job"}` 添加单条数据：

```
echo "some_metric 3.14" | curl --data-binary @- http://pushgateway.example.org:9091/metrics/job/some_job
```

* 添加更多更复杂数据，通常数据会带上 instance, 表示来源位置：

```
cat <<EOF | curl --data-binary @- http://pushgateway.example.org:9091/metrics/job/some_job/instance/some_instance
# TYPE some_metric counter
some_metric{label="val1"} 42
# TYPE another_metric gauge
# HELP another_metric Just an example.
another_metric 2398.283
EOF
```

* 删除某个组下的某实例的所有数据：

```
 curl -X DELETE http://pushgateway.example.org:9091/metrics/job/some_job/instance/some_instance
```

* 删除某个组下的所有数据：

```
curl -X DELETE http://pushgateway.example.org:9091/metrics/job/some_job
```

可以发现 pushgateway 中的数据我们通常按照 `job` 和 `instance` 分组分类，所以这两个参数不可缺少。

因为 Prometheus 配置 pushgateway 的时候，也会指定 job 和 instance, 但是它只表示 pushgateway 实例，不能真正表达收集数据的含义。所以在 prometheus 中配置 pushgateway 的时候，需要添加 `honor_labels: true` 参数，
从而避免收集数据本身的 `job` 和 `instance` 被覆盖。

注意，为了防止 pushgateway 重启或意外挂掉，导致数据丢失，我们可以通过 `-persistence.file` 和 `-persistence.interval` 参数将数据持久化下来。
