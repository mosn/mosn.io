---
title: "MoE 系列[七] - Envoy Go 扩展之沙箱安全"
linkTitle: "MoE 系列[七] - Envoy Go 扩展之沙箱安全"
date: 2023-05-07
author: "[doujiang24](https://github.com/doujiang24)"
description: "今天来到了安全性的最后一篇，沙箱安全，也是相对来说，最简单的一篇"
aliases: "/zh/blog/posts/moe-extend-envoy-using-golang-7"
---

前两篇介绍了内存安全和并发安全，今天来到了安全性的最后一篇，沙箱安全，也是相对来说，最简单的一篇。

## 沙箱安全

所谓的沙箱安全，是为了保护 Envoy，这个宿主程序的安全，也就是说，扩展的 Go 代码运行在一个沙箱环境中，即使 Go 代码跑飞了，也不会把 Envoy 搞挂。

具体到一个场景，也就是当我们使用 Golang 来扩展 Envoy 的时候，不用担心自己的 Go 代码写的不好，而把整个 Envoy 进程搞挂了。

那么目前 Envoy Go 扩展的沙箱安全做到了什么程度呢？

简单来说，目前只做到了比较浅层次的沙箱安全，不过，也是实用性比较高的一层。

严格来说，Envoy Go 扩展加载的是可执行的机器指令，是直接交给 cpu 来运行的，并不像 Wasm 或者 Lua 一样由虚拟机来解释执行，所以，理论上来说，也没办法做到绝对的沙箱安全。

## 实现机制

目前实现的沙箱安全机制，依赖的是 Go runtime 的 `recover` 机制。

具体来说，Go 扩展底层框架会自动的，或者（代码里显示启动的协程）依赖人工显示的，通过 `defer` 注入我们的恢复机制，所以，当 Go 代码发生了奔溃的时候，则会执行我们注入的恢复策略，此时的处理策略是，使用 `500` 错误码结束当前请求，而不会影响其他请求的执行。

但是这里有一个不太完美的点，有一些异常是 `recover` 也不能恢复的，比如这几个：

```
Concurrent map writes
Out of memory
Stack memory exhaustion
Attempting to launch a nil function as a goroutine
All goroutines are asleep - deadlock
```

好在这几个异常，都是不太容易出现的，唯一一个值得担心的是 `Concurrent map writes`，不熟悉 Go 的话，还是比较容易踩这个坑的。

所以，在写 Go 扩展的时候，我们建议还是小心一些，写得不好的话，还是有可能会把 Envoy 搞挂的。

当然，这个也不是一个很高的要求，毕竟这是 Gopher 写 Go 代码的很常见的基本要求。

好在大多常见的异常，都是可以 `recover` 恢复的，这也就是为什么现在的机制，还是比较有实用性。

## 未来

那么，对于 `recover` 恢复不了的，也是有解决的思路：

比如 `recover` 恢复不了 `Concurrent map writes`，是因为 runtime 认为 `map` 已经被写坏了，不可逆了。

那如果我们放弃整个 `runtime`，重新加载 so 来重建 `runtime` 呢？那影响面也会小很多，至少 Envoy 还是安全的，不过实现起来还是比较的麻烦。

眼下比较浅的安全机制，也足够解决大多数的问题了，嗯。