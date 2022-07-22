---
title: "Configuration overview"
linkTitle: "Configuration overview"
date: 2020-01-20
weight: 4
description: >
  The overview of the MOSN configuration file.
---

The MOSN configuration file comprises the following four parts:

- Server configurations: Currently, only one server can be configured. The server supports some basic configurations and corresponding listener configurations.
- `ClusterManager` configurations: This part supports configuring detailed upstream information of MOSN.
- xDS configurations for connecting to the control plane (Pilot)
- Other configurations
   - Configurations for Trace, Metrics, Debug, and Admin API
   - Extension configurations: the custom extension configurations

**Configuration file overview**

The basic configurations of MOSN are shown as follows.

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

## Configuration modes

MOSN supports the following configuration modes:

- Static configuration
- Dynamic configuration
- Hybrid configuration

### Static configuration

- In the static configuration mode, MOSN starts without connecting to Pilot of the control plane. This mode applies to some relatively fixed and simple scenarios (for example, demonstration of MOSN).
- Even if MOSN starts in the static configuration mode, you can use the extension code to call the dynamic configuration update API to dynamically modify the configuration.
- To start MOSN in the static configuration mode, one server and at least one cluster are required.

### Dynamic configuration

- In the dynamic configuration mode, MOSN starts with only configurations for accessing the control plane, but no configuration for running.

- After starting in dynamic configuration mode, MOSN will request the control plane to provide configurations required for running. The control plane can also push configuration updates when MOSN is running.

- To start MOSN in the dynamic configuration mode, the `DynamicResources` and `StaticResources` configurations are required.

### Hybrid configuration

MOSN can start in a hybrid mode with both static and dynamic configurations. In this mode, MOSN first completes initialization in the static configuration mode and then obtains configuration updates from the control plane.

## Configuration examples

### Static configuration example

The example of static configuration is as follows.

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

### Dynamic configuration example

The example of dynamic configuration is as follows.

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

