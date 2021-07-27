---
title: "Announcing MOSN v0.22.0"
linkTitle: "Announcing MOSN v0.22.0"
date: 2021-04-02
author: MOSN Team
description: "MOSN v0.22.0 changelog."
---

We are happy to announce the release of [MOSN v0.22.0](https://github.com/mosn/mosn/releases/tag/v0.22.0)

## v0.22.0

### New Features
- Add Wasm extension framework [@antJack](https://github.com/antJack)
- Add x-bolt sub-protocol to allow wasm-based codec for XProtocol [@zonghaishang](https://github.com/zonghaishang)
- Support fallback through SO_ORIGINAL_DST when protocol auto-matching got failed [@antJack](https://github.com/antJack)
- Support Go Plugin mode for XProtocol [@fdingiit](https://github.com/fdingiit)
- Support for network extension [@wangfakang](https://github.com/wangfakang)
- Update to Istio xDS v3 API [@champly](https://github.com/champly)  Branch: [istio-1.7.7](https://github.com/mosn/mosn/tree/istio-1.7.7)

### Optimization

- Remove redundant file path clean when resolving StreamFilter configs [@eliasyaoyc](https://github.com/eliasyaoyc)
- Allow setting a unified callback handler for the StreamFilterChain [@antJack](https://github.com/antJack)
- Support multi-stage execution and remove state lock for the FeatureGate [@nejisama](https://github.com/nejisama)
- Add trace support for HTTP2 [@OrezzerO](https://github.com/OrezzerO)

### Refactoring
- Add StageManger to divide the bootstrap procedure of MOSN into four configurable stages [@nejisama](https://github.com/nejisama)
- Unify the type definitions of XProtocol and move into mosn.io/api package [@fdingiit](https://github.com/fdingiit)
- Add GetTimeout method for XProtocol to replace the variable getter [@nejisama](https://github.com/nejisama)

### Bug fixes
- Fix concurrent read and write for RequestInfo in Proxy [@nejisama](https://github.com/nejisama)
- Fix the safety bug when forwarding the request URI [@antJack](https://github.com/antJack)
- Fix concurrent slice read and write for Router configurations when doing persistence [@nejisama](https://github.com/nejisama)
