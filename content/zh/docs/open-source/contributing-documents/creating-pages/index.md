---
title: "创建页面"
linkTitle: "创建页面"
date: 2020-02-11
aliases: "/zh/docs/open-source/contributing-documents/creating-pages"
description: >
  本页介绍了如何创建 MOSN 文档页面。
---

## 开始之前

在开始编写 MOSN 文档之前，首先需要你创建一个 MOSN 文档存储库，和熟悉 MOSN 的文档结构。

## 页面类型

### 文档

系统化介绍 MOSN 使用的文档，由 MOSN 团队官方维护。

### 博客

周期化发布的 MOSN 博客，来自社区贡献。

### 新闻

关于 MOSN 社区的新闻信息。

### 发布

MOSN 的新版本发布信息。

## 文档结构

所有文档都位于 `content` 目录下，`content/zh` 为中文文档，`content/en` 为英文文档，要想在某一层级的文档下再创建一个新的文档需要先创建一个目录，并根据：

- 所有没有子目录的文档都以 `index.md` 命名。
- 所有包含子目录的文档都以 `_index.md` 命名。

## 文档元数据

每个文档都有元数据信息，元数据信息是介于两个 YAML 块之间通过 3 个“-”分割的信息。下面就是你必须填写的元数据信息：

```yaml
title: "标题"
linkTitle: "标题"
date: 2020-02-11
weight: 1
description: >
  关于本页内容的简介。
```

以下是详细介绍：

- title：即本文章的标题。
- linkTitle：显示在侧边栏的文档标题，一般写成跟 `title` 的内容一致即可。
- date：该文档的创作日期，格式为 `YYYY-MM-dd`。
- weight：在同一文档层级，weight 数字越小的文档在侧边栏中显示约靠前，对于非 `docs` 目录下的文章不需要设置。
- description：对本文档的简介。

对于博客、发布、新闻文档，还需要填写作者信息：

```yaml
author: "作者信息"
```

注意：作者信息的值支持 Markdown。

## 文档命名

文档的 URL 是根据该篇文档所在的目录层级而确定的，文档的目录名称规范：

- 使用英文单词命名
- 不同的单词间使用连字符连接
- 不得出现其他标点符号
- 名称尽量简短
