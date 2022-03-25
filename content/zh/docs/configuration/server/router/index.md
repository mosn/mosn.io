---
title: "Router 配置"
linkTitle: "Router 配置"
date: 2020-03-13
weight: 2
aliases: "/zh/docs/server/router"
description: >
  路由配置说明。
---

`router` 用于描述 MOSN 的路由配置，通常与 proxy 配合使用。

```
{
  "router_config_name":"",
  "virtual_hosts": [
  ]
}
```

- `router_config_name`，唯一的路由配置标识，与 `proxy` 中配置的字段对应。
- `virtual_hosts`，描述具体的路由规则细节。

## VirtualHost

```json
{
  "name":"",
  "domains":[],
  "routers":[]
}
```

- `name`，字符串。用作 virtual host 的唯一标识。
- `domains`，字符串数组。表示一组可以匹配到该 virtual host 的 domain，支持配置通配符。domain 的匹配优先级如下：
  - 首先匹配精确的，如 `www.foo.com`。
  - 其次匹配最长后缀的通配符，如 `*.foo.com`、`*-bar.foo.com`，其中如果一个 domain 是 `foo-bar.foo.com`，那么会优先匹配 `*-bar.foo.com`。
  - 最后匹配任意domain的通配符 `*` 。
- `routers`，一组具体的路由匹配规则。

## Router

```json
{
  "match":{},
  "route":{},
  "redirect":{},
  "direct_response":{},
  "per_filter_config":{}
}
```

- `match`，路由的匹配参数。
- `route`，路由行为，描述请求将被路由的 upstream 信息。
- `redirect`，路由行为，直接转发。
- `direct_response`， 路由行为，直接响应。
- `per_filter_config`，是一个 `key: json` 格式的 json。
- 其中 key 需要匹配一个 stream filter 的 type，key 对应的 json 是该 stream filter 的 config。
  - 当配置了该字段时，对于某些 stream filter（依赖具体 filter 的实现），可以使用该字段表示的配置覆盖原有 stream filter 的配置，以此做到路由匹配级别的 stream filter 配置。

## match

```json
{
  "prefix":"",
  "path":"",
  "regex":"",
  "headers": [],
  "variables": [],
  "dsl_expressions": []
}
```

- 路径（path）匹配
  - `prefix`，表示路由会匹配 path 的前缀，该配置的优先级高于 path 和 regex。 如果 prefix 被配置，那么请求首先要满足 path 的前缀与 prefix 配置相符合。
  - `path`，表示路由会匹配精确的 path，该配置的优先级高于 regex。如果 path被配置，那么请求首先要满足 path 与 path 配置相符合。
  - `regex`，表示路由会按照正则匹配的方式匹配 path。如果 regex 被配置，那么请求首先要满足 path 与 regex 配置相符合。
  - 路径匹配配置同时存在时，只有高优先级的配置会生效。
- Header 匹配
  - headers，表示一组请求需要匹配的 header。请求需要满足配置中所有的 Header 配置条件才算匹配成功。
- Variable 匹配
  - variables，表示一组请求需要匹配的 variable，请求需要满足配置中所有的 variable 配置条件才算匹配成功。
- DSL 匹配
  - dsl_expressions，表示一组请求需要匹配的 dsl，请求满足配置条件才算匹配成功。

### header

```json
{
  "name":"",
  "value":"",
  "regex":""
}
```

- `name`，表示 header 的 key。
- `value`，表示 header 对应 key 的 value。
- `regex`，bool 类型，如果为 true，表示 value 支持按照正则表达式的方式进行匹配。

### variable

```json
{
  "name":"",
  "value":"",
  "regex":"",
  "model":""
}
```

- `name`，表示 variable 的 key。
- `value`，表示 variable 对应 key 的 value。
- `regex`，表示按照正则表达式的方式进行匹配。
- `model`，可配置 "and" 和 “or”。



## route

