---
title: "MOSN 简介"
linkTitle: "简介"
weight: 1
date: 2022-03-28
aliases: "/zh/docs/overview"
description: >
  MOSN 基础介绍。
---

MOSN（Modular Open Smart Network）是一款主要使用 Go 语言开发的云原生网络代理平台，由蚂蚁集团开源并经过双 11 大促几十万容器的生产级验证。
MOSN 为服务提供多协议、模块化、智能化、安全的代理能力，融合了大量云原生通用组件，同时也可以集成 Envoy 作为网络库，具备高性能、易扩展的特点。
MOSN 可以和 Istio 集成构建 Service Mesh，也可以作为独立的四、七层负载均衡，API Gateway、云原生 Ingress 等使用。

## 快速开始

请参考[快速开始](../quick-start)。

## 核心能力

+ Istio 集成
    + 集成 Istio 1.10 版本，可基于全动态资源配置运行
+ 核心转发
    + 自包含的网络服务器
    + 支持 TCP 代理
    + 支持 UDP 代理
    + 支持透明劫持模式
+ 多协议
    + 支持 HTTP/1.1，HTTP/2
    + 支持基于 XProtocol 框架的多协议扩展
    + 支持多协议自动识别
    + 支持 gRPC 协议
+ 核心路由
    + 支持基于 Domain 的 VirtualHost 路由
    + 支持 Headers/Path/Prefix/Variable/DSL 等多种匹配条件的路由
    + 支持重定向、直接响应、流量镜像模式的路由
    + 支持基于 Metadata 的分组路由、支持基于权重的路由
    + 支持基于路由匹配的重试、超时配置
    + 支持基于路由匹配的请求头、响应头处理
+ 后端管理 & 负载均衡
    + 支持连接池管理
    + 支持长连接心跳处理
    + 支持熔断、支持后端主动健康检查
    + 支持 Random/RR/WRR/EDF 等多种负载均衡策略
    + 支持基于 Metadata 的分组负载均衡策略
    + 支持 OriginalDst/DNS/SIMPLE 等多种后端集群模式，支持自定义扩展集群模式
+ 可观察性
    + 支持格式可扩展的 Trace 模块，集成了 jaeger/skywalking 等框架
    + 支持基于 prometheus 的 metrics 格式数据
    + 支持可配置的 AccessLog
    + 支持可扩展的 Admin API
    + 集成 [Holmes](https://github.com/mosn/holmes)，自动监控 pprof
+ TLS
    + 支持多证书匹配模式、支持 TLS Inspector 模式
    + 支持基于 SDS 的动态证书获取、更新机制
    + 支持可扩展的证书获取、更新、校验机制
    + 支持基于 CGo 的国密套件
+ 进程管理
    + 支持平滑升级，包括连接、配置的平滑迁移
    + 支持优雅退出
+ 扩展能力
    + 支持基于 go-plugin 的插件扩展模式的
    + 支持基于进程的扩展模式
    + 支持基于 WASM 的扩展模式
    + 支持自定义扩展配置
    + 支持自定义的四层、七层Filter扩展

## 社区

MOSN 开源仍在高速发展中，有很多能力需要补全，欢迎所有人参与进来与我们一起共建。

关于 MOSN 社区的详细介绍请查看 [mosn/community](https://github.com/mosn/community) 仓库，如有任何疑问欢迎[提交 Issue](https://github.com/mosn/mosn/issues)。
