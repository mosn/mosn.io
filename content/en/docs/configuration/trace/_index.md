---
title: "Trace configurations"
linkTitle: "Trace configurations"
date: 2020-04-08
weight: 3
description:
  The trace configurations of MOSN.
---

This topic describes trace configurations of MOSN.

`trace` describes the following basic global parameters of MOSN.

```josn
{
  "tracing": {
    "enable": true,
    "driver": "",
    "tracer": "",
    "config": {
      
    }
  }
}
```

## enable

Boolean. Enable or disable trace.

## driver

Currently, supports `SOFATracer` and `SkyWalking`.

## tracer

Currently, supports `SOFATracer` and `SkyWalking`.

