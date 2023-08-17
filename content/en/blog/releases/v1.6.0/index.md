---
title: "Announcing MOSN v1.6.0"
linkTitle: "Announcing MOSN v1.6.0"
date: 2023-8-17
author: MOSN Team
description: "MOSN v1.6.0 changelog."
---

We are happy to announce the release of [MOSN v1.6.0](https://github.com/mosn/mosn/releases/tag/v1.6.0).

## v1.6.0

### New Features

- PeakEWMA supports configuring activeRequestBias (#2301) @jizhuozhi
- gRPC filter supports UDS (#2309) @wenxuwan
- Support init inherit old mosn config function (#2241) @dengqian
- Allow custom proxy defaultRouteHandlerName (#2308) @fibbery

### Changes

- Example http-sample README adding configuration file link (#2226) @mimani68
- Update wazero to 1.2.1 (#2254) @codefromthecrypt
- Update dependencies (#2230) (#2233) (#2247) (#2302) (#2326) (#2332) (#2333) @dependabot

### Refactoring

- Refactor debug log content, move data into tracef (#2316) @antJack

### Optimization

- Optimize the default EWMA of newly added hosts (#2301) @jizhuozhi
- PeakEwma LB no longer counts error responses (#2323) @jizhuozhi

### Bug fixes

- Fix edfScheduler incorrectly fixing weight to 1 in dynamic load algorithm (#2306) @jizhuozhi
- Fix unstable LB behavior caused by changing the order of cluster hosts (#2258) @dengqian
- Fix panic caused by missing EWMA() method in NilMetrics (#2310) @antJack (#2312) @jizhuozhi
- Fix panic caused by empty cluster hosts when xDS is updated (#2314) @dengqian
- Fix reboot failure due to undeleted UDS socket file when MOSN exits abnormally (#2318) @wenxuwan
- Fix xDS status code not converted error. Fix unhandled istio inbound IPv6 address error (#2144) @kkrrsq
- Fix new connection error when Listener does not close directly during graceful exit of non-hot upgrade (#2234) @hui-cha
- Fix goimports lint error (#2313) @spacewander