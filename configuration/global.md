# 全局配置

`global` 属于全局的默认配置，它主要包含 4 个属性，

- scrape_interval: 拉取 targets 的默认时间间隔。
- scrape_timeout: 拉取一个 target 的超时时间。
- evaluation_interval: 执行 rules 的时间间隔。
- external_labels: 额外的属性，会添加到拉取的数据并存到数据库中。


其代码结构体定义为：

```
 // GlobalConfig configures values that are used across other configuration
// objects.
type GlobalConfig struct {
	// How frequently to scrape targets by default.
	ScrapeInterval model.Duration `yaml:"scrape_interval,omitempty"`
	// The default timeout when scraping targets.
	ScrapeTimeout model.Duration `yaml:"scrape_timeout,omitempty"`
	// How frequently to evaluate rules by default.
	EvaluationInterval model.Duration `yaml:"evaluation_interval,omitempty"`
	// The labels to add to any timeseries that this Prometheus instance scrapes.
	ExternalLabels model.LabelSet `yaml:"external_labels,omitempty"`

	// Catches all undefined fields and must be empty after parsing.
	XXX map[string]interface{} `yaml:",inline"`
}
```

配置文件结构大概为：

```
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.
  evaluation_interval: 15s # By default, scrape targets every 15 seconds.
  scrape_timeout: 10s # is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'
```
