---
title: "proxy"
linkTitle: "proxy"
weight: 1
date: 2020-01-20
description: >
  The proxy configurations in Network Filter.
---

proxy is the most common network filter in MOSN and is configured in the following format:

```json
{
  "downstream_protocol":"",
  "upstream_protocol":"",
  "router_config_name":"",
  "extend_config":{}
}
```

- `downstream_protocol`: The request protocol that the proxy expects to receive. When receiving data from a connection, MOSN uses this protocol to parse and forward packets. If the received packet protocol does not comply with the configuration, MOSN closes the connection.
- `upstream_protocol`: The protocol used by the proxy to forward data. Usually, this field needs to be consistent with `downstream_protocol`. Protocol conversion is performed only in special scenarios.
- `router_config_name`: The router configuration index of the proxy. Usually, this field is consistent with `router_config_name` configured in `connection_manager` of the listener.
- `extend_config`: The extension configuration, used only in `XProtocol` of MOSN currently.

