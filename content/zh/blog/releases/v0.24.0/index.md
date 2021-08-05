---
title: "MOSN v0.24.0 发布"
linkTitle: "MOSN v0.24.0 发布"
date: 2021-08-05
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.24.0"
description: "MOSN v0.24.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.24.0](https://github.com/mosn/mosn/releases/tag/v0.24.0) 发布，恭喜付建豪（[@alpha-baby](https://github.com/alpha-baby)）成为 MOSN Committer，感谢他为 MOSN 社区所做的贡献。

以下是该版本的变更日志。

## v0.24.0

### 新功能

- 支持使用 jaeger 收集 OpenTracing 信息 [@Roger](https://github.com/Magiczml)
- 路由配置新增变量配置模式，可通过修改变量的方式修改路由结果 [@wangfakang](https://github.com/wangfakang)
- 路由 virtualhost 匹配支持端口匹配模式 [@jiebin](https://github.com/jiebinzhuang)
- 实现 envoy 中的 filter: [header_to_metadata](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/header_to_metadata_filter) [@antJack](https://github.com/antJack)
- 支持 UDS 的热升级 [@taoyuanyuan](https://github.com/taoyuanyuan)
- 新增 subset 负载均衡逻辑，在没有元数据匹配的场景下使用全量机器列表进行负载均衡 [@nejisama](https://github.com/nejisama)
- MOSN 的 gRPC 框架支持优雅关闭 [@alpha-baby](https://github.com/alpha-baby)

### 优化

- 优化 Cluster 配置更新时的健康检查更新模式 [@alpha-baby](https://github.com/alpha-baby)
- api.Connection 新增 OnConnectionEvent 接口 [@CodingSinger](https://github.com/CodingSinger)
- 权重轮询负载均衡兜底策略调整为普通轮询负载均衡 [@alpha-baby](https://github.com/alpha-baby)
- 在mosn变量模块中增加 interface 值类型 [@antJack](https://github.com/antJack)
- Subset 判断机器个数与是否存在时，同样遵循兜底策略 [@antJack](https://github.com/antJack)

### Bug 修复

- dubbo stream filter 支持协议自动识别 [@Thiswang](https://github.com/Thiswang)
- 修复轮询负载均衡在并发情况下结果异常 [@alpha-baby](https://github.com/alpha-baby)
- 修复 unix 地址解析异常 [@taoyuanyuan](https://github.com/taoyuanyuan)
- 修复 HTTP1 短连接无法生效的异常 [@taoyuanyuan](https://github.com/taoyuanyuan)
- 修复国密 TLS SM3 套件在连接断开后存在的内存泄漏 [@ZengKe](https://github.com/william-zk)
- 当连接被对端重置或管道断裂时 HTTP2 支持重试 [@taoyuanyuan](https://github.com/taoyuanyuan)
- 修复从连接池中获取到的 HOST 信息错误 [@Sharember](https://github.com/Sharember)
- 修复在 route 模块中选择权重集群的数据竞争 [@alpha-baby](https://github.com/alpha-baby)
- 如果 host 不健康时，在Edf负载均衡算法中不能正确返回 [@alpha-baby](https://github.com/alpha-baby)
- 修复 XProtocol 路由配置超时无效的问题 [@nejisama](https://github.com/nejisama)