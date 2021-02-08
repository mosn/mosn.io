---
title: "MOSN v0.19.0 发布"
linkTitle: "MOSN v0.19.0 发布"
date: 2020-12-01
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.19.0"
description: "MOSN v0.19.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.19.0](https://github.com/mosn/mosn/releases/tag/v0.19.0) 发布。

以下是该版本的变更日志。

## v0.19.0

### 优化

- 使用最新的 TLS 内存优化方案 [@cch123](https://github.com/cch123)
- proxy log 优化，减少内存逃逸 [@taoyuanyuan](https://github.com/taoyuanyuan)
- 增加最大连接数限制 [@champly](https://github.com/champly)
- AccessLog 获取变量失败时，使用”-”代替 [@champly](https://github.com/champly)
- MaxProcs 支持配置基于 CPU 使用限制自动识别 [@champly](https://github.com/champly)
- 支持指定 Istio cluster 的网络 [@champly](https://github.com/champly)

### 重构

- 重构了 StreamFilter 框架，减少 streamfilter 框架与 proxy 的耦合，支持其他 network filter 可复用 stream filter 框架 [@antJack](https://github.com/antJack)

### Bug 修复

- 修复 HTTP Trace 获取 URL 错误 [@wzshiming](https://github.com/wzshiming)
- 修复 xds 配置解析时没有解析连接超时的错误 [@dengqian](https://github.com/dengqian)
- 修复变量获取 Hostname 的错误 [@dengqian](https://github.com/dengqian)
- 修复 tcp proxy 没有正确关闭连接的错误 [@dengqian](https://github.com/dengqian)
- 修复 mixer filter 缺少默认配置，导致空指针问题 [@glyasai](https://github.com/glyasai)
- 修复 HTTP2 直接响应没有正确地设置 `Content-length` 的问题 [@wangfakang](https://github.com/wangfakang)
- 修复 getAPISourceEndpoint 方法空指针问题 [@dylandee](https://github.com/dylandee)
- 修复 Write 堆积时，过多的 Timer 申请导致内存上涨的问题 [@champly](https://github.com/champly)
- 修复 Dubbo Filter 收到非法响应时，stats 统计缺失的问题 [@champly](https://github.com/champly)
