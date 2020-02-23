---
title: MOSN 源码解析 - 共享内存模型
linkTitle: MOSN 源码解析 - 共享内存模型
date: 2020-02-23
weight: 1
author: "[@trainyao](https://trainyao.github.io)"
authorlink: "https://trainyao.github.io"
originallink: "https://trainyao.github.io/post/mosn/source_shm/"

---

本文记录了对 MOSN 的源码研究 - MOSN 的共享内存模型。

本文的内容基于 MOSN v0.9.0，commit id [b2a239f5](https://github.com/mosn/mosn/tree/b2a239f5)。

## 机制

MOSN 用共享内存来存储 metrics 信息。MOSN 用 mmap 将文件映射到内存，在内存数组之上封装了一层关于 metrics 的存取逻辑，实现了 [go-metrics](https://github.com/rcrowley/go-metrics) 
包的关于 metrics 的接口。

## 创建共享内存：Mmap

和共享内存相关的逻辑都在 `pkg/shm` 文件夹目录下。里面内容比较少，`types.go` 定义了内存共享相关的 

## 操作共享内存


## 总结

本文通过分析 MOSN 源码，简述了 MOSN 的filter扩展机制，并简述了实现自己的 filter 需要做的东西。大家可以通过该机制，使用 MONS 轻松 cover 自己具体的使用场景。

---

参考资料:

- [MOSN 源码](https://github.com/mosn/mosn)
