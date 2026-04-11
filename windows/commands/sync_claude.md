---
allowed-tools: Bash(cp), Read, Glob
description: 同步 Claude 全局配置
---

# 同步 Claude 全局配置

将本地系统 Claude 的全局配置备份到当前 vault 的 `` 目录，按操作系统分类管理。

## 备份范围

从 `~/.claude/` 同步以下内容：
- `settings.json` — Claude 全局设置
- `commands/` — 自定义命令
- `plugins/` — 插件配置
- `skills/` — 技能配置

**不含**：backups/、sessions/、history.jsonl、cache/、file-history/、projects/ 等运行时或自动快照数据。

> **敏感信息**：`settings.json` 可能包含 `ANTHROPIC_AUTH_TOKEN` 等 API 密钥。备份前建议检查并模糊化处理，发布备份时勿包含真实凭证。

## 流程

### 1. 检测操作系统
根据当前系统类型确定子目录名：
- Windows → `windows`
- Linux → `linux`
- macOS → `mac`

### 2. 确定目标路径
`{vault_root}/{os_name}/`

### 3. 执行备份
使用 `cp -r` 将上述目录/文件复制到目标路径，同名直接覆盖。

**命令示例**（Windows PowerShell）：
```bash
mkdir -p "windows"
cp -r "$HOME/.claude/settings.json" "windows/"
cp -r "$HOME/.claude/commands" "windows/"
cp -r "$HOME/.claude/plugins" "windows/"
cp -r "$HOME/.claude/skills" "windows/"
```

### 4. 验证结果
备份完成后列出目标目录结构，确认文件完整。

## 注意事项
- 不修改源文件，只读取并复制
- vault 的 `.claude-backup/` 目录不纳入版本控制（建议加入 `.gitignore`）
- 可随时从备份恢复：`cp -r {os}/ ~/.claude/`