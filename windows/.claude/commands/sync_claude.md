---
allowed-tools: Bash(*:*), Read
description: 同步claude配置
---

# 将本地系统全局.claude配置下的settings.json、commands目录备份
1. 根据系统类型复制到到到当前目录下，windows挪入windows子目录,linux挪入linux,mac对应mac
2. 不存在直接创建子目录，同名直接覆盖
3. 目录层级要求：当前目录/系统名/.claude/*,*中保持原配置目录层级