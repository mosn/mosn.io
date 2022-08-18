---
title: "Announcing MOSN v1.1.0"
linkTitle: "Announcing MOSN v1.1.0"
date: 2022-08-18
author: MOSN Team
description: "MOSN v1.1.0 changelog."
---

We are happy to announce the release of [MOSN v1.1.0](https://github.com/mosn/mosn/releases/tag/v1.1.0).

## v1.1.0

### New Features

- TraceLog support for zipkin (#2014) [@fibbery](https://github.com/fibbery)
- Support MOSN cloud edge interconnection (#1640) [@CodingSinger](https://github.com/CodingSinger)
- Trace supports plugin extension in the form of Driver, using SkyWalking as trace implementation (#2047) [@YIDWang](https://github.com/YIDWang)
- Supports Parsing Extended xDS Stream Filter (#2095) [@Bryce-huang](https://github.com/Bryce-huang)
- stream filter: ipaccess extension implements xDS parsing logic (#2095) [@Bryce-huang](https://github.com/Bryce-huang)
- Add package tar command to MakeFile (#1968) [@doujiang24](https://github.com/doujiang24)

### Changes

- Adjust connection read timeout from buffer.ConnReadTimeout to types.DefaultConnReadTimeout (#2051) [@fibbery](https://github.com/fibbery)
- Fix typo in documentation (#2056) (#2057)[@threestoneliu](https://github.com/threestoneliu) (#2070) [@chenzhiguo](https://github.com/chenzhiguo)
- Update the configuration file of license-checker.yml (#2071) [@kezhenxu94](https://github.com/kezhenxu94)
- New interface for traversing SubsetLB (#2059) (#2061) [@nejisama](https://github.com/nejisama)
- Add SetConfig interface for tls.Conn (#2088) [@antJack](https://github.com/antJack)
- Fix the method of judging the upstream address (#2093) [@dengqian](https://github.com/dengqian)
- Add xds-server example (#2075) [@Bryce-huang](https://github.com/Bryce-huang)
- Added error log when HTTP request parsing fails (#2085) [@taoyuanyuan](https://github.com/taoyuanyuan) (#2066) [@fibbery](https://github.com/fibbery)
- Load balancing skips last selected host on retry (#2077) [@dengqian](https://github.com/dengqian)
- Access logs support printing traceID, connectionID and UpstreamConnectionID (#2107) [@Bryce-huang](https://github.com/Bryce-huang)

### Refactoring

- Refactor how HostSet is used (#2036) [@dzdx](https://github.com/dzdx)
- Change the connection write data to only support synchronous write mode (#2087) [@taoyuanyuan](https://github.com/taoyuanyuan)

### Optimization

- Optimize the algorithm for creating subset load balancing to reduce memory usage (#2010) [@dzdx](https://github.com/dzdx)
- Support scalable cluster update method operation (#2048) [@nejisama](https://github.com/nejisama)
- Optimize multi-certificate matching logic: match servername first, and match ALPN only after all servernames are unmatched (#2053) [@MengJiapeng](https://github.com/MengJiapeng)

### Bug fixes

- Fix the latest image version in the wasm example to be a fixed version (#2033) [@antJack](https://github.com/antJack)
- Adjust the order of log closing execution when MOSN exits, and fix the problem that some exit logs cannot be output correctly (#2034) [@doujiang24](https://github.com/doujiang24)
- Fixed the problem that OriginalDst was not properly processed after matching successfully (#2058) [@threestoneliu](https://github.com/threestoneliu)
- Fix the problem that the protocol conversion scene does not handle exceptions correctly, and add the protocol conversion implementation specification (#2062) [@YIDWang](https://github.com/YIDWang)
- Fix stream proxy not properly handling exception events such as connection write timeout/disconnect (#2080) [@dengqian](https://github.com/dengqian)
- Fix the panic problem that may be caused by the wrong timing of connection event listening (#2082) [@dengqian](https://github.com/dengqian)
- Avoid closing event before event listener connection (#2098) [@dengqian](https://github.com/dengqian)
- HTTP/HTTP2 protocol save protocol information in context when processing (#2035) [@yidwang](https://github.com/YIDWang)
- Fix possible concurrency issues when pushing xDS (#2101) [@yzj0911](https://github.com/yzj0911)
- If the upstream address variable is not found, it no longer returns null and returns ValidNotFound (#2049) [@songzhibin97](https://github.com/songzhibin97)
- Fix health check does not support xDS (#2084) [@Bryce-huang](https://github.com/Bryce-huang)