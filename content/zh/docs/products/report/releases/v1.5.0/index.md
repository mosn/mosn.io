---
title: "MOSN v1.5.0 发布"
linkTitle: "MOSN v1.5.0 发布"
date: 2023-4-25
author: "MOSN 团队"
aliases: "/zh/blog/releases/v1.5.0"
description: "MOSN v1.5.0 变更日志。"
---

我们很高兴的宣布 [MOSN v1.5.0](https://github.com/mosn/mosn/releases/tag/v1.5.0) 发布，以下是该版本的变更日志。

## v1.5.0

### 新功能

- EdfLoadBalancer 支持慢启动 (#2178) @jizhuozhi
- 支持集群独占连接池 (#2281) @yejialiango
- LeastActiveRequest 和 LeastActiveConnection 负载均衡器支持设置 active_request_bias (#2286) @jizhuozhi
- 支持配置指标采样器 (#2261) @jizhuozhi
- 新增 PeakEWMA 负载均衡器 (#2253) @jizhuozhi

### 变更

- README 更新 partners & users (#2245) @doujiang24
- 更新依赖 (#2242) (#2248) (#2249) @dependabot
- 升级 MOSN 支持的最低 Go 版本至 1.18 (#2288) @muyuan0

### 优化

- 使用逻辑时钟使 edf 调度器更加稳定 (#2229) @jizhuozhi
- proxywasm 中缺少 proxy_on_delete 的日志级别从 error 更改为 warn (#2246) @codefromthecrypt
- 修正 connection 对象接收者命名不同的问题 (#2262) @diannaowa
- 禁用 workflow 中过于严格的 linters (#2259) @jizhuozhi
- 当 PR 是未完成状态时不启用 workflow (#2269) @diannaowa
- 使用指针减少 duffcopy 和 duffzero 开销 (#2272) @jizhuozhi
- 删除不必要的导入 (#2292) @spacewander
- CI 增加 goimports 检测 (#2297) @spacewander

### Bug 修复

- 修复健康检查时多个 host 使用同一个 rander 引发的异常 (#2240) @dengqian
- 修复连接池绑定连接标识错误 (#2263) @antJack 
- 修复在上下文中将 client stream 协议信息保存到 DownStreamProtocol 的错误 (#2270) @nejisama
- 修复未使用正确的 Go 版本进行测试 (#2288) @muyuan0
- 修复无法找到实际值为 '-' 的变量 (#2174) @3062
- 修复 cluster 证书配置错误导致的空接口异常 (#2291) @3062
- 修复 leastActiveRequestLoadBalancer 配置中使用了接口类型导致的解析错误 (#2284) @jizhuozhi
- 修复配置文件 lbConfig 未生效的问题 (#2299) @3062
- 修复 activeRequestBias 缺少默认值和一些命名大小写错误 (#2298) @jizhuozhi