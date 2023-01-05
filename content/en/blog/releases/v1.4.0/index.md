---
title: "Announcing MOSN v1.4.0"
linkTitle: "Announcing MOSN v1.4.0"
date: 2023-2-28
author: MOSN Team
description: "MOSN v1.4.0 changelog."
---

We are happy to announce the release of [MOSN v1.4.0](https://github.com/mosn/mosn/releases/tag/v1.4.0).

## v1.4.0

### New Features

- Support record HTTP health check log (#2096) @dengqian
- Add least_connection load balancer (#2184) @dengqian
- Add API to force reconnect and send ADS request (#2183) @dengqian
- Support pprof debug server endpoint configure (#2202) @dengqian
- Integrate with mosn.io/envoy-go-extension, consult [document](https://github.com/mosn/mosn/blob/master/examples/codes/envoy-go-extension/README_EN.md) (#2200) @antJack (#2222) @3062
- Add API to support override registration (mosn/pkg#72) @antJack
- Add a field in variables to record mosn processing time (#2235) @z2z23n0
- Support setting cluster idle_timeout to zero to indicate never timeout (#2197) @antJack

### Refactoring
- Move pprof import to pkg/mosn (#2216) @3062

### Optimization

- Reduce logging for proxywasm benchmarks (#2189) @Crypt Keeper

### Bug fixes

- Enlarge UDP DNS resolve buffer (#2201) @dengqian
- Fix the debug server not inherited during smooth upgrade (#2204) @dengqian
- Fix where new mosn would delete reconfig.sock when smooth upgrade failed (#2205) @dengqian
- Fix HTTP health check URL with query string will be unescaped (#2207) @dengqian
- Fix lb ReqRoundRobin fails to choose host when index exceeds hosts length (#2209) @dengqian
- Fix the problem that after RDS creates a route, the established connection cannot find the route (#2199) @dengqian (#2210) @3062
- Fix old cache values are read after the execution of Variable.Set (mosn/pkg#73) @antJack
- Fix panic caused by DefaultRoller not setting time (mosn/pkg#74) @dengqian
- Advance the timing of metrics initialization to make it work for static config (#2221) @dengqian
- Fix a concurrency problem caused by multiple health checkers sharing rander (#2228) @dengqian
- Set HTTP/1.1 as the HTTP protocol to be sent upstream (#2225) @dengqian
- Completing the missing statistics (#2215) @3062