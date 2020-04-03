---
title: "Announcing MOSN v0.11.0"
linkTitle: "Announcing MOSN v0.11.0"
date: 2020-04-03
author: MOSN Team
description: >
  MOSN v0.11.0 changelog.
---

We are happy to announce the release of [MOSN v0.11.0](https://github.com/mosn/mosn/releases/tag/v0.11.0). You can see the changelog below.

### New features

- Support the extension of Listener Filter, the transparent hijacking capability is implemented based on Listener Filter [@wangfakang](https://github.com/wangfakang)
- New Set method for variable mechanism [@pxzero](https://github.com/pxzero)
- Added automatic retry and exception handling when SDS Client fails [@taoyuanyuan](https://github.com/taoyuanyuan)
- Improve TraceLog and support injecting context [@nejisama](https://github.com/nejisama)

### Refactoring

- Refactored XProtocol Engine and reimplemented SOFARPC protocol [@neverhook](https://github.com/neverhook)
  - Removed SOFARPC Healthcheck filter and changed to xprotocol's built-in heartbeat implementation
  - Removed the original protocol conversion (protocol conv) support of the SOFARPC protocol, and added a new protocol conversion extension implementation capability based on stream filter
  - xprotocol adds idle free and keepalive
  - Protocol analysis and optimization
- Modify the Encode method parameters of HTTP2 protocol [@taoyuanyuan](https://github.com/taoyuanyuan)
- Streamlined LDS interface parameters [@nejisama](https://github.com/nejisama)
- Modified the routing configuration model, abandoned `connection_manager` [@nejisama](https://github.com/nejisama)

### Optimization

- Optimize Upstream dynamic domain name resolution mechanism [@wangfakang](https://github.com/wangfakang)
- Optimized TLS encapsulation, added error log, and modified timeout period in compatibility mode [@nejisama](https://github.com/nejisama)
- Optimize timeout setting, use variable mechanism to set timeout [@neverhook](https://github.com/neverhook)
- Dubbo parsing library dependency upgraded to 1.5.0 [@cch123](https://github.com/cch123)
- Reference path migration script adds OS adaptation [@taomaree](https://github.com/taomaree)

### Bug fix

- Fix the problem of losing query string during HTTP2 protocol forwarding [@champly](https://github.com/champly)