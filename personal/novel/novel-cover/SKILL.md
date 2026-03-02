---
name: novel-cover
description: 生成番茄小说风格封面。当用户要求"生成封面"、"制作封面"、"小说封面"、"番茄小说封面"、"封面设计"、"书封面"时触发。根据小说大纲和设定，生成符合番茄小说风格的封面图片描述和AI绘画提示词。封面尺寸600×800像素，包含小说标题文字。
---

# 番茄小说封面生成器

根据小说大纲和设定，生成符合番茄小说风格的封面图片。

## 封面规格要求

### 番茄小说封面标准

```
尺寸：600×800 像素（3:4 比例）
格式：JPG / PNG
文件大小：≤ 500KB

布局结构：
┌────────────────────────────┐
│                            │
│      [主视觉区域]          │
│      人物/场景图           │
│                            │
│                            │
├────────────────────────────┤
│                            │
│   【小说标题】             │
│   （大字醒目）             │
│                            │
│   [作者名/标签]            │
│                            │
└────────────────────────────┘
```

### 封面元素

```
必须元素：
├── 小说标题（必须包含）
│   ├── 位置：底部 1/3 区域
│   ├── 字体：醒目易读
│   ├── 颜色：与背景对比强烈
│   └── 特效：可加描边/阴影
│
├── 主视觉图
│   ├── 人物形象（适合言情/都市）
│   ├── 场景氛围（适合玄幻/科幻）
│   └── 抽象元素（适合悬疑/奇幻）
│
└── 装饰元素
    ├── 类型标签
    ├── 作者名（可选）
    └── 系列标识（可选）
```

## 封面风格分类

### 言情/都市类

```
视觉特点：
├── 人物为主：男主/女主/双人
├── 氛围：浪漫、甜蜜、都市感
├── 色调：粉色、暖色、渐变
└── 元素：光晕、花瓣、城市背景

标题风格：
├── 字体：圆润/手写体
├── 颜色：白色/金色/粉色
├── 效果：柔光、发光
└── 位置：居中偏下

示例描述：
"现代都市背景，暖色调，女主背影面向城市天际线，长发飘逸，氛围浪漫，标题'时光与你都很甜'，白色大字，发光效果"
```

### 玄幻/仙侠类

```
视觉特点：
├── 人物为主：主角形象
├── 氛围：神秘、大气、仙气
├── 色调：蓝紫、金色、深邃
└── 元素：云雾、光芒、符文

标题风格：
├── 字体：古风/书法体
├── 颜色：金色/白色/青色
├── 效果：描边、发光、浮雕
└── 位置：居中

示例描述：
"仙侠风格，主角持剑立于云端，身后金色光芒万丈，蓝色渐变天空，仙气飘飘，标题'万古神帝'，金色立体字，古风字体"
```

### 科幻/末世类

```
视觉特点：
├── 场景为主：未来城市/宇宙
├── 氛围：科技、冷酷、宏大
├── 色调：蓝色、银色、黑色
└── 元素：机械、光束、星空

标题风格：
├── 字体：现代/科技体
├── 颜色：白色/青色/霓虹
├── 效果：霓虹光、故障风
└── 位置：居中/偏上

示例描述：
"科幻末世风格，废墟城市背景，破败建筑与霓虹光对比，冷色调，主角剪影面向远方，标题'末日曙光'，白色霓虹字，科技感"
```

### 悬疑/推理类

```
视觉特点：
├── 氛围为主：神秘、压抑
├── 色调：暗色、黑白、深蓝
└── 元素：剪影、影子、迷雾

标题风格：
├── 字体：尖锐/现代体
├── 颜色：白色/红色/黄色
├── 效果：撕裂、血迹（适度）
└── 位置：居中

示例描述：
"悬疑暗黑风格，深蓝色调，剪影人物站在迷雾中，若隐若现，氛围压抑，标题'消失的她'，白色尖锐字体，带裂纹效果"
```

### 历史/古言类

```
视觉特点：
├── 人物为主：古装人物
├── 氛围：古典、优雅
├── 色调：暖色、古铜、水墨
└── 元素：山水、建筑、花卉

标题风格：
├── 字体：毛笔/古风体
├── 颜色：金色/黑色/红色
├── 效果：印章、水墨
└── 位置：居中/竖排

示例描述：
"古风水墨风格，女子古装背影，手持团扇，淡雅色调，山水背景，标题'盛世长歌'，金色毛笔字，古风竖排"
```

