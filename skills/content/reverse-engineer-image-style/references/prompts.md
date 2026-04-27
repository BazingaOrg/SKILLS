# 风格反向工程提示词库

本文件为 `SKILL.md` 中各原子操作（A1-A7）的具体执行模板。使用时，把对应模板的内容注入当前对话（连同用户提供的参考图），按模板要求输出。

每个模板结构：**用途 → 何时调用 → 模板正文**。

---

## T1 — L1-L4 分层观察与标注（对应 A1）

**用途**：所有场景的第一步，把图片做结构化的视觉分析，为后续步骤铺垫。

**调用指令**：

> 请仔细观察附带的图片，按以下四个层级输出结构化观察笔记。目标是把"风格"与"主体"解耦，为后续跨主题迁移做准备。若有多张图，先对每张独立做 L1-L4，再进入 T2 求共性。

### L1 主体内容（将被替换，仅作记录）
- 主体是什么（人/物/场景）、在做什么（动作/状态）
- 画面中出现的具体对象清单
- （这一层**不进入**最终 prompt，仅用于理解画面）

### L2 表层耦合属性（条件保留）
- 具体配色（主要 hex 色值 2-4 个）
- 服装 / 道具 / 局部环境细节
- 特定时间 / 天气（如"夏日午后""暴雨中"）
- 标注：哪些是"可替换"（新主体有等效属性就换），哪些是"建议保留"（与风格强相关）

### L3 美学框架（核心保留，请尽可能详细）
- **构图逻辑**：三分法 / 对角线 / 中心对称 / 黄金分割 / 留白策略 / 前中后景层次
- **光影方案**：光源方向、硬/软、色温、阴影处理、高光处理、是否有辅光
- **镜头语言**：焦距推测（广角/标准/长焦）、角度、距离、景深、畸变特征、bokeh 形态
- **材质与笔触**：写实 / 绘画 / 3D / 像素？质感粗糙度？
- **后期与技法**：胶片 / 数码？分离调色？grain / halation / bloom / vignette？

### L4 元艺术哲学（强制保留）
- **情绪基调**：静谧 / 躁动 / 疏离 / 温情 / 史诗 / 挽歌 ...
- **叙事视角**：旁观者 / 亲历者 / 上帝视角 / 第一人称
- **时代/流派锚定**：尽量具体到可检索的名词。例："1970s Kodachrome"、"Wes Anderson 对称构图"、"Studio Ghibli 田园"、"浮世绘"、"新海诚雨后城市"、"北欧极简"、"Cinestill 800T 夜景"
- **文化语境/隐喻**：如有，简要说明

**输出要求**：用简洁但具体的语言，避免 `beautiful` / `amazing` 等空词。L3/L4 是重点，字数可以多；L1/L2 简短即可。

---

## T2 — 多图共性提取（对应 A2）

**用途**：用户提供 2 张及以上参考图时使用，从多张图中提炼"风格骨架"，而不是机械拼接。

**调用指令**：

> 你已对 N 张参考图分别执行了 T1 分层观察。现在请做一次"共性提取"，输出三类特征。

### 共性特征（所有图都具备）
这是风格库的**骨架**。列出 L3 / L4 层级中，**每张图都明确具备**的特征。每条后面标注"出现率：N/N"。

### 高频特征（多数图具备）
出现在 ≥ 60% 图中的次级特征。标注"出现率：x/N"。这些可作为"推荐但非必须"的特征。

### 异常特征（仅单张具备）
仅单张图具备的特征。对每条判定：
- **排除**（偶然的、非风格性）
- **作为变体标签保留**（风格允许的自由度空间）

**输出格式**：

```
共性特征（骨架，必进最终 prompt）：
- [特征描述] （N/N）
- ...

高频特征（次级，可选）：
- [特征描述] （x/N）
- ...

异常特征（单张）：
- [特征描述]（仅图 X） → 建议：[排除 / 变体]
- ...
```

---

## T3 — 美学指纹 JSON（对应 A3）

**用途**：把 T1（+ T2）的观察结果结构化为可复用的 JSON 美学指纹。

**调用指令**：

> 基于前面的观察结果，输出完整的美学指纹 JSON，严格按下面的 schema。所有字段必须填写，无法判断时标注 `"N/A"` 并简要说明原因。

