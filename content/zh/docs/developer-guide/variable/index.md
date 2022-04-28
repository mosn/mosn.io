---
title: "MOSN 变量机制"
linkTitle: "MOSN 变量机制"
weight: 2
date: 2022-03-25
author: "[叶永杰](https://github.com/antJack)"
aliases: "/zh/docs/developer-guide/variable"
description: >
  本页介绍了 MOSN 的变量机制。
---


# 一、前言

MOSN 为开发者提供了灵活的变量机制，用户通过变量机制能够实现以下目的：

- 获取请求上下文的相关信息
- 在一次请求的生命周期内传递自定义数据 (跨 Filter 传递数据)
- 影响 MOSN 路由框架的运行结果


# 二、快速开始

假设我们现在正在开发一个 `simpleFilter`，该 Filter 处理 Http 请求，且需要实现以下功能：

- 判断请求的 `Method`，只接受`Post`请求 
- 修改 `Post`请求 URL 中的 `Query String`

显然，通过请求的 `Header`、`Body` 和 `Trailer`是无法获取请求的`Method`信息，也无法修改请求的 URL，但我们可以通过变量机制实现以上功能：

```go
func (f *simpleFilter) OnReceive(ctx context.Context, headers api.HeaderMap, buf buffer.IoBuffer, trailers api.HeaderMap) api.StreamFilterStatus {   
    // 1. 通过变量机制获取请求 Method
    method, err := variable.GetString(ctx, types.VarMethod)
    if err != nil {
        return api.StreamFilterStop
    }

    // 2. 拦截非 Post 请求
    if method != fasthttp.MethodPost {
        return api.StreamFilterStop
    }

    // 3. 修改请求 URL 中的 Query String
    variable.SetString(ctx, types.VarQueryString, "hello=world&foo=bar")
    
	return api.StreamFilterContinue
}
```

从上述简单例子可见，MOSN 变量机制允许开发者灵活地获取请求上下文的相关信息，并按需修改请求的内容。当然，变量机制的功能远不止上述 Filter 展现的能力，下文将详细介绍变量机制的方方面面。


# 三、现有变量

上文的例子展示了如何通过变量机制获取请求的 `Method`信息。除此之外，MOSN 还提供了大量变量，用于提供请求的上下文信息。

下面的表格中，`变量名`一列方便开发者在编写代码的过程中获取变量内容；`字符串值`一列则方便在配置文件中获取变量值 (例如配置 `access_log`时使用 `%bytes_received%` 获取变量内容)。

需要注意的是：

1. 大部分变量通常具有只读属性，若不清楚后续影响，请不要试图修改变量值
1. MOSN 的变量机制支持任意类型 `interface{}`的变量，但目前提供的绝大多数变量都是 string 类型的，下文如无特殊备注，均为 string 类型。


###  3.1 通用变量

