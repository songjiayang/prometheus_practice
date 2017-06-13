# 远程可读存储

`remote_read` 主要用于可读远程存储配置，主要包含以下参数：

- url: 访问地址
- remote_timeout: 请求超时时间

其代码结构体为：

```
// RemoteReadConfig is the configuration for reading from remote storage.
type RemoteReadConfig struct {
	URL           *URL           `yaml:"url,omitempty"`
	RemoteTimeout model.Duration `yaml:"remote_timeout,omitempty"`

	// We cannot do proper Go type embedding below as the parser will then parse
	// values arbitrarily into the overflow maps of further-down types.
	HTTPClientConfig HTTPClientConfig `yaml:",inline"`

	// Catches all undefined fields and must be empty after parsing.
	XXX map[string]interface{} `yaml:",inline"`
}
```

一份完整的配置大致为:

```
# The URL of the endpoint to query from.
url: <string>

# Timeout for requests to the remote read endpoint.
[ remote_timeout: <duration> | default = 30s ]

# Sets the `Authorization` header on every remote read request with the
# configured username and password.
basic_auth:
  [ username: <string> ]
  [ password: <string> ]

# Sets the `Authorization` header on every remote read request with
# the configured bearer token. It is mutually exclusive with `bearer_token_file`.
[ bearer_token: <string> ]

# Sets the `Authorization` header on every remote read request with the bearer token
# read from the configured file. It is mutually exclusive with `bearer_token`.
[ bearer_token_file: /path/to/bearer/token/file ]

# Configures the remote read request's TLS settings.
tls_config:
  [ <tls_config> ]

# Optional proxy URL.
[ proxy_url: <string> ]
```

注意： remote_read 属于试验阶段，慎用，因为在以后的版本中可能发生改变。
