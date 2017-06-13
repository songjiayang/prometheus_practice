# 服务发现

在 Prometheus 的配置中，一个最重要的概念就是数据源 target，而数据源的配置主要分为静态配置和动态发现, 大致为以下几类：

- static_configs: 静态服务发现
- dns_sd_configs: DNS 服务发现
- file_sd_configs: 文件服务发现
- consul_sd_configs: Consul 服务发现
- serverset_sd_configs: Serverset 服务发现
- nerve_sd_configs: Nerve 服务发现
- marathon_sd_configs: Marathon 服务发现
- kubernetes_sd_configs: Kubernetes 服务发现
- gce_sd_configs: GCE 服务发现
- ec2_sd_configs: EC2 服务发现
- openstack_sd_configs: OpenStack 服务发现
- azure_sd_configs: Azure 服务发现
- triton_sd_configs: Triton 服务发现


它们具体使用以及配置模板，请参考服务发现[配置模板](https://prometheus.io/docs/operating/configuration/#<tls_config>)。

它们中最重要的，也是使用最广泛的应该是 `static_configs`, 其实那些动态类型都可以看成是某些通用业务使用静态服务封装的结果。
