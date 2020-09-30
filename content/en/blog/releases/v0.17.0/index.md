---
title: "Announcing MOSN v0.17.0"
linkTitle: "Announcing MOSN v0.17.0"
date: 2020-09-30
author: MOSN Team
description: "MOSN v0.17.0 changelog."
---

We are happy to announce the release of [MOSN v0.17.0](https://github.com/mosn/mosn/releases/tag/v0.17.0).

## v0.17.0

### New Features

- Add header max size configuration option. [@wangfakang](https://github.com/wangfakang)
- Add protocol impement choice whether need workerpool mode. And support workerpool mode concurrent configuration. [@cch123](https://github.com/cch123)
- Add UDS feature for listener. [@CodingSinger](https://github.com/CodingSinger)
- Add dubbo protocol use xDS httproute config filter. [@champly](https://github.com/champly)

### Optimization

- Optimiza http situation buffer malloc. [@wangfakang](https://github.com/wangfakang)
- Optimize RWMutex for SDS StreamClient. [@nejisama](https://github.com/nejisama)
- Update hessian2 v1.7.0 lib. [@cch123](https://github.com/cch123)
- Modify NewStream interface, use callback replace direct. [@cch123](https://github.com/cch123)
- Refactor XProtocol connect pool, support pingpong mode, mutiplex mode and bind mode. [@cch123](https://github.com/cch123)
- Optimize XProtocol mutiplex mode, support Host max connect configuration. [@cch123](https://github.com/cch123)
- Optimize route regex config avoid dump unuse config. [@wangfakang](https://github.com/wangfakang)

### Bug fixes

- Fix README ant logo invalid address. [@wangfakang](https://github.com/wangfakang)
- Fix header override content when set a longer header to request header. [@cch123](https://github.com/cch123)
- Fix Dubbo protocol analysis attachment maybe panic. [@champly](https://github.com/champly)
