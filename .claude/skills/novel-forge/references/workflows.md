# 工作流详细定义

---

## CREATE_FULL: 完整创建流程

### 前置条件
无（新项目）

### 流程
分为 Phase 0-6 共7个阶段，产出 stage0-9 共10个文件。

#### Phase 0: 参数收集
从用户消息中提取参数（缺失则询问）：平台、题材标签、情绪赛道、目标读者画像、目标字数（8000-15000）、场景数（8-15）、可选灵感种子。
写入 `workspace/stage0_params.md`。创建 `workspace/manifest.md`。

#### Phase 1: 灵感 + 人设（Agent-1）
读取 agent-creative.md + platform-profiles.md。
产出: stage1_ideas.md, stage2_characters.md。
**⏸ 暂停**: 展示3个灵感请用户选择。

#### Phase 2: 风格 + 大纲 + 细纲（Agent-2）
读取 agent-architect.md。
产出: stage3_style.md, stage4_outline.md, stage5_detailed_outline.md。
**⏸ 暂停**: 展示大纲请用户确认。

#### Phase 3: 逐场景写作（Agent-3）
逐场景调用，每次传入当前场景细纲 + 上一场景末尾300字。
产出: stage6_scene_{i}.md → 合并为 stage6_draft.md。

#### Phase 4: 整体修订（Agent-4）
产出: stage7_revised.md, stage7_edit_notes.md。

#### Phase 5: 读者测试（Agent-5）
最多重试3轮。产出: stage8_test_report.md。

#### Phase 6: 终审（Agent-6）
产出: stage9_final.md, stage9_qa_report.md。

---

## CONTINUE_NEXT: 续写下一个场景

### 前置条件
- stage5_detailed_outline.md 存在
- 至少已有一个 stage6_scene_{i}.md

### 流程
1. 扫描已有的 stage6_scene_*.md，确定下一个未写场景编号 `next_i`
2. 如果 `next_i` > 总场景数 → 提示"所有场景已写完，建议执行整体修订"
3. 检查脏标记：如果 stage2_characters 或 stage3_style 或 stage5_detailed_outline 是 dirty → 警告用户
4. 读取 agent-writer.md
5. 传入: stage0_params + stage2_characters + stage3_style + 场景 next_i 的细纲 + 上一场景末尾300字
6. 调用 Agent-3 写作
7. 产出: `stage6_scene_{next_i}.md`
8. 更新 manifest
9. 如果全部场景写完 → 自动合并为 stage6_draft.md，建议执行 REVISE_FULL

### 级联影响
无（下游产物）

---

## CONTINUE_INSERT: 插入新场景

### 前置条件
- stage5_detailed_outline.md 存在

### 流程
1. 用户指定插入位置（在场景x之后）
2. 调用 Agent-2，传入现有细纲 + 用户描述，生成新场景的细纲
3. 更新 stage5_detailed_outline.md（插入新场景，后续场景重新编号）
4. 调用 Agent-3 写作新场景
5. 产出: 新的 stage6_scene_{i}.md
6. 标记后续场景文件名需要重编号（或在合并时处理）
7. 标记 stage6_draft.md / stage7_revised.md 等下游为 dirty

---

## EDIT_CHARACTER: 编辑人物设定

### 前置条件
- stage2_characters.md 存在

### 流程
1. 读取当前 stage2_characters.md 展示给用户
2. 理解用户的修改意图：
   - 修改现有角色的属性（性格/背景/说话风格...）
   - 添加新角色
   - 删除角色
   - 调整角色关系
3. 读取 agent-creative.md 中的"人物设定"任务部分
4. 调用 Agent-1，传入: 当前人设 + 用户修改指令 + stage0_params
5. Agent-1 执行修改并运行自我检验
6. 旧版本保存为 stage2_characters_v{n}.md
7. 写入新的 stage2_characters.md
8. **级联**: 读取 cascade-rules.md，标记所有涉及该角色的已写场景为 dirty
9. 展示: 修改摘要 + 受影响的场景列表 + 建议操作

### 暂停点
展示修改后的人设差异对比，请用户确认。

---

## EDIT_BACKGROUND: 编辑故事背景

### 前置条件
- stage1_ideas.md 存在

