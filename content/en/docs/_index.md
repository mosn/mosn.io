---
title: "Documentation"
linkTitle: "Documentation"
weight: 20
menu:
  main:
    weight: 20
---

{{% pageinfo %}}
当前只提供<a href="/zh/docs">中文版文档</a>。The English documentation is under construction.

{{% /pageinfo %}}

MOSN is a network proxy written in Golang. It can be used as a cloud-native network data plane, providing services with the following proxy functions: multi-protocol, modular, intelligent, and secure. MOSN is the short name of Modular Open Smart Network-proxy. MOSN can be integrated with any Service Mesh wich support xDS API. It can also be used as an independent Layer 4 or Layer 7 load balancer, API Gateway, cloud-native Ingress, etc.

## Core competence

- Integrated with Istio
  - Integrated with Istio 1.0 and V4 APIs to run based on full dynamic resource configuration
- Core forwarding
  - Self-contained Web server
  - Support TCP proxy
  - Support TProxy mode
- Multi-protocol
  - Support HTTP/1.1 and HTTP/2
  - Support SOFARPC
  - Support Dubbo protocol (under development)
- Core routing
  - Support Virtual Host routing
  - Support Headers/URL/Prefix routing
  - Support Host Metadata-based Subset routing
  - Support retry
- Backend Management and load balancing
  - Support connection pool
  - Support throttling
  - Support active backend health check
  - Support load balancing strategies, such as Random and RR
  - Support Host Metadata-based Subset load balancing strategy
- Observability
  - Observe network data
  - Observing protocol data
- TLS
  - Support HTTP/1.1 on TLS
  - Support HTTP/2.0 on TLS
  - Support SOFARPC on TLS
- Process management   + Support smooth reload   + Support smooth upgrade
- Extension capability   + Support custom private protocols   + Support adding custom extensions in protocol at the TCP IO layer