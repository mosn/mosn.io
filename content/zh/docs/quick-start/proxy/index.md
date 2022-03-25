---
title: "快速开始"
linkTitle: "快速开始"
weight: 1
date: 2022-03-25
aliases: "/zh/docs/quick-start/proxy"
description: >
  快速搭建开发环境，完成构建，测试，打包和示例代码的运行。
---

本文用于帮助初次接触 MOSN 项目的开发人员，快速搭建开发环境，完成构建，测试，打包和示例代码的运行。


## 准备运行环境

+ 如果您使用容器运行 MOSN，请先 [安装 docker](https://docs.docker.com/install/)
+ 如果您使用本地机器，请使用类 Unix 环境
+ 安装 Go 的编译环境

## 获取代码

MOSN 项目的代码托管在 [Github](https://github.com/mosn/mosn)，获取方式如下：

```bash
go get -u mosn.io/mosn
```

如果您的 go get 下载存在问题，请手动创建项目工程

```bash
# 进入 GOPATH 下的 src 目录
cd $GOPATH/src
# 创建 mosn.io 目录
mkdir -p mosn.io
cd mosn.io

# 克隆 MOSN 代码
git clone git@github.com:mosn/mosn.git
cd mosn
```

最终 MOSN 的源代码代码路径为 `$GOPATH/src/mosn.io/mosn`

## 导入 IDE

使用您喜爱的 Go IDE 导入 `$GOPATH/src/mosn.io/mosn` 项目，推荐 Goland。

## 编译代码

在项目根目录下，根据自己机器的类型以及欲执行二进制的环境，选择以下命令编译 MOSN 的二进制文件。

### 切换 Istio 支持版本

MOSN 目前支持xDS v2 与 xDS v3，分别以Istio 1.5.2 和 Istio 1.10.6 为代表，可以根据需求在不同的版本支持之间切换。默认使用的是1.10.6版本。

切换到1.5.2 版本（xDS v2）

```bash
make istio-1.5.2 
```

切换到1.10.6版本(xDS v3)

```bash
make istio-1.10.6
```

### 使用 docker 镜像编译

```bash
make build // 编译出 linux 64bit 可运行二进制文件
```

### 本地编译

使用下面的命令编译本地可运行二进制文件。

```bash
make build-local
```

完成后可以在 `build/bundles/${version}/binary` 目录下找到编译好的二进制文件。

## 运行测试

在项目根目录下执行如下命令运行单元测试：

```bash
make unit-test
```

在项目根目录下执行如下命令运行集成测试（较慢）。

```bash
make integrate
make integrate-new
```

## 从配置文件启动 MOSN

运行下面的命令使用配置文件启动 MOSN。

```bash
./mosn start -c '$CONFIG_FILE'
```

## 开启 MOSN 转发示例程序

参考 `examples` 目录下的示例工程[运行 Samples](../../samples)。

## 使用 MOSN 搭建 Service Mesh 平台

请参考[与 Istio 集成](../istio)。
