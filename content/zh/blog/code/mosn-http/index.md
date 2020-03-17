---
title: MOSN 源码解析 - HTTP系能力
linkTitle: MOSN 源码解析 - HTTP系能力
date: 2020-03-07
weight: 1
author: "陈爱祥（嘀嘀打车）"
description: "对 MOSN Log系统的源码解析。"
---

本文的目的是分析 MOSN 源码中的`http 系能力`。

本文的内容基于 MOSN 0.9.0

# 概述
  http 是互联网界最常用的一种协议之一，mosn 也提供了对其强大的支持。

# mosn http报文处理

![](message.jpg)

上图是[图解Http](https://book.douban.com/subject/25863515/) 中关于http报文报文的介绍。mosn 对于Http报文的处理并没有使用go 官网 `net/http`中的结构也没有独立设计一套相关结构 而是复用了业界开源的[fasthttp](https://github.com/valyala/fasthttp) 的结构。

```
type stream struct {
	str.BaseStream

	id               uint64
	readDisableCount int32
	ctx              context.Context
    // 请求报文
	request  *fasthttp.Request
    // 响应报文
	response *fasthttp.Response

	receiver types.StreamReceiveListener
}
```

# mosn http 处理流程

## 流程注册

```
func init() {
	str.Register(protocol.HTTP1, &streamConnFactory{})
}
```
在pkg/stream/http 包加载过程中将包含http对于mosn上下游连接的处理逻辑的结构体值注册到统一的stream处理工厂。

## 捕获请求
    mosn 作为一个sidecar的落地方式，其重要作用就是处理上游到mson的请求。其主要处理逻辑在serverStreamConnection 的server成员函数中。
```
func (conn *serverStreamConnection) serve() {
	for {
		// 1. pre alloc stream-level ctx with bufferCtx
		ctx := conn.contextManager.Get()
		// 通过sync.Pool 实现实现内存复用
		buffers := httpBuffersByContext(ctx)
		request := &buffers.serverRequest

		// 2. blocking read using fasthttp.Request.Read
		err := request.ReadLimitBody(conn.br, defaultMaxRequestBodySize)
		if err == nil {
			// 3. 'Expect: 100-continue' request handling.
			// See http://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html for details.
			if request.MayContinue() {
				// Send 'HTTP/1.1 100 Continue' response.
				conn.conn.Write(buffer.NewIoBufferBytes(strResponseContinue))

				// read request body
				err = request.ContinueReadBody(conn.br, defaultMaxRequestBodySize)

				// remove 'Expect' header, so it would not be sent to the upstream
				request.Header.Del("Expect")
			}
		}
		// 读取错误的处理
		if err != nil {
			// "read timeout with nothing read" is the error of returned by fasthttp v1.2.0
			// if connection closed with nothing read.
			if err != errConnClose && err != io.EOF && err.Error() != "read timeout with nothing read" {
				// write error response
				conn.conn.Write(buffer.NewIoBufferBytes(strErrorResponse))

				// close connection with flush
				conn.conn.Close(api.FlushWrite, api.LocalClose)
			}
			return
		}

		// 数据读取结束
		id := protocol.GenerateID()
		s := &buffers.serverStream

		// 4. request processing
		s.stream = stream{
			id:       id,
			ctx:      mosnctx.WithValue(ctx, types.ContextKeyStreamID, id),
			request:  request,
			response: &buffers.serverResponse,
		}
		s.connection = conn
		s.responseDoneChan = make(chan bool, 1)
		s.header = mosnhttp.RequestHeader{&s.request.Header, nil}

		var span types.Span
		if trace.IsEnabled() {
			tracer := trace.Tracer(protocol.HTTP1)
			if tracer != nil {
				span = tracer.Start(ctx, s.header, time.Now())
			}
		}
		// 上下文 中注入链式追踪信息
		s.stream.ctx = s.connection.contextManager.InjectTrace(ctx, span)

		if log.Proxy.GetLogLevel() >= log.DEBUG {
			log.Proxy.Debugf(s.stream.ctx, "[stream] [http] new stream detect, requestId = %v", s.stream.id)
		}

		s.receiver = conn.serverStreamConnListener.NewStreamDetect(s.stream.ctx, s, span)

		conn.mutex.Lock()
		conn.stream = s
		conn.mutex.Unlock()

		if atomic.LoadInt32(&s.readDisableCount) <= 0 {
			s.handleRequest()
		}

		// 5. wait for proxy done
		select {
		case <-s.responseDoneChan:
		case <-conn.connClosed:
			return
		}

		conn.contextManager.Next()
	}
}
```
由以上代码可以得知，因为不能判断连接是长连接还是短连接，所以mosn 上游不主动关闭连接，mosn也不会主动关闭，除非出现错误。

## 转发请求
    mosn 捕获请求之后并不是重点还需要继续转发给目标服务。其中最重要的逻辑就是维护一个高效的对于目标服务的连接处理逻辑。其重要逻辑在connPool的getAvailableClient成员函数。

```

type connPool struct {
	MaxConn int

	host types.Host

	statReport bool

	clientMux        sync.Mutex
	availableClients []*activeClient // available clients
	totalClientCount uint64          // total clients
}
func (p *connPool) getAvailableClient(ctx context.Context) (*activeClient, types.PoolFailureReason) {
	p.clientMux.Lock()
	defer p.clientMux.Unlock()

	n := len(p.availableClients)
	// no available client
	if n == 0 {
		// max conns is 0 means no limit
		maxConns := p.host.ClusterInfo().ResourceManager().Connections().Max()
		if maxConns == 0 || p.totalClientCount < maxConns {
			ac, reason := newActiveClient(ctx, p)
			if ac != nil && reason == "" {
				p.totalClientCount++
			}
			return ac, reason
		} else {
			p.host.HostStats().UpstreamRequestPendingOverflow.Inc(1)
			p.host.ClusterInfo().Stats().UpstreamRequestPendingOverflow.Inc(1)
			return nil, types.Overflow
		}
	} else {
		n--
		c := p.availableClients[n]
		p.availableClients[n] = nil
		p.availableClients = p.availableClients[:n]
		return c, ""
	}
}
```
由上述代码可知，mosn 维护了连接池来实现高效的连接复用
# 内存复用
    http的处理会频繁的申请空间来解析http报文，为了减少频繁的内存申请，实现内存复用，mosn 基于sync.pool 设计了内存复用模块。

```
func httpBuffersByContext(context context.Context) *httpBuffers {
	ctx := buffer.PoolContext(context)
	return ctx.Find(&ins, nil).(*httpBuffers)
}
```

# 总结
    本文简单的分析了mosn 中对于http 请求的处理，其核心点在于解决：

1. tcp 黏包：直接使用fasthttp 的request和response
2. 实现连接复用：连接池
3. 实现内存复用：sync.pool