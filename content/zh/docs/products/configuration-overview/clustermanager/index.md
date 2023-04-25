---
title: "ClusterManager 配置"
linkTitle: "ClusterManager 配置"
date: 2022-03-25
aliases: "zh/docs/configuration/clustermanager"
weight: 2
description:
  MOSN ClusterManager 配置说明。
---

本文是关于 MOSN ClusterManager 配置的说明。

MOSN 中通过 `cluster_manager` 来管理转发的集群地址，通常与 [Router](https://mosn.io/docs/configuration/server/router/#route) 配合使用。

```josn
"cluster_manager":{
  "tls_context":"",
  "cluster_pool_enable": "",
  "clusters_configs":"",
  "clusters":[]
}
```

- `tls_context`，可选配置，用于描述 Cluster 全局共享的 TLS 配置，该配置项需要结合 clusters 配置中的 cluster_manager_tls 配置项一起使用，TLS 详细配置见 [tls_context 文档](https://mosn.io/docs/configuration/custom/#tls_context)。
- `cluster_pool_enable`，可选配置，bool 类型，用于控制所有Cluster是否使用独占的连接池，为 true 则使用Cluster独占连接池，默认值为 false。
- `clusters_configs`，可选配置，字符串类型，用于设置 Cluster 列表从 clusters_configs 指定的文件中解析。
- `clusters`，用于描述每个 Cluster 所采用的负载均衡算法、类型等细节信息。

注意：cluster_manager 中的 clusters 和 clusters_configs 不能同时配置。

## cluster

```josn
{
  "name":"",
  "type":"",
  "sub_type":"",
  "lb_type":"",
  "max_request_per_conn":"",
  "conn_buffer_limit_bytes":"",
  "circuit_breakers":""，
  "health_check":""，
  "spec":""，
  "lb_subset_config":""，
  "original_dst_lb_config":""，
  "cluster_manager_tls":""，
  "tls_context":""，
  "hosts":[],
  "connect_timeout":"",
  "idle_timeout":"",
  "lbconfig":"",
  "dns_refresh_rate":"",
  "respect_dns_ttl":"",
  "dns_lookup_family":"",
  "dns_resolvers":"",
  "dns_resolver_file":"",
  "dns_resolver_port":"",
  "cluster_pool_enable":""
}
```

- `name`，字符串。用作 Cluster 的唯一标识。
- `type`，字符串。用于表示 Cluster 的类型，目前支持的类型如下：
  - SIMPLE，是最基础的类型。
  - ORIGINAL_DST，该种类型的
  - STRICT_DNS，该种类型会动态解析 Cluster 中的域名列表，并将域名对应的 A 记录全部加入转发列表中。
- `sub_type`，已废弃。
- `lb_type`，字符串。在集群中选择主机时使用的负载平衡器类型，目前支持的类型如下：
  - LB_ROUNDROBIN，不带权重的轮训转发。
  - LB_RANDOM，随机转发。
  - LB_WEIGHTED_ROUNDROBIN，根据 host 的权重转发。
  - LB_ORIGINAL_DST，在透明劫持场景下使用原始目标地址做转发，也可以通过请求 header 设置目标地址。
  - LB_LEAST_REQUEST，选择请求数最少的 host 转发。
  - LB_MAGLEV，一致性 hash 转发。
  - LB_REQUEST_ROUNDROBIN，同一个请求粒度的轮训转发。
  - LB_LEAST_CONNECTION，选择连接数最少的 host 转发。
- `max_request_per_conn`，uint32 类型，暂未实现。
- `conn_buffer_limit_bytes`，uint32 类型，暂未实现。
- `circuit_breakers`，CircuitBreakers 类型，既 [Thresholds](#Thresholds) 类型的数组，用于配置 Cluster 的熔断配置。
- `health_check`，[HealthCheck](#HealthCheck) 类型，群集可选的健康检查配置。如果未指定该配置，则不会执行主动健康检查，且默认群集中的成员都将是健康状态。
- `spec`，暂未使用。
- `lb_subset_config`，[LBSubsetConfig](#LBSubsetConfig) 类型，用于配置负载均衡的子集。
- `original_dst_lb_config`，[LBOriDstConfig](#LBOriDstConfig) 类型，用于配置 LB_ORIGINAL_DST 类型的负载均衡器配置。
- `cluster_manager_tls`，bool 类型，用于控制每个 Cluster 是否使用全局共享的 TLS 配置，为 true 则共享，默认值为 false。
- `tls_context`，TLSConfig 类型，连接到上游群集的 TLS 配置。若没有指定 TLS 配置，则新连接不会使用 TLS，配置实例参考 [tls_context 文档](https://mosn.io/docs/configuration/custom/#tls_context)。
- `hosts`，[][Host](#Host)  类型，用于配置 Cluster 中的机器列表。
- `connect_timeout`，[Duration](../custom#duration-string) 类型，连接到该群集中主机的超时时长，默认值为 3 秒。
- `idle_timeout`，[Duration](../custom#duration-string) 类型，用于设置 Cluster 中的连接空闲超时时间，若发生超时则会断开连接，中默认值为 0，表示不设置连接的空闲超时。
- `lbconfig`，为负载均衡器提供的扩展配置，目前只有 LB_LEAST_REQUEST 类型的负载均衡器有使用。
- `dns_refresh_rate`，[Duration](../custom#duration-string) 类型，在群集类型是 STRICT_DNS 时，用于设置 DNS 刷新频率，此值默认为 5 秒。
- `respect_dns_ttl`，bool 类型，用于设置当 Cluster 类型为 STRICT_DNS 时，其域名对应的解析频率是否遵循 DNS 返回的 TTL。
- `dns_lookup_family`，字符串类型，DNS IP 地址解析策略。 如果未指定此设置，则该值默认为 V4_ONLY。取值列表如下：
   - V4_ONLY，表示只解析 IPv4
   - V6_ONLY，表示只解析 IPv6
- `dns_resolvers`，[DnsResolverConfig](#DnsResolverConfig) 类型，在群集类型是 STRICT_DNS，此值用于指定群集的 DNS 解析相关配置。
- `dns_resolver_file`，字符串类型，用于设置 DNS server 列表的文件路径，该值默认为使用 /etc/resolv.conf 配置的默认解析器，该配置项仅在 dns_resolvers 未配置时生效。
- `dns_resolver_port`，字符串类型，用于设置 DNS server 地址的 port，默认值为 53，该配置项仅在 dns_resolvers 未配置时生效。
- `cluster_pool_enable`，bool 类型，用于控制当前Cluster是否使用独占的连接池，为 true 则使用独占连接池，默认值为 false。

## Thresholds

```josn
{
  "max_connections":"",
  "max_pending_requests":"",
  "max_requests":"",
  "max_retries":""
}
```

- `max_connections`，uint32 类型。用于设置 Cluster 中每台机器的最大连接数，对于 HTTP 协议，超过后会响应 502，对于多路复用协议则是控制单个 host 建立的最大连接数。默认值为 0 表示不启用该配置。
- `max_pending_requests`，uint32 类型。代表 Cluster 的最大排队数量，暂未使用到。
- `max_requests`，uint32 类型。将对上游群集执行的最大并行请求数，若超过限制则会响应 502，目前仅在 HTTP 系协议下生效。默认值为 0 表示不启用该配置。
- `max_retries`，uint32 类型。允许上游集群执行的最大并行重试次数，目前只在 HTTP 系协议下生效。默认值为 0 表示不启用该配置。

## HealthCheck

HealthCheckConfig

```josn
{
  "protocol":"",
  "timeout":"",
  "interval":"",
  "interval_jitter":"",
  "healthy_threshold":"",
  "unhealthy_threshold":"",
  "service_name":"",
  "check_config":"",
  "event_log_path":"",
  "common_callbacks":""
}
```

- `protocol`，字符串类型，用于设置 Cluster 发起健康检查使用的协议类型，目前只支持 TCP。
- `timeout`，[Duration](../custom#duration-string) 类型，等待健康检查响应的时间。如果达到超时，则尝试健康检查将被视为失败。
- `interval`，[Duration](../custom#duration-string) 类型，每次尝试健康检查之间的时间间隔。
- `interval_jitter`，[Duration](../custom#duration-string) 类型，用于设置 interval 的随机抖动量，设置后将抖动量叠加到 interval 上。
- `healthy_threshold`，uint32 类型，主机在标记为健康之前所需的连续健康检查次数，默认值为 1。
- `unhealthy_threshold`，uint32 类型，在主机被标记为不健康之前，需要进行连续不健康的健康检查次数，默认值为 1。
- `service_name`，字符串类型，暂未支持。
- `check_config`，map[string]interface{} 类型，用于健康检查的扩展配置，当前支持 "http_check_config"。
- `event_log_path`, 字符串类型，健康检查日志路径。
- `common_callbacks`，[]string 类型，用于设置对应 Cluster 健康检查时执行的 callback。


## LBSubsetConfig

LBSubsetConfig 主要用于 Cluster 中更为灵活的请求路由，列如 ABTesting、金丝雀发布、单元化等。详细使用可以参考 [MOSN subset 路由实践](https://mosn.io/blog/posts/how-use-dynamic-metadata/#mosn-subset)。


```josn
{
  "fall_back_policy":"",
  "default_subset":"",
  "subset_selectors":""
}
```

- `fall_back_policy`，uint8 类型，用于设置在查找子集群失败时的容灾策略，当前支持如下配置：
   - 0，表示 NoFallBack，没有查找到匹配的子集群则不使用容灾策略
   - 1，表示 AnyEndPoint，既在 Cluster 的 Host 列表中轮训选择目标机器
   - 2，表示使用 DefaultSubset 策略重新查找子集群
- `default_subset`，map[string]string 类型，如果 fallback_policy 为 2 既 DEFAULT_SUBSET，则指定在回退期间使用的端点的默认子集。
- `subset_selectors`，[][]string 类型，用于设置子集群匹配规则查找的条目。


## LBOriDstConfig

```josn
{
  "use_header":"",
  "header_name":""
}
```

- `use_header`，bool 类型，将该配置设置为 true 且负载均衡器使用 LB_ORIGINAL_DST 类型时，则转发的目标地址通过 header_name 从当前请求 header 中获取。
- `header_name`，字符串类型，用于设置目标地址从 header_name 对应的 header 中获取，默认值为 host。


## Host

```josn
{
  "address":"",
  "hostname":""
  "weight":"",
  "metadata":""，
  "tls_disable":""
}
```

- `address`，字符串类型，用于设置集群的地址和端口。
- `hostname`，字符串类型，对应地址的名称，可以是一个域名。
- `weight`，uint32 类型，对应地址的权重。
- `metadata`，*MetadataConfig 类型，用于设置对应机器的 metadata 信息，通常和 LBSubsetConfig 一起使用。
- `tls_disable`，bool 类型，用于标记对应机器是否开启 TLS。

## DnsResolverConfig

```josn
{
  "servers":"",
  "search":""
  "port":"",
  "ndots":""，
  "timeout":""，
  "attempts":""
}
```

- `servers`，[]string 类型，用于设置 DNS 服务器列表。
- `search`，[]string 类型，用于设置和目标域名拼接的后缀列表，结合 ndots 使用。
- `port`，字符串类型，设置发起 DNS 请求的端口，默认值为 53。
- `ndots`，int 类型，用于设置 DNS 查询域名是否将 search 中的列表依次追加到待查询域名末尾，如果该值大于待查询域名中的 “.” 数量，则将待查询域名末尾拼依次接 search 中设置的后缀，默认值为 0。
- `timeout`，int 类型，用于设置 DNS 更新超时时间，单位为秒。
- `attempts`，int 类型，一次 DNS 请求中尝试查询的 DSN server 的次数。
