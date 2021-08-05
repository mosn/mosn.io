---
title: "Announcing MOSN v0.24.0"
linkTitle: "Announcing MOSN v0.24.0"
date: 2021-08-05
author: MOSN Team
description: "MOSN v0.24.0 changelog."
---

We are happy to announce the release of [MOSN v0.24.0](https://github.com/mosn/mosn/releases/tag/v0.24.0), congratulations to Jianhao Fu([@alpha-baby](https://github.com/alpha-baby)) on becoming a MOSN Committer and thanks for his contribution to the MOSN community.

## v0.24.0

### New Features

- Support jaeger to collect OpenTracing message [@Roger](https://github.com/Magiczml)
- Routing configuration new variable configuration mode, you can modify the routing results by modifying the variable [@wangfakang](https://github.com/wangfakang)
- Routing virtualhost matching supports port matching mode [@jiebin](https://github.com/jiebinzhuang)
- Impl envoy filter: [header_to_metadata](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/header_to_metadata_filter) [@antJack](https://github.com/antJack)
- Support graceful for uds [@taoyuanyuan](https://github.com/taoyuanyuan)
- New subset load balancing logic to use the full list of machines for load balancing in scenarios where there is no metadata matching [@nejisama](https://github.com/nejisama)
- MOSN's grpc framework supports graceful stop [@alpha-baby](https://github.com/alpha-baby)

### Optimization

- Optimize health check update mode for Cluster configuration updates [@alpha-baby](https://github.com/alpha-baby)
- Add OnConnectionEvent interface in api.Connection [@CodingSinger](https://github.com/CodingSinger)
- Weighted roundrobin loadbalancer underwriting policy adjusted to normal roundrobin load balancer [@alpha-baby](https://github.com/alpha-baby)
- Enhance interface value in mosn variable model [@antJack](https://github.com/antJack)
- Subset also follows the same underwriting strategy when determining the number of machines and whether they exist [@antJack](https://github.com/antJack)

### Bug fixes

- Dubbo stream filter supports automatic protocol recognition [@Thiswang](https://github.com/Thiswang)
- Fix roundrobin loadbalancer result exception in case of concurrency [@alpha-baby](https://github.com/alpha-baby)
- Fix unix address resolution exception [@taoyuanyuan](https://github.com/taoyuanyuan)
- Fix the exception that HTTP short connection cannot take effect [@taoyuanyuan](https://github.com/taoyuanyuan)
- Fix a memory leak in the TLS over SM3 suite after disconnection [@ZengKe](https://github.com/william-zk)
- HTTP2 support doretry when connection reset by peer or broken pipe [@taoyuanyuan](https://github.com/taoyuanyuan)
- Fix the host information error from the connection pool [@Sharember](https://github.com/Sharember)
- Fix data race bug when choose weighted cluster [@alpha-baby](https://github.com/alpha-baby)
- Return invalid host if host is unhealthy in EdfLoadBalancer [@alpha-baby](https://github.com/alpha-baby)
- Fix the problem that XProtocol routing configuration timeout is invalid [@nejisama](https://github.com/nejisama)
