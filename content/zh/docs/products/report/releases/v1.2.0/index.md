---
title: "MOSN v1.2.0 发布"
linkTitle: "MOSN v1.2.0 发布"
date: 2022-10-28
author: "MOSN 团队"
aliases: "/zh/blog/releases/v1.2.0"
description: "MOSN v1.2.0 变更日志。"
---

我们很高兴的宣布 [MOSN v1.2.0](https://github.com/mosn/mosn/releases/tag/v1.2.0) 发布，以下是该版本的变更日志。

## v1.2.0

### 新功能

- 支持配置 HTTP 重试状态码 (#2097) [@dengqian](https://github.com/dengqian)
- 新增 dev 容器构建配置与说明 (#2108) [@keqingyuan](https://github.com/keqingyuan)
- 支持 connpool_binding GoAway (#2115) [@EraserTime](https://github.com/EraserTime)
- 支持配置 listener 默认读缓存大小 (#2133) [@3062](https://github.com/3062)
- 支持 proxy-wasm v2 ABI (#2089) [@lawrshen](https://github.com/lawrshen)
- 支持基于 iptables tproxy 的透明代理 (#2142) [@3062](https://github.com/3062)

### 重构

- 删除 MOSN 扩展的 context 框架，使用变量机制代替。将 MOSN 中的变量机制(variable)和内存复用框架(buffer)迁移到 mosn.io/pkg 中 (#2055) [@nejisama](https://github.com/nejisama)
- 迁移 metrics 接口到 mosn.io/api 中 (#2124) [@YIDWang](https://github.com/YIDWang)

### Bug 修复

- 修复部分日志参数缺失 (#2141) [@lawrshen](https://github.com/lawrshen)
- 通过 error 判断获取的 cookie 是否存在 (#2136) [@greedying](https://github.com/greedying)
