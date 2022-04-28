---
title: "Server 配置"
linkTitle: "Server 配置"
date: 2020-01-20
weight: 2
aliases: "/zh/docs/configuraion/server"
description: 
  MOSN server 配置说明。
---

本文是关于 MOSN server 配置的说明。

虽然 MOSN 的配置结构里 `servers` 是一个 server 数组，但是目前最多只支持配置一个server。

`server` 描述的 MOSN 的基本的全局参数如下所示。

```josn
{
  "mosn_server_name":"",
  "default_log_path":"",
  "default_log_level":"",
  "global_log_roller":"",
  "use_netpoll_mode":"",
  "graceful_timeout":"",
  "optimize_local_write":"",
  "processor":"",
  "listeners":[],
  "routers":[]
}
```

## mosn_server_name

字符串类型，用于设置当前 server 的标识。

## default_log_path

默认的错误日志文件路径，支持配置完整的日志路径，以及标准输出（stdout）和标准错误（stderr）。

- 如果配置为空，则默认输出到标准错误（stderr）。

## default_log_level

默认的错误日志级别，支持`DEBUG`、`INFO`、`WARN`、`ERROR`、`FATAL`。

- 如果配置为空，则默认为 `INFO`。

## global_log_roller

- 日志轮转配置，会对所有的日志生效，如 tracelog、accesslog、defaultlog。
- 字符串配置，支持两种模式的配置，一种是按时间轮转，一种是按日志大小轮转。同时只能有一种模式生效。
- 按照日志大小轮转
  - size， 表示日志达到多少 M 进行轮转。
  - age，表示最多保存多少天的日志。
  - keep，表示最多保存多少个日志。
  - compress，表示日志是否压缩，on 为压缩，off 为不压缩。

```
"global_log_roller": "size=100 age=10 keep=10 compress=off"
```

- 按照时间轮转
  - time，表示每个多少个小时轮转一次。

```
"global_log_roller":"time=1"
```

## use_netpoll_mode

- bool 类型，设置为 true 则表示开启 MOSN 的网络处理采用 netpoll 模型，默认值为 false。

## graceful_timeout

- [Duration String ](../custom#duration-string)的字符串配置，表示 MOSN 在进行平滑升级时，等待连接关闭的最大时间。
- 如果没有配置，默认为 30s。

## optimize_local_write

- bool 类型，设置为 true 则表示当连接的目标地址是 Localhost 时，将使用 goroutine 进行异步写入，这样可以获得更好的性能，但降低了写入时间的准确性，默认值为 false。

### processor

MOSN 使用的 `GOMAXPROCS` 数量
- 如果没有配置，默认为 CPU 数量。
- 如果配置为 0，等价于没有配置。

## listeners

一组 [Listener](./listener) 的配置。

## routers

一组[Router](./router)的配置。
