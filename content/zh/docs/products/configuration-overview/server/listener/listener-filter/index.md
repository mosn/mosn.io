---
title: "ListenerFilter"
linkTitle: "ListenerFilter"
date: 2022-10-28
weight: 1
aliases: "/zh/docs/configuration/server/listener/ListenerFilter"
description: >
  MOSN 的 ListenerFilter 配置说明。
---

本文描述的是 MOSN 的 ListenerFilter 配置。

ListenerFilter 主要用于 listener 透明代理配置。

目前 MOSN 一个 Listener 只支持一个 ListenerFilter。

ListenerFilter 的配置结构如下所示。

```json
{
  "type":"",
  "fallback_to_local":""
}
```

### type

- 透明代理类型，当前版本下此处 type 需要与 listener 中的 use_original_dst 配置一致。

### fallback_to_local

- bool 类型，用于标记匹配本地监听的 ip 地址，`true` 使用 127.0.0.1，`false` 使用 0.0.0.0，当所有 listener 的 ip 都不匹配代理到的请求时将尝试使用监听本地 ip 的 listener 处理。
