---
title: "Announcing MOSN v0.20.0"
linkTitle: "Announcing MOSN v0.20.0"
date: 2021-01-05
author: MOSN Team
description: "MOSN v0.20.0 changelog."
---

We are happy to announce the release of [MOSN v0.20.0](https://github.com/mosn/mosn/releases/tag/v0.20.0), congratulations to Huang Runhao([@GLYASAI](https://github.com/GLYASAI)) on becoming a MOSN Committer and thanks for his contribution to the MOSN community.

## v0.20.0

### Optimization

- Add UDS address prefix check before UDS resolution when TCP address resolution fails [@wangfakang](https://github.com/wangfakang)
- Optimized the retrial interval for connection pool acquisition [@nejisama](https://github.com/nejisama)
- Add global switch for write loop mode [@nejisama](https://github.com/nejisama)
- Optimize auto protocol matching and add test cases [@taoyuanyuan](https://github.com/taoyuanyuan)
- Replace the headers with more efficient variables [@CodingSinger](https://github.com/CodingSinger)
- Pool the writeBufferChan timer to reduce overhead [@cch123](https://github.com/cch123)
- Add MOSN failure detail info into TraceLog [@nejisama](https://github.com/nejisama)
- New read done channel in HTTP protocol processing [@alpha-baby](https://github.com/alpha-baby)
- Enhance logger rotator [@nejisama](https://github.com/nejisama)

### Refactoring

- Upgrade to golang 1.14.13 [@nejisama](https://github.com/nejisama)
- Refactor router chain extension mode to the route handler extension mode, support different router handler configuration [@nejisama](https://github.com/nejisama)
- Refactor MOSN extended configuration, support load config according to configuration order [@nejisama](https://github.com/nejisama)

### Bug fixes

- Fix the bug no provider available occurred after dubbo2.7.3 [@cadeeper](https://github.com/cadeeper)
- Fix the bug that UDS connections were treated as TCP connections in netpoll mode [@wangfakang](https://github.com/wangfakang)
- Fix the problem that the HTTP Header cannot be obtained correctly when it is set to an empty value [@ianwoolf](https://github.com/ianwoolf)

### New Features

- Support old Mosn transfer configuration to new Mosn through UDS to solve the issue that Mosn in XDS mode cannot be smoothly upgraded [@alpha-baby](https://github.com/alpha-baby)
- Automatic protocol identification supports the identification of XProtocol [@cadeeper](https://github.com/cadeeper)
- Support configuration of the keepalive parameters for XProtocol [@cch123](https://github.com/cch123)
- Support more detailed time tracking [@nejisama](https://github.com/nejisama)
- Support metrics lazy registration to optimize metrics memory when number of service in cluster is too large [@champly](https://github.com/champly)
- Add setter function for default XProtocol multiplex connection pool size [@cch123](https://github.com/cch123)
- Support netpoll [@cch123](https://github.com/cch123)
- Support broadcast [@dengqian](https://github.com/dengqian)
- Support get tls configurations from LDS response [@wZH-CN](https://github.com/wZH-CN)
- Add ACK response for SDS [@wZH-CN](https://github.com/wZH-CN)
