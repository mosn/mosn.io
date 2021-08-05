---
title: "MOSN v0.21.0 发布"
linkTitle: "MOSN v0.21.0 发布"
date: 2021-02-02
author: "MOSN 团队"
aliases: "/zh/blog/releases/v0.21.0"
description: "MOSN v0.21.0 变更日志。"
---

我们很高兴的宣布 [MOSN v0.21.0](https://github.com/mosn/mosn/releases/tag/v0.21.0) 发布，恭喜郑泽超（[@CodingSinger](https://github.com/CodingSinger)）成为 MOSN Committer，感谢他为 MOSN 社区所做的贡献。

以下是该版本的变更日志。

## v0.21.0

### 优化

- 升级sentinel版本到v1.0.2 [@ansiz](https://github.com/ansiz)
- 读超时收缩tls的read buffer，降低tls内存消耗 [@cch123](https://github.com/cch123)
- 增加注释，简化xprotocol协议连接池实现 [@cch123](https://github.com/cch123)
- 更新mosn registry版本 [@cadeeper](https://github.com/cadeeper) [@cch123](https://github.com/cch123)

### 重构

- 优化路由Header匹配逻辑,支持通用的RPC路由匹配 [@nejisama](https://github.com/nejisama)
- 删除原有部分常量，新增用于描述变量机制的常量 [@nejisama](https://github.com/nejisama)
- 限流模块重构，支持自定义回调扩展，可实现自定义的过滤条件，上下文信息修改等能力 [@ansiz](https://github.com/ansiz)

### Bug修复

- 修复请求异常时metrics统计错误 [@cch123](https://github.com/cch123)
- 修复http场景转发前没有对url进行转义的问题 [@antJack](https://github.com/antJack)
- 修复HTTP协议中变量注入错误的问题, 修复HTTP2协议中不支持路由Rewrite的bug [@nejisama](https://github.com/nejisama)

### 新功能

- 支持Domain-Specific Language路由实现 [@CodingSinger](https://github.com/CodingSinger)
- StreamFilter支持go编写的动态链接库加载的方式 [@CodingSinger](https://github.com/CodingSinger)
- 路由配置中VirtualHost支持per_filter_config配置 [@machine3](https://github.com/machine3)
- 支持dubbo thrift协议 [@cadeeper](https://github.com/cadeeper)

