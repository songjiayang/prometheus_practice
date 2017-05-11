[Prometheus](https://prometheus.io) 是由 SoundCloud 开源监控告警解决方案，从 2012 年开始编写代码，再到 2015 年 github 上开源以来，已经吸引了 9k+ 关注，以及很多大公司的使用；2016 年 Prometheus 成为继 k8s 后，第二名 CNCF\([Cloud Native Computing Foundation](https://cncf.io/)\) 成员。

作为新一代开源解决方案，很多理念与 Google SRE 运维之道不谋而合。

#### **主要功能：**

* 多维 [数据模型](https://prometheus.io/docs/concepts/data_model/)（时序由 metric 名字和 k/v 的 labels 构成）。
* 灵活的查询语句（[PromQL](https://prometheus.io/docs/querying/basics/)）。
* 无依赖存储，支持 local 和 remote 不同模型 。
* 采用 http 协议，使用 pull 模式，拉取数据，简单易懂 。
* 监控目标，可以采用服务发现或静态配置的方式 。
* 支持多种统计数据模型
* #### 为什么选择 Prometheus
* Prometheus 是按照 Google SRE 运维之道的理念构建的，具有实用性和前瞻性。

* Prometheus 社区非常活跃，基本稳定在 1个月1个版本的迭代速度；从去年 v1.01 开始接触使用以来，到目前发布的 v1.6.1 以及最新最新的 v2.0 ，你会发现 Prometheus 一直在进步，在优化。

* Go 语言开发，性能不错，安装部署简单，跨平台。

* 丰富的数据收集客户端，官方提供了各种常用 exporter。

* 丰富强大的查询能力。



