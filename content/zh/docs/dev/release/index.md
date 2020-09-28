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

+ 在准备一个版本发布期间，停止代码往master分支的合并

### 二、整理Release notes

+ 基于Github的PullRequest记录, 整理本次发布的内容与上一个版本之间的差异，需要注意仅统计目标分支是master且正常合并的PullRequest
+ 通常情况下，一个PullRequest对应一个改动记录，但是也不排除有多个PullRequest是针对同一个改动的情况
+ 每个改动记录都需要记录对应的改动描述，有些描述可能在PullRequest的描述中并不准确，需要理解以后重新组织语言。每个改动需要记录对应的改动者(可能有多个)
+ 具体的Release notes记录格式可以参考[CHANGELOG](https://github.com/mosn/mosn/blob/master/CHANGELOG_ZH.md)
+ Rlease notes需要有英文版本的记录

### 三、测试报告

+ 所有的改动点都需要有对应的测试记录，测试方式可以有多种，包括但不限于
  + 完整的单元测试覆盖，确保基本的功能场景正确，默认代码合并的时候会执行
  + 性能测试Benchmark，针对可能对性能有影响的改动，需要执行性能测试。
  + 手动模拟测试，主要针对一些单元测试无法很好覆盖的场景（如涉及网络IO、Proxy转发等多模块交互），通过手动搭建测试环境（配置、模拟Server与Client）进行场景复现与验证
    + 手动模拟测试完成以后记录详细的测试步骤，包括：模拟的场景、使用的配置、使用的Server、使用的Client等；后续可以考虑将手动场景实现为integrate测试
+ 测试完成以后，产出测试报告，说明对应的功能点使用哪种测试方式测试与测试的结果


### 四、版本发布

+ 版本发布PullRequest 
  + 修改`VERSION`
  + 修改`CHANGELOG.md`、`CHNAGELOH_ZH.md`
  + 上传测试报告`reports/${VERSION}.md`

+ 官网文档的同步修改
  + github.com/mosn/mosn.io/content/zh/blog/releases下新增对应的release notes.
  + 英文版release notes 也需要做一样的更新

+ 正式release
  + 合并完`版本发布PullRequest`后，基于master分支完成release
  + release时的描述内容填写英文版release notes的内容
  + 通过`make build`命令编译出对应的二进制并且上传
  + 完成release 
