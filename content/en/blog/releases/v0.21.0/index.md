---
title: "Announcing MOSN v0.21.0"
linkTitle: "Announcing MOSN v0.21.0"
date: 2021-02-02
author: MOSN Team
description: "MOSN v0.21.0 changelog."
---

We are happy to announce the release of [MOSN v0.21.0](https://github.com/mosn/mosn/releases/tag/v0.21.0), congratulations to Zechao Zheng([@CodingSinger](https://github.com/CodingSinger)) on becoming a MOSN Committer and thanks for his contribution to the MOSN community.

## v0.21.0

### Optimization

- Upgrade sentinel version to v1.0.2 [@ansiz](https://github.com/ansiz)
- Shrink the read buffer of tls when read timeout, reduce tls memory consumption [@cch123](https://github.com/cch123)
- Add comments and simplify the implementation of the xprotocol protocol connpool [@cch123](https://github.com/cch123)
- Update the mosn registry version [@cadeeper](https://github.com/cadeeper) [@cch123](https://github.com/cch123)

### Refactoring

- Optimize header matching logic when routing, support general RPC routing matching implementation [@nejisama](https://github.com/nejisama)
- Delete some of the original constants and add constants used to describe the mechanism of variables [@nejisama](https://github.com/nejisama)
- Refactor flow control module, support custom callback extension, realize the ability to customize filter conditions and modify context information, etc [@ansiz](https://github.com/ansiz)

### Bug fixes

- Fix metrics statistics error when request is abnormal [@cch123](https://github.com/cch123)
- Fix the bug that the URL is not escaping before forwarding HTTP request [@antJack](https://github.com/antJack)
- Fix the variable injection errors in HTTP protocol, Fix the bug that routing rewrite is not supported in the HTTP2 protocol [@nejisama](https://github.com/nejisama)

### New Features

- Support Domain-Specific Language route implementation [@CodingSinger](https://github.com/CodingSinger)
- StreamFilter supports the dynamic link libraries written in Go [@CodingSinger](https://github.com/CodingSinger)
- VirtualHost supports per_filter_config configuration in routing configuration [@machine3](https://github.com/machine3)
- Xprotocol supports dubbo thrift protocol [@cadeeper](https://github.com/cadeeper)
