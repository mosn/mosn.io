---
title: MOSN 源码解析 - log系统
linkTitle: MOSN 源码解析 - log系统
date: 2020-03-01
weight: 1
author: "[@champly](https://github.com/champly)"
description: "对 MOSN Log系统的源码解析。"
---

本文的目的是分析 MOSN 源码中的`Log系统`。

本文的内容基于 MOSN v0.10.0。

## 概述

MOSN日志系统分为`日志`和`Metric`两大部分，其中`日志`主要包括`errorlog`和`accesslog`，`Metric`主要包括`console`和`prometheus`

## 日志

### errorlog

errorlog主要主要是用来记录mosn运行时候的日志信息，[配置文件](https://github.com/mosn/mosn/blob/07cd4afe4d76619fdfbdff858239885f9a358bb2/pkg/config/v2/server.go#L28):

``` golang
type ServerConfig struct {
……
	DefaultLogPath  string `json:"default_log_path,omitempty"`
	DefaultLogLevel string `json:"default_log_level,omitempty"`
	GlobalLogRoller string `json:"global_log_roller,omitempty"`
……
}
```

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
