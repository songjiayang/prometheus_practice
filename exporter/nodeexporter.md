# Node Exporter

[node_exporter](https://github.com/prometheus/node_exporter) 主要用于 *NIX 系统监控,
用 Golang 编写。

## 功能对照表

### 默认开启的功能

| 名称 | 说明 | 系统 |
| ------| ------ | ------ |
| arp | 从 `/proc/net/arp` 中收集 ARP 统计信息 | Linux |
| conntrack | 从 `/proc/sys/net/netfilter/` 中收集 conntrack 统计信息 | Linux |
| cpu | 收集 cpu 统计信息 | Darwin, Dragonfly, FreeBSD, Linux |
| diskstats | 从 `/proc/diskstats` 中收集磁盘 I/O 统计信息  | Linux |
| edac | 错误检测与纠正统计信息 | Linux |
| entropy | 可用内核熵信息 | Linux |
| exec | execution 统计信息 | Dragonfly, FreeBSD |
| filefd | 从 `/proc/sys/fs/file-nr` 中收集文件描述符统计信息 | Linux |
| filesystem | 文件系统统计信息，例如磁盘已使用空间 | Darwin, Dragonfly, FreeBSD, Linux, OpenBSD |
| hwmon | 从 `/sys/class/hwmon/` 中收集监控器或传感器数据信息 | Linux |
| infiniband | 从 InfiniBand 配置中收集网络统计信息 | Linux |
| loadavg | 收集系统负载信息 | 	Darwin, Dragonfly, FreeBSD, Linux, NetBSD, OpenBSD, Solaris |
| mdadm | 从 `/proc/mdstat` 中获取设备统计信息 | Linux |
| meminfo | 内存统计信息 | Darwin, Dragonfly, FreeBSD, Linux |
| netdev | 网口流量统计信息，单位 bytes | Darwin, Dragonfly, FreeBSD, Linux, OpenBSD |
| netstat | 从 `/proc/net/netstat` 收集网络统计数据，等同于 `netstat -s` | Linux |
| sockstat | 从 `/proc/net/sockstat` 中收集 socket 统计信息 | Linux |
| stat | 从 `/proc/stat` 中收集各种统计信息，包含系统启动时间，forks, 中断等 | Linux |
| textfile | 通过 `--collector.textfile.directory` 参数指定本地文本收集路径，收集文本信息 | any |
| time | 系统当前时间 | any |
| uname | 通过 `uname` 系统调用, 获取系统信息  | any |
| vmstat | 从 `/proc/vmstat` 中收集统计信息  | Linux |
| wifi | 收集 wifi 设备相关统计数据  | Linux |
| xfs | 收集 xfs 运行时统计信息  | Linux (kernel 4.4+) |
| zfs | 收集 zfs 性能统计信息 | Linux |

### 默认关闭的功能

| 名称 | 说明 | 系统 |
| ------| ------ | ------ |
| bonding | 收集系统配置以及激活的绑定网卡数量 | Linux |
| buddyinfo | 从 `/proc/buddyinfo` 中收集内存碎片统计信息 | Linux |
| devstat | 收集设备统计信息 | Dragonfly, FreeBSD |
| drbd |  收集远程镜像块设备（DRBD）统计信息  | Linux |
| interrupts | 收集更具体的中断统计信息 | Linux，OpenBSD |
| ipvs | 从 `/proc/net/ip_vs` 中收集 IPVS 状态信息，从 `/proc/net/ip_vs_stats` 获取统计信息 | Linux |
| ksmd | 从 `/sys/kernel/mm/ksm` 中获取内核和系统统计信息 | Linux |
| logind | 从 `logind` 中收集会话统计信息 | Linux |
| meminfo_numa | 从 `/proc/meminfo_numa` 中收集内存统计信息 | Linux |
| mountstats | 从 `/proc/self/mountstat` 中收集文件系统统计信息，包括 NFS 客户端统计信息 | Linux |
| nfs | 从 `/proc/net/rpc/nfs` 中收集 NFS 统计信息，等同于 `nfsstat -c` | Linux |
| qdisc | 收集队列推定统计信息 | Linux |
| runit | 收集 runit 状态信息 | any |
| supervisord | 收集 supervisord 状态信息 | any |
| systemd | 从 `systemd` 中收集设备系统状态信息 | Linux |
| tcpstat | 从 `/proc/net/tcp` 和 `/proc/net/tcp6` 收集 TCP 连接状态信息 | Linux |

### 将被废弃功能

| 名称 | 说明 | 系统 |
| ------| ------ | ------ |
| gmond | 收集 Ganglia 统计信息 | any |
| megacli | 从 MegaCLI 中收集 RAID 统计信息 | Linux |
| ntp | 从 NTP 服务器中获取时钟 | any |

注意：我们可以使用 `--collectors.enabled` 运行参数指定 node_exporter 收集的功能模块, 如果不指定，将使用默认模块。

## 程序安装和启动

### 二进制安装

我们可以到[下载页面](https://prometheus.io/download/) 选择对应的二进制安装包，下面我将以 `0.14.0` 作为例子，

- 使用 wget 下载 Node Exporter

```
cd ~/Download
https://github.com/prometheus/node_exporter/releases/download/v0.14.0/node_exporter-0.14.0.linux-amd64.tar.gz
```

- 使用 tar 解压缩 node_exporter-0.14.0.linux-amd64.tar.gz

```
cd ~/Prometheus
tar -xvzf ~/Download/node_exporter-0.14.0.linux-amd64.tar.gz
cd node_exporter-0.14.0.linux-amd64
```

- 启动 Node Exporter

我们可以使用 `./node_exporter -h` 查看运行选项，`./node_exporter` 运行 Node Exporter, 如果看到类似输出，表示启动成功。

```
INFO[0000] Starting node_exporter (version=0.14.0, branch=master, revision=840ba5dcc71a084a3bc63cb6063003c1f94435a6)  source="node_exporter.go:140"
INFO[0000] Build context (go=go1.7.5, user=root@bb6d0678e7f3, date=20170321-12:13:32)  source="node_exporter.go:141"
INFO[0000] No directory specified, see --collector.textfile.directory  source="textfile.go:57"
INFO[0000] Enabled collectors:                           source="node_exporter.go:160"
.....
INFO[0000] Listening on :9100                            source="node_exporter.go:186"
```

### Docker 安装

我们可以使用 [docker 镜像](https://quay.io/repository/prometheus/node-exporter) 安装，命令为：

```
docker run -d -p 9100:9100 \
  -v "/proc:/host/proc:ro" \
  -v "/sys:/host/sys:ro" \
  -v "/:/rootfs:ro" \
  --net="host" \
  quay.io/prometheus/node-exporter \
    -collector.procfs /host/proc \
    -collector.sysfs /host/sys \
    -collector.filesystem.ignored-mount-points "^/(sys|proc|dev|host|etc)($|/)"
```

当 Node Exporter 运行起来后，在浏览器中访问 http://IP:9100/metrics， 将看到类似输出

```
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0
go_gc_duration_seconds{quantile="0.25"} 0
go_gc_duration_seconds{quantile="0.5"} 0
. . .
```

## 数据存储

我们可以利用 Prometheus 的 static_configs 来拉取 node_exporter 的数据。

打开 prometheus.yml 文件, 在 scrape_configs 中添加如下配置：

```
- job_name: "node"
    static_configs:
      - targets: ["127.0.0.1:9100"]
```

重启加载配置，然后到 Prometheus Console 查询，你会看到 node_exporter 的数据。