```json
{
  "era_genre_anchor": "时代/流派锚定，例如 '1970s Kodachrome' / 'Studio Ghibli' / 'Wes Anderson 对称构图' / '浮世绘'",
  "art_medium": "艺术媒介，例如 'analog film photography' / 'digital painting' / '3D render' / 'traditional ink wash'",
  "art_style": "艺术风格，例如 'cinematic realism' / 'painterly impressionism' / 'cel-shaded anime'",
  "aspect_ratio": "画幅比例，例如 '2.39:1' / '16:9' / '1:1' / '4:5' / '3:2'",
  "composition_logic": "构图逻辑与视觉引导，具体描述（包含前中后景层次、视觉重心、留白策略）",
  "lighting_scheme": "光照方案（光源方向、硬软、色温、阴影/高光处理、是否有辅光）",
  "camera_perspective": {
    "angle": "角度（正面/低/高/鸟瞰/虫瞰/微仰/微俯）",
    "distance": "距离（特写/中景/全景/远景）",
    "lens_characteristics": "镜头特性（焦距推测、景深、畸变、bokeh 形态）"
  },
  "palette": {
    "dominant": ["#hex1", "#hex2"],
    "accent": ["#hex3"],
    "tonal_logic": "调色逻辑，例如 'warm shadows cool highlights, split-toning' / 'teal-orange complement' / 'muted earthy monochrome'"
  },
  "rendering_details": {
    "texture_and_materiality": "材质与纹理通用描述",
    "visual_effects": "特殊视觉效果（halation / grain / bloom / motion blur / chromatic aberration）",
    "brushwork_or_technique": "笔触或技法（厚涂/水彩/数码扁平/点彩/cel-shading）"
  },
  "post_processing": "后期风格综合描述，例如 'halation, mild grain, lifted blacks, desaturated reds, subtle vignette'",
  "mood_and_atmosphere": "情绪氛围",
  "narrative_elements": "叙事风格或视觉隐喻（如有）",
  "negative_cues": [
    "需要明确排除的反模式，例如 'no HDR', 'no oversaturation', 'no digital sharpness', 'no plastic skin'"
  ],
  "transfer_risk_tags": {
    "universal": ["可无条件迁移的属性列表（构图/光影逻辑/镜头/画幅等）"],
    "conditional": ["按新主体需适配的属性列表（调色/时辰/具体材质等）"],
    "subject_bound": ["与原主体强绑定、迁移风险高的属性列表（特定笔触×主题、pose×气质等）"]
  }
}
```

**注**：
- `palette.dominant/accent` 用目视估计给出 hex（2-4 主色 + 1-2 点缀色即可，不必精确）
- `transfer_risk_tags` 三个桶的内容加起来应覆盖 L3/L4 的主要特征
- 多图模式下，此 JSON 只反映 T2 的"共性 + 高频"特征，不含异常特征

---

## T4 — 迁移适配度检查（对应 A4）

**用途**：跨主题迁移前，检查新主体与原美学的兼容性，生成"经过适配"的最终关键词集。

**调用指令**：

> 用户希望把上述美学指纹应用到新主体：
>
> **[NEW_SUBJECT_DESCRIPTION]**
>
> 请执行迁移适配度检查。

### 1. 兼容性评分（1-10）
整体兼容度打分，并用一句话说明主要矛盾点（如有）。

### 2. 风险标签复核
对 JSON 中 `transfer_risk_tags.subject_bound` 的每一项，判断：
- 是否真的会与新主体冲突（有时担忧是多余的）
- 建议：**保留 / 降权 / 丢弃 / 替换为等效属性**
- 若建议"替换"，给出替换方案

### 3. Conditional 属性的主体适配
对 `transfer_risk_tags.conditional` 中的属性，给出针对新主体的具体适配建议。

示例：原图 "warm golden hour palette" + 新主体 "深夜赛博都市" → 适配为 "cool neon-lit palette retaining the same low-key rim-lighting logic and split-toning structure"。关键是**保留底层逻辑，替换表层表达**。

### 4. 最终可迁移视觉 DNA
输出一组英文关键词（8-15 个），这些是经过适配度检查、确认可用的美学框架关键词。**严格按权重顺序公式排列**：

```
[主体] → [流派锚定] → [构图] → [光照] → [镜头] → [色彩/调色] → [材质/技法] → [氛围] → [后期] → [排除项]
```

---

## T5a — Midjourney 提示词生成（对应 A5）

**何时调用**：用户目标工具是 Midjourney（v6+）。

**调用指令**：

> 基于前面的美学指纹和（如有的话）T4 适配结果，为 Midjourney 生成提示词。严格遵循下方规范。

### 参数速查

| 参数 | 作用 | 建议值 |
|---|---|---|
| `--ar` | 画幅比例 | 映射 `aspect_ratio`：2.39:1 → `21:9` 或 `7:3`；16:9 → `16:9` |
| `--sref <url>` | 风格参考图（强力，推荐） | 如用户提供原图 URL 必用 |
| `--sw` | 风格参考权重 | 默认 100；严格还原 400-700；弱引用 20-50 |
| `--stylize` (`--s`) | MJ 自身艺术化程度 | 写实类 50-150；艺术化 500-1000 |
| `--style raw` | 减少 MJ 美化偏好 | 需要真实感、电影感时 |
| `--chaos` (`--c`) | 结果多样性 | 0-30；需要构图多样用 20-40 |
| `--v` | 模型版本 | 6.1（当前稳定） |

