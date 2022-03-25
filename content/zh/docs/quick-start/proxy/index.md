---
title: "快速开始"
linkTitle: "快速开始"
weight: 1
date: 2020-01-20
aliases: "/zh/docs/quick-start/proxy"
description: >
  快速搭建开发环境，完成编译，测试，镜像制作和示例配置的运行。
---

本文用于帮助初次接触 MOSN 项目的开发人员，快速搭建开发环境，完成编译，测试，镜像制作和示例配置的运行。

## 准备运行环境

+ 如果您使用容器运行 MOSN，请先 [安装 docker](https://docs.docker.com/install/)
+ 如果您使用本地机器，请使用类 Unix 环境
+ 安装 Go 的编译环境 

## 获取代码

MOSN 项目的代码托管在 [Github](https://github.com/mosn/mosn)，获取方式如下：

```bash
git clone git@github.com:mosn/mosn.git
```

## 导入IDE

使用您喜爱的 Go IDE 导入 mosn 项目，推荐 Goland。

## 编译代码

在项目根目录下，根据自己机器的类型以及欲执行二进制的环境，选择以下命令编译 MOSN 的二进制文件。

完成后可以在 `build/bundles/${version}/binary` 目录下找到编译好的二进制文件。

### 使用 docker 镜像编译

```bash
make build // 编译出 linux 64bit 可运行二进制文件
```

### 本地编译

```bash
make build-local // 编译 当前本地系统 可运行二进制文件
```

## 运行测试

支持两种环境来运行测试，如果有 docker 环境的，推荐使用 docker 环境，环境更干净可控。
MOSN 项目集成的 CI 是使用的 docker 环境来运行的。

### 使用 docker 环境运行测试

在项目根目录下执行如下命令：

```bash
# 单元测试
make unit-test
# 集成测试（较慢）
make integrate
# 新版集成测试（较慢）
make integrate-new
```

### 使用本地环境运行测试

在项目根目录下执行如下命令：

```bash
# 单元测试
make ut-local
# 集成测试（较慢）
make integrate-local
# 新版集成测试（较慢）
make integrate-framework
```

## 运行 MOSN

运行下面的命令，将使用一个 [示例配置文件](https://github.com/mosn/mosn/blob/master/configs/mosn_config.json) 启动 MOSN。

```bash
./build/bundles/${version}/binary start -c configs/mosn_config.json
```

### MOSN 配置说明

这个示例，我们模拟了经典的 service mesh 中，MOSN 作为 sidecar 的场景。

```
      POD A                      POD B
------------------        -------------------
| App A => MOSN A |  ==>  | MOSN B => App B |
-------------------       -------------------
```

建议打开 [示例配置文件](https://github.com/mosn/mosn/blob/master/configs/mosn_config.json) ，阅读如下配置说明。

### 应用服务

其中，`appListener` 这个 listener 监听了 `2047` 端口，使用 `application` 这个 router，
router 内配置了 `direct_response`，输出静态配置内容。
在这个示例里，模拟一个应用服务，`App B`。

我们可以使用如下命令测试：

```shell
$ curl 'http://localhost:2047/'
Welcome to MOSN!
The Cloud-Native Network Proxy Platform.
```

### 流量代理转发

其中，`serverListener` 这个 listener 监听了 `2046` 端口，使用 `server_router` 这个 router，
router 内配置启用了 `proxy` 这个 filter，转发到 `serverCluster` 这个 cluster。

在 `cluster_manager` 中可以看到 `serverCluster` 的具体配置：`127.0.0.1:2047`，也就是上面的 `App B`。
`serverListener` 在示例里模拟了 `MOSN B`，代理了 `App B` 的入口流量。

`clientListener` 和 `serverListener` 类似，由 `2045` 端口转发到 `2046`。
模拟了 `MOSN A`，代理了 `App A` 的入口流量。

剩下 `App A`，我们可以通过 `curl` 来模拟：

```shell
$ curl 'http://localhost:2045/'
Welcome to MOSN!
The Cloud-Native Network Proxy Platform.
```

以上就构建了 MOSN 作为 sidecar 的典型使用示例。
其他更多场景的用法，请参考 [配置概览](../../configuration/)。

## 创建镜像

执行如下命令创建 docker image

```bash
make image
```

## 更多 MOSN 示例程序

参考 `examples` 目录下的示例工程[运行 Samples](../../samples)。

## 使用 MOSN 搭建 Service Mesh 平台

请参考[与 Istio 集成](../istio)。
