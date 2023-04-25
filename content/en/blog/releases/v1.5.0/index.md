---
title: "Announcing MOSN v1.5.0"
linkTitle: "Announcing MOSN v1.5.0"
date: 2023-4-25
author: MOSN Team
description: "MOSN v1.5.0 changelog."
---

We are happy to announce the release of [MOSN v1.5.0](https://github.com/mosn/mosn/releases/tag/v1.5.0).

## v1.5.0

### New Features

- EdfLoadBalancer supports slow start mode (#2178) @jizhuozhi
- Support cluster exclusive connectionPool (#2281) @yejialiango
- LeastActiveRequest and LeastActiveConnection load balancers support setting active_request_bias (#2286) @jizhuozhi
- Support configuration of metric sampler (#2261) @jizhuozhi
- Add PeakEWMA load balancer (#2253) @jizhuozhi

### Changes

- README update partners & users (#2245) @doujiang24
- Update dependencies (#2242) (#2248) (#2249) @dependabot
- Upgrade the floor Go version supported by MOSN to 1.18 (#2288) @muyuan0

### Optimization

- Use logical clock to make edf scheduler more stable (#2229) @jizhuozhi
- Change log level on missing proxy_on_delete from ERROR to WARN in proxywasm (#2246) @codefromthecrypt
- Receiver names are different for connection object (#2262) @diannaowa
- Disable over strict linters in workflow (#2259) @jizhuozhi
- Disable workflow when PR in draft mode (#2269) @diannaowa
- Use pointer to avoid overhead of duffcopy and duffzero (#2272) @jizhuozhi
- Remove unnecessary imports (#2292) @spacewander
- CI add goimports check (#2297) @spacewander

### Bug fixes

- Fix panic caused by different hosts using the same rander during health check (#2240) @dengqian
- Fix connpool binding conn id (#2263) @antJack 
- Fix saved client stream protocol information to DownStreamProtocol in the context (#2270) @nejisama
- Fix not using the correct Go version for testing (#2288) @muyuan0
- Fix incorrectly assume the variable not found if the real value is '-' (#2174) @3062
- Fix nil interface panic caused by cluster certificate configuration error (#2291) @3062
- Fix parsing error caused by using interface type in the leastActiveRequestLoadBalancer configuration (#2284) @jizhuozhi
- Fix configuration lbConfig not effective (#2299) @3062
- Fix activeRequestBias missing default value and some naming case error (#2298) @jizhuozhi