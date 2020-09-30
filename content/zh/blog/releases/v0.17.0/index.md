---
title: "MOSN v0.17.0 发布"
linkTitle: "MOSN v0.17.0 发布"
date: 2020-09-30
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.17.0"
description: "MOSN v0.17.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.17.0](https://github.com/mosn/mosn/releases/tag/v0.17.0) 发布。

以下是该版本的变更日志。

## v0.17.0

### 新功能

- 新增最大 Header 大小限制的配置选项 [@wangfakang](https://github.com/wangfakang)
- 支持协议实现时选择是否需要 workerpool 模式，在 workerpool 模式下，支持可配置的连接并发度
  [@cch123](https://github.com/cch123)
- Listener 配置新增对 UDS 的支持 [@CodingSinger](https://github.com/CodingSinger)
- 添加在 Dubbo 协议下通过 xDS HTTP 配置进行转换的过滤器 [@champly](https://github.com/champly)

### 优化

- 优化 http 场景下的 buffer 申请 [@wangfakang](https://github.com/wangfakang)
- 优化 SDS Client 使用读写锁获取 [@chainhelen](https://github.com/chainhelen)
- 更新 hessian2 v1.7.0 库 [@cch123](https://github.com/cch123)
- 修改 NewStream 接口，从回调模式调整为同步调用的模式 [@cch123](https://github.com/cch123)
- 重构 XProtocol 连接池，支持 pingpong 模式、多路复用模式与连接绑定模式 [@cch123](https://github.com/cch123)
- 优化 XProtocol 多路复用模式，支持单机 Host 连接数可配置，默认是 1 [@cch123](https://github.com/cch123)
- 优化正则路由配置项，避免 dump 过多无用配置 [@wangfakang](https://github.com/wangfakang)

### Bug 修复

- 修复 README 蚂蚁 logo 地址失效的问题 [@wangfakang](https://github.com/wangfakang)
- 修复当请求 header 太长覆盖请求内容的问题 [@cch123](https://github.com/cch123)
- 修复 Dubbo 协议解析 attachment 异常的问题 [@champly](https://github.com/champly)