### 流程
1. 读取 stage1_ideas.md（选定灵感部分）+ stage0_params.md
2. 理解用户修改意图（改时代背景、改地点、加设定...）
3. 调用 Agent-1，传入当前灵感设定 + 修改指令
4. 更新 stage1_ideas.md 中的选定灵感部分
5. **级联**: 标记 stage2_characters, stage4_outline, stage5_detailed_outline, 所有已写场景为 dirty
6. 建议用户依次审查并更新受影响的产物

---

## EDIT_SKELETON: 修订大纲骨架

### 前置条件
- stage4_outline.md 存在

### 流程
1. 展示当前大纲
2. 理解修改意图（调整转折点、改情绪曲线、增删幕...）
3. 读取 agent-architect.md
4. 调用 Agent-2，传入: 当前大纲 + 修改指令 + stage1_ideas + stage2_characters
5. 保存旧版本，写入新大纲
6. **级联**: 标记 stage5_detailed_outline 及所有已写场景为 dirty
7. **⏸ 暂停**: 展示新大纲差异，请用户确认

---

## EDIT_DETAILED_OUTLINE: 修订细纲

### 前置条件
- stage5_detailed_outline.md 存在

### 流程
1. 用户可指定修改范围：全部 / 特定场景
2. 读取 agent-architect.md
3. 调用 Agent-2，传入: 当前细纲 + 修改指令 + 大纲 + 人设
4. 保存旧版本，写入新细纲
5. **级联**: 标记修改范围内的已写场景为 dirty
6. 展示修改摘要

---

## EDIT_STYLE: 修改文笔风格

### 前置条件
- stage3_style.md 存在

### 流程
1. 展示当前风格指南
2. 理解修改意图（换视角、改句长、调对话比...）
3. 调用 Agent-2，传入当前风格 + 修改指令
4. 保存旧版本，写入新风格
5. **级联**: 标记所有已写场景为 dirty（风格变更影响全局）
6. 建议用户决定：全部重写 / 只润色 / 仅对新场景生效

---

## REVISE_SCENE: 修订指定场景

### 前置条件
- 目标场景文件存在

### 流程
1. 确定目标场景编号（从用户消息解析）
2. 读取该场景 + agent-editor.md
3. 理解修改意图：
   - 用户给出具体修改方向 → 定向修订
   - 用户只说"这段不行" → Agent-4 自主诊断修订
4. 调用 Agent-4，传入: 目标场景 + 人设 + 风格 + 细纲 + 修改指令
5. Agent-4 执行定向修订（仅该场景）
6. 保存旧版本，写入修订后场景
7. **级联**: 标记 stage6_draft.md, stage7_revised.md 为 dirty
8. 检查与前后场景的衔接是否受影响

---

## REVISE_TYPO: 修订错别字/格式

### 前置条件
- 至少有一个已写场景或修订稿

### 流程
1. 确定检查范围：全文 / 指定场景
2. 读取 agent-final-qa.md 中的基础质量检查清单
3. 调用 Agent-6，仅执行机械性检查: 错别字、标点、格式、人名统一
4. 直接修正所有问题（不涉及内容层面）
5. 产出: 修正后文件 + 修正清单
6. 不产生级联（纯机械修正）

---

## REVISE_POLISH: 润色打磨

### 前置条件
- 目标内容存在

### 流程
1. 确定范围：全文 / 指定场景
2. 读取 agent-editor.md + stage3_style.md
3. 调用 Agent-4，指令限定为: 仅优化语言表达，不改动情节和人物行为
4. 修改范围: 替换平庸措辞、优化节奏、加强感官描写、删除冗余
5. 保存旧版本，写入润色后内容
6. 产出: 润色后文件 + 润色说明（标注每处修改理由）

---

## REVISE_FULL: 整体修订

### 前置条件
- stage6_draft.md 存在（或所有场景已完成可合并）

### 流程
与 CREATE_FULL 的 Phase 4 一致。如 draft 不存在则先合并场景文件。
调用 Agent-4 执行完整四遍审查。

---

## EVAL_CHAPTER: 评价指定章节/场景

### 前置条件
- 目标场景文件存在

### 流程
1. 确定目标场景
2. 读取 agent-reader.md + reader-personas.md
3. 调用 Agent-5，但范围限定为该场景
4. 选择2个读者人设（1个核心 + 1个严格）进行阅读模拟
5. 产出: 场景评价报告，包含:
   - 两个人设的逐段反应（KEEP/HESITATE/DROP + 内心OS）
   - 场景评分（1-10）
   - 亮点与问题清单
   - 具体修改建议
