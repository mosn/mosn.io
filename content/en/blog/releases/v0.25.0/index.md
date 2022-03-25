---
title: "Announcing MOSN v0.25.0"
linkTitle: "Announcing MOSN v0.25.0"
date: 2021-10-12
author: MOSN Team
description: "MOSN v0.25.0 changelog."
---

We are happy to announce the release of [MOSN v0.25.0](https://github.com/mosn/mosn/releases/tag/v0.25.0).

## v0.25.0

### New Features

- Routing configuration supports remove request headers. [@wangfakang](https://github.com/wangfakang)
- Support WASM Reload. [@zu1k](https://github.com/zu1k)
- Integrated SEATA TCC mode, support HTTP protocol. [@dk-lockdown]((https://github.com/dk-lockdown)
- Support boltv2 protocol tracelog. [@nejisama](https://github.com/nejisama)
- New metrics stream filter for gRPC framework.  [@wenxuwan](https://github.com/wenxuwan)
- Support DNS related configuration in xDS cluster config. [@antJack](https://github.com/antJack)

### Refactoring

- Decouple MOSN core and Istio related xDS code. [@nejisama](https://github.com/nejisama)
- Upgrade proxy-wasm-go-host version. [@zhenjunMa](https://github.com/zhenjunMa)
- Refactor networkfilter configuration parse functions, support `AddOrUpdate` and `Get`. [@antJack](https://github.com/antJack)

### Optimization

- Use `mod vendor` instead of `GO111MODULE=off` in Makefile. [@scaat](https://github.com/scaat)
- Move some archived repo code into `mosn.io/pkg`. [@nejisama](https://github.com/nejisama)
- Optimize EDF loadbalancer: random pick host at the first select time. [@alpha-baby](https://github.com/alpha-baby)
- Optimize EDF loadbalancer performance. [@alpha-baby](https://github.com/alpha-baby)
- Optimize boltv2 protocol heartbeat's trigger and reply. [@nejisama](https://github.com/nejisama)
- Optimize HTTP2 retry processing in stream mode, optimize HTTP2 unary request processing in stream mode. [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Ignore CPU numbers limit when use environment variable to set GOMAXPROCS. [@wangfakang](https://github.com/wangfakang)
- Reduce memory alloc when create subset loadbalancer.  [@dzdx](https://github.com/dzdx)
- Support different listener can independent run same name gRPC Server. [@nejisama](https://github.com/nejisama)

### Bug fixes

- Fix MOSN hangs up when host is empty when retry. [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Fix connections in `msgconnpool` cannot handle connect event. [@RayneHwang](https://github.com/RayneHwang)
- Fix MOSN panic when tracer driver is not inited and someone calls tracer `Enable`. [@nejisama](https://github.com/nejisama)
- Fix boltv2 protocol constructs hijack response version wrong. [@nejisama](https://github.com/nejisama)
- Fix HTTP2 handle connection termination event. [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Fix typo.  [@jxd134](https://github.com/jxd134) [@yannsun](https://github.com/yannsun)
- Fix `ResponseFlag` outputs in `RequestInfo`. [@wangfakang](https://github.com/wangfakang)
- Fix bolt/boltv2 protocol not recalculated the empty data's length. [@hui-cha](https://github.com/hui-cha)

