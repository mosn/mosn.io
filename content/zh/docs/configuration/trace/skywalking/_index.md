---
title: "SkyWalking 配置"
linkTitle: "SkyWalking 配置"
date: 2020-04-08
weight: 1
description: 
  MOSN SkyWalking trace 配置说明。
---

本文描述的是 SkyWalking Trace  配置。

目前支持 `HTTP1` 协议追踪。

SkyWalking 描述的 MOSN 的基本全局参数如下所示。

```json
{
  "tracing": {
    "enable": true,
    "driver": "SkyWalking",
    "tracer": "SkyWalking",
    "config": {
      "reporter": "gRPC",
      "backend_service": "127.0.0.1:11800",
      "service_name": "mosn",
      "with_register": true
    }
  }
}
```

## reporter

trace 数据上报模式， 支持 `log`（仅用于测试） 和 `gRPC` 两种模式 <br>
- 如果配置为空，则默认为 `log`。

## backend_service

SkyWalking 后端服务地址，仅在上报模式为 `gRPC` 模式时使用。<br>
- 示例：`127.0.0.1:11800`。

## service_name

注册到 SkyWalking 的服务名称。<br>
- 如果配置为空，则默认为 `mosn`。

## with_register

bool类型，当此值为true时，会阻塞协程等待当前实例注册到 SkyWalking 。<br>
- 如果配置为空，则默认为 `true`。