### 权重顺序（严格遵守）

```
[主体] → [艺术流派/时代锚定] → [构图] → [光照] → [镜头] →
[色彩/调色] → [材质/技法] → [氛围/情绪] → [后期/胶片特征] → [工具参数]
```

### 输出三个变体

1. **忠实复刻版**：严格还原原图气质，`--sw` 偏高（300-500），`--stylize` 保守（100-200）
2. **主体适配版**：按 T4 调整 conditional 属性，`--sw` 中等（100-200），允许 MJ 对新主体发挥
3. **SREF 极简版**：假设已用 `--sref`，prompt 只保留新主体 + 流派锚定 + 必要构图，其他交给 sref

每个变体给出：
- 完整 prompt 字符串（含所有参数）
- 一行设计取舍说明

---

## T5b — Stable Diffusion 提示词生成（对应 A5）

**何时调用**：用户目标工具是 SD 1.5 / SDXL / Pony / Illustrious / 其他 SD 衍生模型。

**调用指令**：

> 为 Stable Diffusion 生成提示词。输出 **Positive + Negative + 采样建议** 三部分。

### 权重语法规则

| 写法 | 效果 |
|---|---|
| `(keyword:1.2)` | 加强 1.2 倍 |
| `(keyword:1.3)` ~ `(keyword:1.5)` | 强化核心特征 |
| `(keyword:0.8)` | 弱化 |
| `[keyword]` | 小幅弱化（约 0.9） |

**铁律**：总权重词（> 1.0 的）控制在 ≤ 8 个，否则画面扭曲。核心特征才加权，装饰性词汇不加。

### Positive Prompt 结构

严格按权重顺序公式排列。核心特征用 `(:1.2)` - `(:1.3)` 加权：

```
[主体描述], [流派锚定], (构图关键词:1.2), (光照关键词:1.3), [镜头], 
[色彩/调色词], [材质/技法], [氛围], [后期特征]
```

### Negative Prompt

必包含：
- `negative_cues` JSON 字段中的每一项（翻译为英文）
- 通用 neg（按题材取舍）：`lowres, blurry, bad anatomy, extra fingers, watermark, signature, jpeg artifacts, oversaturated, plastic skin, deformed`

### 采样建议

| 参数 | 建议值 |
|---|---|
| Steps | 25-35 |
| CFG Scale | 写实 5-6.5；艺术化 7-9 |
| Sampler | DPM++ 2M Karras（通用）/ Euler a（艺术向） |
| Size | 按 `aspect_ratio` 映射：2.39:1 → 1536×640；16:9 → 1344×768；1:1 → 1024×1024（SDXL） |
| Hires.fix | 推荐开启，denoise 0.3-0.45 |

### 输出两个变体

1. **写实向**：适配 Realistic Vision / Juggernaut / RealVisXL 等，权重保守
2. **艺术向**：适配 DreamShaper / AnythingXL / PonyDiffusion 等，权重可激进

---

## T5c — Flux 提示词生成（对应 A5）

**何时调用**：用户目标工具是 Flux.1（dev / pro / schnell）或基于 Flux 的衍生服务。

**调用指令**：

> 为 Flux 生成提示词。Flux 与 SD 的提示词范式**不同**，需用自然语言长句而非标签堆。

### Flux 特性

- **偏好自然语言长句**，不喜欢逗号分隔的短词堆（与 SD 相反）
- 对**摄影 / 电影术语**理解力很强（"shot on 35mm film"、"anamorphic lens"、"golden hour bokeh"）
- 对**艺术史术语**也理解好（"impressionist brushwork"、"ukiyo-e linework"）
- 权重语法支持有限，不要堆括号
- Negative prompt 支持弱，靠**正向描述**控制画面

### 输出格式

一段 60-120 词的自然语言描述，按权重顺序公式的**信息密度**分布：
- **前半段**（高权重区）：主体 + 流派锚定 + 构图 + 光照
- **后半段**（中低权重区）：镜头 + 材质 + 氛围 + 后期

参数层（Flux API 常用）：
- `guidance_scale`：写实 2.5-3.5；艺术化 4-6
- `num_inference_steps`：dev 版 28-40；schnell 4-8
- 尺寸按 `aspect_ratio` 映射

### 输出两个变体

1. **摄影写实向**：用电影摄影术语（"shot on Cinestill 800T, anamorphic lens flare, 35mm wide shot, natural halation"）
2. **艺术绘画向**：用艺术史术语（"painted in the style of late impressionism, visible brushstrokes, muted palette reminiscent of early Monet"）

