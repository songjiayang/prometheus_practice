# Grafana 使用

Grafana 是一套开源的分析监视平台，支持 Graphite, InfluxDB, OpenTSDB, Prometheus, Elasticsearch, CloudWatch 等数据源，其 UI 非常漂亮且高度定制化。

这是 Prometheus web console 不具备的，在上一节中我已经说明了选择它的原因。

## 版本说明

* Mac version 4.3.2

## 安装和运行程序

这里我使用 brew 安装，命令为

```
brew update
brew install grafana
```

当安装成功后，你可以使用默认配置启动程序

```
grafana-server -homepath /usr/local/Cellar/grafana/4.3.2/share/grafana/
```

如果顺利，你将看到如下日志

```
INFO[06-11|15:20:14] Starting Grafana                         logger=main version=4.3.2 commit=unknown-dev compiled=2017-06-01T05:47:48+0800
INFO[06-11|15:20:14] Config loaded from                       logger=settings file=/usr/local/Cellar/grafana/4.3.2/share/grafana/conf/defaults.ini
INFO[06-11|15:20:14] Path Home                                logger=settings path=/usr/local/Cellar/grafana/4.3.2/share/grafana/
INFO[06-11|15:20:14] Path Data                                logger=settings path=/usr/local/Cellar/grafana/4.3.2/share/grafana/data
INFO[06-11|15:20:14] Path Logs                                logger=settings path=/usr/local/Cellar/grafana/4.3.2/share/grafana/data/log
INFO[06-11|15:20:14] Path Plugins                             logger=settings path=/usr/local/Cellar/grafana/4.3.2/share/grafana/data/plugins
INFO[06-11|15:20:14] Initializing DB                          logger=sqlstore dbtype=sqlite3
INFO[06-11|15:20:14] Starting DB migration                    logger=migrator
INFO[06-11|15:20:14] Executing migration                      logger=migrator id="copy data account to org"
INFO[06-11|15:20:14] Skipping migration condition not fulfilled logger=migrator id="copy data account to org"
INFO[06-11|15:20:14] Executing migration                      logger=migrator id="copy data account_user to org_user"
INFO[06-11|15:20:14] Skipping migration condition not fulfilled logger=migrator id="copy data account_user to org_user"
INFO[06-11|15:20:14] Starting plugin search                   logger=plugins
INFO[06-11|15:20:14] Initializing Alerting                    logger=alerting.engine
INFO[06-11|15:20:14] Initializing CleanUpService              logger=cleanup
INFO[06-11|15:20:14] Initializing Stream Manager
INFO[06-11|15:20:14] Initializing HTTP Server                 logger=http.server address=0.0.0.0:3000 protocol=http subUrl= socket=

```

此时，你可以打开页面 `http://localhost:3000`， 访问 Grafana 的 web 界面。

其他平台安装方案，请参考[更多安装](https://grafana.com/grafana/download)。

## 登录并设置 Prometheus 数据源

Grafana 本身支持 Prometheus 数据源，故不需要安装其他插件。

使用默认账号 admin/admin 登录 grafana

![grafana-login](/images/visualiztion/grafana-login.png)

在 Dashboard 首页，点击添加数据源

![grafana-datasource](/images/visualiztion/grafana-datasource.png)

配置 Prometheus 数据源

![grafana-prometheus-data-source](/images/visualiztion/grafana-prometheus-data-source.png)

目前为止，Grafana 已经和 Prometheus 连上了，你将看到这样的 Dashboard

![grafana-default-dashbord](/images/visualiztion/grafana-default-dashbord.png)

## 自定义监视画板

由顶部 `Manage dashboard` -> `Settings` 进入管理页面

![rafana-into-manage-dashboard](/images/visualiztion/grafana-into-manage-dashboard.png)

在管理页面中取消 `Hide Controls`

![grafana-hide-controls](/images/visualiztion/grafana-hide-controls.png)

点击页面底部 `+ ADD ROW` 按钮, 并选择 `Graph` 类型

![grafana-add-graph](/images/visualiztion/grafana-add-graph.png)

点击  `Panel Title` -> `Edit` 进入 Panel 编辑页面，并在 `Metrics` 中
的 `Metric lookup` 选择 `go_goroutines`

![grafana-edit-panel](/images/visualiztion/grafana-edit-panel.png)

你也可以直接在管理界面中填写 Prometheus 的查询语句，以及修改查询的 step 数值。

当你修改了 Dashboard 后，记得点击顶部的 `Save dashboard` 按钮，或直接 `CTRL+S` 保存。

至此，我们自定义的 Panel 已添加完成

![grafana-added-panel](/images/visualiztion/grafana-added-panel.png)

我们可以通过拖拽，拉升调节 panel 的位置和尺寸，我们调节的目的是尽量在一个屏幕显示更多信息。

## 总结

Grafana 是一款非常漂亮，强大的监视分析平台，本身支持了 Prometheus 数据源，所以在做数据和监视可视化的时候，Grafana + Prometheus 是个不错的选择。
