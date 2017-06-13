# 远程可写存储

`remote_write` 主要用于可写远程存储配置，主要包含以下参数：

- url: 访问地址
- remote_timeout: 请求超时时间
- write_relabel_configs: 标签重置配置, 拉取到的数据，经过重置处理后，发送给远程存储

其代码结构体为：

```
// RemoteWriteConfig is the configuration for writing to remote storage.
type RemoteWriteConfig struct {
	URL                 *URL             `yaml:"url,omitempty"`
	RemoteTimeout       model.Duration   `yaml:"remote_timeout,omitempty"`
	WriteRelabelConfigs []*RelabelConfig `yaml:"write_relabel_configs,omitempty"`

	// We cannot do proper Go type embedding below as the parser will then parse
	// values arbitrarily into the overflow maps of further-down types.
	HTTPClientConfig HTTPClientConfig `yaml:",inline"`

	// Catches all undefined fields and must be empty after parsing.
	XXX map[string]interface{} `yaml:",inline"`
}
```

一份完整的配置大致为:

```
# The URL of the endpoint to send samples to.
url: <string>

# Timeout for requests to the remote write endpoint.
[ remote_timeout: <duration> | default = 30s ]

# List of remote write relabel configurations.
write_relabel_configs:
  [ - <relabel_config> ... ]

# Sets the `Authorization` header on every remote write request with the
# configured username and password.
basic_auth:
  [ username: <string> ]
  [ password: <string> ]

# Sets the `Authorization` header on every remote write request with
# the configured bearer token. It is mutually exclusive with `bearer_token_file`.
[ bearer_token: <string> ]

# Sets the `Authorization` header on every remote write request with the bearer token
# read from the configured file. It is mutually exclusive with `bearer_token`.
[ bearer_token_file: /path/to/bearer/token/file ]

# Configures the remote write request's TLS settings.
tls_config:
  [ <tls_config> ]

# Optional proxy URL.
[ proxy_url: <string> ]
```

注意： remote_write 属于试验阶段，慎用，因为在以后的版本中可能发生改变。
