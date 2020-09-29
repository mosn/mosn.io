---
title: "版本发布 介绍"
linkTitle: "版本发布 介绍"
date: 2020-09-28
weight: 2
aliases: "/zh/docs/dev/release"
description: >
  本页介绍了 MOSN 的 版本发布步骤。
---

## MOSN 版本发布步骤

###  一、冻结代码

+ 在准备一个版本发布期间，停止代码往 master 分支的合并

### 二、整理 Release notes

+ 基于 Github 的 PullRequest 记录，整理本次发布的内容与上一个版本之间的差异，需要注意仅统计目标分支是 master 且正常合并的 PullRequest
+ 首先记录原始的信息，统一记录在 [MOSN Release notes 整理文档](https://docs.google.com/document/d/15wu2Ug4nN38A_odKv3ubmfu1OxmxqlUUFnvN4mbKtIc/edit?usp=sharing)
  + 文档打开需要权限
+ 整理完原始信息以后，进行提炼和总结
  + 通常情况下，一个 PullRequest 对应一个改动记录
  + 存在部分特殊情况是一个 PullRequest 包含多个改动的情况，可以请 PullRequest 提供者提供详细信息
  + 也可能存在多个 PullRequest 是针对同一个改动的情况（如新功能，分开提 PullRequest，或者在同一个版本迭代中不断优化）
+ 提炼后的完整 Release notes 记录格式可以参考 [CHANGELOG](https://github.com/mosn/mosn/blob/master/CHANGELOG_ZH.md)
+ 提炼后的 Release notes 需要有英文版本的记录

### 三、测试报告

+ 所有的改动点都需要有对应的测试记录，测试方式可以有多种，包括但不限于
  + 完整的单元测试覆盖，确保基本的功能场景正确，默认代码合并的时候会执行
  + 性能测试 Benchmark，针对可能对性能有影响的改动，需要执行性能测试。
  + 手动模拟测试，主要针对一些单元测试无法很好覆盖的场景（如涉及网络 IO、Proxy 转发等多模块交互），通过手动搭建测试环境（配置、模拟 Server 与 Client）进行场景复现与验证
    + 手动模拟测试完成以后记录详细的测试步骤，包括：模拟的场景、使用的配置、使用的 Server、使用的 Client 等；后续可以考虑将手动场景实现为 integrate 测试
+ 测试完成以后，产出测试报告，说明对应的功能点使用哪种测试方式测试与测试的结果


### 四、版本发布

+ 版本发布 PullRequest
  + 修改`VERSION`
  + 修改`CHANGELOG.md`、`CHNAGELOH_ZH.md`
  + 上传测试报告`reports/${VERSION}.md`

+ 官网文档的同步修改
  + github.com/mosn/mosn.io/content/zh/blog/releases 下新增对应的 release notes.
  + 英文版 release notes 也需要做一样的更新

+ 正式 release
  + 合并完`版本发布 PullRequest`后，基于 master 分支完成 release
  + release 时的描述内容填写英文版 release notes 的内容
  + 通过`make build`命令编译出对应的二进制并且上传
  + 完成 release
