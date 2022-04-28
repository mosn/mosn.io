
---
title: "MOSN v0.18.0 发布"
linkTitle: "MOSN v0.18.0 发布"
date: 2020-11-02
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.18.0"
description: "MOSN v0.18.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.18.0](https://github.com/mosn/mosn/releases/tag/v0.18.0) 发布。

以下是该版本的变更日志。

## v0.18.0

### 新功能

- 新增 MOSN 配置文件扩展机制 [@nejisama](https://github.com/nejisama)
- 新增 MOSN 配置工具，提升用户配置体验 [mosn/configure](https://github.com/mosn/configure) [@cch123](https://github.com/cch123)

### 优化

- HTTP 协议 stream 处理过程中，避免多次拷贝 HTTP body [@wangfakang](https://github.com/wangfakang)
- 升级了 `github.com/TarsCloud/TarsGo` 包到 v1.1.4 版本 [@champly](https://github.com/champly)
- 补充了连接池的单元测试 [@cch123](https://github.com/cch123)
- 使用内存池减少了 TLS 连接的内存占用 [@cch123](https://github.com/cch123)
- 减少 xprotocol stream 处理过程的临界区大小，提升性能 [@cch123](https://github.com/cch123)
- 删除 `network.NewClientConnection` 方法冗余参数，删除 `streamConn` 结构体 `Dispatch` 方法 `ALPN` 检查 [@nejisama](https://github.com/nejisama)
- `StreamReceiverFilterHandler` 增加 `TerminateStream` API，可在处理流的时候传入 HTTP code 异步关闭流 [@nejisama](https://github.com/nejisama)
- client 端 TLS handshake 失败时增加降级逻辑 [@nejisama](https://github.com/nejisama)
- 修改 TLS hashvalue 计算方式 [@nejisama](https://github.com/nejisama)
- 修正 disable_log admin api typo [@nejisama](https://github.com/nejisama)

### Bug 修复

- 修复执行 `go mod tidy` 失败 [@champly](https://github.com/champly)
- 修复 MOSN 接收 XDS 消息大于 4M 时的 `ResourceExhausted: grpc: received message larger than max` 错误 [@champly](https://github.com/champly) 
- 修复容错单元测试用例 [@wangfakang](https://github.com/wangfakang)
- 修复 `MOSNConfig.servers[].listeners[].bind_port` 设置为 `false` 时热重启出错 [@alpha-baby](https://github.com/alpha-baby)
- 本地写 buffer 增加超时时间，避免本地写失败导致 goroutine 过多 OOM [@cch123](https://github.com/cch123)
- 修复 TLS 超时导致死循环 [@nejisama](https://github.com/nejisama)
- 修复 `dubbo.Frame` struct 使用 `SetData` 方法之后数据没有被修改的问题 [@lxd5866](https://github.com/lxd5866)
