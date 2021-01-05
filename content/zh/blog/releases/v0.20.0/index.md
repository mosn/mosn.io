---
title: "MOSN v0.20.0 发布"
linkTitle: "MOSN v0.20.0 发布"
date: 2021-01-05
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.20.0"
description: "MOSN v0.20.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.20.0](https://github.com/mosn/mosn/releases/tag/v0.20.0) 发布，恭喜
黄润豪（[@GLYASAI](https://github.com/GLYASAI)）成为 MOSN Committer，感谢他为 MOSN 社区所做的贡献。

以下是该版本的变更日志。

## v0.20.0

### 优化

- 优化 TCP 地址解析失败默认解析 UDS 地址的问题，地址解析前添加前缀判断 [@wangfakang](https://github.com/wangfakang)
- 优化连接池获取的尝试间隔 [@nejisama](https://github.com/nejisama)
- 支持通过全局配置关闭循环写模式 [@nejisama](https://github.com/nejisama)
- 优化协议自动识别的配置示例和测试用例 [@taoyuanyuan](https://github.com/taoyuanyuan)
- 用更高效的变量机制替换请求头 [@CodingSinger](https://github.com/CodingSinger)
- 将 WriteBufferChan 的定时器池化以降低负载 [@cch123](https://github.com/cch123)
- TraceLog 中新增 MOSN 处理失败的信息 [@nejisama](https://github.com/nejisama)
- HTTP协议处理中，新增读完成channel [@alpha-baby](https://github.com/alpha-baby)
- 日志轮转功能加强 [@nejisama](https://github.com/nejisama)

### 重构

- 使用的 Go 版本升级到 1.14.13 [@nejisama](https://github.com/nejisama)
- 将路由链扩展方式修改为路由Handler扩展方式，支持配置不同的路由Handler [@nejisama](https://github.com/nejisama)
- MOSN 扩展配置修改，支持按照配置顺序进行解析 [@nejisama](https://github.com/nejisama)

### Bug 修复

- 修复 doubbo 版本升级至 2.7.3 之后 Provider 不可用的问题 [@cadeeper](https://github.com/cadeeper)
- 修复 netpoll 模式下，错误将UDS连接处理成TCP连接的问题 [@wangfakang](https://github.com/wangfakang)
- 修复 HTTP Header 被设置为空字符串时无法正确 Get 的问题 [@ianwoolf](https://github.com/ianwoolf)

### 新功能

- 支持新旧 MOSN 之间通过 UDS 转移配置，解决 MOSN 使用 XDS 获取配置无法平滑升级的问题 [@alpha-baby](https://github.com/alpha-baby)
- 协议自动识别支持 XProtocol [@cadeeper](https://github.com/cadeeper)
- 支持配置 XProtocol 的 keepalive 参数 [@cch123](https://github.com/cch123)
- 支持更详细的用时追踪 [@nejisama](https://github.com/nejisama)
- 支持度量指标懒加载的方式，以解决服务数目过多 metrics 空间占用过大的问题 [@champly](https://github.com/champly)
- 添加设置 XProtocol 连接池大小默认值的函数 [@cch123](https://github.com/cch123)
- 支持 netpoll 模式 [@cch123](https://github.com/cch123)
- 支持广播功能 [@dengqian](https://github.com/dengqian)
- 支持从 LDS 响应中获取 tls 配置 [@wZH-CN](https://github.com/wZH-CN)
- SDS 新增 ACK response [@wZH-CN](https://github.com/wZH-CN)