| 变量名 | 字符串值 | 含义 | 备注 |
| --- | --- | --- | --- |
| VarStartTime | "start_time" | 请求开始时间，七层 stream 开始创建的时间 | 格式："2006/01/02 15:04:05.000"。|  |
| VarRequestReceivedDuration | "request_received_duration" | 接收 downstream 请求的耗时，从请求开始，到开始向 upstream 转发请求为止的耗时 |  |
| VarResponseReceivedDuration | "response_received_duration" | 接收 upstream 响应的耗时，从请求开始，到收到 upstream 响应为止的耗时 |  |
| VarRequestFinishedDuration | "request_finished_duration" | 请求耗时，从请求开始，到请求结束为止的耗时 |  |
| VarBytesSent | "bytes_sent" | 发出的字节数 |  |
| VarBytesReceived | "bytes_received" | 收到的字节数 |  |
| VarProtocol | "protocol" | 请求的具体协议 |  |
| VarResponseCode | "response_code" | 响应码 |  |
| VarDuration | "duration" | 请求开始至今的耗时 |  |
| VarResponseFlag | "response_flag" | 响应 flag |  |
| VarResponseFlags | "response_flags" | 响应 flags |  "response_flag" 和 "response_flags" 两个变量的内容完全一致，后者 "flags" 是为了与 istio 日志格式保持一致，[参考](https://github.com/mosn/mosn/commit/f46acc354897e9b1ca20e91c43871a167b4eba27) |
| VarUpstreamLocalAddress | "upstream_local_address" | upstream 本端地址 |  |
| VarDownstreamLocalAddress | "downstream_local_address" | downstream 本端地址 |  |
| VarDownstreamRemoteAddress | "downstream_remote_address" | downstream 远端地址 |  |
| VarUpstreamHost | "upstream_host" | upstream 主机名 |  |
| VarUpstreamTransportFailureReason | "upstream_transport_failure_reason" | upstream 传输失败原因 |  |
| VarUpstreamCluster | "upstream_cluster" | upstream 集群名 |  |
| VarProtocolConfig | "protocol_config" | 请求的协议配置 |  |
|  |  |  |  |
| VarProxyTryTimeout | "proxy_try_timeout" | 每次 upstream 请求的超时时间 | 每个 upstream 请求的超时时间 (包含正常请求和重试请求)  |
| VarProxyGlobalTimeout | "proxy_global_timeout" | 所有 upstream 请求的总超时时间 | 所有 upstream 请求的总超时 (包含正常请求和重试请求) |
| VarProxyHijackStatus | "proxy_hijack_status" | hijack 状态 |  |
| VarProxyGzipSwitch | "proxy_gzip_switch" | gzip 开关 | 用于 `pkg/filter/stream/gzip`  |
| VarProxyIsDirectResponse | "proxy_direct_response" | 是否是 direct response |  |
| VarProxyDisableRetry | "proxy_disable_retry" | 是否不允许请求重试 |  |
| VarDirection | "x-mosn-direction" | 流向 |  |
| VarScheme | "x-mosn-scheme" | 请求 Scheme |  |
| VarHost | "x-mosn-host" | 请求 Host |  |
| VarPath | "x-mosn-path" | 请求 Path | 已转义，human-readable |
| VarPathOriginal | "x-mosn-path-original" | 请求原始 Path | 
 |
| VarQueryString | "x-mosn-querystring" | 请求 Query |  |
| VarMethod | "x-mosn-method" | 请求 Method |  |
| VarIstioHeaderHost | "authority" | 请求 Host | 内容同 VarHost，与 istio 保持一致 |
| VarHeaderStatus | "x-mosn-status" | 状态码 |  |
| VarHeaderRPCService | "x-mosn-rpc-service" | RPC Service |  |
| VarHeaderRPCMethod | "x-mosn-rpc-method" | RPC Method |  |
|  |  |  |  |
| VarRouterMeta | "x-mosn-router-meta" | 路由 meta |  |


### 3.2 Http1 协议变量

| 变量名 | 字符串值 | 含义 | 备注 |
| --- | --- | --- | --- |
| VarHttpRequestScheme | "Http1_request_scheme" | 请求 Scheme |  |
| VarHttpRequestMethod | "Http1_request_method" | 请求 Method |  |
| VarHttpRequestLength | "Http1_request_length" | 请求长度 |  |
| VarHttpRequestUri | "Http1_request_uri" | 请求 URI |  |
| VarHttpRequestPath | "Http1_request_path" | 请求 Path |  |
| VarHttpRequestPathOriginal | "Http1_request_path_original" | 请求原始 Path |  |
| VarHttpRequestArg | "Http1_request_arg" | 请求 Query |  |
|  |  |  |  |
| VarPrefixHttpHeader | "Http1_request\_header\_" | 获取请求 Header 值 | 前缀变量，需 + "headerName" 获取对应 header 的值 |
| VarPrefixHttpArg | "Http1_request\_arg\_" | 获取请求 Query 值  | 前缀变量，需 + "QueryName" 获取对应 query 的值 |
| VarPrefixHttpCookie | "Http1\_cookie\_" | 获取请求 Cookie 值 | 前缀变量，需 + "CookieName" 获取对应 cookie 的值 |


### 3.3 Http2 协议变量

| 变量名 | 字符串值 | 含义 | 备注 |
| --- | --- | --- | --- |
| VarHttp2RequestScheme | "Http2_request_scheme" | 请求 Scheme |  |
| VarHttp2RequestMethod | "Http2_request_method" | 请求 Method | 未实现 |
| VarHttp2RequestLength | "Http2_request_length" | 请求长度 | 未实现 |
| VarHttp2RequestUri | "Http2_request_uri" | 请求 URI | 未实现 |
| VarHttp2RequestPath | "Http2_request_path" | 请求 Path | 未实现 |
| VarHttp2RequestPathOriginal | "Http2_request_path_original" | 请求原始 Path | 未实现 |
| VarHttp2RequestArg | "Http2_request_arg" | 请求 Query | 未实现 |
| VarHttp2RequestUseStream | "Http2_request_use_stream" | 请求是否为流式 |  |
| VarHttp2ResponseUseStream | "Http2_response_use_stream" | 响应是否为流式 |  |
|  |  |  |  |
| VarPrefixHttp2Header | "Http2_request\_header\_" | 获取请求 Header 值 | 前缀变量，需 + "headerName" 获取对应 header 的值 |
| VarPrefixHttp2Arg | "Http2_request\_arg\_" | 获取请求 Query 值  | 未实现 |
| VarPrefixHttp2Cookie | "Http2\_cookie\_" | 获取请求 Cookie 值 | 前缀变量，需 + "CookieName" 获取对应 cookie 的值 |


### 3.4 其它变量

MOSN 还包含一些特殊模块的变量，对于这些变量，基本原则是：如果你不知道要用这些变量，那就不要用

| 变量名 | 字符串值 | 含义 | 备注 |
| --- | --- | --- | --- |
| grpc.VarGrpcServiceName | "gRPC_serviceName" | GRPC 请求 Service |  |
| grpc.VarGrpcRequestResult | "requestResult" | GRPC 请求结果 |  |
|  |  |  |  |
| dubbo.VarDubboRequestService | "Dubbo_service" | Dubbo 请求 Service |  |
| dubbo.VarDubboRequestMethod | "Dubbo_method" | Dubbo 请求方法 |  |
|  |  |  |  |
| seata.XID | "x_seata_xid" |  |  |
| seata.BranchID | "x_seata_branch_id" |  |  |


# 四、增加自定义变量

上文罗列了 MOSN 中已经预定义的所有变量，基本能够满足 MOSN 路由转发场景的需求，若仍有未能满足的定制化需求，可以考虑通过自定义变量的方式来实现。

一个典型的使用场景是跨 Filter 传递数据，即 `Filter1` 处理完请求后希望将自定义数据传递给后续 `Filter2、3` 处理。实现这一需求的方法有以下几种：

- 方案一：使用请求 Header 携带自定义数据
- 方案二：使用请求 ctx 携带自定义数据

方案一的缺点在于会污染用户的原始请求，`Filter1`塞进 Header 的数据需要在后续 `Filter`中清理掉，否则就是被带到上游服务端。此外，受限于 Header 值的类型是 string，方案一无法携带非 string 类型的数据。

方案二的缺点在于性能，ctx 的底层实现是单链表结构，每使用 `context.WithValue`添加一个数据时均会将单链表延长一节，且从 ctx 获取数据时，需要遍历单链表，时间复杂度为 `O(n)`

MOSN 提供的变量机制即是解决上述问题的推荐方案。使用变量机制传递自定义数据具有以下优点：

- 数据类型无限制：除了最基础的 string 类型外，支持 interface{} 类型的自定义数据
- 性能优异：读写自定义变量的时间复杂度均为 `O(1)`
- 无侵入性：不侵入用户原始请求、不侵入其它 Filter

MOSN 变量框架提供以下 API 来操作变量：
本节所有 API 均位于 [mosn.io/mosn/pkg/variable](https://github.com/mosn/mosn/tree/master/pkg/variable) 包

### 4.1 注册变量

```go
// 注册新变量
// 需要在 init 函数中调用，否则可能不生效
func Register(variable Variable) error

// 注册前缀变量
// 需要在 init 函数中调用，否则可能不生效
func RegisterPrefix(prefix string, variable Variable) error

// 检查是否已注册某变量
func Check(name string) (Variable, error)
```

### 4.2 创建变量

Variable 类型的变量通过以下 API 创建：

```go
// 新建 string 类型变量
func NewStringVariable(name string, data interface{}, getter StringGetterFunc, setter StringSetterFunc, flags uint32) Variable

// 新建 interface{} 类型变量
func NewVariable(name string, data interface{}, getter GetterFunc, setter SetterFunc, flags uint32) Variable
```

对于绝大多数 (99.99%) 使用场景而言，并不需要去理解上述两个 API 的所有入参的含义，MOSN 变量框架提供了默认实现，直接 Ctrl-CV 即可：

```go
// 方式一：
// 创建并注册新变量：Hello_Variable_1
variable.NewVariable("Hello_Variable_1", nil, nil, variable.DefaultSetter, 0)

// 方式二：
// 创建并注册新变量：Hello_Variable_2
variable.NewVariable("Hello_Variable_2", nil, valueGetter, nil, 0)
```

上述两种方式的主要区别在于新创建的变量是否有初始值：

- 方式一创建的变量没有初始值，需要先调用 `Set`方法设置内容后，`Get`方法才能获取到
- 方式二创建的变量有初始值，初始值由自定义函数 `valueGetter`提供，无需先 `Set`便可获取到内容

方式一通常更符合自定义变量的使用场景，即自定义变量需要先 `Set`后 `Get`

方式二则是 MOSN 框架自身常用的用法，例如：对于 `"Http1_request_method"`这个变量而言，① 它是有初始值的，无需先`Set`；② 它的获取方式较为复杂，需要用底层 Http 请求的结构体中获取，因此在 MOSN 中，该变量的创建方式为：

> variable.NewStringVariable(types._VarHttpRequestMethod_, nil, requestMethodGetter, nil, 0)

其中 `requestMethodGetter`便是从 Http 请求的底层结构体获取 Method 值。

### 4.3 使用变量

一旦注册好变量，便可通过以下 API 来读写变量：

```go
// 获取 string 类型变量值
func GetString(ctx context.Context, name string) (string, error)

// 设置 string 类型变量值
func SetString(ctx context.Context, name, value string) error

// 获取 interface{} 类型变量值，string 类型变量也可通过该 API 获取，Set 同理
func Get(ctx context.Context, name string) (interface{}, error)

// 设置 interface{} 类型变量值
func Set(ctx context.Context, name string, value interface{}) error

// 创建 VariableContext
// 需要注意的是，使用变量机制时，ctx 必须是以下函数创建的 VariableContext
// MOSN 框架中的 ctx 已经默认是 VariableContext，无需额外调用该函数。但在单测场景，需要使用该函数创建 VariableContext，否则无法正常使用变量
func NewVariableContext(ctx context.Context) context.Context
```

# 五、通过变量机制与 MOSN 路由框架进行交互

变量机制除了上文所展现的 “数据搬运工” 的身份外，还具备影响 MOSN 核心路由框架的能力。MOSN 核心路由框架本身会在路由过程中读取某些变量来进行路由决策，因此，开发者可以直接通过变量机制来影响 MOSN 路由的结果。MOSN 变量机制支持以下能力：

### 5.1 通过变量修改路由元信息

| 变量名 | 含义 | 详细解释 |
| --- | --- | --- |
| VarProxyTryTimeout | 每次 upstream 请求的超时时间 | 用于设置每个 upstream 请求的超时时间 (单个正常和重试请求的超时)  |
| VarProxyGlobalTimeout | 所有 upstream 请求的总超时时间 | 用于设置所有 upstream 请求的总超时 (正常 + 重试请求的总超时) |
| VarProxyDisableRetry | 是否不允许请求重试 | upstream 请求失败时，是否允许重试 |
| VarRouterMeta | 路由 meta | 该变量设置的 metadata 后续会被用于 MOSN 路由时选择与之匹配的 Host。典型应用场景是 [header_to_metadata](https://github.com/mosn/mosn/tree/master/pkg/filter/stream/headertometadata) 这个 Filter，它将请求 Header 中的数据作为 metadata 塞入变量中，用于后续的 MOSN 路由，使用者便可直接通过 Header 值来控制路由结果。 |

### 5.2 通过变量指定路由集群

用户可以在 mosn.json 配置中指定用于匹配路由集群的变量名，例如：
```
{
	"routers": [{
		"router_config_name": "hello_mosn",
		"virtual_hosts": [{
			"name": "mosnHost",
			"domains": ["*"],
			"routers": [{
				"match": {"prefix": "/"},
				"route": {"cluster_variable": "VAR_NAME"}
			}]
		}]
	}]
}
```

在上述配置下，MOSN 在路由时会获取变量 `VAR_NAME`的值，并将请求路由到该变量值对应的 Cluster 上：用户可以在路由前 (`BeforeRoute`) 的 Filter 中，通过以下代码让请求被路由到 `dst_cluster`这个集群上：

> variable.SetString(ctx, VAR_NAME, "dst_cluster")


### 5.3 通过变量影响路由匹配结果

用于可以在 mosn.json 配置中指定用于匹配路由的变量名，例如：

```json
{
	"routers": [{
		"router_config_name": "hello_mosn",
		"virtual_hosts": [{
			"name": "mosnHost",
			"domains": ["*"],
			"routers": [{
				"match": {
					"variables": [{
						"name": "VAR_NAME",
						"value": "VAR_VALUE"
					}]
				},
				"route": {"cluster_name": "dst_cluster"}
			}]
		}]
	}]
}
```

在上述配置下，MOSN 在路由时，会将具有变量 `VAR_NAME`且变量值为 `VAR_VALUE`的请求转发到 `dst_cluster`这个集群上

# 六、总结

综上，变量机制是 MOSN 提供高可扩展性的重要一环，本文对 MOSN 中的变量机制进行了详细介绍，由浅及深，从介绍现有变量开始，到增加自定义变量，再到使用变量与 MOSN 核心路由框架进行交互，供用户一窥变量机制的全貌。
