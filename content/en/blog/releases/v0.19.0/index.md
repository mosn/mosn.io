---
title: "Announcing MOSN v0.19.0"
linkTitle: "Announcing MOSN v0.19.0"
date: 2020-12-01
author: MOSN Team
description: "MOSN v0.19.0 changelog."
---

We are happy to announce the release of [MOSN v0.19.0](https://github.com/mosn/mosn/releases/tag/v0.19.0).

## v0.19.0

### Optimization

- Use the latest TLS memory optimization scheme [@cch123](https://github.com/cch123)
- Proxy log optimization to reduce memory escape [@taoyuanyuan](https://github.com/taoyuanyuan)
- Increase the maximum number of connections limit [@champly](https://github.com/champly)
- When AccessLog fails to obtain variables, use "-" instead [@champly](https://github.com/champly)
- MaxProcs supports configuring automatic recognition based on CPU usage limits [@champly](https://github.com/champly)
- Refactored the StreamFilter framework. The network filter can reuse the stream filter framework [@antJack](https://github.com/antJack)
- Allow specifying network for cluster [@champly](https://github.com/champly)

### Bug fixes

- Fix HTTP Trace get URL error [@wzshiming](https://github.com/wzshiming)
- Fix the ConnectTimeout parameter of xDS cluster is not converted [@dengqian](https://github.com/dengqian)
- Fix the upstreamHostGetter method gets the wrong hostname [@dengqian](https://github.com/dengqian)
- Fix tcp proxy close the connection abnormally [@dengqian](https://github.com/dengqian)
- Fix the lack of default configuration of mixer filter, resulting in a nil pointer reference [@glyasai](https://github.com/glyasai)
- Fix HTTP2 direct response not setting `Content-length` correctly [@wangfakang](https://github.com/wangfakang)
- Fix the nil pointer reference in getAPISourceEndpoint [@dylandee](https://github.com/dylandee)
- Fix memory increase caused by too many Timer applications when Write is piled up [@champly](https://github.com/champly)
- Fix the problem of missing stats when Dubbo Filter receives an illegal response [@champly](https://github.com/champly)
