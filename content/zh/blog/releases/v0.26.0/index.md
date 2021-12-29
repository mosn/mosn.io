---
title: "MOSN v0.26.0 发布"
linkTitle: "MOSN v0.26.0 发布"
date: 2021-10-12
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.26.0"
description: "MOSN v0.26.0 变更日志。"
---


我们很高兴的宣布 [MOSN v0.26.0](https://github.com/mosn/mosn/releases/tag/v0.26.0) 发布，以下是该版本的变更日志。

## v0.26.0

### 不兼容变更

为了更自然的添加扩展协议，新版对 XProtocol 进行了重构，XProtocol 不再是一种协议，而是便于协议扩展实现的框架。
扩展协议的实现需要一些调整，具体请见 [XProtocol协议改造适配指南](reports/xprotocol_0.26.0.md)

### 新功能

- 新增 ip_access filter，基于来源 IP 的 ACL 控制器 (#1797) [@Bryce-huang](https://github.com/Bryce-huang)
- 允许 Admin Api 扩展验证方法 (#1834) [@nejisama](https://github.com/nejisama)
- transcoder filter：支持通过配置指定阶段，取代固定的阶段 (#1815) [@YIDWang](https://github.com/YIDWang)
- 为 tls connection 增加 SetConnectionState 方法，在 pkg/mtls/crypto/tls.Conn 中 (#1804) [@antJack](https://github.com/antJack)
- 增加了 after-start 和 after-stop 这两个新的执行阶段，并允许在这两个阶段注册处理函数 [@doujiang24](https://github.com/doujiang24)
- 新增 uds_dir 配置项，用于指定 unix domain socket 的目录 (#1829) [@dengqian](https://github.com/dengqian)
- 支持go plugin加载协议转化插件，并支持动态选择协议转换插件 [@Tanc010](https://github.com/Tanc010)
- 增加更多的 HTTP 协议方法，使动态协议匹配更加精准 (#1870) [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- 支持动态设置上游协议 (#1808) [@YIDWang](https://github.com/YIDWang)
- 支持动态设置 HTTP 默认最大值配置 #1886 [@nejisama](https://github.com/nejisama)

### 变更

- 将 HTTP 协议的默认最大请求头大小调整到 8KB (#1837) [@nejisama](https://github.com/nejisama)
- 重构默认的 HTTP1 和 HTTP2 的协议转换，删除了 proxy 中的转换，使用 transcoder filter 来代替 [@nejisama](https://github.com/nejisama)
- transcoder filter：使用注册转换器工厂来替代注册转换器 (#1879) [@YIDWang](https://github.com/YIDWang)

### Bug 修复

- 修复：HTTP buffer 复用在高并发场景下可能导致 nil panic [@nejisama](https://github.com/nejisama)
- 修复：response_flag 变量值获取错误 (#1814) [@lemonlinger](https://github.com/lemonlinger)
- 修复：prefix_write 在 "/" 的场景下不能正常工作 [@Bryce-huang](https://github.com/Bryce-huang)
- 修复：在热升级过程中，手动 kill 老的 MOSN，可能会导致新 MOSN 的 reconfig.sock 会被错误的删除 (#1820) [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- 修复：请求上游失败时，在 doretry 中不应该直接设置 setupRetry (#1807) [@taoyuanyuan](https://github.com/taoyuanyuan)
- 修复：热升级中继承了老 MOSN 的配置之后，应该将配置设置到新的 MOSN 结构体中 [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- 修复：当取消客户端的 grpc 的时候，没有发送 resetStreamFrame 到上游，使得 server 端没有及时结束 [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- 修复：应该在关闭 stream connection 之前设置 resetReason，否则可能导致获取不到真实的原因 (#1828) [@wangfakang](https://github.com/wangfakang)
- 修复：当有多个匹配的 listener 的时候，应该选择最优的匹配的 listener，否则可能导致 400 错误 [@MengJiapeng](https://github.com/MengJiapeng)
- 修复：HTTP2 协议处理 broadcast 可能导致 map 并发读写 panic [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- 修复：XProtocol 连接池(binding connpool) 中的内存泄漏 (#1821) [@Dennis8274](https://github.com/Dennis8274)
- 修复：应该将 close logger 放在最后，否则在关闭 MOSN 实例过程中将没有日志输出 (#1845) [@doujiang24](https://github.com/doujiang24)
- 修复：XProtocol PingPong 类型连接超时的时候，因为 codecClient 没有初始化，会导致 panic (#1849) [@cuiweixie](https://github.com/cuiweixie)
- 修复：当 unhealthyThreshold 是一个空值时，健康检查将不会工作，修改为空值时使用默认值 (#1853) [@Bryce-huang](https://github.com/Bryce-huang)
- 修复：WRR 负载均衡算法可能导致死循环（发生在 unweightChooseHost）#1860 [@alpha-baby](https://github.com/alpha-baby)
- 修复：direct response 中 hijack 不应该再执行转换 [@nejisama](https://github.com/nejisama)
- 修复：当一个不健康的 host 有很高的权重时，EDF wrr 将不再选择其他健康的 host [@lemonlinger](https://github.com/lemonlinger)
- 修复：Istio LDS 中的 CACert 文件名获取错误，导致 MOSN listen 失败，不会接受请求 (#1893). [@doujiang24](https://github.com/doujiang24)
- 修复：DNS 解析 STRICT_DNS_CLUSTER 中 host 的 goroutine 没法停止 #1894 [@bincherry](https://github.com/bincherry)