6. 写入 `workspace/eval_scene_{i}.md`
7. 不产生级联（仅评价不修改）

---

## EVAL_CHARACTER: 评价角色

### 前置条件
- stage2_characters.md 存在
- 至少有部分已写场景

### 流程
1. 确定目标角色（从用户消息解析）
2. 读取 agent-reader.md
3. 调用 Agent-5，执行角色专项评估:
   - 人设与实际表现一致性（对比设定与已写台词/行为）
   - 角色弧光（是否有成长/变化）
   - 记忆点（读者能记住什么）
   - 台词辨识度（遮住名字能否认出说话人）
   - 与其他角色的化学反应
   - 角色功能是否到位
4. 产出: `workspace/eval_character_{name}.md`

---

## EVAL_SCENE: 评价指定场景/片段

### 前置条件
- 目标内容存在

### 流程
1. 用户指定具体内容（"那段打戏"/"开头的对话"/"高潮部分"）
2. 定位到具体文本范围
3. 调用 Agent-5，从以下维度评估:
   - 节奏感（快慢是否得当）
   - 画面感（感官描写是否充分）
   - 对话质量（是否推进情节、是否有潜台词）
   - 情绪传递（是否达到预期情绪效果）
   - 信息效率（有无冗余/缺失信息）
4. 产出: 内联评价报告

---

## EVAL_FULL: 整体评价

### 前置条件
- stage7_revised.md 存在（或 stage6_draft.md）

### 流程
与 CREATE_FULL 的 Phase 5 一致。完整3人设读者测试。

---

## STATUS: 查看项目状态

### 前置条件
- workspace/ 目录存在

### 流程
1. 读取 manifest.md
2. 扫描所有 workspace/ 文件
3. 输出状态面板:
```
📊 NovelForge 项目状态
━━━━━━━━━━━━━━━━━━━━
📋 基本信息: {平台} | {题材} | 目标{字数}字 | {场景数}个场景
━━━━━━━━━━━━━━━━━━━━
✅ 参数设定      stage0_params.md
✅ 灵感方案      stage1_ideas.md
✅ 人物设定      stage2_characters.md  ⚠️ dirty
✅ 文笔风格      stage3_style.md
✅ 大纲骨架      stage4_outline.md
✅ 逐场景细纲    stage5_detailed_outline.md
🔶 场景写作      5/10 完成 [■■■■■□□□□□]
   ✅ 场景1-5  ⚠️ 场景3 dirty（人设变更）
   ⬜ 场景6-10
⬜ 整体修订
⬜ 读者测试
⬜ 终审出稿
━━━━━━━━━━━━━━━━━━━━
⚠️ 脏标记: stage2_characters → scene_3, scene_5
💡 建议: 续写场景6，或先修订 dirty 场景
```

---

## EXPORT: 导出成稿

### 前置条件
- stage7_revised.md 或 stage6_draft.md 存在

### 流程
1. 如果 stage9_final.md 已存在且无 dirty → 直接输出
2. 否则 → 先执行 REVISE_FULL → EVAL_FULL → 终审（Agent-6）
3. 产出 stage9_final.md + stage9_qa_report.md

---

## ROLLBACK: 回退版本

### 前置条件
- 目标文件有历史版本（_v{n} 文件）

### 流程
1. 用户指定要回退的产物（如"回退人物设定"）
2. 列出该产物的所有版本，展示差异摘要
3. 用户选择要恢复的版本
4. 当前版本标记为新的 _v{n}，选定版本成为当前版本
5. 按级联规则标记下游为 dirty

## SAMPLE_ADD: 收录新样本

### 前置条件
无

### 流程
1. 获取用户提供的文本片段
   - 用户直接粘贴内容
   - 或用户引用项目中已写的某段（"把场景3的开头存起来"）
2. 询问（如用户未主动说明）：
   - 来源（可选，"不记得了"也行）
   - 你觉得这段好在哪里？（可选，用户可以只说"就是觉得好"）
