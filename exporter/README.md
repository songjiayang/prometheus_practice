# Exporter

在 Prometheus 中负责数据汇报的程序统一叫做 Exporter, 而不同的 Exporter 负责不同的业务。
它们具有统一命名格式，即 xx_exporter, 例如负责主机信息收集的 node_exporter。

Prometheus 社区已经提供了很多 exporter, 详情请参考[这里](https://prometheus.io/docs/instrumenting/exporters/#exporters-and-integrations) 。
