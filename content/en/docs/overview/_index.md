---
title: "MOSN overview"
linkTitle: "Overview"
weight: 1
date: 2020-01-20
description: >
  The basic description of MOSN.
---

MOSN is a powerful cloud native proxy written in Golang. As a cloud-native network data plane, MOSN is designed to provide services with multi-protocol, modular, intelligent, and secure proxy capabilities. MOSN is the short name of Modular Open Smart Network-proxy. MOSN can be integrated with any Service Mesh that supports the xDS API. It can also be used for other purposes, such as independent Layer 4 or 7 load balancer, API gateway, and cloud-native ingress.

## Quick start

For details, see [Quick start](../quick-start).

## Core capabilities

+ Istio integration
   + Integrates Istio 1.0 and V4 API to run in full dynamic resource configuration mode
+ Core forwarding
   + Supports a self-contained server
   + Supports the TCP proxy
   + Supports the TProxy mode
+ Multi-protocol
   + Supports HTTP/1.1 and HTTP/2
   + Supports SOFARPC
   + Supports Dubbo
   + Supports Tars
+ Core routing
   + Supports virtual host-based routing
   + Supports headers/URL/prefix-based routing
   + Supports host metadata-based subset routing
   + Supports retries
+ Back-end management & load balancing
   + Supports connection pools
   + Supports circuit breaker
   + Supports active back-end health check
   + Supports load balancing policies such as random and RR
   + Supports host metadata-based subset load balancing policies
+ Observability
   + Observes network data
   + Observes protocol data
+ TLS
   + Supports HTTP/1.1 on TLS
   + Supports HTTP/2.0 on TLS
   + Supports SOFARPC on TLS
+ Process management
   + Supports smooth reload
   + Supports hot upgrades
+ Extension capabilities
   + Supports custom private protocols
   + Supports custom extensions at the TCP I/O layer and protocol layer

## Community

The MOSN open-source community is developing rapidly and expecting continuous improvements. You are all welcome to join us.

If you have any questions, [submit issues](https://github.com/mosn/mosn/issues).

