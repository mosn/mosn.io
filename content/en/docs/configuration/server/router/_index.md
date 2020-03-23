---
title: "Router configurations"
linkTitle: "Router configurations"
date: 2020-03-13
weight: 2
description: >
  This topic describes router configurations.
---

`router`: Specifies the router configurations of MOSN and is usually used with proxy.

```json
{
  "router_config_name":"",
  "virtual_hosts": [
  ]
}
```

- `router_config_name`: The unique router configuration ID, which corresponds to the field configured in `proxy`.
- `virtual_hosts`: Specifies the specific routing rules.

## VirtualHost

```json
{
  "name":"",
  "domains":[],
  "routers":[]
}
```

- `name`: String. The unique ID of a virtual host.
- `domains`: String array. Indicates a group of domains that match the virtual host. Wildcard characters are supported. The matching priorities of domains are as follows:
   - First, exact match, for example, `www.foo.com`.
   - Second, wildcard match with the longest suffix. For example, `*-bar.foo.com` is a preferred match for the domain `foo-bar.foo.com` between `*.foo.com` and `*-bar.foo.com`.
   - At last, wildcard match for any domains, that is, `*`.
- `routers`: A group of specific router rules.

## Router

```json
{
  "match":{},
  "route":{},
  "per_filter_config":{}
}
```

- `match`: The route match parameter.
- `route`: The routing behavior. Specifies the upstream information of a request to be routed.
- `per_filter_config`: The filter configuration in the `key: json` format.
- The key needs to match a type of the stream filter, and the json corresponding to the key is the value configured for the stream filter.
   - When this field is configured, the configurations indicated by this field can be used to overwrite the original configurations of some stream filters (depending on specific filters), to generate stream filter configurations at the router match level.

## match

```json
{
  "prefix":"",
  "path":"",
  "regex":"",
  "headers": []
}
```

- Path match
   - `prefix`: Indicates that the router will match the path prefix. This configuration has a higher priority than path and regex. If this field is configured, the path prefix of the request needs to first comply with this configuration.
   - `path`: Indicates that the router will match the exact path. This configuration has a higher priority than regex. If this field is configured, the path of the request needs to first comply with this configuration.
   - `regex`: Indicates that the router will match the path based on a regular expression. If this field is configured, the path of the request needs to first comply with this configuration.
   - When multiple path match configurations are available, only the one with a higher priority takes effect.
- Header match
   - headers: The group of headers that the request needs to match. The match succeeds only when the request meets all header configuration conditions.

## header

```json
{
  "name":"",
  "value":"",
  "regex":""
}
```

- `name`: The key of the header.
- `value`: The value of the header key.
- `regex`: Boolean. Specifies whether the value can be matched by a regular expression. Default values: True and False.

## route

```
{
  "cluster_name":"",
  "metadata_match":"",
  "timeout":"",
  "retry_policy":{}
}
```

- `cluster_name`: The upstream cluster to which the request is to be routed.
- `metadata_match`: The [metadata](.. /../custom#metadata) configuration. If this field is configured, the router will match against the subset of the upstream cluster based on the metadata.
- `timeout`: The [Duration String](.. /../custom#duration-string) configuration. Indicates the default forwarding timeout value of the request. If the timeout value is specified in the request, this configuration will be ignored.
- `retry_policy`: The retry configuration, which indicates the retry policy applied when the request encounters a specific error. If this field is left unconfigured, no retry will be performed by default.

## retry_policy

```json
{
  "retry_on":"",
  "retry_timeout":"",
  "num_retries":""
}
```

- `retry_on`: Boolean. Specifies whether to enable retry.
- `retry_timeout`: The [Duration String](.. /../custom#duration-string) configuration. Indicates the timeout value for each retry. When `retry_timeout` exceeds the timeout value specified in the route field or request, this configuration is invalid.
- `num_retries`: Indicates the maximum number of retries supported.

