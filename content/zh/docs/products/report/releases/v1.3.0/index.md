---
title: "MOSN v1.3.0 发布"
linkTitle: "MOSN v1.3.0 发布"
date: 2022-11-28
author: "MOSN 团队"
aliases: "/zh/blog/releases/v1.3.0"
description: "MOSN v1.3.0 变更日志。"
---

我们很高兴的宣布 [MOSN v1.3.0](https://github.com/mosn/mosn/releases/tag/v1.3.0) 发布，以下是该版本的变更日志。

## v1.3.0

### 重构
- 迁移合并 Proxy-Wasm 的实现，并默认启用 wazero (#2172) [@Crypt Keeper](https://github.com/codefromthecrypt)

### 优化

- 优化解析 xDS 透明代理配置：增加对未识别地址的透传配置 (#2171) [@3062](https://github.com/3062)
- 优化 CI 测试中 golangci 执行流程 (#2166) [@taoyuanyuan](https://github.com/taoyuanyuan) (#2167) [@taoyuanyuan](https://github.com/taoyuanyuan)
- 为 Proxy-Wasm 添加集成基准测试 (#2164) [@Crypt Keeper](https://github.com/codefromthecrypt) (#2169) [@Crypt Keeper](https://github.com/codefromthecrypt)
- 升级 MOSN 支持的 Go 的最低版本至 1.17 (#2160) [@Crypt Keeper](https://github.com/codefromthecrypt)
- 改正 README.md 中的一些问题 (#2161) [@liaolinrong](https://github.com/liaolinrong)
- 新增基准测试 (#2173) [@3062](https://github.com/3062)
- subsetLoadBalancer 重用子集条目以优化分配/使用内存 (#2119) [@dzdx](https://github.com/dzdx) (#2188) [@liwu](https://github.com/chuailiwu)

### Bug 修复

- 修复 connpool_binging 在连接 upstream timeout 时出现的 panic 问题 (#2180) [@EraserTime](https://github.com/EraserTime)
- 修复 cluster LB 算法为 LB_ORIGINAL_DST 时 retryTime 是 0 的问题 (#2170) [@3062](https://github.com/3062)
- 修复平滑升级失败 (#2129) [@Bryce-Huang](https://github.com/Bryce-huang) (#2193) [@3062](https://github.com/3062)
- 修改解析 xDS Listener 日志的方式 (#2182) [@3062](https://github.com/3062)
- 修复示例代码打印错误 (#2190) [@liaolinrong](https://github.com/liaolinrong)