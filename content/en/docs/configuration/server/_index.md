---
title: "Server configurations"
linkTitle: "Server configurations"
date: 2020-01-20
weight: 2
description:
  The server configurations of MOSN.
---

This topic describes server configurations of MOSN.

In the configuration structure of MOSN, `servers` indicates a server array, but only one server is supported currently.

`server` describes the following basic global parameters of MOSN.

```josn
{
  "default_log_path":"",
  "default_log_level":"",
  "global_log_roller":"",
  "graceful_timeout":"",
  "processor":"",
  "listeners":[]
}
```

## default_log_path

The default error log path, which supports a complete log path, stdout, and stderr.

- If this field is left empty, logs are output to stderr by default.

## default_log_level

The default error log level. Valid values: `DEBUG`, `INFO`, `WARN`, `ERROR`, and `FATAL`.

- Default value: `INFO`.

## global_log_roller

- The log roller configuration, which is valid for all logs, for example, tracelog, accesslog, and defaultlog.
- The string type, which supports two modes: rolling by time and rolling by log size. Only one mode is valid at a time.
- Rolling by log size
   - `size`: The log size threshold (in MB) to start rolling.
   - `age`: The number of days during which the logs will be saved.
   - `keep`: The maximum number of logs to be saved.
   - `compress`: Specifies whether to compress logs. Valid values: on and off.

```
"global_log_roller": "size=100 age=10 keep=10 compress=off"
```

- Rolling by time
   - `time`: The time interval (hours) for rolling.

```
"global_log_roller":"time=1"
```

## graceful_timeout

- The [Duration String](../custom#duration-string) configuration. Indicates the maximum waiting time for disabling a connection in a hot upgrade of MOSN.
- Default value: 30s.

### processor

The number of `GOMAXPROCS` used by MOSN.

- Default value: the number of CPUs.
- If it is not set or is set to 0, the default value is used.

## Listeners

Settings for a group of [Listeners](../listener).

