---
title: "Announcing MOSN v0.16.0"
linkTitle: "Announcing MOSN v0.16.0"
date: 2020-09-01
author: MOSN Team
description: "MOSN v0.16.0 changelog."
---

We are happy to announce the release of [MOSN v0.16.0](https://github.com/mosn/mosn/releases/tag/v0.16.0).

## v0.16.0

### Optimization

+ Logger Roller supports the custom Roller. [@wenxuwan](https://github.com/wenxuwan)
+ Add a SendHijackReplyWithBody API for streamFilter. [@wenxuwan](https://github.com/wenxuwan)
+ The configuration adds option of turning off the smooth upgrade. If the smooth upgrade is turned off, different instances of MOSN can be started on the same machine. [@cch123](https://github.com/cch123)+ Optimize the MOSN integration test framework and add more unit test cases. [@nejisama](https://github.com/nejisama) [@wangfakang](https://github.com/wangfakang) [@taoyuanyuan](https://github.com/taoyuanyuan)
+ DirectResponse route configuration supports the update mode of XDS. [@wangfakang](https://github.com/wangfakang)
+ Add a new field of TLSContext for clusterManager configuration. [@nejisama](https://github.com/nejisama)

### Bug fixes

+ Fix the bug that UDP connection timeout during the smooth upgrade will cause an endless loop. [@dengqian](https://github.com/dengqian)
+ Fix the bug that call DirectResponse in the SendFilter will cause an endless loop. [@taoyuanyuan](https://github.com/taoyuanyuan)
+ Fix concurrency conflicts in HTTP2 stream counting. [@wenxuwan](https://github.com/wenxuwan)
+ Fix the bug that UDP connection read timeout cause data loss. [@dengqian](https://github.com/dengqian)
+ Fix the bug that the response StatusCode cannot be recorded correctly due to the loss of the protocol flag when doing a retry. [@dengqian](https://github.com/dengqian)
+ Fix the protocol boltv2 decode error. [@nejisama](https://github.com/nejisama)
+ Fix the bug that listener cannot be restarted automatically when listener panic. [@alpha-baby](https://github.com/alpha-baby)
+ Fix the bug that NoCache flag is invalid in variable. [@wangfakang](https://github.com/wangfakang)
+ Fix concurrency conflicts in SDS reconnect. [@nejisama](https://github.com/nejisama)