3. 调用 Agent-3（写手身份），执行样本分析：
   - 自动识别技巧标签（从标签体系中选，最多5个）
   - 自动识别情绪标签（最多3个）
   - 自动识别题材标签（最多2个）
   - 技法拆解（这段到底好在哪，2-4条）
   - 提炼可迁移技法（未来写作时怎么用）
   - 量化特征（句均字数、对话占比等）
4. 生成样本文件 `samples/sample_{NNN}.md`
5. 更新 `samples/INDEX.md`（追加行 + 更新统计）
6. 展示收录结果：标签 + 技法摘要

### 暂停点
展示自动分析结果，请用户确认标签是否准确、是否要补充备注。

---

## SAMPLE_REMOVE: 删除样本

### 流程
1. 确定目标样本（ID 或关键词搜索）
2. 展示样本摘要，确认删除
3. 删除文件，更新 INDEX.md

---

## SAMPLE_LIST: 查看样本库

### 流程
1. 读取 INDEX.md
2. 格式化展示：

```
📚 风格样本库（共 X 个样本）
━━━━━━━━━━━━━━━━━━━━━━━━━━
#001 "她放下离婚协议书，笑了" 
     🏷 hook · dialogue · satisfying · revenge | 320字
     📝 开篇反转+角色声音区分

#002 "雨巷追逐戏"
     🏷 pacing-fast · sensory · suspense | 580字
     📝 短句密集制造紧迫感

━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 按标签筛选: 说"查看 hook 标签的样本"
```

3. 支持按标签筛选展示

---

## SAMPLE_ANALYZE: 分析片段技法（不收录）

### 流程
1. 获取用户提供的文本
2. 执行与 SAMPLE_ADD 相同的分析步骤
3. 输出分析报告但不写入样本库
4. 询问："要收录到样本库吗？"

---

## SAMPLE_APPLY: 指定参考样本写作

### 流程
1. 用户指定参考的样本（ID / 标签 / "上次那个风格"）
2. 读取对应样本的"可迁移技法"和"量化特征"
3. 将这些信息注入到当前写作/修订任务的 Agent 输入中
4. 正常执行 CONTINUE_NEXT / REVISE_SCENE 等工作流，但 Agent-3/4 额外收到样本参考指令

---

## PROJECT_SETTINGS: 修改项目基础参数

### 前置条件
- stage0_params.md 存在

### 流程
1. 读取当前 stage0_params.md
2. 理解用户要修改的参数：
   - 平台切换（番茄 ↔ 知乎）
   - 目标字数调整
   - 场景数调整
   - 目标读者画像调整
3. 执行修改，保存旧版本
4. **级联评估**：
   - 平台切换 → 标记 stage3_style（平台规范不同）+ 所有已写内容为 dirty
   - 字数调整 → 标记 stage5_detailed_outline（需重新分配）+ 可能影响已写场景
   - 场景数调整 → 标记 stage5_detailed_outline（需增删场景）
5. 输出变更摘要 + 级联影响提醒

### 暂停点
平台切换时展示对比："番茄（句均≤15字，对话≥60%）→ 知乎（句均≤20字，对话≥40%）"，请用户确认。

---

## BLURB_GENERATE: 生成故事简介/营销文案

### 前置条件
- stage4_outline.md 或 stage7_revised.md 存在

### 流程
1. 确定生成依据：如有终稿用终稿，否则用大纲
2. 读取 platform-profiles.md 中对应平台的标题风格
3. 调用 Agent-1（创意策划师身份）生成：
   - **一句话简介**（≤30字，用于推荐位）
   - **三行简介**（用于详情页）
   - **平台标题候选**（3-5个，符合平台风格）
4. 输出供用户选择和修改

### 输出
写入 `workspace/blurb.md`

---

## 错误恢复与边界情况处理

### 无 manifest 的遗留项目
如果 workspace/ 存在但无 manifest.md：
1. 扫描已有文件，推断当前进度
2. 生成 manifest.md
3. 提示用户确认推断是否正确

### 草稿超出平台限制
如果 stage6_draft.md 或 stage7_revised.md 超出目标字数±10%：
1. 在 STATUS 中高亮提示
2. 建议执行 EDIT_DETAILED_OUTLINE 调整场景分配
3. 或建议用户手动删减后重新修订

### 文件损坏/不完整
如果某个 stage 文件为空或格式异常：
1. 记录到 error_log.md
2. 提示用户该文件需要重新生成
3. 提供重生成选项（读取上游文件重新执行对应工作流）
