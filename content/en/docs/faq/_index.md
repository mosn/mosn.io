---
title: "FAQ"
linkTitle: "FAQ"
date: 2020-03-02
author: "MOSN Team"
menu:
  main:
    weight: 50
description: >
  MOSN frequently asked questions.
---

### Why use MOSN instead of Istio's default data plane?

Before the Service Mesh transformation, we have expected that as the next generation of Ant Financial's infrastructure, Meshization will inevitably bring revolutionary changes and evolution costs. We have a very ambitious blueprint: ready to integrate the original network and middleware various capabilities have been re-precipitated and polished to create a low-level platform for the next-generation architecture of the future, which will carry the responsibility of various service communications.

This is a long-term planning project that takes many years to build and meets the needs of the next five or even ten years. The team will cooperate to build a cross-business, SRE, middleware, infrastructure and other departments. We must have a network proxy forwarding plane with flexible expansion, high performance, and long-term evolution. Nginx and Envoy are unsatisfactory in terms of R & D efficiency and flexible expansion. At the same time, the entire Mesh transformation involves a large number of departments and R & D personnel. The landing costs of cross-team cooperation must be considered. Therefore, we based our own research on Golang. New network proxy MOSN in cloud native scenario. For the performance of Golang, we also did a full investigation and test in the early stage to meet the performance requirements of Ant Financial Services.

### What is the difference between MOSN and Envoy? What are the advantages of MOSN?

**Differences in language stacks**

MOSN is written in Go. Go has strong guarantees in terms of production efficiency and memory security. At the same time, Go has an extensive library ecosystem in the cloud-native era. The performance is acceptable and usable in the Service Mesh scenario. Therefore, MOSN has a lower intellectual cost for companies and individuals using languages such as Go and Java.

**Differentiation of core competence**

- MOSN supports a multi-protocol framework, and users can easily access private protocols with a unified routing framework;
- Multi-process plug-in mechanism, which can easily extend the plug-ins of independent MOSN processes through the plug-in framework, and do some other management, bypass and other functional module extensions;
- Transport layer national secret algorithm support with Chinese encryption compliance;

### Is the open source MOSN the same version as the MOSN used internally by Ant Financial?

First of all, there is no so-called independent MOSN version inside Ant Financial. Ant Financial has many modules developed based on MOSN, and the internal modules rely on the open source version of MOSN. The research and development of business-independent MOSN core capabilities are carried out directly on the open source version.

### What is the difference between the open source and commercial versions of MOSN?

Ant Financial has commercial Mesh products. Commercial products mainly provide a complete solution from development to delivery runtime. At the same time, in order to meet the business needs of enterprise users, MOSN will be extended, so the so-called MOSN commercial version It mainly carries the version of the business user's own business module.

### What is MOSN's open source plan?

The release cycle of MOSN open source is one month. We are about to announce Roadmap for 2020, and we look forward to co-building with more enterprises.

### What version of Istio does MOSN support? When will it be available in Istio?

Currently, MOSN can be based on Istio 1.1.4 and run through the bookinfo example. Due to the latest version of Istio that has upgraded the XDS protocol and some enhancements, MOSN is currently adapting. It is expected that in October 2020, it will fully support HTTP of high-level Istio. Department ability.

### Will MOSN contribute to an Open Source Foundation?

Currently, MOSN has no clear plans to contribute to the foundation.

### What service registration and discovery mechanisms does MOSN support?

MOSN mainly supports two service registration and discovery mechanisms: one is to directly adapt to Istio, and the other is to integrate the SDK and use it with different registration centers and configuration centers.

### How does MOSN compare to Envoy?

Because Envoy is based on HTTP testing, currently MOSN does not have a large optimization for HTTP, so we expect that a benchmark will be released after this year's optimization.

### How to participate in the MOSN open source community?

Join the MOSN slack worksapce: <https://mosnproxy.slack.com> to participate in the open source community. You can also visit the [Community repository](https://github.com/mosn/community) to learn about the organizational structure of the MOSN open source community and to obtain community materials.