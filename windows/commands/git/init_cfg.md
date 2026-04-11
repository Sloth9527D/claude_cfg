---
allowed-tools: Bash(git config:*)
description: 初始化 Git 全局配置
---

# 初始化 Git 全局配置

为当前用户配置 Git 全局设置，包含身份信息和常用选项。

## 配置内容

### 1. 用户身份
- `user.name` — 从参数 `$ARGUMENTS` 获取，或询问用户输入
- `user.email` — 固定为 `p479764650@gmail.com`

### 2. 网络代理（可选）
```
http.https://github.com.proxy socks5://127.0.0.1:10808
```
**如无需代理，跳过此条。**

### 3. 其他选项
- `core.longpaths true` — 解决 Windows 长路径问题

## 执行流程

1. **获取用户名**：优先使用命令参数 `$ARGUMENTS`，否则询问用户输入
2. **执行配置**：
   ```bash
   git config --global user.name "$USER_NAME"
   git config --global user.email "p479764650@gmail.com"
   git config --global core.longpaths true
   ```
3. **代理配置**（如适用）：
   ```bash
   git config --global http.https://github.com.proxy socks5://127.0.0.1:10808
   ```
4. **验证结果**：显示当前全局配置
   ```bash
   git config --list --global
   ```

## 注意事项
- 此命令为一次性初始化，后续修改直接使用 `git config`
- 代理地址 `127.0.0.1:10808` 为本地 SOCKS5 代理，如有变动需手动更新
