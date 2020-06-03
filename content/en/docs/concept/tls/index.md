---
title: "TLS connections"
linkTitle: "TLS connections"
date: 2020-01-20
weight: 4
description: >
  The TLS security capability of MOSN.
---

This topic describes the TLS security capability of MOSN.

## Certificate solution

MOSN provides a certificate issuance solution based on Istio Citadel. It configures Sidecar certificates by using the [Secret Discovery Service (SDS)](https://www.envoyproxy.io/docs/envoy/latest/configuration/security/secret) of the Istio community. Dynamic certificate discovery and hot updates are supported. To provide advanced security capabilities, MOSN obtains certificates by connecting to the internal Key Management Service (KMS) system, without relying on the self-certificate issuance capability of Citadel. MOSN also supports certificate caching, pushing, and updates.

The following figure shows the architecture of the MOSN's certificate solution.

![MOSN's certificate solution](mosn-certificate-arch.png)

Roles of the components are described as follows:

- Pilot: sends policies and SDS configurations (not shown in the figure for brevity).
- Citadel: acts as the Certificate Provider as well as the MCP Server to provide resources to the Citadel Agent such as Pods and custom resources (CRs).
- Citadel Agent: provides SDS Server services and sends certificates and CRs for MOSN, DB Sidecar, and Security Sidecar.
- KMS: issues certificates.

### Certificate acquisition procedure

After having a general idea about the overall architecture, we can break down the certificate acquisition procedure for the sidecar as shown in the following figure.

![Certificate acquisition procedure](certificate-request-process.png)

Each step in the figure is described as follows:

1. Citadel synchronizes Pod and CR information with the Citadel Agent (nodeagent) through the Mesh Configuration Protocol (MCP). This avoids overload of the API Server caused by direct requests from the Citadel Agent to the API Server.
2. MOSN sends an SDS request to the Citadel Agent by using the Unix Domain Socket.
3. The Citadel Agent performs tamper-proof verification and extracts the appkey.
4. The Citadel Agent uses the appkey to request Citadel to issue a certificate.
5. Citadel checks whether a certificate has been cached. If a valid certificate is cached, Citadel directly returns the cached certificate.
6. If no certificate is cached, Citadel constructs a certificate issuance request and requests the KMS to issue a certificate.
7. The KMS returns a certificate to Citadel. The KMS also generates certificate expiration and rotation notifications.
8. After receiving the certificate, Citadel transfers it to the corresponding Citadel Agent.
9. After receiving the certificate, the Citadel Agent caches it in memory and sends it to MOSN.

