#通过 Email 接收告警

本章将通过一个简单的实验介绍如何通过 Email 接受告警。

相关信息说明：
- Prometheus 版本：prometheus-1.7.1.darwin-amd64
- Alertmanager 版本：alertmanager-0.8.0.darwin-amd64
- 发送告警邮件的邮箱：qq email

假设该实验运行在本地机器上, Prometheus 默认端口为 9090，Alertmanager 默认端口为 9093。

### 修改 AlertManager 配置文件

其中一些关键配置如下：

```
global:
  smtp_smarthost: 'smtp.qq.com:587'
  smtp_from: 'xxx@qq.com'
  smtp_auth_username: 'xxx@qq.com'
  smtp_auth_password: 'your_email_password'

route：
  # If an alert has successfully been sent, wait 'repeat_interval' to resend them.
  repeat_interval: 10s    
  #  A default receiver
  receiver: team-X-mails  

receivers:
  - name: 'team-X-mails'
    email_configs:
    - to: 'team-X+alerts@example.org'
```

### 在prometheus下添加 alert.rules 文件

文件中写入以下简单规则作为示例。
```
ALERT memory_high
  IF prometheus_local_storage_memory_series >= 0
  FOR 15s
  ANNOTATIONS {
    summary = "Prometheus using more memory than it should {{ $labels.instance }}",
    description = "{{ $labels.instance }} has lots of memory man (current value: {{ $value }}s)",
  }
```

### 修改 prometheus.yml 文件

添加以下规则：
```
rule_files:
  - "alert.rules"
```

### 启动AlertManager服务
```
./Alertmanager -config.file=simple.yml
```

### 启动prometheus服务
```
./prometheus -Alertmanager.url=http://localhost:9093
```

根据以上步骤设置，此时 “team-X+alerts@example.org” 应该就可以收到 “xxx@qq.com” 发送的告警邮件了。
