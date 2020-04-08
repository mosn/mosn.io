---
title: "SkyWalking configurations"
linkTitle: "SkyWalking configurations"
date: 2020-04-08
weight: 1
description: >
  The SkyWalking configurations of MOSN.
---

This topic describes the SkyWalking configurations of MOSN.

Currently, support `HTTP1` protocol.

The SkyWalking configuration structure is shown as follows.

```json
{
  "tracing": {
    "enable": true,
    "driver": "SkyWalking",
    "tracer": "SkyWalking",
    "config": {
      "reporter": "gRPC",
      "backend_service": "127.0.0.1:11800",
      "service_name": "mosn",
      "with_register": true
    }
  }
}
```

## reporter

Reporter mode, support `log` (only test) and `gRPC`.
- If this field is left empty, `log` by default.

## backend_service

SkyWalking backend service address, used only if the reporter is `gRCP`.
- eg: `127.0.0.1:11800`.

## service_name

To register the service name to SkyWalking.
- If this field is left empty, `mosn` by default.

## with_register

Boolean. When true, the coroutine is blocked until the registration is successful.
- If this field is left empty, `true` by default.