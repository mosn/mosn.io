---
title: "MOSN 简介"
linkTitle: "简介"
weight: 1
date: 2020-01-20
aliases: "/zh/docs/overview"
description: >
  MOSN 基础介绍。
---

MOSN（Modular Open Smart Network）是一款主要使用 Go 语言开发的云原生网络代理平台，由蚂蚁集团开源并经过双11大促几十万容器的生产级验证。
MOSN 为服务提供多协议、模块化、智能化、安全的代理能力，融合了大量云原生通用组件，同时也可以集成 Envoy 作为网络库，具备高性能、易扩展的特点。
MOSN 可以和 Istio 集成构建 Service Mesh，也可以作为独立的四、七层负载均衡，API Gateway、云原生 Ingress 等使用。

## 快速开始

请参考[快速开始](../quick-start)。

## 核心能力

+ Istio集成
    + 集成 Istio 1.0 版本与 V4 API，可基于全动态资源配置运行
+ 核心转发
    + 自包含的网络服务器
    + 支持 TCP 代理
    + 支持 TProxy 模式
+ 多协议
    + 支持 HTTP/1.1，HTTP/2
    + 支持 SOFARPC
    + 支持 Dubbo 协议
    + 支持 Tars 协议
+ 核心路由
    + 支持 Virtual Host 路由
    + 支持 Headers/URL/Prefix 路由
    + 支持基于 Host Metadata 的 Subset 路由
    + 支持重试
+ 后端管理&负载均衡
    + 支持连接池
    + 支持熔断
    + 支持后端主动健康检查
    + 支持 Random/RR 等负载策略
    + 支持基于 Host Metadata 的 Subset 负载策略
+ 可观察性
    + 观察网络数据
    + 观察协议数据
+ TLS
    + 支持 HTTP/1.1 on TLS
    + 支持 HTTP/2.0 on TLS
    + 支持 SOFARPC on TLS
+ 进程管理
    + 支持平滑 reload
    + 支持平滑升级
+ 扩展能力
    + 支持自定义私有协议
    + 支持在 TCP IO 层，协议层面加入自定义扩展

## 社区

MOSN 开源仍在高速发展中，有很多能力需要补全，欢迎所有人参与进来与我们一起共建。

关于 MOSN 社区的详细介绍请查看 [mosn/community](https://github.com/mosn/community) 仓库，如有任何疑问欢迎[提交 Issue](https://github.com/mosn/mosn/issues)。
