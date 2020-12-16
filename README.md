# Prometheus 实战

v0.1.0

在过去一年左右时间里，我们使用 Prometheus 完成了对几个机房的基础和业务监控，大大提高了服务质量以及 oncall 水平，在此特别感谢 Promethues 这样优秀的开源软件。

当初选择 Prometheus 并不是偶然，因为：

* Prometheus 是按照 Google SRE 运维之道的理念构建的，具有实用性和前瞻性。

* Prometheus 社区非常活跃，基本稳定在 1个月1个版本的迭代速度，从 2016 年 v1.01 开始接触使用以来，到目前发布的 v1.8.2 以及最新最新的 v2.1 ，你会发现 Prometheus 一直在进步、在优化。

* Go 语言开发，性能不错，安装部署简单，多平台部署兼容性好。

* 丰富的数据收集客户端，官方提供了各种常用 exporter。

* 丰富强大的查询能力。

Prometheus 作为监控后起之秀，虽然还有做的不够好的地方，但是不妨碍我们使用和喜爱它。根据我们长期的使用经验来看，它足以满足大多数场景需求，只不过对于新东西，往往需要花费更多力气才能发挥它的最大能力而已。

本书主要根据个人过去一年多的使用经验总结而成，内容主要包括 Prometheus 基本知识、进阶、实战以及常见问题列表等方面，希望对大家有所帮助。

本开源书籍既适用于具备基础 Linux 知识的运维初学者，也可供渴望理解 Prometheus 原理和实现细节的高级用户参考，同时也希望书中给出的实践案例在实际部署监控中对大家有所帮助。

你准备好了吗？接下来就让我们一起开始这段神奇旅行吧！


## 目录

* [前言](README.md)
* [修订记录](revision-record.md)
* [如何贡献](how-to-contribute.md)
* [Prometheus 简介](introduction/README.md)
    * [Prometheus 是什么](introduction/what.md)
    * [为什么选择 Prometheus](introduction/why.md)
* [Prometheus 安装](install/README.md)
  * [二进制包安装](install/binary.md)
  * [Docker 安装](install/docker.md)
* [基础概念](concepts/README.md)
  * [数据模型](concepts/data-model.md)
  * [Metric types](concepts/metric-types.md)
  * [作业与实例](concepts/jobs-and-instances.md)
* [PromQL](promql/README.md)
  * [PromQL 基本使用](promql/summary.md)
  * [与 SQL 对比](promql/sql.md)
* [数据可视化](visualiztion/README.md)
  * [Web Console](visualiztion/console.md)
  * [Grafana](visualiztion/grafana.md)
* [Prometheus 配置](configuration/README.md)
  * [全局配置](configuration/global.md)
  * [告警配置](configuration/alerting.md)
  * [规则配置](configuration/rule_files.md)
  * [数据拉取配置](configuration/scrape_configs.md)
  * [远程可写存储](configuration/remote_write.md)
  * [远程可读存储](configuration/remote_read.md)
  * [服务发现](configuration/server_discovery.md)
  * [配置样例](configuration/demo.md)
* [Exporter](exporter/README.md)
  * [文本格式](exporter/text.md)
  * [Sample Exporter](exporter/sample.md)
  * [Node Exporter 安装使用](exporter/nodeexporter.md)
  * [Node Exporter 常用查询](exporter/nodeexporter_query.md)
  * [Node Exporter Grafana 模版](exporter/nodeexporter_grafana_template.md)
  * [其他 Exporter 介绍](exporter/other.md)
* [Pushgateway](pushgateway/README.md)
    * [Pushgateway 是什么](pushgateway/why.md)
    * [如何使用 Pushgateway? ](pushgateway/how.md)  
* [数据存储](store/README.md)
    * [Local Store](store/local.md)
    * [Remote Store](store/remote.md)
* [告警规则](rule/README.md)
    * [如何配置](rule/config.md)
    * [触发逻辑](rule/what.md)  
* [Alertmanager](alertmanager/README.md)
    * [Alertmanager 是什么](alertmanager/what.md)
    * [配置详情](alertmanager/config.md)  
    * [通过 Email 接收告警](alertmanager/email.md)  
    * [通过企业微信接收告警](alertmanager/wechat.md)
    * [通过 Slack 接收告警](alertmanager/slack.md)  
    * [通过 Webhook 接收告警](alertmanager/webhooks.md)  
    * [其他告警接收方案](alertmanager/others.md)
* [主机监控完整示例](demo/README.md)
    * [NodeExporter](demo/target.md)
    * [配置告警规则](demo/rule.md)
    * [Grafana 集成](demo/grafana.md)
    * [通过 Alertmanager 告警](demo/alertmanager.md)
* [Prometheus 工具](tools/README.md)
    * [Promtool 介绍和使用](tools/promu.md)
    * [Client SDK](tools/client.md)
* [Prometheus 性能调优](optimize/README.md)
    * [通过 Metrics 查看 Prometheus 运行状态](optimize/status.md)
    * [通过日志分析 Prometheus 运行状态](optimize/logger.md)
    * [启动参数详解](optimize/config.md)
* [Prometheus 与容器](container/README.md)
    * [Docker](container/docker.md)
    * [Kubernetes](container/k8s.md)
* [Prometheus 常见服务监控](service/README.md)
    * [Nginx](service/nginx.md)
    * [Memcached](service/memcached.md)
    * [MongoDB](service/mongodb.md)
    * [MySQL](service/mysql.md)
    * [Redis](service/redis.md)
* [Prometheus 与 DevOps](devops/README.md)
    * [从 0 开发一个 exporter](devops/exporter.md)
    * [使用 Webhooks 开发一个 alert receiver](devops/receiver.md)
* [高可用方案探讨](ha/README.md)
    * [Prometheus Server 的高可靠](ha/prometheus.md)
    * [AlertManager 的高可靠](ha/alertmanger.md)
* [常见问题收录](qa/README.md)
    * [如何热加载新配置](qa/hotreload.md)
    * [为什么重启 Prometheus 过后，数据无法查询](qa/hotreload.md)
    * [如何删除 Pushgateway 的数据](qa/pushgateway.md)
    * [为什么内存使用这么高](qa/memory.md)
    * [为什么有数据丢失](qa/losedata.md)
    * [如何通过认证后拉取数据](qa/auth.md)

## 技术交流

欢迎加入 Prometheus 技术交流 QQ 群，分享 Prometheus 资源，交流 Prometheus 技术。

* QQ 群：465362780 (已满) 申请加入请备注：prometheus 实战
* 微信群：(待补充)

## 关于作者

* small_fish__

  * [微博](https://weibo.com/songjiayang1)
  * [github](https://github.com/songjiayang)
  * 个人公众号
  
  ![人人都懂云原生](https://git.io/vAQvJ)
 
- 薛锦

  * [微博](https://weibo.com/1660913012/profile?topnav=1&wvr=6)
  * [github](https://github.com/csxuejin)
  * 个人公众号

  ![GitHub Logo](https://songjiayang.gitbooks.io/go-basic-courses/content/pics/easy-hacking.jpg)
  
## GitHub 关注曲线

[![Stargazers over time](https://starcharts.herokuapp.com/songjiayang/prometheus_practice.svg)](https://starcharts.herokuapp.com/songjiayang/prometheus_practice)
  
