# 告警配置

通常我们可以使用运行参数 `-alertmanager.xxx` 来配置 Alertmanager， 但是这样不够灵活，没有办法做到动态更新加载，以及动态定义告警属性。

所以 `alerting` 配置主要用来解决这个问题，它能够更好的管理 Alertmanager, 主要包含 2 个参数：

- alert_relabel_configs: 动态修改 alert 属性的规则配置。
- alertmanagers: 用于动态发现 Alertmanager 的配置。

其代码结构体定义为：

```
// AlertingConfig configures alerting and alertmanager related configs.
type AlertingConfig struct {
	AlertRelabelConfigs []*RelabelConfig      `yaml:"alert_relabel_configs,omitempty"`
	AlertmanagerConfigs []*AlertmanagerConfig `yaml:"alertmanagers,omitempty"`

	// Catches all undefined fields and must be empty after parsing.
	XXX map[string]interface{} `yaml:",inline"`
}
```

配置文件结构大概为：

```
# Alerting specifies settings related to the Alertmanager.
alerting:
  alert_relabel_configs:
    [ - <relabel_config> ... ]
  alertmanagers:
    [ - <alertmanager_config> ... ]
```

其中 alertmanagers 为 alertmanager_config 数组，而 alertmanager_config 的代码结构体为,

```
// AlertmanagerConfig configures how Alertmanagers can be discovered and communicated with.
type AlertmanagerConfig struct {
	// We cannot do proper Go type embedding below as the parser will then parse
	// values arbitrarily into the overflow maps of further-down types.

	ServiceDiscoveryConfig ServiceDiscoveryConfig `yaml:",inline"`
	HTTPClientConfig       HTTPClientConfig       `yaml:",inline"`

	// The URL scheme to use when talking to Alertmanagers.
	Scheme string `yaml:"scheme,omitempty"`
	// Path prefix to add in front of the push endpoint path.
	PathPrefix string `yaml:"path_prefix,omitempty"`
	// The timeout used when sending alerts.
	Timeout time.Duration `yaml:"timeout,omitempty"`

	// List of Alertmanager relabel configurations.
	RelabelConfigs []*RelabelConfig `yaml:"relabel_configs,omitempty"`

	// Catches all undefined fields and must be empty after parsing.
	XXX map[string]interface{} `yaml:",inline"`
}
```

配置文件结构大概为:

```
# Per-target Alertmanager timeout when pushing alerts.
[ timeout: <duration> | default = 10s ]

# Prefix for the HTTP path alerts are pushed to.
[ path_prefix: <path> | default = / ]

# Configures the protocol scheme used for requests.
[ scheme: <scheme> | default = http ]

# Sets the `Authorization` header on every request with the
# configured username and password.
basic_auth:
  [ username: <string> ]
  [ password: <string> ]

# Sets the `Authorization` header on every request with
# the configured bearer token. It is mutually exclusive with `bearer_token_file`.
[ bearer_token: <string> ]

# Sets the `Authorization` header on every request with the bearer token
# read from the configured file. It is mutually exclusive with `bearer_token`.
[ bearer_token_file: /path/to/bearer/token/file ]

# Configures the scrape request's TLS settings.
tls_config:
  [ <tls_config> ]

# Optional proxy URL.
[ proxy_url: <string> ]

# List of Azure service discovery configurations.
azure_sd_configs:
  [ - <azure_sd_config> ... ]

# List of Consul service discovery configurations.
consul_sd_configs:
  [ - <consul_sd_config> ... ]

# List of DNS service discovery configurations.
dns_sd_configs:
  [ - <dns_sd_config> ... ]

# List of EC2 service discovery configurations.
ec2_sd_configs:
  [ - <ec2_sd_config> ... ]

# List of file service discovery configurations.
file_sd_configs:
  [ - <file_sd_config> ... ]

# List of GCE service discovery configurations.
gce_sd_configs:
  [ - <gce_sd_config> ... ]

# List of Kubernetes service discovery configurations.
kubernetes_sd_configs:
  [ - <kubernetes_sd_config> ... ]

# List of Marathon service discovery configurations.
marathon_sd_configs:
  [ - <marathon_sd_config> ... ]

# List of AirBnB's Nerve service discovery configurations.
nerve_sd_configs:
  [ - <nerve_sd_config> ... ]

# List of Zookeeper Serverset service discovery configurations.
serverset_sd_configs:
  [ - <serverset_sd_config> ... ]

# List of Triton service discovery configurations.
triton_sd_configs:
  [ - <triton_sd_config> ... ]

# List of labeled statically configured Alertmanagers.
static_configs:
  [ - <static_config> ... ]

# List of Alertmanager relabel configurations.
relabel_configs:
  [ - <relabel_config> ... ]
```
