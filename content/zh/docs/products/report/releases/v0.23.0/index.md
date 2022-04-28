---
title: "MOSN v0.23.0 发布"
linkTitle: "MOSN v0.23.0 发布"
date: 2021-06-04
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.23.0"
description: "MOSN v0.23.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.23.0](https://github.com/mosn/mosn/releases/tag/v0.23.0) 发布

以下是该版本的变更日志。

## v0.23.0

### 新功能

- 新增 networkfilter:grpc，支持通过 networkfilter 扩展方式在 MOSN 中实现可复用 MOSN 其他能力的 grpc server [@nejisama](https://github.com/nejisama) [@zhenjunMa](https://github.com/zhenjunMa)
- StreamFilterChain 新增遍历调用的扩展接口 [@wangfakang](https://github.com/wangfakang)
- bolt 协议新增 HTTP 403 状态码的映射 [@pxzero](https://github.com/pxzero)
- 新增主动关闭 upstream 连接的能力 [@nejisama](https://github.com/nejisama)

### 优化

- networkfilter 配置解析能力优化 [@nejisama](https://github.com/nejisama)
- proxy 配置解析支持按照协议扩展，配置解析时机优化 [@nejisama](https://github.com/nejisama)
- TLS 连接新增证书缓存，减少重复证书的内存占用 [@nejisama](https://github.com/nejisama)
- 优化 Quick Start Sample [@nobodyiam](https://github.com/nobodyiam)
- 优化默认路由处理时的 context 对象生成 [@alpha-baby](https://github.com/alpha-baby)
- 优化 Subset LoadBalancer 的创建函数接口 [@alpha-baby](https://github.com/alpha-baby)
- 新增使用 so plugin 扩展方式接入协议扩展的示例 [@yichouchou](https://github.com/yichouchou)
- 优化 makefile 中获取 GOPATH 环境变量的方式 [@bincherry](https://github.com/bincherry)
- 支持 darwin + arrch64 架构的编译 [@nejisama](https://github.com/nejisama)
- 优化日志打开方式 [@taoyuanyuan](https://github.com/taoyuanyuan)

### Bug 修复

- HTTP1 修复 URL 处理编码问题 [@morefreeze](https://github.com/morefreeze)
- HTTP1 修复 URL 处理大小写敏感错误问题 [@GLYASAI](https://github.com/GLYASAI)
- TLS 修复 SM4 套件异常处理时存在的内存泄漏问题 [@william-zk](https://github.com/william-zk)
