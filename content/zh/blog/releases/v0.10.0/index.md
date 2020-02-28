---
title: "MOSN v0.10.0 变更日志"
linkTitle: "MOSN v0.10.0 变更日志"
date: 2020-02-28
weight: 2
author: MOSN 团队
description: >
  MOSN v0.10.0 变更日志
---

### 新功能

- 支持多进程插件模式 ( [#979](https://github.com/mosn/mosn/pull/979), [taoyuanyuan](https://github.com/taoyuanyuan) )
- 启动参数支持service-meta参数 ( [#952](https://github.com/mosn/mosn/pull/952),[trainyao](https://github.com/trainyao) )
- 支持abstract uds模式挂载sds socket

### 重构

- 分离部分mosn基础库代码到[mosn.io/pkg](github.com/mosn/pkg)
- 分离部分mosn接口定义到[mosn.io/api](github.com/mosn/api)

### 优化

- 日志基础模块分离到mosn.io/pkg，mosn的日志实现优化
- 优化FeatureGate ( [#927](https://github.com/mosn/mosn/pull/927), [nejisama](https://github.com/nejisama) )
- 新增处理获取SDS配置失败时的处理
- CDS动态删除Cluster时，会同步停止对应Cluster的健康检查
- sds触发证书更新时的回调函数新增证书配置作为参数

### Bug 修复

- 修复在SOFARPC Oneway请求失败时，导致的内存泄漏问题
- 修复在收到非标准的HTTP响应时，返回502错误的问题
- 修复DUMP配置时可能存在的并发冲突
- 修复TraceLog统计的Request和Response Size错误问题
- 修复因为并发写连接导致写超时失效的问题
- 修复serialize序列化的bug 
- 修复连接读取时内存复用保留buffer过大导致内存占用过高的问题
- 优化XProtocol中Dubbo相关实现

