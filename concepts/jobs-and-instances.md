# 作业和实例

Prometheus 中，将任意一个独立的数据源（target）称之为实例（instance），相同类型实例的集合被称为作业（job）。

如下是一个包含四个实例的作业：

```
- job: api-server
    - instance 1: 1.2.3.4:5670
    - instance 2: 1.2.3.4:5671
    - instance 3: 5.6.7.8:5670
    - instance 4: 5.6.7.8:5671
```

## 自动生成标签和时序

Prometheus 在采集数据的同时，会自动在时序的基础上添加一些标签，作为数据源（target）的标识，以便用于区分：

```
job: The configured job name that the target belongs to.
instance: The <host>:<port> part of the target's URL that was scraped.
```

如果其中任一标签已经在此前采集的数据中存在，那么将会根据 `honor_labels` 设置选项来决定是否进行覆盖。详见官网解释： [scrape configuration documentation](https://prometheus.io/docs/operating/configuration/#%3Cscrape_config%3E)

Prometheus 也提供了针对每个实例抓取的一些指标信息：

```
up{job="<job-name>", instance="<instance-id>"}: 1 表示该实例正常工作
up{job="<job-name>", instance="<instance-id>"}: 0 表示该实例故障

scrape_duration_seconds{job="<job-name>", instance="<instance-id>"} 表示拉取数据的时间间隔

scrape_samples_post_metric_relabeling{job="<job-name>", instance="<instance-id>"} 表示采用重定义标签（relabeling）操作后仍然剩余的样本数

scrape_samples_scraped{job="<job-name>", instance="<instance-id>"}  表示从该数据源获取的样本数
```

其中 `up` 可以用来检测实例是否正常工作，值为 1 表示 Prometheus 抓取正常，从而反映被抓取的实例工作健康；反之表示实例不能正常抓取，很可能实例已经 panic。


