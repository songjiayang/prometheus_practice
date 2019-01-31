# Prometheus Web

Prometheus 自带了 Web Console， 安装成功后可以访问 `http://localhost:9090/graph` 页面，用它可以进行任何 PromQL 查询和调试工作，非常方便，例如：

![prometheus web console ](/images/visualiztion/prometheus-web-console.png)

![prometheus web graph ](/images/visualiztion/prometheus-web-graph.png)

通过上图你不难发现，Prometheus 自带的 Web 界面比较简单，因为它的目的是为了及时查询数据，方便 PromeQL 调试。

它并不是像常见的 Admin Dashboard，在一个页面尽可能展示多的数据，如果你有这方面的需求，不妨试试 Grafana。
