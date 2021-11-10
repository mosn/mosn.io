---
title: "MOSN v0.25.0 发布"
linkTitle: "MOSN v0.25.0 发布"
date: 2021-10-12
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.25.0"
description: "MOSN v0.25.0 变更日志。"
---


我们很高兴的宣布 [MOSN v0.25.0](https://github.com/mosn/mosn/releases/tag/v0.25.0) 发布，以下是该版本的变更日志。

## v0.25.0

### 新功能

- 路由支持删除请求头指定字段的配置 [@wangfakang](https://github.com/wangfakang)
- WASM 支持 Reload [@zu1k](https://github.com/zu1k)
- 集成 SEATA TCC 模式，支持 HTTP 协议 [@dk-lockdown]((https://github.com/dk-lockdown)
- 新增 boltv2 协议的 tracelog 支持 [@nejisama](https://github.com/nejisama)
- gRPC 框架新增 Metrics 统计相关 Filter 扩展 [@wenxuwan](https://github.com/wenxuwan)
- 新增 xds cluster 解析支持 DNS 相关字段 [@antJack](https://github.com/antJack)

### 重构

- MOSN 核心代码和 Istio 引入相关 xDS 代码解耦合 [@nejisama](https://github.com/nejisama)
- 更新 proxy-wasm-go-host 版本 [@zhenjunMa](https://github.com/zhenjunMa)
- 修改 networkfilter 配置解析逻辑，支持更新添加接口、查询接口 [@antJack](https://github.com/antJack)

### 优化

- Makefile 中执行模式使用`mod vendor`代替`GO111MODULE=off` [@scaat](https://github.com/scaat)
- 转移部分 archived 到 mosn.io/pkg 路径下 [@nejisama](https://github.com/nejisama)
- 优化 EDF 负载均衡：在首次选择时的机器进行随机选择 [@alpha-baby](https://github.com/alpha-baby)
- 提升 EDF 负载均衡函数的性能 [@alpha-baby](https://github.com/alpha-baby)
- 调整 boltv2 心跳请求和心跳响应的处理 [@nejisama](https://github.com/nejisama)
- 优化 HTTP2 在 Stream 模式下的重试处理和 Unary 请求优化 [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- 当通过环境变量设置 GOMAXPROCS 时，无视 CPU 数量的限制 [@wangfakang](https://github.com/wangfakang)
- 优化 subset 创建时的内存使用 [@dzdx](https://github.com/dzdx)
- 优化 gRPC 框架，支持不同的 Listener 可以支持同名 Server 独立运行 [@nejisama](https://github.com/nejisama)

### Bug 修复

- 修复重试时如果返回的机器地址为空会导致卡死的问题 [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- 修复消息连接池处理连接事件的 BUG [@RayneHwang](https://github.com/RayneHwang)
- 修复没有初始化 Trace Driver 时调用 Enable Trace 导致的 panic 问题 [@nejisama](https://github.com/nejisama)
- 修复 boltv2 协议在构造异常响应时数据错误的问题 [@nejisama](https://github.com/nejisama)
- 修复 HTTP2 连接失败时异常处理的问题 [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- typo 错误修复 [@jxd134](https://github.com/jxd134) [@yannsun](https://github.com/yannsun)
- 修复 `RequestInfo` 输出 `ResponseFlag` 的错误 [@wangfakang](https://github.com/wangfakang)
- 修复 bolt/boltv2 协议编码时，在空数据时没有重新计算长度位的问题 [@hui-cha](https://github.com/hui-cha)
