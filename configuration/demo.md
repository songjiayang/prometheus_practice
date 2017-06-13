# 配置样例

Prometheus 的配置参数比较多，但是个人使用较多的是 global, rules, scrap_configs, statstic_config, rebel_config 等。

我平时使用的配置文件大致为这样：

```
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.
  evaluation_interval: 15s # By default, scrape targets every 15 seconds.

rule_files:
  - "rules/node.rules"

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node'
    scrape_interval: 8s
    static_configs:
      - targets: ['127.0.0.1:9100', '127.0.0.12:9100']

  - job_name: 'mysqld'
    static_configs:
      - targets: ['127.0.0.1:9104']
  - job_name: 'memcached'
    static_configs:
      - targets: ['127.0.0.1:9150']
```
