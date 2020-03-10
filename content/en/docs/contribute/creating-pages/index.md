---
title: "Create a page"
linkTitle: "Create a page"
date: 2020-02-11
weight: 2
description: >
  The topic shows you about how to create a MOSN document page.
---

## Prerequisites

Before you start to write a MOSN document, create a MOSN document repository and be familiar with the MOSN document structure.

## Page types

### Documentation

The documentation that systematically describe how to use MOSN are maintained by the MOSN team.

### Blogs

The periodically published MOSN blogs are contributed by the community.

### News

The news about the MOSN community.

### Releases

Release notes about new versions of MOSN.

## Topic structure

All topics of the MOSN documentation are saved under the `content` directory: `content/zh` for Chinese topics, and `content/en` for English topics. To create a topic under an existing one, you need to first create a directory and comply with the following naming rules:

- Name all topics without sub-directories as `index.md`.
- Name all topics with sub-directories as `_index.md`.

## Topic metadata

Each topic has its metadata, which is separated by three hyphens ("-") between two YAML blocks. The following metadata is required:

```yaml
title="Title"
linkTitle: "Title"
date: 2020-02-11
weight: 1
description: >
  The brief description of the page.
```

Details are described as follows:

- `title`: The title of the topic.
- `linkTitle`: The topic title displayed in the left-side content pane, which is usually consistent with `title`.
- `date`: The date when the topic was created, in the `YYYY-MM-dd` format.
- `weight`: Among topics of the same level, the one with the smallest weight is displayed at the top in the left-side content pane.
- `description`: The brief description of the document.

Author information is required for blogs, releases notes, and news articles:

```yaml
author: "Author information"
```

Note: The value of author can be edited in the Markdown format.

## Naming conventions

The URL of a document is determined based on the level of its directory. Document directories are named according to the following conventions:

- Use English names
- Connect words with hyphens
- Avoid punctuation marks
- Minimize the name