## 工作流程

### 步骤1：读取小说信息

```
读取内容：
├── 小说标题
├── 小说类型
├── 主角设定
├── 核心场景
├── 整体氛围
└── 关键元素

来源文件：
├── 00-设定/故事大纲.md
├── 00-设定/人物设定.md
└── 00-设定/世界观.md
```

### 步骤2：确定封面方案

```
根据类型选择风格：
├── 言情/都市 → 浪漫人物风
├── 玄幻/仙侠 → 仙气大气风
├── 科幻/末世 → 科技未来风
├── 悬疑/推理 → 神秘暗黑风
├── 历史/古言 → 古风水墨风
└── 其他 → 根据具体内容定制

确认信息：
1. 封面风格
2. 主视觉内容
3. 标题呈现方式
4. 色调偏好
```

### 步骤3：生成封面描述

```
输出格式：

【封面设计方案】

小说信息：
├── 书名：《XXX》
├── 类型：XXX
└── 核心元素：XXX

封面设计：
├── 尺寸：600×800像素
├── 风格：XXX风格
├── 主视觉：[详细描述]
├── 色调：[颜色描述]
└── 氛围：[氛围描述]

标题设计：
├── 文字：《XXX》
├── 字体：[字体风格]
├── 颜色：[颜色]
├── 效果：[特效描述]
└── 位置：[位置描述]

AI绘画提示词：
[详细的英文prompt，用于AI生成图片]

标题叠加说明：
[如何在图片上添加标题文字]
```

### 步骤4：生成封面图片

```
方式一：AI生成（推荐）
1. 使用AI绘画工具（如Midjourney/DALL-E/SD）
2. 输入生成的prompt
3. 生成主视觉图片
4. 使用图片编辑工具添加标题文字

方式二：图库素材
1. 从正版图库选择合适素材
2. 进行合成和调色
3. 添加标题文字和装饰元素

方式三：设计工具
1. 使用Canva/PS等工具
2. 根据设计方案创建封面
3. 调整细节确保效果
```

## AI绘画Prompt模板

### 言情都市类

```
Base Template:
"A romance novel book cover, [character description], [setting], [mood], warm lighting, soft color palette, dreamy atmosphere, professional book cover design, 600x800 aspect ratio"

示例：
"A romance novel book cover, a beautiful young woman in elegant dress, back view facing modern city skyline, long flowing hair, warm sunset lighting, soft pink and gold color palette, romantic dreamy atmosphere, professional book cover design, 600x800 aspect ratio"
```

### 玄幻仙侠类

```
Base Template:
"A fantasy/xianxia novel book cover, [character description], [setting], [magical elements], ethereal atmosphere, dramatic lighting, mystical color palette, epic composition, professional book cover design, 600x800 aspect ratio"

示例：
"A xianxia novel book cover, a powerful cultivator in white robes holding a glowing sword, standing on clouds with golden divine light behind, blue and purple sky, ethereal atmosphere, dramatic lighting, mystical color palette, epic composition, professional book cover design, 600x800 aspect ratio"
```

### 科幻末世类

```
Base Template:
"A sci-fi/apocalyptic novel book cover, [setting description], [character silhouette], [technology elements], futuristic atmosphere, cold lighting, blue and silver color palette, cinematic composition, professional book cover design, 600x800 aspect ratio"

示例：
"A sci-fi apocalyptic novel book cover, ruined futuristic city skyline, lone silhouette figure standing on debris facing neon-lit horizon, cold blue and silver color palette, atmospheric fog, cinematic lighting, professional book cover design, 600x800 aspect ratio"
```

### 悬疑推理类

```
Base Template:
"A mystery/thriller novel book cover, [atmosphere description], [silhouette or shadow], [mysterious elements], dark and moody atmosphere, dramatic lighting, dark blue and black color palette, professional book cover design, 600x800 aspect ratio"

示例：
"A mystery thriller novel book cover, mysterious silhouette figure standing in thick fog, barely visible, dark atmospheric, deep blue and black tones, dramatic lighting, sense of mystery, professional book cover design, 600x800 aspect ratio"
```

### 历史古言类

