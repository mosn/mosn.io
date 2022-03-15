---
title: "贡献指引"
linkTitle: "贡献指引"
date: 2022-03-15
aliases: "/zh/docs/dev/contribute"
weight: 1
description: >
本页介绍如何为 MOSN 贡献代码。
---

## MOSN 贡献指引

首先很感谢您有兴趣给 MOSN 提交 PR。
为了能更高效的推进 PR 的合并，我们有一些推荐的做法，希望能有所帮助。

1. 创建分支
   推荐使用新分支来开发，master 分支推荐保持跟 MOSN 上游主分支保持一致
2. PR 需要说明意图
   如果已经有对应讨论的 issue，可以引用 issue
   如果没有 issue，需要描述清楚 PR 的意图，比如 bug 的情况。
   如果改动比较大，最好可以比较详细的改动说明介绍。
3. 提交新的 commit 来处理 review 意见
   当 PR 收到 review 意见后，有新的改动，推荐放到新的 commit，不要追加到原来的 commit，这样方便 reviewer 查看新的变更
4. 尽量提交小 PR
   不相关的改动，尽量放到不同的 PR，这样方便快速 review 合并
5. 尽量减少 force push
   因为 force push 之后，review 意见就对不上原始的代码记录了，这样不利于其他人了解 review 的过程。除非是因为需要 rebase 处理跟 master 的冲突，这种 rebase 后就只能 force push 了。

最后，很重要的一点，写清楚 commit log。

首先 commit log 推荐以一个单词开头，这样方便快速知晓 commit 的类型，比如下面的这些：

1. feature: 实现了一个新的功能/特性
2. change: 没有向后兼容的变更
3. refactor: 代码重构
4. bugfix: bug 修复
5. optimize: 性能优化相关的变更
6. doc: 文档变更，包括注释
7. tests: 测试用例相关的变更
8. style: 代码风格相关的调整
9. sample: 示例相关的变更
10. chore: 其他不涉及核心逻辑的小改动

开头单词之后，是简要介绍一下改动的内容，比如新增了什么功能，如果是 bugfix 的话，还需要说明 bug 复现的条件，以及危害。
比如：

```
bugfix: got the wrong CACert filename when converting the listen filter from istio LDS, mosn may not listen success then.
```

如果比较复杂，一句话写不清楚的话，也可以写多行，commit log 不用怕太长。

如果英文不容易写清楚，在 PR comment 里用中文描述清楚也可以的。

最后，祝玩得开心。