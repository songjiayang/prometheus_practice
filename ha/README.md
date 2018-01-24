# 高可用

Prometheus 监控告警系统的高可用主要分为两部分:

- Prometheus Server 的高可用，无单点风险。
- Alertmanager 的高可用，避免告警消息丢失和重复。

下面我们就针对这两个方面展开讨论。
