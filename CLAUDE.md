# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个收集和整理 Claude Code Skills 的仓库。Skills 是可复用的指令模板，通过 `/skill-name` 命令调用。

## 目录结构

- `personal/` - 个人创建的 skills
  - `novel/` - 小说创作工作流技能集（8个技能，覆盖完整创作流程）
- `collected/` - 从网上收集的优秀 skills（需注明来源）
- `templates/` - skill 模板文件

## Skill 文件格式

每个 skill 是一个独立的 `.md` 文件，文件名即为 skill 名称。基本结构：

```
# Skill 名称
简短描述用途

## 触发条件
描述何时使用

## 指令
具体指令内容

## 示例（可选）
使用示例
```

## 使用 Skills

将 skill 文件复制到 `~/.claude/skills/` 目录，然后在 Claude Code 中使用 `/skill-name` 调用。