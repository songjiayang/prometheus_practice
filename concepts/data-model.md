# 数据模型

Prometheus 存储的是[时序数据](https://en.wikipedia.org/wiki/Time_series), 即按照相同时序(相同的名字和标签)，以时间维度存储连续的数据的集合。

## 时序索引

时序(time series) 是由名字(Metric)，以及一组 key/value 标签定义的，具有相同的名字以及标签属于相同时序。

时序的名字由 ASCII 字符，数字，下划线，以及冒号组成，它必须满足正则表达式 `[a-zA-Z_:][a-zA-Z0-9_:]* `, 其名字应该具有语义化，一般表示一个可以度量的指标，例如: `http_requests_total`, 可以表示 http 请求的总数。

时序的标签可以使 Prometheus 的数据更加丰富，能够区分具体不同的实例，例如 `http_requests_total{method="POST"}` 可以表示所有 http 中的 POST 请求。

标签名称由 ASCII 字符，数字，以及下划线组成， 其中 `__` 开头属于 Prometheus 保留，标签的值可以是任何 Unicode 字符，支持中文。

## 时序样本

按照某个时序以时间维度采集的数据，称之为样本，其值包含：

* 一个 float64 值
* 一个毫秒级的 unix 时间戳

## 格式

Prometheus 时序格式与 [OpenTSDB](http://opentsdb.net/) 相似：

```
<metric name>{<label name>=<label value>, ...}
```

其中包含时序名字以及时序的标签。
