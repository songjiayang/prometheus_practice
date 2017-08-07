# 文本格式

在讨论 Exporter 之前，有必要先介绍一下 Prometheus 文本数据格式，因为一个 Exporter 本质上就是将收集的数据，转化为对应的文本格式，并提供 http 请求。

Exporter 收集的数据转化的文本内容以行 (`\n`) 为单位，空行将被忽略, 文本内容最后一行为空行。

## 注释

文本内容，如果以 `#` 开头通常表示注释。

- 以 `# HELP` 开头表示 metric 帮助说明。
- 以 `# TYPE ` 开头表示定义 metric 类型，包含 `counter`, `gauge`, `histogram`, `summary`, 和 `untyped` 类型。
- 其他表示一般注释，供阅读使用，将被 Prometheus 忽略。

## 采样数据

内容如果不以 `#` 开头，表示采样数据。它通常紧挨着类型定义行，满足以下格式：

```
metric_name [
  "{" label_name "=" `"` label_value `"` { "," label_name "=" `"` label_value `"` } [ "," ] "}"
] value [ timestamp ]
```

下面是一个完整的例子：

```
# HELP http_requests_total The total number of HTTP requests.
# TYPE http_requests_total counter
http_requests_total{method="post",code="200"} 1027 1395066363000
http_requests_total{method="post",code="400"}    3 1395066363000

# Escaping in label values:
msdos_file_access_time_seconds{path="C:\\DIR\\FILE.TXT",error="Cannot find file:\n\"FILE.TXT\""} 1.458255915e9

# Minimalistic line:
metric_without_timestamp_and_labels 12.47

# A weird metric from before the epoch:
something_weird{problem="division by zero"} +Inf -3982045

# A histogram, which has a pretty complex representation in the text format:
# HELP http_request_duration_seconds A histogram of the request duration.
# TYPE http_request_duration_seconds histogram
http_request_duration_seconds_bucket{le="0.05"} 24054
http_request_duration_seconds_bucket{le="0.1"} 33444
http_request_duration_seconds_bucket{le="0.2"} 100392
http_request_duration_seconds_bucket{le="0.5"} 129389
http_request_duration_seconds_bucket{le="1"} 133988
http_request_duration_seconds_bucket{le="+Inf"} 144320
http_request_duration_seconds_sum 53423
http_request_duration_seconds_count 144320

# Finally a summary, which has a complex representation, too:
# HELP rpc_duration_seconds A summary of the RPC duration in seconds.
# TYPE rpc_duration_seconds summary
rpc_duration_seconds{quantile="0.01"} 3102
rpc_duration_seconds{quantile="0.05"} 3272
rpc_duration_seconds{quantile="0.5"} 4773
rpc_duration_seconds{quantile="0.9"} 9001
rpc_duration_seconds{quantile="0.99"} 76656
rpc_duration_seconds_sum 1.7560473e+07
rpc_duration_seconds_count 2693
```

需要特别注意的是，假设采样数据 metric 叫做 `x`, 如果 `x` 是 `histogram` 或 `summary` 类型必需满足以下条件：

- 采样数据的总和应表示为 `x_sum`。
- 采样数据的总量应表示为 `x_count`。
- `summary` 类型的采样数据的 quantile 应表示为 `x{quantile="y"}`。
- `histogram` 类型的采样分区统计数据将表示为 `x_bucket{le="y"}`。
- `histogram` 类型的采样必须包含 `x_bucket{le="+Inf"}`, 它的值等于 `x_count` 的值。
- `summary` 和 `historam` 中 `quantile` 和 `le` 必需按从小到大顺序排列。
