# PromQL 基本使用

PromQL (Prometheus Query Language) 是 Prometheus 自己开发的数据查询 DSL 语言，语言表现力非常丰富，内置函数很多，在日常数据可视化以及rule 告警中都会使用到它。

在页面 `http://localhost:9090/graph` 中，输入下面的查询语句，查看结果，例如：

```
http_requests_total{code="200"}
```

## 字符串和数字

**字符串**: 在查询语句中，字符串往往作为查询条件 labels 的值，和 Golang 字符串语法一致，可以使用 `""`, `''`, 或者 ` `` `, 格式如：

```
"this is a string"
'these are unescaped: \n \\ \t'
`these are not unescaped: \n ' " \t`
```

**正数，浮点数**: 表达式中可以使用正数或浮点数，例如：

```
3
-2.4
```

## 查询结果类型

PromQL 查询结果主要有 3 种类型：

* 瞬时数据 (Instant vector): 包含一组时序，每个时序只有一个点，例如：`http_requests_total`
* 区间数据 (Range vector): 包含一组时序，每个时序有多个点，例如：`http_requests_total[5m]`
* 纯量数据 (Scalar): 纯量只有一个数字，没有时序，例如：`count(http_requests_total)`

## 查询条件

Prometheus 存储的是时序数据，而它的时序是由名字和一组标签构成的，其实名字也可以写出标签的形式，例如 `http_requests_total` 等价于 {__name__="http_requests_total"}。

一个简单的查询相当于是对各种标签的筛选，例如：

```
http_requests_total{code="200"} // 表示查询名字为 http_requests_total，code 为 "200" 的数据
```

查询条件支持正则匹配，例如：

```
http_requests_total{code!="200"}  // 表示查询 code 不为 "200" 的数据
http_requests_total{code=～"2.."} // 表示查询 code 为 "2xx" 的数据
http_requests_total{code!～"2.."} // 表示查询 code 不为 "2xx" 的数据
```

## 操作符

Prometheus 查询语句中，支持常见的各种表达式操作符，例如

**算术运算符**:

支持的算术运算符有 `+，-，*，/，%，^`, 例如 `http_requests_total * 2` 表示将 http_requests_total 所有数据 double 一倍。

**比较运算符**:

支持的比较运算符有 `==，!=，>，<，>=，<=`, 例如 `http_requests_total > 100` 表示 http_requests_total 结果中大于 100 的数据。

**逻辑运算符**:

支持的逻辑运算符有 `and，or，unless`, 例如 `http_requests_total == 5 or http_requests_total == 2` 表示 http_requests_total 结果中等于 5 或者 2 的数据。

**聚合运算符**:

支持的聚合运算符有 `sum，min，max，avg，stddev，stdvar，count，count_values，bottomk，topk，quantile，`, 例如 `max(http_requests_total)` 表示 http_requests_total 结果中最大的数据。

注意，和四则运算类型，Prometheus 的运算符也有优先级，它们遵从（^）> (*, /, %) > (+, -) > (==, !=, <=, <, >=, >) > (and, unless) > (or) 的原则。

## 内置函数

Prometheus 内置不少函数，方便查询以及数据格式化，例如将结果由浮点数转为整数的 floor 和 ceil，

```
floor(avg(http_requests_total{code="200"}))
ceil(avg(http_requests_total{code="200"}))
```

查看 http_requests_total 5分钟内，平均每秒数据

```
rate(http_requests_total[5m])
```

更多请参见[详情](https://prometheus.io/docs/querying/functions/)。
