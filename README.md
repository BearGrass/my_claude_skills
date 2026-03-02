# My Claude Code Skills

收集和整理 Claude Code Skills 的仓库。

## 目录结构

```
├── personal/          # 个人创建的 skills
│   └── novel/        # 小说创作工作流技能集
│       ├── novel-project/      # 创建项目目录
│       ├── novel-ideas/        # 生成创意方案
│       ├── novel-worldbuilding/# 构建世界观
│       ├── novel-characters/   # 创建人物设定
│       ├── novel-outline/      # 创作故事大纲
│       ├── novel-draft/        # 撰写初稿
│       ├── novel-revision/     # 结构修订
│       └── novel-polish/       # 润色成品
├── collected/         # 收集的优秀 skills
│   └── *.md          # 从网上收集的 skill 文件
└── templates/         # skill 模板
    └── template.md   # 创建新 skill 的模板
```

## 已收录技能

### 小说创作工作流（Novel 系列）

一套完整的小说创作辅助技能，覆盖从灵感到成品的完整流程：

| 技能 | 触发词 | 功能 |
|------|--------|------|
| `novel-project` | 创建小说项目 | 创建标准化项目目录结构 |
| `novel-ideas` | 给我创意 | 生成3-5个小说创意方案 |
| `novel-worldbuilding` | 创建背景设定 | 构建世界观设定 |
| `novel-characters` | 创建人物设定 | 塑造完整人物形象 |
| `novel-outline` | 创建故事大纲 | 构建故事骨架结构 |
| `novel-draft` | 写初稿 | 撰写小说初稿 |
| `novel-revision` | 修订小说 | 结构性修订优化 |
| `novel-polish` | 润色小说 | 文字润色与成品输出 |

详见 [personal/novel/README.md](personal/novel/README.md)。

## 使用方法

Skills 可以通过以下方式使用：
- 将 skill 文件放入 `~/.claude/skills/` 目录
- 在 Claude Code 中使用 `/skill-name` 命令调用

## Skill 文件格式

每个 skill 文件应包含：
- 清晰的描述和用途
- 触发条件
- 具体的指令或行为

## 贡献

欢迎提交 PR 分享优秀的 skills！