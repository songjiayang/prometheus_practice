[Prometheus](https://prometheus.io) 是由 SoundCloud 开源监控告警解决方案，从 2012 年开始编写代码，再到 2015 年 github 上开源以来，已经吸引了 9k+ 关注，以及很多大公司的使用；2016 年 Prometheus 成为继 k8s 后，第二名 CNCF\([Cloud Native Computing Foundation](https://cncf.io/)\) 成员。

作为新一代开源解决方案，很多理念与 Google SRE 运维之道不谋而合。

#### **主要功能**

* 多维 [数据模型](https://prometheus.io/docs/concepts/data_model/)（时序由 metric 名字和 k/v 的 labels 构成）。
* 灵活的查询语句（[PromQL](https://prometheus.io/docs/querying/basics/)）。
* 无依赖存储，支持 local 和 remote 不同模型。
* 采用 http 协议，使用 pull 模式，拉取数据，简单易懂。
* 监控目标，可以采用服务发现或静态配置的方式。
* 支持多种统计数据模型，图形化友好。

#### **核心组件**

* [Prometheus Server](https://github.com/prometheus/prometheus)， 主要用于抓取数据和存储时序数据，另外还提供查询和 Alert Rule 配置管理。
* [client libraries](https://prometheus.io/docs/instrumenting/clientlibs/)，用于对接 Prometheus Server, 可以查询和上报数据。 
* [push gateway](https://github.com/prometheus/pushgateway) ，用于批量，短期的监控数据的汇总节点，主要用于业务数据汇报等。
* 各种汇报数据的 [exporters](https://prometheus.io/docs/instrumenting/exporters/) ，例如汇报机器数据的 node\_exporter,  汇报 MongoDB 信息的 [MongoDB exporter](https://github.com/dcu/mongodb_exporter) 等等。
* 用于告警通知管理的 [alertmanager](https://github.com/prometheus/alertmanager) 。

#### **基础架构**

一图胜千言，先来张官方的架构图

![](https://prometheus.io/assets/architecture.svg)

从这个架构图，也可以看出 Prometheus 的主要模块包含， Server,  Exporters, Pushgateway, PromQL, Alertmanager, WebUI 等。

它大致逻辑是这样：

1. Prometheus server 定期从静态配置的 targets 或者服务发现的 targets 拉取数据。
2. 当新拉取的数据大于配置的 buffer 的时候，Prometheus 会将数据持久化到磁盘（使用 local storage, 如果使用 remote storage 将持久化到云端）
3. Prometheus 可以配置 rules，然后定时查询数据，当触发条件的时候，会将 alert 推送到配置的 Alertmanager。
4. Alertmanager 收到警告的时候，可以根据配置，聚合，去重，降噪，最后发送警告。



#### 为什么选择 Prometheus

* Prometheus 是按照 Google SRE 运维之道的理念构建的，具有实用性和前瞻性。

* Prometheus 社区非常活跃，基本稳定在 1个月1个版本的迭代速度；从去年 v1.01 开始接触使用以来，到目前发布的 v1.6.1 以及最新最新的 v2.0 ，你会发现 Prometheus 一直在进步，在优化。

* Go 语言开发，性能不错，安装部署简单，跨平台。

* 丰富的数据收集客户端，官方提供了各种常用 exporter。

* 丰富强大的查询能力。



