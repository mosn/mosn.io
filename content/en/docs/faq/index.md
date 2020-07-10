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

Before the Service Mesh transformation, we have expected that as the next generation of Ant Group's infrastructure, Meshization will inevitably bring revolutionary changes and evolution costs. We have a very ambitious blueprint: ready to integrate the original network and middleware various capabilities have been re-precipitated and polished to create a low-level platform for the next-generation architecture of the future, which will carry the responsibility of various service communications.

This is a long-term planning project that takes many years to build and meets the needs of the next five or even ten years, and cooperates to build a team that spans business, SRE, middleware, and infrastructure departments. We must have a network proxy forwarding plane with flexible expansion, high performance, and long-term evolution. Nginx and Envoy have a very long-term capacity accumulation and active community in the field of network agents. We have also borrowed from other excellent open source network agents such as Nginx and Envoy. At the same time, we have enhanced research and development efficiency and flexible expansion. Mesh transformation involves a large number of departments and R & D personnel. We must consider the landing cost of cross-team cooperation. Therefore, we have developed a new network proxy MOSN based on Go in the cloud-native scenario. For Go's performance, we also did a full investigation and test in the early stage to meet the performance requirements of Ant Group's services.

At the same time, we received a lot of feedback and needs from the end user community. Everyone has the same needs and thoughts. So we combined the actual situation of the community and ourselves to conduct the research and development of MOSN from the perspective of satisfying the community and users. We believe that the open source competition is mainly competition between standards and specifications. We need to make the most suitable implementation choice based on open source standards.

### What is the difference between MOSN and Envoy? What are the advantages of MOSN?

**Differences in language stacks**

MOSN is written in Go. Go has strong guarantees in terms of production efficiency and memory security. At the same time, Go has an extensive library ecosystem in the cloud-native era. The performance is acceptable and usable in the Service Mesh scenario. Therefore, MOSN has a lower intellectual cost for companies and individuals using languages such as Go and Java.

**Differentiation of core competence**

- MOSN supports a multi-protocol framework, and users can easily access private protocols with a unified routing framework;
- Multi-process plug-in mechanism, which can easily extend the plug-ins of independent MOSN processes through the plug-in framework, and do some other management, bypass and other functional module extensions;
- Transport layer national secret algorithm support with Chinese encryption compliance;

### Is the open source MOSN the same version as the MOSN used internally by Ant Group?

First of all, there is no so-called independent MOSN version inside Ant Group. Ant Group has many modules developed based on MOSN, and the internal modules rely on the open source version of MOSN. The research and development of business-independent MOSN core capabilities are carried out directly on the open source version.

### What is the difference between the open source and commercial versions of MOSN?

Ant Group has commercial Mesh products. Commercial products mainly provide a complete solution from development to delivery runtime. At the same time, in order to meet the business needs of enterprise users, MOSN will be extended, so the so-called MOSN commercial version It mainly carries the version of the business user's own business module.

### What is MOSN's open source plan?

The release cycle of MOSN open source is one month. We are about to announce Roadmap for 2020, and we look forward to co-building with more enterprises.

### What version of Istio does MOSN support? When will it be available in Istio?

MOSN currently runs the [bookinfo example](/docs/quick-start/istio/) based on Istio 1.5.2. In September 2020, Istio is expected to fully support the capabilities of Istio and become an integral part of Istio Sidecar deployment options available. Please join the [MOSN community](/docs/community) to learn about working with Istio.

### What service registration and discovery mechanisms does MOSN support?

MOSN mainly supports two service registration and discovery mechanisms: one is to directly adapt to Istio, and the other is to integrate the SDK and use it with different registration centers and configuration centers.

### How to participate in the MOSN open source community?

Join the MOSN slack worksapce <https://mosnproxy.slack.com> to participate in the open source community. You can also visit the [Community repository](https://github.com/mosn/community) to learn about the organizational structure of the MOSN open source community and to obtain community materials.
