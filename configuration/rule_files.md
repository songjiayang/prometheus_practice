# 规则配置

`rule_files` 主要用于配置 rules 文件，它支持多个文件以及文件目录。

其代码结构定义为：

```
RuleFiles      []string        `yaml:"rule_files,omitempty"`
```

配置文件结构大致为：

```
rule_files:
  - "rules/node.rules"
  - "rules2/*.rules"
```
