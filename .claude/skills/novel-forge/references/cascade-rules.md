# 级联规则（Cascade Rules）

当上游产物被修改后，下游产物可能失效。本文件定义修改的传播规则。

## 依赖关系图

```
stage0_params（参数）
  └→ stage1_ideas（灵感）
      └→ stage2_characters（人设）★
          └→ stage3_style（风格）
          └→ stage4_outline（大纲）
              └→ stage5_detailed_outline（细纲）★
                  └→ stage6_scene_*（场景正文）★
                      └→ stage6_draft（合并稿）
                          └→ stage7_revised（修订稿）
                              └→ stage8_test_report（测试报告）
                                  └→ stage9_final（终稿）
```

★ = 高频修改点

## 级联规则表

| 被修改的产物 | 直接影响（标记 dirty） | 建议操作 |
|---|---|---|
| stage0_params | 全部下游 | 罕见，通常意味着重新开始 |
| stage1_ideas | characters, outline, detailed_outline, all scenes | 评估影响范围，可能需大规模重写 |
| stage2_characters | 涉及该角色的 scenes + draft + revised + final | 列出涉及场景，逐一修订 |
| stage3_style | all scenes + draft + revised + final | 建议 REVISE_POLISH 全文 |
| stage4_outline | detailed_outline + all scenes + draft + revised + final | 先更新细纲，再修订受影响场景 |
| stage5_detailed_outline | 被修改的场景 + draft + revised + final | 仅修订变更场景 |
| stage6_scene_{i} | draft + revised + final（且检查 i±1 衔接） | 自动更新合并稿 |
| stage7_revised | test_report + final | 重新测试 |

## 脏标记格式
在 manifest.md 中记录：
```
## 脏标记
- stage6_scene_3: dirty (原因: stage2_characters 修改于 {timestamp}, 角色"林晚"说话风格变更)
- stage6_scene_5: dirty (原因: stage2_characters 修改于 {timestamp}, 角色"林晚"出场场景)
- stage7_revised: dirty (原因: stage6_scene_3 尚未更新)
```

## 智能级联：精确定位受影响场景
修改角色/细纲时，不盲目标记所有场景。而是：
1. 读取 stage5_detailed_outline.md 中每个场景的"在场人物"
2. 仅标记包含被修改角色的场景
3. 如果修改的是角色关系 → 标记涉及关系双方的场景

## 级联修订执行
当用户决定执行级联修订时：
1. 按场景顺序处理（因为后续场景可能依赖前面的修订结果）
2. 每个 dirty 场景调用 REVISE_SCENE
3. 全部完成后重新合并 draft
4. 清除已处理的脏标记

## 样本库与级联

样本库的变更（增/删/改样本）**不产生脏标记**。
原因：样本是参考资料而非项目依赖，增删样本不会导致已写内容"失效"。

但以下情况例外：
- 如果用户执行 SAMPLE_APPLY 指定某样本写了某场景，
  然后删除了该样本 → 不影响（技法已经融入正文）
- 如果用户大批量更新样本库后说"按新样本风格重写" → 按 EDIT_STYLE 处理