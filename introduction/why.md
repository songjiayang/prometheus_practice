# 为什么选择 Prometheus

在前言中，简单介绍了我们选择 Prometheus 的理由，以及使用后给我们带来的好处。

在这里主要和其他监控方案对比，方便大家更好的了解 Prometheus。

## Prometheus vs Zabbix

* Zabbix 使用的是 C 和 PHP, Prometheus 使用 Golang, 整体而言 Prometheus 运行速度更快一点。
* Zabbix 属于传统主机监控，主要用于物理主机，交换机，网络等监控，Prometheus 不仅适用主机监控，还适用于 Cloud, SaaS, Openstack，Container 监控。
* Zabbix 在传统主机监控方面，有更丰富的插件。
* Zabbix 可以在 WebGui 中配置很多事情，但是 Prometheus 需要手动修改文件配置。

## Prometheus vs Graphite

* [Graphite](http://graphite.readthedocs.io/en/latest/overview.html) 功能较少，它专注于两件事，存储时序数据，
可视化数据，其他功能需要安装相关插件，而 Prometheus 属于一站式，提供告警和趋势分析的常见功能，它提供更强的数据存储和查询能力。
* 在水平扩展方案以及数据存储周期上，Graphite 做的更好。

## Prometheus vs InfluxDB

* [InfluxDB](https://www.influxdata.com/) 是一个开源的时序数据库，主要用于存储数据，如果想搭建监控告警系统，
需要依赖其他系统。
* InfluxDB 在存储水平扩展以及高可用方面做的更好, 毕竟核心是数据库。

## Prometheus vs OpenTSDB

* [OpenTSDB](http://opentsdb.net/) 是一个分布式时序数据库，它依赖 Hadoop 和 HBase，能存储更长久数据，
如果你系统已经运行了 Hadoop 和 HBase, 它是个不错的选择。
* 如果想搭建监控告警系统，OpenTSDB 需要依赖其他系统。

## Prometheus vs Nagios

* [Nagios](https://www.nagios.org/) 数据不支持自定义 Labels, 不支持查询，告警也不支持去噪，分组, 没有数据存储，如果想查询历史状态，需要安装插件。
* Nagios 是上世纪 90 年代的监控系统，比较适合小集群或静态系统的监控，显然 Nagios 太古老了，很多特性都没有，相比之下Prometheus 要优秀很多。

## Prometheus vs Sensu

* [Sensu](https://sensuapp.org/) 广义上讲是 Nagios 的升级版本，它解决了很多 Nagios 的问题，如果你对 Nagios 很熟悉，使用 Sensu 是个不错的选择。
* Sensu 依赖 RabbitMQ 和 Redis，数据存储上扩展性更好。

## 总结

* Prometheus 属于一站式监控告警平台，依赖少，功能齐全。
* Prometheus 支持对云或容器的监控，其他系统主要对主机监控。
* Prometheus 数据查询语句表现力更强大，内置更强大的统计函数。
* Prometheus 在数据存储扩展性以及持久性上没有 InfluxDB，OpenTSDB，Sensu 好。
