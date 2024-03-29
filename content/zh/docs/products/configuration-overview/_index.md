---
title: "配置概览"
linkTitle: "配置概览"
date: 2020-01-20
weight: 4
aliases: "/zh/docs/configuration"
description: >
  MOSN 配置文件概览。
---

MOSN 的配置文件可以分为以下四大部分：

- Servers 配置，目前仅支持最多 1 个 Server 的配置，Server 中包含一些基础配置以及对应的 Listener 配置
- `ClusterManager` 配置，包含 MOSN 的 Upstream 详细信息
- 对接控制平面（Pilot）的 xDS 相关配置
- 其他配置
  - Trace、Metrics、Debug、Admin API 相关配置
  - 扩展配置，提供自定义配置扩展需求

**配置文件概览**

MOSN 的基本配置部分如下所示：

```json
{
  "servers": [],
  "cluster_manager": {},
  "dynamic_resources": {},
  "static_resources": {},
  "admin":{},
  "pprof":{},
  "tracing":{},
  "metrics":{}
}
```

## 配置类型

MOSN 的配置包括以下几种类型：

- 静态配置
- 动态配置
- 混合模式

### 静态配置

- 静态配置是指 MOSN 启动时，不对接控制平面 Pilot 的配置，用于一些相对固定的简单场景（如 MOSN 的示例）。
- 使用静态配置启动的 MOSN，也可以通过扩展代码，调用动态更新配置的接口实现动态修改。
- 静态配置启动时必须包含一个 Server 以及至少一个 Cluster。

### 动态配置

- 动态配置是指 MOSN 启动时，只有访问控制平面相关的配置，没有 MOSN 运行时所需要的配置。

- 使用动态配置启动的 MOSN，会向管控面请求获取运行时所需要的配置，管控面也可能在运行时推送更新 MOSN 运行配置。

- 动态配置启动时必须包含 `DynamicResources` 和 `StaticResources` 配置。

### 混合模式

MOSN 启动时的配置可以同时包含静态模式与动态模式，以混合模式启动的 MOSN 会先以静态配置完成初始化，随后可能由控制平面获取配置更新。

## 配置示例

### 静态配置示例

静态配置的示例如下所示。

```json
{
  "servers": [
    {
      "default_log_path": "/home/admin/logs/mosn/default.log",
      "default_log_level": "DEBUG",
      "processor": 4,
      "listeners": [
        {
          "address": "0.0.0.0:12220",
          "bind_port": true,
          "filter_chains": [
            {
              "filters": [
                {
                  "type": "proxy",
                  "config": {
                      "downstream_protocol": "SofaRpc",
                      "upstream_protocol": "SofaRpc",
                      "router_config_name": "test_router"
                  }
                },
                {
                  "type": "connection_manager",
                  "config": {
                       "router_config_name": "test_router",
                      "virtual_hosts": []
                  }
                }
              ]
            }
          ]
        }
      ]
    }
  ],
  "cluster_manager": {
      "clusters": [
        {
          "name":"example",
          "lb_type": "LB_ROUNDROBIN",
          "hosts": [
            {"address": "127.0.0.1:12200"}
          ]
        }
      ]
  }
}
```

### 动态配置示例

动态配置的示例如下所示。

```json
{
  "servers": [
    {
      "default_log_path": "stdout",
      "default_log_level": "DEBUG"
    }
  ],
  "static_resources": {
    "clusters": [
      {
        "connect_timeout": "1s",
        "load_assignment": {
          "cluster_name": "xds_cluster",
          "endpoints": [
            {
              "lb_endpoints": [
                {
                  "endpoint": {
                    "address": {
                      "socket_address": {
                        "address": "127.0.0.1",
                        "port_value": 9002
                      }
                    }
                  }
                }
              ]
            }
          ]
        },
        "http2_protocol_options": {},
        "name": "xds_cluster"
      }
    ]
  },
  "dynamic_resources": {
    "ads_config": {
      "api_type": "GRPC",
      "transport_api_version": "V3",
      "grpc_services": [
        {
          "envoy_grpc": {
            "cluster_name": "xds_cluster"
          }
        }
      ],
      "set_node_on_first_message_only": true
    },
    "cds_config": {
      "resource_api_version": "V3",
      "api_config_source": {
        "api_type": "GRPC",
        "transport_api_version": "V3",
        "grpc_services": [
          {
            "envoy_grpc": {
              "cluster_name": "xds_cluster"
            }
          }
        ],
        "set_node_on_first_message_only": true
      }
    },
    "lds_config": {
      "resource_api_version": "V3",
      "api_config_source": {
        "api_type": "GRPC",
        "transport_api_version": "V3",
        "grpc_services": [
          {
            "envoy_grpc": {
              "cluster_name": "xds_cluster"
            }
          }
        ],
        "set_node_on_first_message_only": true
      }
    }
  },
  "node": {
    "cluster": "test-cluster",
    "id": "test-id"
  },
  "layered_runtime": {
    "layers": [
      {
        "name": "runtime-0",
        "rtds_layer": {
          "rtds_config": {
            "resource_api_version": "V3",
            "api_config_source": {
              "transport_api_version": "V3",
              "api_type": "GRPC",
              "grpc_services": {
                "envoy_grpc": {
                  "cluster_name": "xds_cluster"
                }
              }
            }
          },
          "name": "runtime-0"
        }
      }
    ]
  },
  "admin": {
    "access_log_path": "/dev/null",
    "address": {
      "socket_address": {
        "address": "127.0.0.1",
        "port_value": 9003
      }
    }
  }
}

```
