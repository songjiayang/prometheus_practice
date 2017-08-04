# Alertmanager

在prometheus 中告警分为两部分:
- prometheus 服务根据所设置的告警规则将告警信息发送给 Alertmanager
- Alertmanager 对收到的告警信息进行处理，包括：忽略，禁止，聚合或者通过邮件等方式发送告警消息

使用告警服务主要的步骤如下：
- 下载配置 Alertmanager
- 通过设置 `-alertmanager.url` 让prometheus服务与Alertmanager进行通信
- 在prometheus服务中设置告警规则