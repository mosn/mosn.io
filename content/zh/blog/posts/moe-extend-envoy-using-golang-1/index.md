---
title: "MoE 系列 - 如何使用 Golang 扩展 Envoy [一]"
linkTitle: "MoE 系列 - 如何使用 Golang 扩展 Envoy [一]"
date: 2023-02-27
author: "[doujiang24](https://github.com/doujiang24)"
description: "本文作为 MoE 系列第一篇，主要介绍用 Golang 扩展 Envoy 的极速开发体验"
aliases: "/zh/blog/posts/moe-extend-envoy-using-golang-1"
---

## 背景

MoE，MOSN on Envoy 是 MOSN 团队提出的技术架构，经过近两年的发展，在蚂蚁内部已经得到了很好的验证；并且去年我们也将底层的 Envoy Go 七层扩展贡献了 Envoy 官方，MOSN 也初步支持了使用 Envoy 作为网络底座的能力。

准备写一系列的文章，逐一介绍这里面的技术，本文是开篇，重点介绍 MoE 中的基础技术，Envoy Go 扩展。

## FAQ

开始前，先回答几个基本的问题：

1. MoE 与 Envoy Go 扩展的区别

   MoE 是技术架构，Envoy Go 扩展是连接 MOSN 和 Envoy 的基础技术

2. Envoy Go 扩展，与用 Go 来编译 Wasm 有什么区别

   Envoy Go 支持 Go 语言的所有特性，包括 Goroutine，Go Wasm 则只能使用少量的 Go 语言特性，尤其是没有 Goroutine 的支持

3. Go 是静态链接到 Envoy 么？

   不是的，Go 扩展编译成为 so，Envoy 动态加载 so，不需要重新编译 Envoy

4. Envoy Go 支持流式处理么？

   支持的。

   由于 Go 扩展提供的是底层的 API，非常的灵活，使用上相对会稍微复杂一些；如果只想简单的使用，可以使用 MOSN 的 filter，后面我们也会介绍。

## 需求

我们先实现一个小需求，来实际体会一下：

对请求需要进行验签，大致是从 URI 上的某些参数，以及私钥计算一个 `token`，然后和 header 中的 `token` 进行对比，对不上就返回 403。

很简单的需求，仅仅作为示例，主要是体验一下过程。

## 代码实现

完整的代码可以看 [envoy-go-filter-example](https://github.com/doujiang24/envoy-go-filter-example/tree/master/example-1) 这个仓库

这里摘录最核心的两个函数：
```go
const secretKey = "secret"

func verify(header api.RequestHeaderMap) (bool, string) {
	token, ok := header.Get("token")
	if ok {
		return false, "missing token"
	}

	path, _ := header.Get(":path")
	hash := md5.Sum([]byte(path + secretKey))
	if hex.EncodeToString(hash[:]) != token {
		return false, "invalid token"
	}
	return true, ""
}

func (f *filter) DecodeHeaders(header api.RequestHeaderMap, endStream bool) api.StatusType {
	if ok, msg := verify(header); !ok {
		f.callbacks.SendLocalReply(403, msg, map[string]string{}, 0, "bad-request")
        return api.LocalReply
    }
	return api.Continue
}
```

`DecodeHeaders` 是扩展 `filter` 必须实现的方法，我们就是在这个阶段对请求 `header` 进行校验。

`verfiy` 是校验函数，这里的 `RequestHeaderMap` 是 Go 扩展提供的 `interface`，我们可以通过它来读写 header，其他都是常见的 Go 代码写法。

## 编译

编译很简单，与常见的 Go 编译一样，这里我们使用 Golang 官方的 docker 镜像来编译：

```shell
docker run --rm -v `pwd`:/go/src/go-filter -w /go/src/go-filter \
        -e GOPROXY=https://goproxy.cn \
        golang:1.19.8 \
        go build -v -o libgolang.so -buildmode=c-shared .
```

Go 编译还是很快的，只需要几秒钟，当前目录下，就会产生一个 libgolang.so 的文件。

反观 Envoy 的编译速度，一次全量编译，动辄几十分钟，上小时的，这幸福感提升了不止一个档次。

## 运行

我们可以使用 Envoy 官方提供的镜像来运行，如下示例：

```shell
docker run --rm -v `pwd`/envoy.yaml:/etc/envoy/envoy.yaml \
        -v `pwd`/libgolang.so:/etc/envoy/libgolang.so \
        -p 10000:10000 \
        envoyproxy/envoy:contrib-v1.26-latest \
        envoy -c /etc/envoy/envoy.yaml
```

只需要把上一步编译的 `libgolang.so` 和 `envoy.yaml` 挂载进去就可以了。

值得一提的是，我们需要在 envoy.yaml 配置中启用 Go 扩展，具体是这段配置：

```yaml
http_filters:
  - name: envoy.filters.http.golang
    typed_config:
      "@type": type.googleapis.com/envoy.extensions.filters.http.golang.v3alpha.Config
      library_id: example
      library_path: /etc/envoy/libgolang.so
      plugin_name: example-1
```

跑起来之后，我们测试一下：

```shell
$ curl 'http://localhost:10000/'
missing token

$ curl -s 'http://localhost:10000/' -H 'token: c64319d06364528120a9f96af62ea83d' -I
HTTP/1.1 200 OK
```

符合期望，是不是很简单呢

## 后续

什么？这个示例太简单？

是的，这里主要是体验下开发流程，下篇我们再介绍更高级的玩法：

Go 接受来自 Envoy 侧的配置，异步 Goroutine，以及与 Istio 配合的用法。
