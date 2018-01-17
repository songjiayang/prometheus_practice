
# Node Exporter 常用查询语句

收集到 node_exporter 的数据后，我们可以使用 PromQL 进行一些业务查询和监控，下面是一些比较常见的查询。

注意：以下查询均以单个节点作为例子，如果大家想查看所有节点，将 `instance="xxx"` 去掉即可。

## CPU 使用率

```
100 - (avg by (instance) (irate(node_cpu{instance="xxx", mode="idle"}[5m])) * 100)
```

## CPU 各 mode 占比率

```
avg by (instance, mode) (irate(node_cpu{instance="xxx"}[5m])) * 100
```

## 机器平均负载

```
node_load1{instance="xxx"} // 1分钟负载
node_load5{instance="xxx"} // 5分钟负载
node_load15{instance="xxx"} // 15分钟负载
```

## 内存使用率
```
100 - ((node_memory_MemFree{instance="xxx"}+node_memory_Cached{instance="xxx"}+node_memory_Buffers{instance="xxx"})/node_memory_MemTotal) * 100
```

## 磁盘使用率

```
100 - node_filesystem_free{instance="xxx",fstype!~"rootfs|selinuxfs|autofs|rpc_pipefs|tmpfs|udev|none|devpts|sysfs|debugfs|fuse.*"} / node_filesystem_size{instance="xxx",fstype!~"rootfs|selinuxfs|autofs|rpc_pipefs|tmpfs|udev|none|devpts|sysfs|debugfs|fuse.*"} * 100
```

或者你也可以直接使用 {fstype="xxx"} 来指定想查看的磁盘信息

## 网络 IO

```
// 上行带宽
sum by (instance) (irate(node_network_receive_bytes{instance="xxx",device!~"bond.*?|lo"}[5m])/128)

// 下行带宽
sum by (instance) (irate(node_network_transmit_bytes{instance="xxx",device!~"bond.*?|lo"}[5m])/128)
```

## 网卡出/入包

```
// 入包量
sum by (instance) (rate(node_network_receive_bytes{instance="xxx",device!="lo"}[5m]))

// 出包量
sum by (instance) (rate(node_network_transmit_bytes{instance="xxx",device!="lo"}[5m]))
```
