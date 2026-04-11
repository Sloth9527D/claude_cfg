---
allowed-tools: Glob, Read, Write, Edit, Bash
description: 摄入 raw/ 中的原始资料为 wiki 页面
---

# Ingest 命令

将 `raw/` 中的原始资料摄入为 wiki 页面，遵循 CLAUDE.md 定义的 llm-wiki 工作流。

## 流程

### 1. 列出 raw/ 中的可用文件或目录
扫描 `raw/` 目录，返回所有待处理内容（含文件、子目录及其修改时间）。

**跳过以下文件：**
- `state: ingest` — 已摄入，无需重复处理
- `state: skip` — 标记为不需要摄入（如元文件、说明文档等）

**输入支持两种模式：**
- **文件模式**：选择单个 `.md` 文件摄入
- **目录模式**：选择 `raw/` 下的子目录，批量摄入该目录下所有 `.md` 文件（同样跳过已标记的文件）

### 2. 用户选择摄入目标
- 从列表中选择一个文件或目录
- 或直接输入文件路径/目录路径
- 支持正则过滤（如输入 `claude` 只显示含 claude 的文件）

### 3. 读取并分析来源
读取选定的源文件内容，提炼：
- **核心要点**（Key Points）：3-8 条要点
- **实体**：人物、地点、组织、工具等
- **概念**：理论、框架、方法论等
- **来源元信息**：标题、日期、作者（从 frontmatter 或内容中提取）

**批量模式（目录）**：依次处理每个 `.md` 文件，按顺序摄入。

### 4. 创建来源页面
在 `wiki/sources/` 下创建对应页面，命名使用小写和连字符。
- 文件名：`raw/` 下的相对路径，如 `raw/tools/claude-code.md` → `wiki/sources/tools-claude-code.md`
- 目录名作为前缀：`raw/ai/transformer.md` → `wiki/sources/ai-transformer.md`

Frontmatter 必须包含：
```yaml
---
title: "来源标题"
created: {{DATE}}
updated: {{DATE}}
sources: []
tags: []
related: []
---
```

### 5. 更新 raw 文件 frontmatter
摄入完成后，更新 `raw/` 中的原始文件 frontmatter，补充以下字段：
```yaml
---
source:          # 原始来源链接/URL（无则留空）
author:          # 作者（无则留空）
published:       # 发布时间（从内容中提取或留空）
ingest_time: YYYY-MM-DD
description:    # 一句话描述
tags: []        # 标签
state: ingest    # 已摄入
---
```

如不需要摄入（如元文件、说明文档等），可标记为 `state: skip`，后续扫描会自动跳过。

- 如文件已有 frontmatter，只补充缺失字段，保留原有内容
- 正文内容不得修改

### 6. 更新实体/概念页面
- 检查提炼出的实体是否已在 `wiki/entities/` 中存在
- 检查概念是否已在 `wiki/concepts/` 中存在
- 如不存在，创建对应页面
- 如已存在，更新相关链接

### 7. 更新 wiki/index.md
在对应分类下追加新页面的链接。批量摄入时批量更新。

### 8. 追加日志到 wiki/log.md
以 `## [YYYY-MM-DD]` 格式追加摄入记录，包含：
- 摄入文件名或目录名
- 创建/更新的页面列表
- 简要说明

## 注意事项
- raw/ 中的原始文件**正文内容不可修改**
- 摄入完成后可更新 raw 文件的 frontmatter 元信息
- 生成的 wiki 页面至少包含一个内部链接
- 所有路径使用相对路径
- 目录摄入时只处理 `.md` 文件，忽略子目录中的其他格式
- 目录摄入按文件名顺序批量处理，最后统一更新 index.md 和 log.md
- 全局命令备份时放入./claudian/子目录