---

## T5d — 通用自然语言版 / 跨主题迁移模板（对应 A5）

**何时调用**：
- 用户没有指定目标工具
- 或在 ChatGPT / Claude 等对话式环境中让模型辅助生图 / 迭代

**调用指令**：

> 按通用自然语言模板生成提示词。该模板跨工具兼容（MJ / SD / Flux 都能吃），但不含工具特定参数。

### 模板结构

```
[NEW_SUBJECT], in the style of [ERA_GENRE_ANCHOR], 
[COMPOSITION], [LIGHTING], [LENS], 
[PALETTE], [TEXTURE_TECHNIQUE], 
[MOOD], [POST_PROCESSING]. 
Avoid [NEGATIVE_CUES]. 
Aspect ratio [AR].
```

### 示例（原图：宫崎骏式山村 → 新主体：赛博朋克城市）

> A cyberpunk city street at dusk, in the style of Studio Ghibli hand-drawn animation meets 1980s Kodachrome, gentle wide composition with layered foreground/midground/background, soft diffused rim lighting with warm accent glow from neon signs, 35mm wide shot with mild atmospheric haze, palette dominated by muted teal and amber with dusty pink highlights, painterly cel-shaded textures with subtle grain, wistful and quietly optimistic mood, mild halation and lifted blacks. Avoid HDR, avoid digital sharpness, avoid neon oversaturation. Aspect ratio 16:9.

### 输出要求

一次给出 **3 个不同主体**的样例 prompt，覆盖不同类型（人物 / 风景 / 物件 / 抽象场景），用以验证风格的跨主题稳定性。三个样例必须共享**同一套 L3/L4 描述**，只替换 L1 主体和适配后的 L2 属性。

---

## T6 — 自检盲测（对应 A6）

**用途**：生成 prompt 后的最后一道验证，确保"神韵"没有在转译中流失。

**调用指令**：

> 扮演一个**没看过原图的读者**，仅凭你刚才生成的 prompt / 视觉 DNA 关键词，完成盲测。

### 1. 脑补描述
仅凭 prompt，描述你想象中的画面（100 字以内）。刻意不回头看原图。

### 2. 神韵对比
然后再看原图，把脑补画面与原图的**气质/神韵**（不是主体）做对比：

- ✅ **成功保留的特征**：
- ⚠️ **损失或偏差的特征**：
- ❌ **引入了原图没有的假特征**：

### 3. Prompt 修订建议
如果存在 ⚠️ 或 ❌，给出具体修订动作：
- 加哪个词 / 删哪个词
- 调整哪个位置（权重顺序）
- SD 场景：调整哪个权重数值
- MJ 场景：调整 `--sw` / `--stylize` 数值

**迭代规则**：若发现 ≥ 2 条偏差，修订后再跑一次 T6，直到最多 2 轮。超过 2 轮仍有明显偏差，回到 T3 检查美学指纹是否在源头就漏掉了关键维度。

---

## T7 — 中文风格配方报告（对应 A7）

**何时调用**：用户需要深入理解风格、沉淀到个人风格库、或向非技术伙伴解释时。

**调用指令**：

> 生成一份详细的中文"风格配方"报告。

### 1. 风格概述
一句话概括"神韵"，再用 2-3 句展开背景。

### 2. 核心美学框架分析（L3）
- **构图逻辑**
- **光影方案**
- **色彩倾向与情绪**
- **镜头语言与视角**
- **质感与细节**
- **笔触或技法**

### 3. 元艺术哲学（L4）
- **情绪基调**
- **叙事视角**
- **时代/流派锚定**
- **文化语境**（如有）

### 4. 跨主题迁移风险速查
把 JSON 的 `transfer_risk_tags` 翻译为中文表格：

| 属性 | 风险等级 | 迁移建议 |
|---|---|---|
| ... | Universal / Conditional / Subject-bound | ... |

### 5. 风格配方：跨主题应用指南
- **主料（Subject）**：新主体如何描述
- **调料（Visual DNA）**：英文关键词集合
- **火候（Parameters）**：目标工具的参数建议
  - MJ：`--ar`、`--sref`、`--sw`、`--stylize` 建议值
  - SD：权重分配、采样器、CFG、分辨率
  - Flux：guidance_scale、steps

### 6. 英文提示词示例
给一个假想新主体的完整 prompt 示例（使用 T5d 通用模板），体现"风格不变，主体可变"。

### 7. 沉淀建议（可选）
如果这个风格值得长期复用，建议用户：
- 保存此 JSON 指纹
- 如用 MJ，保存 `--sref` 图片 URL
- 如用 SD，考虑训练 LoRA 或保存关键词模板
