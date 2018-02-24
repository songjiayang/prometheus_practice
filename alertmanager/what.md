# Alertmanager 是什么？

![架构图](https://raw.githubusercontent.com/prometheus/alertmanager/4e6695682acd2580773a904e4aa2e3b927ee27b7/doc/arch.jpg)

Alertmanager 主要用于接收 Prometheus 发送的告警信息，它支持丰富的告警通知渠道，而且很容易做到告警信息进行去重，降噪，分组，策略路由，是一款前卫的告警通知系统。

### 安装

使用 `wget` 下载按转包

```
cd ~/Download
wget https://github.com/prometheus/alertmanager/releases/download/v0.14.0/alertmanager-0.14.0.linux-amd64.tar.gz
cd Prometheus
```

使用 `tar` 解压缩 alertmanager-0.14.0.linux-amd64.tar.gz

```
tar -xvzf ~/Download alertmanager-0.14.0.linux-amd64.tar.gz
cd alertmanager-0.14.0.linux-amd64
```

解压成功后，使用 `./alertmanager --version` 来检查是否安装成功

```
alertmanager, version 0.14.0 (branch: HEAD, revision: 30af4d051b37ce817ea7e35b56c57a0e2ec9dbb0)
  build user:       root@37b6a49ebba9
  build date:       20180213-08:16:42
  go version:       go1.9.2
```

### 基本配置

执行命令 `mv simple.yml alertmanager.yml`，并修改 `alertmanager.yml` 配置：

```
global:
  resolve_timeout: 2h

route:
  group_by: ['alertname']
  group_wait: 5s
  group_interval: 10s
  repeat_interval: 1h
  receiver: 'webhook'

receivers:
- name: 'webhook'
  webhook_configs:
  - url: 'http://example.com/xxxx'
    send_resolved: true
```

说明： 这里我们使用 Alertmanager 的 `webhook_configs` 选项来接收消息，当接收到新的告警信息，它会将消息转发到配置的 `url` 地址。
