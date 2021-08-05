---
title: "MOSN v0.22.0 发布"
linkTitle: "MOSN v0.22.0 发布"
date: 2021-04-02
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.22.0"
description: "MOSN v0.22.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.22.0](https://github.com/mosn/mosn/releases/tag/v0.22.0) 发布

以下是该版本的变更日志。

## v0.22.0

### 新功能

- 新增 Wasm 扩展框架 [@antJack](https://github.com/antJack)
- XProtocol 协议新增 x-bolt 子协议，支持基于 Wasm 的协议编解码能力 [@zonghaishang](https://github.com/zonghaishang)
- 支持自动协议识别失败时根据 SO_ORIGINAL_DST 进行自动转发报文的能力 [@antJack](https://github.com/antJack)
- XProtocol 支持 Go Plugin 模式扩展 [@fdingiit](https://github.com/fdingiit)
- 新增网络扩展层 [@wangfakang](https://github.com/wangfakang)
- 支持 Istio xDS v3 API [@champly](https://github.com/champly) 所属分支: [istio-1.7.7](https://github.com/mosn/mosn/tree/istio-1.7.7)

### 优化

- 去除 StreamFilter 配置解析中多余的路径清洗 [@eliasyaoyc](https://github.com/eliasyaoyc)
- 支持为 StreamFilterChain 设置统一的回调接口 [@antJack](https://github.com/antJack)
- FeatureGate 支持不同启动阶段执行, 去除 FeatureGate 状态判断的全局锁 [@nejisama](https://github.com/nejisama)
- Http2 模块新增对 trace 能力的支持 [@OrezzerO](https://github.com/OrezzerO)


### 重构

- 新增 StageManager，将 MOSN 启动流程划分为四个可自定义的阶段 [@nejisama](https://github.com/nejisama)
- 统一 XProtocol 模块的类型定义，移动至 mosn.io/api 包 [@fdingiit](https://github.com/fdingiit)
- XProtocol 接口新增 GetTimeout 方法，取代原有的变量获取方式 [@nejisama](https://github.com/nejisama)


### Bug修复

- 修复 Proxy 中请求信息的并发冲突问题 [@nejisama](https://github.com/nejisama)
- 修复 URL 处理时的安全漏洞 [@antJack](https://github.com/antJack)
- 修复配置持久化时 Router 配置的并发冲突问题 [@nejisama](https://github.com/nejisama)
