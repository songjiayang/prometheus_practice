#通过 Slack 接收告警

[Slack](https://slack.com) 作为 IM 办公软件，简单好用，在国外用的特别多，那如何用它来接收 Prometheus 的告警信息，让咱们的运维看上去高大上。

### 目的

- 使用 slack 接受消息。
- 消息能够带有 url， 自动跳转到 prometheus 对应 graph 查询页面。
- 能自定义颜色。
- 能够 @ 某人

### 准备工作

已注册了 slack 账号，并创建了一个 #test 频道。

### 配置步骤

Step1: 为 #test 频道创建一个 incomming webhooks 应用。

- 点击频道标题，选择 `Add an app or integration`

![Add an app or integration](/images/alertmanager/slack-alert1.png)

- 在 app store 中搜索 `incomming webhooks`，选择第一个

![incomming webhooks](/images/alertmanager/slack-alert2.png)

创建成功以后，拷贝 app webhook 地址，后面会用到。

Step2: 修改 prometheus rules，在 ANNOTATIONS 中添加特定字段。

v1.x rule 写法：

```
ALERT InstanceStatus
 IF up {job="node"}== 0
 FOR 15s
 LABELS {
   instance = "",
 }
 ANNOTATIONS {
   summary = "服务器运行状态",
   description = 服务器已当机超过多少时间",
   link="http://xxx",
   color="#xxx",
   username="@sjy"
 }   
```

v2.x alert rule 写法：

```
- alert: InstanceStatus
  expr: up {job="node"} == 0
  for: 15s
  labels:
    instance: ""
  annotations:
    summary: "服务器运行状态"
    description: 服务器已当机超过多少时间"
    link: "http://xxx"
    color: "#xxx"
    username: "@sjy"
```

说明：我们在 rule 的 ANNOTATIONS 中添加了 link, color, username  三个字段，用它们来表示 `消息外链地址`，`消息颜色`和需要 `@` 的人。

Step3: 修改 Alertmanager 配置。

使用 `slack_configs` 来配置 slack 的告警接收渠道：

```

receivers:
  - name: 'slack'
    slack_configs:
      - api_url: "xxx"
        channel: "#test"
        text: "{{ range .Alerts }} {{ .Annotations.description}}\n {{end}} {{ .CommonAnnotations.username}} <{{.CommonAnnotations.link}}| click here>"
        title: "{{.CommonAnnotations.summary}}"
        title_link: "{{.CommonAnnotations.link}}"
        color: "{{.CommonAnnotations.color}}"

```

配置说明：

- 按 alertname 分组。
- slack_configs 配置中，使用了 template 语句，通过 CommonAnnotations 查找字段。
- 插入外链不仅可以使用 title_link, 还可以使用 slack link 标记语法 <htttpxxxxxx| Click here>。

更多 slack 配置，请参考 [incoming-webhooks](https://api.slack.com/incoming-webhooks)。

经过以上配置，我们收到的消息是这样：

![slack.alert.demo](/images/alertmanager/slack-alert5.png)

点击 title 或者 Click here，即可跳转到 Prometheus graph 页面：

![prometheus graph](/images/alertmanager/slack-alert6.png)

这样就很方便了，再也不用担心多个 Prometheus 节点，切换查询带来的烦恼。
