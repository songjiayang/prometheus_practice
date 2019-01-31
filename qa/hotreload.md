# 如何热加载更新配置?

当 Prometheus 有配置文件修改，我们可以采用 Prometheus 提供的热更新方法实现在不停服务的情况下实现配置文件的重新加载。

热更新加载方法有两种：

1. kill -HUP pid
2. curl -X POST http://IP/-/reload

当你采用以上任一方式执行 reload 成功的时候，将在 promtheus log 中看到如下信息：

![hotreload.png](/images/qa/reload_success.png)

如果因为配置信息填写不正确导致更新失败，将看到类似信息：

```
ERRO[0161] Error reloading config: couldn't load configuration (-config.file=prometheus.yml): unknown fields in scrape_config: job_nae  source=main.go:146
```

提示：

1. 我个人更倾向于采用 curl -X POST 的方式，因为每次 reload 过后， pid 会改变，使用 kill 方式需要找到当前进程号。
2. 从 2.0 开始，hot reload 功能是默认关闭的，如需开启，需要在启动 Prometheus 的时候，添加 `--web.enable-lifecycle` 参数。


下面我们再来探讨下这两种方式内部实现原理。

第一种：通过 kill 命令的 HUP (hang up) 参数实现:

首先 Prometheus 在 `cmd/promethteus/main.go` 中实现了对进程系统调用监听，如果收到 syscall.SIGHUP 信号，将执行 reloadConfig 函数。

代码类似:

```golang
hup := make(chan os.Signal)
signal.Notify(hup, syscall.SIGHUP)
go func() {
  for {
    select {
    case <-hup:
      if err := reloadConfig(cfg.configFile, reloadables...); err != nil {
        log.Errorf("Error reloading config: %s", err)
      }
    }
  }
}()
```

第二种：通过 web 模块的 `/-/reload` 请求实现:

1. 首先 Prometheus 在 web(web/web.go) 模块中注册了一个 POST 的 http 请求 `/-/reload`, 它的 handler 是 `web.reload` 函数，该函数主要向 `web.reloadCh` chan 里面发送一个 `error`。
2. 在 Prometheus 的 `cmd/promethteus/main.go` 中有个单独的 goroutine 来监听 `web.reloadCh`，当接受到新值的时候会执行 reloadConfig 函数。

代码类似：

```golang
hupReady := make(chan bool)

go func() {
	<-hupReady
	for {
		select {
		case rc := <-webHandler.Reload():
			if err := reloadConfig(cfg.configFile, reloadables...); err != nil {
				log.Errorf("Error reloading config: %s", err)
				rc <- err
			} else {
				rc <- nil
			}
		}
	}
}()
```

Prometheus 内部提供了成熟的 hot reload 方案，这大大方便配置文件的修改和重新加载，在 Prometheus 生态中，很多 Exporter 也采用类似约定的实现方式。