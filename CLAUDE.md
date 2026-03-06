# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个 Claude Code Skills 收集仓库。Skills 是可复用的指令模板，放入 `~/.claude/skills/` 后通过 `/skill-name` 命令调用。

## 目录结构

```
├── personal/          # 个人创建的 skills
│   └── novel/        # 小说创作工作流技能集（12个技能）
├── collected/         # 收集的优秀 skills
└── templates/         # skill 模板
```

## Skill 文件格式

每个 skill 是独立的 `SKILL.md` 文件，使用 YAML frontmatter 定义元数据：

```yaml
---
name: skill-name
description: 触发条件描述。当用户请求"关键词1"、"关键词2"时触发。
---
```

描述中的触发关键词会被 Claude Code 自动识别。

## Novel 创作技能集架构

`personal/novel/` 包含一套完整的小说创作工作流：

```
阶段一：筹备期
├── novel-project      # 创建项目目录
├── novel-ideas        # 生成创意方案
├── novel-worldbuilding# 构建世界观
├── novel-characters   # 创建人物设定
└── novel-outline      # 创作故事大纲

阶段二：创作期
├── novel-writing      # 写作控制（分层写作/修订/润色）
├── novel-draft        # 撰写初稿
├── novel-revision     # 结构修订
└── novel-polish       # 润色成品

阶段三：成品期
└── novel-cover        # 封面生成
```

辅助技能：
- `novel-standards` - 创作规范基础
- `novel-workflow` - 总控技能，引导完成整个流程

### 技能间协作

技能通过读取项目目录中的前置素材实现协作：
- 后续技能自动读取前置阶段生成的文件
- 文件存放于 `~/novels/项目名/` 目录
- 遵循 01-灵感创意 → 02-背景设定 → ... → 07-成品 的阶段结构

### 写作控制分层

`novel-writing` 提供分层处理避免单次大规模操作：
- 场景层（500-2000字）→ 章节层（3000-8000字）→ 单元层（3-10章）→ 全文层
- 修订和润色也分层处理

## 开发约定

### 新增技能

1. 在适当目录创建 `SKILL.md`
2. 使用 frontmatter 定义 `name` 和 `description`（含触发关键词）
3. 对于需要用户选择的交互，使用 AskUserQuestion 工具而非文字输入
4. 遵循现有技能的结构模式

### 修改技能

修改技能时需注意级联影响：
- 若修改某技能的输出文件格式，需检查下游技能是否依赖该格式
- 技能描述变更可能影响触发准确性

### 测试技能

将技能复制到 `~/.claude/skills/` 后在 Claude Code 中测试触发和执行。