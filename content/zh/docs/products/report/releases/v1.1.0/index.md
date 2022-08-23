---
title: "MOSN v1.1.0 发布"
linkTitle: "MOSN v1.1.0 发布"
date: 2022-08-23
author: "MOSN 团队"
aliases: "/zh/blog/releases/v1.1.0"
description: "MOSN v1.1.0 变更日志。"
---

我们很高兴的宣布 [MOSN v1.1.0](https://github.com/mosn/mosn/releases/tag/v1.1.0) 发布，以下是该版本的变更日志。

## v1.1.0

### 新功能

- TraceLog 支持 zipkin (#2014) [@fibbery](https://github.com/fibbery)
- 支持云边互联 (#1640) [@CodingSinger](https://github.com/CodingSinger)，细节可以参考[博客](https://mosn.io/blog/posts/mosn-tunnel/)
- Trace 以 Driver 的形式支持插件化扩展，使用 Skywalking 作为跟踪实现 (#2047) [@YIDWang](https://github.com/YIDWang)
- xDS 支持 stream filter 解析扩展 (#2095) [@Bryce-huang](https://github.com/Bryce-huang)
- stream filter: ipaccess 扩展实现 xDS 解析逻辑 (#2095) [@Bryce-huang](https://github.com/Bryce-huang)
- MakeFile 添加打包 tar 命令 (#1968) [@doujiang24](https://github.com/doujiang24)

### 变更

- 调整连接读超时从 buffer.ConnReadTimeout 到 types.DefaultConnReadTimeout (#2051) [@fibbery](https://github.com/fibbery)
- 修复文档错字 (#2056) (#2057) [@threestoneliu](https://github.com/threestoneliu) (#2070) [@chenzhiguo](https://github.com/chenzhiguo)
- 更新 license-checker.yml 的配置文件 (#2071) [@kezhenxu94](https://github.com/kezhenxu94)
- 新增遍历 SubsetLB 的接口 (#2059) (#2061) [@nejisama](https://github.com/nejisama)
- 添加 tls.Conn 的 SetConfig 接口 (#2088) [@antJack](https://github.com/antJack)
- 添加 xds-server 作为 MOSN 控制面的示例 (#2075) [@Bryce-huang](https://github.com/Bryce-huang)
- 新增 HTTP 请求解析失败时的错误日志 (#2085) [@taoyuanyuan](https://github.com/taoyuanyuan) (#2066) [@fibbery](https://github.com/fibbery)
- 负载均衡在重试时跳过上一次选择的主机 (#2077) [@dengqian](https://github.com/dengqian)
- 访问日志支持打印 traceID，connectionID 和 UpstreamConnectionID  (#2107) [@Bryce-huang](https://github.com/Bryce-huang)

### 重构

- 重构 HostSet 的使用方式 (#2036) [@dzdx](https://github.com/dzdx)
- 更改连接写数据调整为只支持同步写的模式 (#2087) [@taoyuanyuan](https://github.com/taoyuanyuan)

### 优化

- 优化创建 subset 负载均衡的算法，降低内存占用 (#2010) [@dzdx](https://github.com/dzdx)
- 支持可扩展的集群更新方式操作 (#2048) [@nejisama](https://github.com/nejisama)
- 优化多证书匹配逻辑：优先匹配 servername，全部 servername 匹配不上以后才按照 ALPN 进行匹配 (#2053) [@MengJiapeng](https://github.com/MengJiapeng)

### Bug 修复

- 修复 wasm 示例中的 latest 镜像版本为固定的版本（#2033）[@antJack](https://github.com/antJack)
- 调整 MOSN 退出时日志关闭执行顺序，修复部分退出日志无法正确输出的问题 (#2034) [@doujiang24](https://github.com/doujiang24)
- 修复 OriginalDst 匹配成功以后没有正确处理的问题 (#2058) [@threestoneliu](https://github.com/threestoneliu)
- 修复协议转换场景没有正确处理异常情况的问题，新增协议转换实现规范 (#2062) [@YIDWang](https://github.com/YIDWang)
- 修复 stream proxy 没有正确处理连接写超时/断开等异常事件 (#2080) [@dengqian](https://github.com/dengqian)
- 修复连接事件监听时机错误可能引发的 panic 问题 (#2082) [@dengqian](https://github.com/dengqian)
- 避免在事件监听连接之前发生关闭事件 (#2098) [@dengqian](https://github.com/dengqian)
- HTTP1/HTTP2 协议在处理时在上下文中保存协议信息 (#2035) [@yidwang](https://github.com/YIDWang)
- 修复 xDS 推送时可能存在的并发问题 (#2101) [@yzj0911](https://github.com/yzj0911)
- 找不到 upstream 地址变量时，不再返回空，返回 ValidNotFound (#2049) [@songzhibin97](https://github.com/songzhibin97)
- 修复健康检查不支持 xDS (#2084) [@Bryce-huang](https://github.com/Bryce-huang)
- 修正判断上游地址方法 (#2093) [@dengqian](https://github.com/dengqian)
