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

### 优化

- 优化解析 xds 透明代理配置：增加对未识别地址的透传配置 (#2171) [@3062](https://github.com/3062)
- 优化 CI测试中 golangci 执行流程 (#2166) [@taoyuanyuan](https://github.com/taoyuanyuan) (#2167) [@taoyuanyuan](https://github.com/taoyuanyuan)
- 为 proxywasm 添加集成基准测试 (#2164) [@Crypt Keeper](https://github.com/codefromthecrypt) (#2169) [@Crypt Keeper](https://github.com/codefromthecrypt)
- 升级 mosn 支持的 go 的最低版本至 1.17 (#2160) [@Crypt Keeper](https://github.com/codefromthecrypt)
- 改正 README.md 中的一些问题 (#2161) [@liaolinrong](https://github.com/liaolinrong)

### Bug 修复

- 修复 connpool_binging 在连接 upstream timeout 时出现的 panic 问题 (#2180) [@EraserTime](https://github.com/EraserTime)
- 修复 cluster LB算法为 LB_ORIGINAL_DST 时 retryTime 是 0 的问题 (#2170) [@3062](https://github.com/3062)
- 修复平滑升级失败 (#2129) [@Bryce-Huang](https://github.com/Bryce-huang)