```
Base Template:
"A historical Chinese novel book cover, [character in hanfu], [classical setting], [traditional elements], elegant classical atmosphere, warm ink wash color palette, Chinese painting style, professional book cover design, 600x800 aspect ratio"

示例：
"A historical Chinese romance novel book cover, elegant woman in traditional hanfu, back view holding a round fan, soft ink wash background with mountains, warm beige and gold tones, classical elegant atmosphere, Chinese painting style, professional book cover design, 600x800 aspect ratio"
```

## 标题文字设计

### 字体选择指南

```
言情/都市：
├── 思源黑体（简洁现代）
├── 汉仪旗黑（圆润可爱）
├── 方正兰亭（优雅柔美）
└── 手写体（个性化）

玄幻/仙侠：
├── 汉仪菱心体（锋利大气）
├── 方正字迹（古风）
├── 叶根友毛笔行书（传统）
└── 创意书法体（个性）

科幻/末世：
├── 思源黑体（现代简洁）
├── 汉仪菱心体（科技感）
├── 站酷高端黑（力量感）
└── 造字工房力黑（冲击力）

悬疑/推理：
├── 思源宋体（正式严谨）
├── 方正黑体（有力）
├── 造字工房劲黑（紧张感）
└── 创意变形字体（个性）

历史/古言：
├── 方正清刻本悦宋（古典）
├── 汉仪尚巍手书（毛笔）
├── 叶根友毛笔楷书（传统）
└── 古风书法体（典雅）
```

### 标题效果

```
基本效果：
├── 描边：2-4像素，深色描边
├── 阴影：投影效果，增加立体感
├── 发光：外发光，增加醒目度
└── 渐变：颜色渐变，增加层次

进阶效果：
├── 金属质感：金色渐变+浮雕
├── 霓虹效果：发光+颜色叠加
├── 水墨效果：墨迹纹理
├── 古风印章：红色印章元素
└── 故障风：位移+颜色分离
```

## 输出文件

### 保存位置

```
~/novels/[小说名]/
└── 成品/
    └── 封面/
        ├── 封面-原始图.png      # AI生成的原始图
        ├── 封面-带标题.png      # 添加标题后的封面
        ├── 封面-缩略图.jpg      # 缩略图版本（200×267）
        └── 封面设计方案.md      # 设计方案文档
```

### 设计方案文档

```markdown
# 《[小说名]》封面设计方案

## 基本信息
- 书名：《XXX》
- 类型：XXX
- 尺寸：600×800像素
- 创建日期：XXXX-XX-XX

## 设计说明
[设计理念说明]

## 封面元素
- 主视觉：[描述]
- 色调：[描述]
- 标题设计：[描述]

## AI Prompt
[用于生成图片的prompt]

## 版本记录
- v1.0：初始版本
```

## 使用示例

**用户输入**：生成番茄小说封面

**系统响应**：
```
正在读取小说信息...
已读取《星河守望者》的设定信息。

┌─────────────────────────────────────────────────────────┐
│                    封面设计方案                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  📚 书名：《星河守望者》                                 │
│  📂 类型：科幻                                           │
│  🎨 风格：科幻未来风                                     │
│                                                         │
│  封面设计：                                              │
│  ├── 主视觉：废墟城市剪影，霓虹光芒                      │
│  ├── 色调：深蓝+银色+霓虹色                              │
│  └── 氛围：末世希望感                                    │
│                                                         │
│  标题设计：                                              │
│  ├── 文字：《星河守望者》                                │
│  ├── 字体：科技感现代体                                  │
│  ├── 颜色：白色+青色霓虹                                 │
│  └── 效果：霓虹发光                                      │
│                                                         │
└─────────────────────────────────────────────────────────┘

AI绘画Prompt：
"A sci-fi apocalyptic novel book cover, ruined city skyline
silhouette against starry night sky, neon blue lights in
distance, lone figure silhouette standing on rooftop, deep
blue and silver color palette, atmospheric, cinematic
lighting, professional book cover design, 600x800 aspect ratio"

是否使用此方案生成封面？[Y/n/edit]
```

## 注意事项

1. **版权合规**：确保使用的素材和字体有合法授权
2. **标题必须**：封面必须包含小说标题文字
3. **风格统一**：封面风格需与小说类型匹配
4. **平台适配**：符合番茄小说封面上传要求
5. **多版本备选**：建议生成2-3个版本供选择