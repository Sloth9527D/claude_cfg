---
allowed-tools: Bash(git status), Bash(git diff), Bash(git add), Bash(git commit), Bash(git push), Bash(echo)
description: 提交代码
---

# 提交代码

标准的 Git 提交流程：查看变更 → 选择文件 → 编写提交信息 → 推送。

## 流程

### 1. 查看变更
```bash
git status       # 查看已修改/新增/删除的文件
git diff --stat  # 查看变更统计（可选）
```

### 2. 选择需提交的文件
手动指定要提交的文件，不使用 `git add .` 全量提交：
```bash
git add <文件1> <文件2> ...
```

### 3. 编写提交信息
遵循 Conventional Commits 规范，格式如下：

```
<类型>: <简短描述>

<详细说明（可选）>

<关闭/关联的 Issue（可选）>
```

**提交类型：**
| 类型 | 说明 |
|---|---|
| `feat` | 新功能 |
| `fix` | 修复 Bug |
| `docs` | 文档变更 |
| `style` | 代码格式（不影响功能） |
| `refactor` | 重构（既非新功能也非修复） |
| `perf` | 性能优化 |
| `test` | 添加或修改测试 |
| `chore` | 构建/工具变更 |

**格式规范：**
- 标题不超过 50 字符，首字母大写，末尾不加句号
- 使用现在时态（`add` 而非 `added`）
- 详细说明每行不超过 72 字符

**示例：**
```
feat: 添加用户登录功能

实现了基于 JWT 的用户认证
- 添加登录 API 接口
- 添加 Token 验证中间件

Closes #123
```

### 4. 执行提交并推送
```bash
git commit -m "<提交信息>"
git push -u origin $(git branch --show-current)
```

## 注意事项
- 不使用 `git add .` 或 `git add -A` 全量提交，避免误提交敏感文件或临时文件
- 推送前确认目标分支是否正确
- 如有 Issue 关联，使用 `Closes #123` 或 `Refs #123` 格式
