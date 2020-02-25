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

操作共享内存的方法主要在 `pkg/shm/shm.go` 文件下：

```go
func Alloc(name string, size int) (*ShmSpan, error) {
	...
	return NewShmSpan(name, data), nil
}

func Free(span *ShmSpan) error {
	Clear(span.name)
	return syscall.Munmap(span.origin)
}

func Clear(name string) error {
	return os.Remove(path(name))
}
```

都是围绕着 `ShmSpan` 结构体的几个操作方法。再来看 `ShmSpan` 结构体：

```go
type ShmSpan struct {
	origin []byte // mmap 返回的数组
	name   string // span 名, 创建时指定

	data   uintptr // 保存 mmap 内存段的首指针
	offset int // span 已经使用的字节长度
	size   int // span 大小
}
```

`Alloc` 方法按照给定的 `name` 参数，在配置文件的目录下创建文件，并执行 `sync.Mmap`，其文件尺寸即 `size` 参数大小。Mmap 过后，将信息保存在 ShmSpan结构内返回。

由此看出，一个 ShmSpan 可以看做是一个共享内存块。

## 操作共享内存

下面来看共享内存块的使用方法，我们可以从 MOSN 里的使用场景：metrics，来追踪到使用方法。

metrics 相关的逻辑在 `pkg/metrics` 包下。




## 总结

本文通过分析 MOSN 源码，简述了 MOSN 的filter扩展机制，并简述了实现自己的 filter 需要做的东西。大家可以通过该机制，使用 MONS 轻松 cover 自己具体的使用场景。

---

参考资料:

- [MOSN 源码](https://github.com/mosn/mosn)
