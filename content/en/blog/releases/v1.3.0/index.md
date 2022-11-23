---
title: "Announcing MOSN v1.3.0"
linkTitle: "Announcing MOSN v1.3.0"
date: 2022-10-28
author: MOSN Team
description: "MOSN v1.3.0 changelog."
---

We are happy to announce the release of [MOSN v1.3.0](https://github.com/mosn/mosn/releases/tag/v1.3.0).

## v1.3.0

### Optimization

- Optimized parsing xds transparent proxy configuration: add pass-through configuration for unrecognized addresses (#2171) [@3062](https://github.com/3062)
- Optimizing the golangci execution flow in CI testing  (#2166) [@taoyuanyuan](https://github.com/taoyuanyuan) (#2167) [@taoyuanyuan](https://github.com/taoyuanyuan)
- Adds integrated benchmarks for proxywasm (#2164) [@Crypt Keeper](https://github.com/codefromthecrypt) (#2169) [@Crypt Keeper](https://github.com/codefromthecrypt)
- Upgrade the minimum version of go supported by mosn to 1.17 (#2160) [@Crypt Keeper](https://github.com/codefromthecrypt)
- Fix some problems in the README.md (#2161) [@liaolinrong](https://github.com/liaolinrong)

### Bug fixes

- Fix a panic problem with connpool_binging when connecting to upstream timeout (#2180) [@EraserTime](https://github.com/EraserTime)
- Fix the problem that retryTime is 0 when cluster LB algorithm is LB_ORIGINAL_DST (#2170) [@3062](https://github.com/3062)
- Fix smooth upgrade failed (#2129) [@Bryce-Huang](https://github.com/Bryce-huang)
