---
title: "MOSN overview"
linkTitle: "Overview"
weight: 1
date: 2022-03-28
description: >
  The basic description of MOSN.
---

MOSN (Modular Open Smart Network) is a cloud-native network proxy written in Go language. It is open sourced by Ant Group and verified by hundreds of thousands of production containers in 11.11 global shopping festival. MOSN provides the capabilities of multiple protocol, modularity, intelligent and security. It integrates a large number of cloud-native components, and also integrates a Envoy network library, which is high-performance and easy to expand. MOSN and Istio can be integrated to build Service Mesh, and can also be used as independent L4/L7 load balancers, API gateways, cloud native Ingress, and etc.

## Quick start

For details, see [Quick start](../quick-start).

## Core capabilities

- Istio integration
   - Integrates Istio 1.10 to run in full dynamic resource configuration mode
- Core forwarding
   - Supports a self-contained server
   - Supports the TCP proxy
   - Supports the UDP proxy
   - Supports transparent traffic hijack mode
- Multi-protocol
   - Supports HTTP/1.1 and HTTP/2
   - Supports protocol extension based on XProtocol framework
   - Supports protocol automatic identification
   - Supports gRPC
- Core routing
   - Supports virtual host-based routing
   - Supports headers/URL/prefix/variable/dsl routing
   - Supports redirect/direct response/traffic mirror routing
   - Supports host metadata-based subset routing
   - Supports weighted routing.
   - Supports retries and timeout configuration
   - Supports request and response headers to add/remove
- Back-end management & load balancing
   - Supports connection pools
   - Supports persistent connection's heart beat handling
   - Supports circuit breaker
   - Supports active back-end health check
   - Supports load balancing policies: random/rr/wrr/edf
   - Supports host metadata-based subset load balancing policies
   - Supports different cluster types: original dst/dns/simple
   - Supports cluster type extension
- Observability
   - Support trace module extension
   - Integrates jaeger/skywalking
   - Support metrics with prometheus style
   - Support configurable access log
   - Support admin API extension
   - Integrates [Holmes](https://github.com/mosn/holmes) to automatic trigger pprof
- TLS
   - Support multiple certificates matches, and TLS inspector mode.
   - Support SDS for certificate get and update
   - Support extensible certificate get, update and verify
   - Support CGo-based cipher suites: SM3/SM4
- Process management
   - Supports hot upgrades
   - Supports graceful shutdown
- Extension capabilities
   - Supports go-plugin based extension
   - Supports process based extension
   - Supports WASM based extension
   - Supports custom extensions configuration
   - Supports custom extensions at the TCP I/O layer and protocol layer

## MOSN Community

The MOSN open-source community is developing rapidly and expecting continuous improvements. You are all welcome to join us.

For a more detailed description of the MOSN community check out the [mosn/community](https://github.com/mosn/community) repo, if you have any questions, [submit issues](https://github.com/mosn/mosn/issues).

