---
title: "Announcing MOSN v0.26.0"
linkTitle: "Announcing MOSN v0.26.0"
date: 2021-10-12
author: MOSN Team
description: "MOSN v0.26.0 changelog."
---

We are happy to announce the release of [MOSN v0.26.0](https://github.com/mosn/mosn/releases/tag/v0.26.0).

## v0.26.0

### Incompatible Change

For implementing new protocols more nature, XProtocol is no longer as a protocol and no subprotocol any more.
XProtocol is a framework to implement protocol easier now.
So, the old existing code for implementing new protocols need some changes,
please see [this doc](reports/xprotocol_0.26.0.md)(In Chinese) for changing the old existing code suit for the new release.

### New Features

- Added the ip_access new filter to manage access control based on IP (#1797). [@Bryce-huang](https://github.com/Bryce-huang)
- Support admin api extends auth functions (#1834). [@nejisama](https://github.com/nejisama)
- The transcode filter module support dynamic phase (#1815). [@YIDWang](https://github.com/YIDWang)
- Added the SetConnectionState method for tls connection in pkg/mtls/crypto/tls.Conn (#1804). [@antJack](https://github.com/antJack)
- Added the after-start and after-stop two new stages, and allow to register handler during these stages. [@doujiang24](https://github.com/doujiang24)
- Support specify the unix domain socket directory by adding the new "uds_dir" configuration (#1829). [@dengqian](https://github.com/dengqian)
- Support choose dynamic protocol convert dynamically and allow register transcoder through go-plugin. [@Tanc010](https://github.com/Tanc010)
- Added more HTTP protocol method to make protocol matcher work properly (#1870). [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Support to set upstream protocol dynamically (#1808). [@YIDWang](https://github.com/YIDWang)
- Support set default HTTP stream config #1886. [@nejisama](https://github.com/nejisama)

### Changes

- Change the default max header size to 8KB (#1837). [@nejisama](https://github.com/nejisama)
- Refactory default HTTP1 and HTTP2 convert, remove the proxy convert, use transcoder filter instead. [@nejisama](https://github.com/nejisama)
- transcoder filter: changed to register trancoder factory instead trancoder (#1879). [@YIDWang](https://github.com/YIDWang)

### Bug fixes

- Fix a HTTP buffer reuse related bug that may leads to nil panics in high concurrency case. [@nejisama](https://github.com/nejisama)
- Fix: get the proper value of variable response_flag, (#1814). [@lemonlinger](https://github.com/lemonlinger)
- Fix: prefix_write not work with "/" (#1826). [@Bryce-huang](https://github.com/Bryce-huang)
- Fix: the reconfig.sock file may be removed unexpectly when killed the old MOSN manually during smoothly upgrade, (#1820). [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Fix the bug in doretry: should not set setupRetry to false directly, since the old response should be skip when the new upstream request has been sent out (#1807). [@taoyuanyuan](https://github.com/taoyuanyuan)
- Should set the inherit config back to the MOSN instance (#1819). [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Should send resetStreamFrame to upstream when cancel grpc context at client side, otherwise server side context won't be done.  [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Should set the resetReason before closing the stream connection, otherwise, may unable to get the real reason (#1828). [@wangfakang](https://github.com/wangfakang)
- Should use the listener that best match when found multi listeners, otherwise, may got 400 error code. [@MengJiapeng](https://github.com/MengJiapeng)
- Fixed panic due to concurrent map iteration and map write during process setting broadcast in HTTP2 protocol. [@XIEZHENGYAO](https://github.com/XIEZHENGYAO)
- Fix memory leak occurred in the binding connpool of XProtocol (#1821). [@Dennis8274](https://github.com/Dennis8274)
- Should close logger at the end, otherwise, may lost log during close MOSN instance (#1845). [@doujiang24](https://github.com/doujiang24)
- Fix panic due to codecClient is nil when got connect timeout event from XProtocol PingPong connection pool (#1849). [@cuiweixie](https://github.com/cuiweixie)
- Health checker not work when the unhealthyThreshold is an empty value (#1853). [@Bryce-huang](https://github.com/Bryce-huang)
- WRR may leads dead recursion in unweightChooseHost #1860. [@alpha-baby](https://github.com/alpha-baby)
- Fix direct response, send hijack should not transcode. [@nejisama](https://github.com/nejisama)
- Fix EDF wrr lb cannot choose a healthy host when there's a unhealthy host with a high weight. [@lemonlinger](https://github.com/lemonlinger)
- Got the wrong CACert filename when converting the listen filter from Istio LDS, MOSN may not listen success (#1893). [@doujiang24](https://github.com/doujiang24)
- The goroutine for resolving hosts in STRICT_DNS_CLUSTER cannot be stopped #1894 [@bincherry](https://github.com/bincherry)
