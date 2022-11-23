---
title: "Announcing MOSN v1.3.0"
linkTitle: "Announcing MOSN v1.3.0"
date: 2022-10-28
author: MOSN Team
description: "MOSN v1.3.0 changelog."
---

We are happy to announce the release of [MOSN v1.3.0](https://github.com/mosn/mosn/releases/tag/v1.3.0).

## v1.3.0

### Refactoring
- Moves to consolidated Proxy-Wasm implementation and enables wazero by default (#2172) [@Crypt Keeper](https://github.com/codefromthecrypt)

### Optimization

- Optimized parsing xDS transparent proxy configuration: add pass-through configuration for unrecognized addresses (#2171) [@3062](https://github.com/3062)
- Optimized the golangci execution flow in CI testing  (#2166) [@taoyuanyuan](https://github.com/taoyuanyuan) (#2167) [@taoyuanyuan](https://github.com/taoyuanyuan)
- Add integrated benchmarks for Proxy-Wasm (#2164) [@Crypt Keeper](https://github.com/codefromthecrypt) (#2169) [@Crypt Keeper](https://github.com/codefromthecrypt)
- Upgrade the minimum version of Go supported by MOSN to 1.17 (#2160) [@Crypt Keeper](https://github.com/codefromthecrypt)
- Fix some problems in the README.md (#2161) [@liaolinrong](https://github.com/liaolinrong)
- Add benchmark (#2173) [@3062](https://github.com/3062)
- subsetLoadBalancer reuse subset entry to optimze alloc/inuse memory (#2119) [@dzdx](https://github.com/dzdx) (#2188) [@liwu](https://github.com/chuailiwu)

### Bug fixes

- Fix a panic problem with connpool_binging when connecting to upstream timeout (#2180) [@EraserTime](https://github.com/EraserTime)
- Fix the problem that retryTime is 0 when cluster LB algorithm is LB_ORIGINAL_DST (#2170) [@3062](https://github.com/3062)
- Fix smooth upgrade failed (#2129) [@Bryce-Huang](https://github.com/Bryce-huang) (#2193) [@3062](https://github.com/3062)
- Modify the way xDS Listener logs are parsed (#2182) [@3062](https://github.com/3062)
- Fix example print error (#2190) [@liaolinrong](https://github.com/liaolinrong)