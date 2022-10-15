# Grafana 使用

Grafana 是一套开源的分析监视平台，支持 Graphite, InfluxDB, OpenTSDB, Prometheus, Elasticsearch, CloudWatch 等数据源，其 UI 非常漂亮且高度定制化。

这是 Prometheus web console 不具备的，在上一节中我已经说明了选择它的原因。

## 版本说明

* Grafana 版本 9.1.8

## 安装和运行程序

这里我使用 docker 安装，与 Promethus 使用相同网络

```
docker network create prometheus
docker run -d --name=grafana --network prometheus  -p 3000:3000 grafana/grafana:9.1.8
docker run -d --name prometheus --network prometheus  -p 9090:9090 quay.io/prometheus/prometheus:v2.39.1
```

此时，你可以打开页面 `http://localhost:3000`， 访问 Grafana 的 web 界面。

## 登录并设置 Prometheus 数据源

Grafana 本身支持 Prometheus 数据源，故不需要安装其他插件。

使用默认账号 admin/admin 登录 grafana

![grafana-login](/images/visualiztion/grafana-login.png)

在 Dashboard 首页，点击添加数据源

![grafana-datasource](/images/visualiztion/grafana-datasource.png)

配置 Prometheus 数据源

![grafana-prometheus-data-source](/images/visualiztion/grafana-prometheus-data-source.png)

目前为止，Grafana 已经和 Prometheus 连上了，此时你如果导入 [prometheus-2-0-overview 模版](https://grafana.com/grafana/dashboards/3662-prometheus-2-0-overview/)，你将看到类似结果：

![grafana-default-dashbord](/images/visualiztion/grafana-default-dashbord.png)

关于 Grafana 更多用法，可以参见其[文档](https://grafana.com/docs/grafana/v9.0/)

## 总结

Grafana 是一款非常漂亮，强大的监视分析平台，本身支持了 Prometheus 数据源，所以在做数据和监视可视化的时候，Grafana + Prometheus 是个不错的选择。