```
{
  "cluster_name":"",
  "cluster_variable":"",
  "metadata_match":"",
  "timeout":"",
  "retry_policy":{},
  "prefix_rewrite":"",
  "regex_rewrite":"",
  "host_rewrite":"",
  "request_headers_to_add":[],
  "request_headers_to_remove":[],
  "response_headers_to_add":[],
  "response_headers_to_remove":[]
}
```
满足`match`之后的路由策略。
- `cluster_name`，表示请求将路由到的 upstream cluster。
- `cluster_variable`，表示请求将路由到的变量指定的 upstream cluster，可动态设置变量路由到不同的后端。
- `metadata_match`，[metadata](../../custom#metadata)，如果配置了该字段，表示该路由会基于该 metadata 去匹配 upstream cluster 的 subset 。
- `timeout`，[Duration String](../../custom#duration-string)，表示默认情况下请求转发的超时时间。如果请求中明确指定了超时时间，那么这个配置会被忽略。
- `retry_policy`，重试配置，表示如果请求在遇到了特定的错误时采取的重试策略，默认没有配置的情况下，表示没有重试。

## redirect

```json
{
	"response_code":"",
	"path_redirect":"",
	"host_redirect":"",
	"scheme_redirect":""
}
```
满足 match 条件之后，对请求进行跳转。
- `response_code`，跳转的 HTTP code，默认为 `301`。
- `path_redirect`，修改跳转的 `path`。
- `host_redirect`，修改跳转的 `host`。
- `scheme_redirect`，修改跳转的 `scheme`。

## direct_response

```json
{
  "status":"",
  "body":""
}
```
直接回复响应， `status`是状态码，`body`是内容。

## retry_policy

```json
{
  "retry_on":"",
  "retry_timeout":"",
  "num_retries":""
}
```

- `retry_on`，bool 类型，表示是否开启重试。
- `retry_timeout`，[Duration String](../../custom#duration-string)，表示每次重试的超时时间。当 `retry_timeout` 大于 route 配置的 timeout 或者请求明确指定的 timeout 时，属于无效配置。
- `num_retries`，表示最大的重试次数。



## 例子
### 默认匹配规则
所有请求转发到名字为 `backend` 的 cluster 集群。
```
"routers": [
    {
        "match":{
            "prefix":"/"
        },
        "route": {
           "cluster_name": "backend"
        }
    }
]
```
### 匹配规则 - 路径
请求 `/index.html` 转发到名字为 `backend` 的 cluster 集群。
```
"routers": [
    {
        "match":{
            "path":"/index.html"
        },
        "route": {
           "cluster_name": "backend"
        }
    }
]
```

### 匹配规则 - 正则
数字开头的请求转发到名字为 `backend` 的 cluster 集群。
```
"routers": [
    {
        "match":{
            "regex":"^/\\d+"
        },
        "route": {
           "cluster_name": "backend"
        }
    }
]
```

### 匹配规则 - header
包含 `a:b` header的请求转发到名字为 `backend` 的 cluster 集群。
```
"routers": [
    {
        "match":{
            "headers": [{
               "name":"a",
               "value":"b",
	       "regex":false
            }]
        },
        "route": {
           "cluster_name": "backend"
        }
    }
]
```

### 匹配规则 - 变量
可以通过 filter 设置新的变量，以及 MOSN 内置的变量，进行路由转发规则。
如下例子就是变量 `x-mosn-path`（ MOSN 内置变量，表示请求的 `path`） 等于 `/b` 满足匹配。
```
"routers": [
    {
        "match":{
            "variables": [{
               "name":"x-mosn-path",
               "value":"/b"
            }]
        },
        "route": {
           "cluster_name": "backend"
        }
    }
]
```

### 匹配动作 - redirect
除了转发到 cluster 之外，也支持 redirect 的匹配动作。
下例将 `301` 跳转，`Location: http://test/b`
```
"routers": [
    {
        "match":{
           "prefix": "/"
        },
       "redirect":{
           "response_code":301,
           "path_redirect":"/b",
           "host_redirect":"test",
           "scheme_redirect":"http"
        }
    }
]
```

### 匹配动作 - 直接响应
满足匹配条件直接响应请求。
```
"routers": [
    {
        "match":{
           "prefix": "/"
        },
       "direct_response":{
           "status":404,
           "body":"no found"
        }
    }
]
```
