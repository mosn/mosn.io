---
title: "Announcing MOSN v0.13.0"
linkTitle: "Announcing MOSN v0.13.0"
date: 2020-06-01
author: MOSN Team
description: >
  MOSN v0.13.0 changelog.
---

We are happy to announce the release of [MOSN v0.13.0](https://github.com/mosn/mosn/releases/tag/v0.13.0). 

### New Features

- Support Strict DNS Cluster [@dengqian](https://github.com/dengqian)
- Stream Filter [@wangfakang](https://github.com/wangfakang) that supports GZip processing
- Dubbo service registry complete Beta version [@cch123](https://github.com/cch123)
- Stream Filter [@NeGnail](https://github.com/NeGnail) that supports standalone fault isolation
- Integrated Sentinel flow limiting capability [@ansiz](https://github.com/ansiz)

### Optimization

- Optimize implementation of EDF LB and re-implement WRR LB using EDF [@CodingSinger](https://github.com/CodingSinger)
- Configure to get ADMIN API optimizations, add features and environment variable related ADMIN API [@nejisama](https://github.com/nejisama)
- Update that triggers a health check when updating Host changed from asynchronous mode to synchronous mode [@nejisama](https://github.com/nejisama)
- Updated the Dubbo library to optimize the performance of Dubbo Decode [@zonghaishang](https://github.com/zonghaishang)
- Optimize Metrics output in Prometheus, using regularity to filter out illegal Key [@nejisama](https://github.com/nejisama)
- Optimize MOSN's return status code [@wangfakang](https://github.com/wangfakang)

### Bug fix

- Fix concurrent conflict issues with health check registration callback functions [@nejisama](https://github.com/nejisama)
- Fix the error where the configuration persistence function did not handle the null configuration correctly [@nejisama](https://github.com/nejisama)
- Fix the problem that DUMP as a file fails when ClusterName/RouterName is too long [@nejisama](https://github.com/nejisama)
- Fix the problem of not getting the XProtocol protocol correctly when getting it [@wangfakang](https://github.com/wangfakang)
- Fix the problem with fetching the wrong context when creating StreamFilter [@wangfakang](https://github.com/wangfakang)