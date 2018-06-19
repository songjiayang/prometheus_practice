# 与 SQL 对比

下面将以 Prometheus server 收集的 `http_requests_total` 时序数据为例子展开对比。

## MySQL 数据准备

```
mysql>
# 创建数据库
create database prometheus_practice;
use prometheus_practice;

# 创建 http_requests_total 表
CREATE TABLE http_requests_total (
    code VARCHAR(256),
    handler VARCHAR(256),
    instance VARCHAR(256),
    job VARCHAR(256),
    method VARCHAR(256),
    created_at DOUBLE NOT NULL,
    value DOUBLE NOT NULL) ENGINE=InnoDB DEFAULT CHARSET=utf8;

ALTER TABLE http_requests_total ADD INDEX created_at_index (created_at);

# 初始化数据
# time at 2017/5/22 14:45:27
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "query_range", "localhost:9090", "prometheus", "get", 1495435527, 3);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("400", "query_range", "localhost:9090", "prometheus", "get", 1495435527, 5);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "prometheus", "localhost:9090", "prometheus", "get", 1495435527, 6418);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "static", "localhost:9090", "prometheus", "get", 1495435527, 9);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("304", "static", "localhost:9090", "prometheus", "get", 1495435527, 19);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "query", "localhost:9090", "prometheus", "get", 1495435527, 87);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("400", "query", "localhost:9090", "prometheus", "get", 1495435527, 26);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "graph", "localhost:9090", "prometheus", "get", 1495435527, 7);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "label_values", "localhost:9090", "prometheus", "get", 1495435527, 7);

# time at 2017/5/22 14:48:27
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "query_range", "localhost:9090", "prometheus", "get", 1495435707, 3);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("400", "query_range", "localhost:9090", "prometheus", "get", 1495435707, 5);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "prometheus", "localhost:9090", "prometheus", "get", 1495435707, 6418);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "static", "localhost:9090", "prometheus", "get", 1495435707, 9);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("304", "static", "localhost:9090", "prometheus", "get", 1495435707, 19);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "query", "localhost:9090", "prometheus", "get", 1495435707, 87);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("400", "query", "localhost:9090", "prometheus", "get", 1495435707, 26);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "graph", "localhost:9090", "prometheus", "get", 1495435707, 7);
INSERT INTO http_requests_total (code, handler, instance, job, method, created_at, value) values ("200", "label_values", "localhost:9090", "prometheus", "get", 1495435707, 7);
```

数据初始完成后，通过查询可以看到如下数据：

```
mysql>
mysql> select * from http_requests_total;
+------+--------------+----------------+------------+--------+------------+-------+
| code | handler      | instance       | job        | method | created_at | value |
+------+--------------+----------------+------------+--------+------------+-------+
| 200  | query_range  | localhost:9090 | prometheus | get    | 1495435527 |     3 |
| 400  | query_range  | localhost:9090 | prometheus | get    | 1495435527 |     5 |
| 200  | prometheus   | localhost:9090 | prometheus | get    | 1495435527 |  6418 |
| 200  | static       | localhost:9090 | prometheus | get    | 1495435527 |     9 |
| 304  | static       | localhost:9090 | prometheus | get    | 1495435527 |    19 |
| 200  | query        | localhost:9090 | prometheus | get    | 1495435527 |    87 |
| 400  | query        | localhost:9090 | prometheus | get    | 1495435527 |    26 |
| 200  | graph        | localhost:9090 | prometheus | get    | 1495435527 |     7 |
| 200  | label_values | localhost:9090 | prometheus | get    | 1495435527 |     7 |
| 200  | query_range  | localhost:9090 | prometheus | get    | 1495435707 |     3 |
| 400  | query_range  | localhost:9090 | prometheus | get    | 1495435707 |     5 |
| 200  | prometheus   | localhost:9090 | prometheus | get    | 1495435707 |  6418 |
| 200  | static       | localhost:9090 | prometheus | get    | 1495435707 |     9 |
| 304  | static       | localhost:9090 | prometheus | get    | 1495435707 |    19 |
| 200  | query        | localhost:9090 | prometheus | get    | 1495435707 |    87 |
| 400  | query        | localhost:9090 | prometheus | get    | 1495435707 |    26 |
| 200  | graph        | localhost:9090 | prometheus | get    | 1495435707 |     7 |
| 200  | label_values | localhost:9090 | prometheus | get    | 1495435707 |     7 |
+------+--------------+----------------+------------+--------+------------+-------+
18 rows in set (0.00 sec)
```

## 基本查询对比

假设当前时间为 2017/5/22 14:48:30

* 查询当前所有数据

```
// PromQL
http_requests_total

// MySQL
SELECT * from http_requests_total WHERE created_at BETWEEN 1495435700 AND 1495435710;
```

我们查询 MySQL 数据的时候，需要将当前时间向前推一定间隔，比如这里的 10s (Prometheus 数据抓取间隔)，这样才能确保查询到数据，而 PromQL 自动帮我们实现了这个逻辑。

* 条件查询

```
// PromQL
http_requests_total{code="200", handler="query"}

// MySQL
SELECT * from http_requests_total WHERE code="200" AND handler="query" AND created_at BETWEEN 1495435700 AND 1495435710;
```

* 模糊查询: code 为 2xx 的数据

```
// PromQL
http_requests_total{code~="2xx"}

// MySQL
SELECT * from http_requests_total WHERE code LIKE "%2%" AND created_at BETWEEN 1495435700 AND 1495435710;
```

* 比较查询: value 大于 100 的数据

```
// PromQL
http_requests_total > 100

// MySQL
SELECT * from http_requests_total WHERE value > 100 AND created_at BETWEEN 1495435700 AND 1495435710;
```

* 范围区间查询: 过去 5 分钟数据

```
// PromQL
http_requests_total[5m]

// MySQL
SELECT * from http_requests_total WHERE created_at BETWEEN 1495435410 AND 1495435710;
```

## 聚合, 统计高级查询

* count 查询: 统计当前记录总数

```
// PromQL
count(http_requests_total)

// MySQL
SELECT COUNT(*) from http_requests_total WHERE created_at BETWEEN 1495435700 AND 1495435710;
```

* sum 查询: 统计当前数据总值

```
// PromQL
sum(http_requests_total)

// MySQL
SELECT SUM(value) from http_requests_total WHERE created_at BETWEEN 1495435700 AND 1495435710;
```

* avg 查询: 统计当前数据平均值

```
// PromQL
avg(http_requests_total)

// MySQL
SELECT AVG(value) from http_requests_total WHERE created_at BETWEEN 1495435700 AND 1495435710;
```

* top 查询: 查询最靠前的 3 个值

```
// PromQL
topk(3, http_requests_total)

// MySQL
SELECT * from http_requests_total WHERE created_at BETWEEN 1495435700 AND 1495435710 ORDER BY value DESC LIMIT 3;
```

* irate 查询，过去 5 分钟平均每秒数值

```
// PromQL
irate(http_requests_total[5m])

// MySQL
SELECT code, handler, instance, job, method, SUM(value)/300 AS value from http_requests_total WHERE created_at BETWEEN 1495435700 AND 1495435710  GROUP BY code, handler, instance, job, method;
```

## 总结

通过以上一些示例可以看出，在常用查询和统计方面，PromQL 比 MySQL 简单和丰富很多，而且查询性能也高不少。
