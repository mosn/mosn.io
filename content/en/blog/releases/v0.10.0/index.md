---
title: "Announcing MOSN v0.10.0"
linkTitle: "MOSN v0.10.0 changelog"
date: 2020-02-28
author: MOSN Team
description: >
  MOSN v0.10.0 changelog。
---

We are happy to announce the release of [MOSN v0.10.0](https://github.com/mosn/mosn/releases/tag/v0.10.0). You can see the changelog below.

### New features

- Support multi-process plugin mode ([#979](https://github.com/mosn/mosn/pull/979), [@taoyuanyuan](https://github.com/taoyuanyuan))
- Startup parameters support service-meta parameters ([#952](https://github.com/mosn/mosn/pull/952), [@trainyao](https://github.com/trainyao))
- Supports abstract UDS mode to mount SDS socket

### Refactoring

- Separate some MOSN base library code into [mosn.io/pkg](https://github.com/mosn/pkg) package
- Separate MOSN interface definition to [mosn.io/api](https://github.com/mosn/api) package

### Optimization

- The log basic module is separated into `mosn.io/pkg`, and the log of MOSN is optimized
- Optimize FeatureGate ([#927](https://github.com/mosn/mosn/pull/927), [@nejisama](https://github.com/nejisama))
- Added processing when failed to get SDS configuration
- When CDS deletes a cluster dynamically, it will stop the health check corresponding to the cluster
- The callback function when SDS triggers certificate update adds certificate configuration as a parameter

### Bug fixes

- Fixed a memory leak issue when SOFARPC Oneway request failed
- Fixed the issue of 502 error when receiving non-standard HTTP response
- Fixed possible conflicts during DUMP configuration
- Fixed the error of Request and Response Size of TraceLog statistics
- Fixed write timeout failure due to concurrent write connections
- Fixed serialize bug
- Fixed the problem that the memory reuse buffer is too large when the connection is read, causing the memory consumption to be too high
- Optimize Dubbo related implementation in XProtocol
