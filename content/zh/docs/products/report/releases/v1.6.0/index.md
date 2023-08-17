---
title: "MOSN v1.6.0 发布"
linkTitle: "MOSN v1.6.0 发布"
date: 2023-8-17
author: "MOSN 团队"
aliases: "/zh/blog/releases/v1.6.0"
description: "MOSN v1.6.0 变更日志。"
---

我们很高兴的宣布 [MOSN v1.6.0](https://github.com/mosn/mosn/releases/tag/v1.6.0) 发布，以下是该版本的变更日志。

## v1.6.0

### 新功能

- PeakEWMA 支持配置 activeRequestBias (#2301) @jizhuozhi
- gRPC filter 支持 UDS (#2309) @wenxuwan
- 支持初始化热升级时 config 继承函数 (#2241) @dengqian
- 允许自定义 proxy defaultRouteHandlerName (#2308) @fibbery

### 变更

- 示例 http-sample README 增加配置文件链接 (#2226) @mimani68
- 将 wazero 更新到 1.2.1 (#2254) @codefromthecrypt
- 更新依赖 (#2230) (#2233) (#2247) (#2302) (#2326) (#2332) (#2333) @dependabot

### 重构

- 重构调试日志内容，打印 data 移至 tracef 中 (#2316) @antJack

### 优化

- EWMA 优化新添加的主机的默认值 (#2301) @jizhuozhi
- PeakEwma LB 不再统计错误响应 (#2323) @jizhuozhi

### Bug 修复

- 修复 edfScheduler 在动态负载算法中错误地将权重固定为1 (#2306) @jizhuozhi
- 修复 cluster hosts 顺序改变导致的 LB 行为不稳定 (#2258) @dengqian
- 修复 NilMetrics 缺少 EWMA() 方法导致的 panic (#2310) @antJack (#2312) @jizhuozhi
- 修复 xDS 更新时，cluster hosts 为空导致的 panic (#2314) @dengqian
- 修复 MOSN 异常退出时 UDS 套接字文件未删除导致重启失败 (#2318) @wenxuwan
- 修复 xDS 状态码未转换错误。修复未处理 istio inbound IPv6 地址错误 (#2144) @kkrrsq
- 修复非热升级优雅退出时 Listener 未直接关闭导致新建连接报错 (#2234) @hui-cha
- 修复 goimports lint 错误 (#2313) @spacewander