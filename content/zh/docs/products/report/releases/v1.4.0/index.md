---
title: "MOSN v1.4.0 发布"
linkTitle: "MOSN v1.4.0 发布"
date: 2023-2-28
author: "MOSN 团队"
aliases: "/zh/blog/releases/v1.4.0"
description: "MOSN v1.4.0 变更日志。"
---

我们很高兴的宣布 [MOSN v1.4.0](https://github.com/mosn/mosn/releases/tag/v1.4.0) 发布，以下是该版本的变更日志。

## v1.4.0

### 新功能

- 支持记录 HTTP 健康检查日志 (#2096) @dengqian
- 新增最小连接负载均衡器 (#2184) @dengqian
- 新增 API: 强制断开并重新连接 ADS 服务 (#2183) @dengqian
- 支持 pprof debug server 配置 endpoint (#2202) @dengqian
- 集成 mosn.io/envoy-go-extension，参考[文档](https://github.com/mosn/mosn/blob/master/examples/codes/envoy-go-extension/README_CN.md) (#2200) @antJack (#2222) @3062
- 新增 API: 支持覆盖注册 Variable (mosn/pkg#72) @antJack
- 新增记录 mosn 处理时间的字段的变量 (#2235) @z2z23n0
- 支持将 cluster idle_timeout 设置为零以表示从不超时 (#2197) @antJack

### 重构
- import pprof 迁移至 pkg/mosn (#2216) @3062

### 优化

- 减少 proxywasm 基准测试的日志记录 (#2189) @Crypt Keeper

### Bug 修复

- 增大 UDP DNS 解析缓冲区 (#2201) @dengqian
- 修复平滑升级时未继承 debug server 的问题 (#2204) @dengqian
- 修复平滑升级失败时，新 mosn 会删除 reconfig.sock 的问题 (#2205) @dengqian
- 修复 HTTP 健康检查 URL query string 未转译的问题 (#2207) @dengqian
- 修复 ReqRoundRobin 负载均衡器在索引超过 host 数量时，host 选择失败的问题 (#2209) @dengqian
- 修复 RDS 创建路由之后，已建连的连接无法找到路由的问题 (#2199) @dengqian (#2210) @3062
- 修复 Variable.Set 执行后会读取到旧缓存值的问题 (mosn/pkg#73) @antJack
- 修复 DefaultRoller 未设置时间导致 panic 的问题 (mosn/pkg#74) @dengqian
- 提前 metrics 初始化时间使其适用于 static config (#2221) @dengqian
- 修复多个 health checker 共用 rander 导致的并发问题 (#2228) @dengqian
- 设置 HTTP/1.1 作为发往上游的 HTTP 协议 (#2225) @dengqian
- 补全缺失的统计信息 (#2215) @3062
