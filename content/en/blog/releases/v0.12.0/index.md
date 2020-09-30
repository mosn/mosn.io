---
title: "Announcing MOSN v0.12.0"
linkTitle: "Announcing MOSN v0.12.0"
date: 2020-04-28
author: MOSN Team
description: >
  MOSN v0.12.0 changelog.
---

We are happy to announce the release of [MOSN v0.12.0](https://github.com/mosn/mosn/releases/tag/v0.12.0). Thanks to [Sun Fuze (@peacocktrain)](https://github.com/peacocktrain) for his great contribution to this release, certified as [commiter](https://github.com/mosn/community/issues/6)🎉 by MOSN Community Leaders.

You can see the changelog below.

### New Features

- Support Skywalking [@arugal](https://github.com/arugal)
- Stream Filter adds a new phase of Receive Filter execution, which allows you to execute Receive Filter [@wangfakang](https://github.com/wangfakang) again after MOSN has finished routing Host
- HTTP2 supports streaming  [@peacocktrain](https://github.com/peacocktrain) [@taoyuanyuan](https://github.com/taoyuanyuan)
- FeatureGate adds interface KnownFeatures to output current FeatureGate status [@nejisama](https://github.com/nejisama)
- Provide a protocol-transparent way to obtain requested resources (PATH, URI, ARG), with the definition of resources defined by each protocol itself [@wangfakang](https://github.com/wangfakang)
- New load balancing algorithm
  - Support for ActiveRequest LB [@CodingSinger](https://github.com/CodingSinger)
  - Support WRR LB  [@nejisama](https://github.com/nejisama)

### Optimize

- XProtocol protocol engine optimization [@neverhook](https://github.com/neverhook)
  - Modifies the XProtocol heartbeat response interface to support the protocol's heartbeat response to return more information
  - Optimize connpool for heartbeat triggering, only heartbeats will be triggered if the protocol for heartbeats is implemented
- Dubbo library dependency version updated from v1.5.0-rc1 to v1.5.0 [@cch123](https://github.com/cch123)
- API Adjustments, HostInfo added health check related interface [@wangfakang](https://github.com/wangfakang)
- Optimize circuit breaking function  [@wangfakang](https://github.com/wangfakang)
- Responsible for balanced selection logic simplification, Hosts of the same address multiplex the same health check mark [@nejisama](https://github.com/nejisama) [@cch123](https://github.com/cch123)
- Optimize HTTP building logic and improve HTTP building performance [@wangfakang](https://github.com/wangfakang)
- Log rotation logic triggered from writing logs, adjusted to timed trigger [@nejisama](https://github.com/nejisama)
- Typo fixes [@xujianhai666](https://github.com/xujianhai666) [@candyleer](https://github.com/candyleer)

### Bug Fix

- Fix the xDS parsing fault injection configuration error  [@champly](https://github.com/champly)
- Fix the request hold issue caused by the MOSN HTTP HEAD method [@wangfakang](https://github.com/wangfakang)
- Fix a problem with missing StatusCode mappings in the XProtocol engine [@neverhook](https://github.com/neverhook)
- Fix the bug for DirectReponse triggering retries [@taoyuanyuan](https://github.com/taoyuanyuan)
