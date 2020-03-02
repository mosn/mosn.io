---
title: MOSN 源码解析 - log系统
linkTitle: MOSN 源码解析 - log系统
date: 2020-03-02
weight: 1
author: "[@champly](https://github.com/champly)"
description: "对 MOSN Log系统的源码解析。"
---

本文的目的是分析 MOSN 源码中的`Log系统`。

本文的内容基于 MOSN v0.10.0。

## 概述

MOSN日志系统分为`日志`和`Metric`两大部分，其中`日志`主要包括`errorlog`和`accesslog`，`Metrics`主要包括`console数据`和`prometheus数据`

## 日志

### errorlog

errorlog主要主要是用来记录mosn运行时候的日志信息，[配置对象](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/config/v2/server.go#L28):

``` golang
type ServerConfig struct {
……
	DefaultLogPath  string `json:"default_log_path,omitempty"`
	DefaultLogLevel string `json:"default_log_level,omitempty"`
	GlobalLogRoller string `json:"global_log_roller,omitempty"`
……
}
```

初始化errorlog包括两个对象`StartLogger`和`DefaultLogger`

- StartLogger主要用来记录mosn启动的日志信息，日志级别是INFO
- DefaultLogger主要是用来记录启动之后的日志信息，默认和StartLogger一样，可以通过配置文件覆盖

[代码如下：](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/log/logger_manager.go#L37)

``` golang
func init() {
	……
	// use console as start logger
	StartLogger, _ = GetOrCreateDefaultErrorLogger("", log.INFO) // 默认INFO
	// default as start before Init
	log.DefaultLogger = StartLogger
	DefaultLogger = log.DefaultLogger
	// default proxy logger for test, override after config parsed
	log.DefaultContextLogger, _ = CreateDefaultContextLogger("", log.INFO)
	……
}
……
func InitDefaultLogger(output string, level log.Level) (err error) {
	// 使用配置文件来覆盖默认配置
	DefaultLogger, err = GetOrCreateDefaultErrorLogger(output, level)
	……
}
```

### accesslog

accesslog主要用来记录请求的数据信息，[配置文件](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/config/v2/server.go#L76):

``` golang
type AccessLog struct {
	Path   string `json:"log_path,omitempty"`
	Format string `json:"log_format,omitempty"`
}
```

每个配置文件下面servers->listener->access_logs，具体配置示例如下：

``` json
{
	"servers": [
		{
			"mosn_server_name": "mosn_server_1",
			……
			"listeners": [
				{
					"name": "ingress_sofa",
					……
					"log_path": "./logs/ingress.log",
					"log_level": "DEBUG",
					"access_logs": [
						{
							"log_path": "./logs/access_ingress.log",
							"log_format": "%start_time% %request_received_duration% %response_received_duration% %bytes_sent% %bytes_received% %protocol% %response_code% %duration% %response_flag% %response_code% %upstream_local_address% %downstream_local_address% %downstream_remote_address% %upstream_host%"
						}
					]
				}
			]
		}
	]
}
```

accesslog实现如下[接口](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/log/accesslog.go#L105):

``` golang
AccessLog interface {
    // Log write the access info.
    Log(ctx context.Context, reqHeaders HeaderMap, respHeaders HeaderMap, requestInfo RequestInfo)
}
```

调用Log记录日志的时候，通过使用 [变量机制](https://mosn.io/zh/blog/posts/mosn-variable) 来填充`log_format`里面的变量，相关信息保存在ctx里面。用于保存变量信息的 `entries` 通过 `NewAccessLog` 初始化的时候，调用 `parseFormat` 方法来初始化的，[参考相关代码](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/log/accesslog.go#L79)。

## Metrics

`Metrics` 是一种规范的度量，分为如下类型，摘抄至 [METRIC TYPES](https://prometheus.io/docs/concepts/metric_types/)

- Gauges: 代表可以任意上下波动的单个数值，通常用来表示测量值。比如内存，cpu，磁盘等信息。
- Counters: 累计度量，代表单调递增的计数器，只有在重启或者重置的时候数量为0，其他时候一般不使用减少。可以用来表示请求的数量。
- Histograms: 直方图，对观察值(通常是请求持续时间或返回大小之类的数据)进行采样，并将其计数放到对应的配置桶中，也提供所有观测值总和信息。
- Summary: 类似于直方图，摘要采样的观测结果，可以计算滑动时间窗口内的可配置分位数。

### 结构

主要代码在 `pkg/metrics` 下面，其中 `pkg/metrics/sink` 包含 `console` 和 `prometheus`，两者都实现了 [types.MetricsSink](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/types/metrics.go#L58) 接口。`prometheus` 是通过工厂方法 [注册](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/metrics/sink/prometheus/prometheus.go#L57) 进去使用的；`console` 是通过直接调用 `console.NewConsoleSink()` 来使用的。

### prometheus

主要是通过 `prometheus` 的 `metrics` 统计请求的信息，配置文件示例:

``` json
{
	"metrics": {
		……
		"sinks": [
			{
				"type": "prometheus",
				"config": {
					"port": 34903
				}
			}
		]
	}
}
```

*其中 `type` 目前只支持 `prometheus`*

通过 `prometheus` 库提供的 http 能力，使用配置信息启动一个http服务，把 `Metrics` 信息通过 `http://host:port/metrics` 的方式供`prometheus`收集或展示。

### console

主要用于 `admin api` 的 `/api/v1/stats` [展示](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/admin/server/server.go#L45)。所以必须配置 `admin` 相关信息，示例：

``` json
{
	"admin": {
		"address": {
			"socket_address": {
				"address": "0.0.0.0",
				"port_value": 34901
			}
		}
	}
}
```

如果不配置会打印 `no admin config, no admin api served` 告警信息，[参考](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/admin/server/server.go#L59)

`admin api` 中还包括如下接口

- /api/v1/config_dump
- /api/v1/stats
- /api/v1/update_loglevel
- /api/v1/enable_log
- /api/v1/disbale_log
- /api/v1/states
- /api/v1/plugin
- /

其中 `update_loglevel` 用于更新 `errorlog` 日志的输出级别，`enable_log` 和 `disbale_log` 用于启用/禁用 `errorlog` 的输出

## 总结

通过分析 `MOSN` 源码的 `log系统` 模块，不单单是了解了日志部分，从配置，到启动流程，到上下游请求都有所涉及。学习了很多，希望 `MOSN` 越来越完善
