# Alertmanager

在 Prometheus 中告警分为两部分:

- Prometheus 服务根据所设置的告警规则将告警信息发送给 Alertmanager。
- Alertmanager 对收到的告警信息进行处理，包括去重，降噪，分组，策略路由告警通知。

使用告警服务主要的步骤如下：

- 下载配置 Alertmanager。
- 通过设置 `-alertmanager.url` 让 Prometheus 服务与 Alertmanager 进行通信。
- 在 Prometheus 服务中设置告警规则。