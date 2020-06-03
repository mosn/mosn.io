---
title: "FilterChain"
linkTitle: "FilterChain"
date: 2020-01-20
weight: 1
description: >
  The FilterChain configurations of MOSN.
---

This topic describes the FilterChain configurations of MOSN.

FilterChain configurations are core logic configurations for MOSN listeners. Different FilterChain configurations describe how a listener handles requests.

Currently, a MOSN listener supports only one FilterChain.

The FilterChain configuration structure is shown as follows.

```json
{
  "tls_context": {},
  "tls_context_set": [],
  "filters": []
}
```

### tls_context_set

- Settings for a group of `tls_context`. MOSN uses `tls_context_set` to describe the TLS certificate information of a listener by default.
- A listener supports multiple TLS certificates simultaneously.

### tls_context

- Setting a single `tls_context` instead of `tls_context_set` is required for backward compatibility of MOSN (for earlier versions that support configuring only one certificate). This setting method will be gradually abandoned.
- For details about the tls_context configuration, see the [tls_context](../../custom#tls-context).

### filters

Settings of a group of network filters.

### network filter

network filter specifies how MOSN processes connection data at layer 4 after a connection is established.

```json
{
  "type":"",
  "config": {}
}
```

- `type` is a string that describes the filter type.
- `config` can be any json values that describe configurations of different filters.
- `network filter` supports custom extensions. The following types are supported by default: `proxy`, `tcp proxy`, and `connection_manager`.
   - `connection_manager` is a special filter used with `proxy`. It describes the router configurations of `proxy` and may be modified subsequently as a compatibility configuration.

