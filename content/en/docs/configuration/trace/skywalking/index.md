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
    "config": {
      "reporter": "gRPC",
      "backend_service": "127.0.0.1:11800",
      "service_name": "mosn",
      "max_send_queue_size": 30000,
      "authentication": "mosn",
      "tls": {
        "cert_file": "cert.crt",
        "server_name_override": "mosn.io"
      }
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

To register the service name to SkyWalking, used only if the reporter is `gRCP`.

- If this field is left empty, `mosn` by default.

## max_send_queue_size

Trace data cache queue size, used only if the reporter is `gRCP`.

- If this field is left empty, `30000` by default.

## authentication

`gRPC` authentication, used only if the reporter is `gRCP`.

- If the field is not empty, this parameter is used for authentication when establishing a connection with the SkyWalking backend service.

## tls

Used only if the reporter is `gRCP`.

-  If the field is not empty, TLS will be used to connect to the SkyWalking backend service.

### cert_file

TLS client certificate.

### server_name_override

Service name.