---
title: "Announcing MOSN v0.14.0"
linkTitle: "Announcing MOSN v0.14.0"
date: 2020-07-01
author: MOSN Team
description: "MOSN v0.14.0 changelog."
---

We are happy to announce the release of [MOSN v0.14.0](https://github.com/mosn/mosn/releases/tag/v0.14.0). Congratulations to [Changyu Yao (@trainyao)](https://github.com/trainyao) for becoming a MOSN Committer and thank him for his contribution to the MOSN community.

## v0.14.0

### New Features

- Support for Istio 1.5.X [@wangfakang](https://github.com/wangfakang) [@trainyao](https://github.com/trainyao) [@champly](https://github.com/champly)
  - go-control-plane upgrade to version 0.9.4
  - xDS support for ACK, new Metrics for xDS.
  - Istio sourceLabels filtering support
  - probe interface with pilot-agent support
  - support for more startup parameters, adapting to Istio agent startup scenarios
  - gzip, strict-dns, original-dst support for xDS updates.
  - Remove Xproxy Logic
- Maglev Load Balancing Algorithm Support [@trainyao](https://github.com/trainyao)
- New connection pool implementation for supporting message class requests [@cch123](https://github.com/cch123)
- New Metrics for TLS Connection Switching [@nejisama](https://github.com/nejisama)
- Metrics for adding HTTP StatusCode [@dengqian]( https://github.com/dengqian)
- Add Metrics Admin API output [@dengqian](https://github.com/dengqian)
- New interface to query the number of current requests for proxy [@zonghaishang](https://github.com/zonghaishang)
- Support for HostRewrite Header [@liangyuanpeng](https://github.com/liangyuanpeng)

### Optimization

- Upgrade tars dependencies to fix compilation issues with higher versions of Golang [@wangfakang](https://github.com/wangfakang)
- xDS Configuration Analysis Upgrade Adaptation Istio 1.5.x [@wangfakang]( https://github.com/wangfakang)
- Optimize log output from proxy [@wenxuwan](https://github.com/wenxuwan)
- DNS Cache default time changed to 15s [@wangfakang](https://github.com/wangfakang)
- HTTP Parameter Route Matching Optimization [@wangfakang](https://github.com/wangfakang)
- Upgrade the fasthttp library [@wangfakang](https://github.com/wangfakang)
- Optimizing Dubbo Request Forwarding Encoding [@zonghaishang](https://github.com/zonghaishang)
- Request max body configurable for HTTP support [@wangfakang]( https://github.com/wangfakang)

### Bug fixes

- Fix Dubbo Decode bug that fails to parse attachment [@champly](https://github.com/champly)
- Fix bug where streams could be created before HTTP2 connection is established [@dunjut](https://github.com/dunjut)
- Fix HTTP2 Handling Trailer null pointer exception [@taoyuanyuan ](https://github.com/taoyuanyuan)
- Fix bug where HTTP request headers are not standardized by default [@nejisama]( https://github.com/nejisama)
- Fix panic exceptions caused by disconnected connections during HTTP request processing [@wangfakang](https://github.com/wangfakang)
- Fix read/write lock copy issue with dubbo registry [@champly]( https://github.com/champly)