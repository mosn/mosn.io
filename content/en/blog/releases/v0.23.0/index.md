---
title: "Announcing MOSN v0.23.0"
linkTitle: "Announcing MOSN v0.23.0"
date: 2021-06-04
author: MOSN Team
description: "MOSN v0.23.0 changelog."
---

We are happy to announce the release of [MOSN v0.23.0](https://github.com/mosn/mosn/releases/tag/v0.23.0)

## v0.23.0

### New Features

- Add new networkfilter:grpc. A grpc server can be extended by networkfilter and implemented in the MOSN, reuse MOSN capabilities. [@nejisama](https://github.com/nejisama) [@zhenjunMa](https://github.com/zhenjunMa)
- Add a new extended function for traversal calls in the StreamFilterChain. [@wangfakang](https://github.com/wangfakang)
- Add HTTP 403 status code mapping in the bolt protocol. [@pxzero](https://github.com/pxzero)
- Add the ability to shutdown the upstream connections.  [@nejisama](https://github.com/nejisama)

### Optimization

- Optimize the networkfilter configuration parsed.  [@nejisama](https://github.com/nejisama)
- Support extend proxy config by protocol, optimize the proxy configuration parse timing. [@nejisama](https://github.com/nejisama)
- Add tls conenction's certificate cache, reduce the memory usage. [@nejisama](https://github.com/nejisama)
- Optimize Quick Start Sample. [@nobodyiam](https://github.com/nobodyiam)
- Reuse context when router handler check available. [@alpha-baby](https://github.com/alpha-baby)
- Modify the NewSubsetLoadBalancer's input parameter to interface instead of the determined structure. [@alpha-baby](https://github.com/alpha-baby)
- Add an example of using so plugin to implement a protocol. [@yichouchou](https://github.com/yichouchou)
+ Optimize the method of get environment variable `GOPATH` in the `MAKEFILE`. [@bincherry](https://github.com/bincherry)
+ Support darwin & aarch64 architecture. [@nejisama](https://github.com/nejisama)
- Optimize the logger file flag. [@taoyuanyuan](https://github.com/taoyuanyuan)

### Bug fixes

- Fix the bug of HTTP1 URL encoding. [@morefreeze](https://github.com/morefreeze)
- Fix the bug of HTTP1 URL case-sensitive process. [@GLYASAI](https://github.com/GLYASAI)
- Fix the bug of memory leak in error handling when the tls cipher suite is SM4. [@william-zk](https://github.com/william-zk)

