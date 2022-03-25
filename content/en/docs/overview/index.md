---
title: "MOSN overview"
linkTitle: "Overview"
weight: 1
date: 2020-01-20
description: >
  The basic description of MOSN.
---

MOSN (Modular Open Smart Network) is a cloud-native network proxy written in Go language. It is open sourced by Ant Group and verified by hundreds of thousands of production containers in 11.11 global shopping festival. MOSN provides the capabilities of multiple protocol, modularity, intelligent and security. It integrates a large number of cloud-native components, and also integrates a Envoy network library, which is high-performance and easy to expand. MOSN and Istio can be integrated to build Service Mesh, and can also be used as independent L4/L7 load balancers, API gateways, cloud native Ingress, and etc.

## Quick start

For details, see [Quick start](../quick-start).

## Core capabilities

- Istio integration
   - Integrates Istio 1.0 and V4 API to run in full dynamic resource configuration mode
- Core forwarding
   - Supports a self-contained server
   - Supports the TCP proxy
   - Supports the TProxy mode
- Multi-protocol
   - Supports HTTP/1.1 and HTTP/2
   - Supports SOFARPC
   - Supports Dubbo
   - Supports Tars
- Core routing
   - Supports virtual host-based routing
   - Supports headers/URL/prefix-based routing
   - Supports host metadata-based subset routing
   - Supports retries
- Back-end management & load balancing
   - Supports connection pools
   - Supports circuit breaker
   - Supports active back-end health check
   - Supports load balancing policies such as random and RR
   - Supports host metadata-based subset load balancing policies
- Observability
   - Observes network data
   - Observes protocol data
- TLS
   - Supports HTTP/1.1 on TLS
   - Supports HTTP/2.0 on TLS
   - Supports SOFARPC on TLS
- Process management
   - Supports smooth reload
   - Supports hot upgrades
- Extension capabilities
   - Supports custom private protocols
   - Supports custom extensions at the TCP I/O layer and protocol layer

## MOSN Community

The MOSN open-source community is developing rapidly and expecting continuous improvements. You are all welcome to join us.

For a more detailed description of the MOSN community check out the [mosn/community](https://github.com/mosn/community) repo, if you have any questions, [submit issues](https://github.com/mosn/mosn/issues).

