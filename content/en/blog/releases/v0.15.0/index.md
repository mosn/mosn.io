---
title: "Announcing MOSN v0.15.0"
linkTitle: "Announcing MOSN v0.15.0"
date: 2020-08-05
author: MOSN Team
description: "MOSN v0.15.0 changelog."
---

We are happy to announce the release of [MOSN v0.15.0](https://github.com/mosn/mosn/releases/tag/v0.15.0), congratulations to Deng Qian ([@dengqian](https://github.com/dengqian)) on becoming a MOSN Committer and thanks for her contribution to the MOSN community.

## v0.15.0

### New Features

+ Routing Path Rewrite supports configuring the content of Rewrite by regular expression [@liangyuanpeng](https://github.com/liangyuanpeng)
+ Configure new fields: Extended configuration fields, you can start the configuration by extending the configuration fields; Dubbo service discovery configuration via extended configuration fields [@cch123](https://github.com/cch123)
+ New DSL feature for easy control of request processing behavior [@wangfakang](https://github.com/wangfakang)
+ Extended implementation of StreamFilter with new traffic mirroring function [@champly](https://github.com/champly)
+ Listener configuration adds UDP support [@dengqian](https://github.com/dengqian)
+ Configuration format support YAML format parsing [@GLYASAI](https://github.com/GLYASAI)
+ Routing support for HTTP redirect configuration [@knight42](https://github.com/knight42)

### Optimization

+ Istio's stats filter for personalizing metrics based on matching criteria [@wzshiming](https://github.com/wzshiming)
+ Metrics configuration support to configure the output percentage of the Histogram [@champly](https://github.com/champly)
+ StreamFilter New state for aborting requests directly and not responding to clients [@taoyuanyuan](https://github.com/taoyuanyuan)
+ XProtocol hijack response support carry body [@champly](https://github.com/champly)
+ Apache SkyWalking upgrade to version 0.5.0 [arugal](https://github.com/arugal)
+ Upstream Connection TLS State Determination Modification to support the determination of whether a connection needs to be re-established via a TLS-configured Hash [@nejisama](https://github.com/nejisama)
+ Optimize DNS cache logic to prevent DNS flooding issues that can be caused when DNS fails [@wangfakang](https://github.com/wangfakang)

### Bug fixes

+ Fix the bug that XProtocol protocols determine protocol errors in scenarios with multiple protocols when TLS encryption is enabled [@nejisama](https://github.com/nejisama)
+ Fix bug in AccessLog where variables of prefix match type don't work [@dengqian](https://github.com/dengqian)
+ Fix bug where Listener configuration parsing is not handled correctly [@nejisama](https://github.com/nejisama)
+ Fix Router/Cluster bug that fails to save when the Name field contains a path separator in the file persistence configuration type [@nejisama](https://github.com/nejisama)
