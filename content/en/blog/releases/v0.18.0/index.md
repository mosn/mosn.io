---
title: "Announcing MOSN v0.18.0"
linkTitle: "Announcing MOSN v0.18.0"
date: 2020-11-02
author: MOSN Team
description: "MOSN v0.18.0 changelog."
---

We are happy to announce the release of [MOSN v0.18.0](https://github.com/mosn/mosn/releases/tag/v0.18.0).

## v0.18.0

### New Features

- Add MOSN configure extension [@nejisama](https://github.com/nejisama)
- Add MOSN configuration tool [mosn/configure](https://github.com/mosn/configure), improve user configure experience [@cch123](https://github.com/cch123)

### Optimization

- Avoid copying http response body [@wangfakang](https://github.com/wangfakang)
- Upgrade `github.com/TarsCloud/TarsGo` package, to v1.1.4 [@champly](https://github.com/champly)
- Add test for various connpool [@cch123](https://github.com/cch123)
- Use sync.Pool to reduce memory cost by TLS connection outBuf [@cch123](https://github.com/cch123)
- Reduce xprotocol lock area [@cch123](https://github.com/cch123)
- Remove useless parameter of `network.NewClientConnection` method, remove ALPN detection in `Dispatch` method of struct `streamConn` [@nejisama](https://github.com/nejisama)
- Add `TerminateStream` API to `StreamReceiverFilterHandler`, with which stream can be reset during handling [@nejisama](https://github.com/nejisama)
- Add client TLS fallback [@nejisama](https://github.com/nejisama)
- Add `payload` field to `dubbo.Frame` struct, for `SetData` method to save encoded frame [@lxd5866](https://github.com/lxd5866)
- Fix TLS HashValue in host [@nejisama](https://github.com/nejisama)
- Fix disable_log admin api typo [@nejisama](https://github.com/nejisama)

### Bug fixes

- Fix `go mod tidy` failing [@champly](https://github.com/champly)
- Fix `ResourceExhausted: grpc: received message larger than max` when MOSN receive > 4M XDS messages [@champly](https://github.com/champly) 
- Fix fault tolerance unit-test [@wangfakang](https://github.com/wangfakang)
- Fix MOSN reconfig fails when `MOSNConfig.servers[].listeners[].bind_port` is `false` [@alpha-baby](https://github.com/alpha-baby)
- Set timeout for local write buffer send, avoid goroutine leak [@cch123](https://github.com/cch123)
- Fix deadloop when TLS timeout [@nejisama](https://github.com/nejisama)
