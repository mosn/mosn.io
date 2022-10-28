---
title: "Announcing MOSN v1.2.0"
linkTitle: "Announcing MOSN v1.2.0"
date: 2022-10-28
author: MOSN Team
description: "MOSN v1.2.0 changelog."
---

We are happy to announce the release of [MOSN v1.2.0](https://github.com/mosn/mosn/releases/tag/v1.2.0).

## v1.2.0

### New Features

- Support for configuring HTTP retry status codes (#2097) [@dengqian](https://github.com/dengqian)
- Add dev container build configuration and instructions (#2108) [@keqingyuan](https://github.com/keqingyuan)
- Support connpool_binding GoAway (#2115) [@EraserTime](https://github.com/EraserTime)
- Support for configuring the listener defaultReadBufferSize (#2133) [@3062](https://github.com/3062)
- Support Proxy-Wasm v2 ABI (#2089) [@lawrshen](https://github.com/lawrshen)
- Support transparent proxy based on iptables tproxy (#2142) [@3062](https://github.com/3062)

### Refactoring

- Remove MOSN's extended context framework and use the variable mechanism instead. Migrate the variable mechanism and memory reuse framework to mosn.io/pkg (#2055) [@nejisama](https://github.com/nejisama)
- Migrating the metrics interface to mosn.io/api (#2124) [@YIDWang](https://github.com/YIDWang)

### Bug fixes

- Fix some missing log parameters (#2141) [@lawrshen](https://github.com/lawrshen)
- Determine if the obtained cookie exists by error (#2136) [@greedying](https://github.com/greedying)
