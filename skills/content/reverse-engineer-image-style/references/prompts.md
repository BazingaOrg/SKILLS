# 风格反向工程提示词库

本文件为 `SKILL.md` 中各步骤的具体执行模板。使用时，把对应模板的内容注入当前对话（连同用户提供的参考图），按模板要求输出。

**与主流程的对应关系**：

| SKILL.md 步骤 | 模板 | 备注 |
|---|---|---|
| Step 1 观察 | T1 | 必跑 |
| Step 2 指纹 | T3 | 必跑 |
| Step 3 适配 | T4 | 仅跨主体场景 |
| Step 4 出 prompt | T5 | 自然语言模板，适用于 ChatGPT/Claude/Gemini 等对话式 LLM |
| 可选 · 多图共性 | T2 | 见 [`multi-image.md`](multi-image.md) |
| 可选 · 自检盲测 | T6 | 用户明确要求时启用 |
| 可选 · 中文配方报告 | T7 | 用户明确要求时启用 |

每个模板结构：**用途 → 何时调用 → 模板正文**。

---

## T1 — L1-L4 分层观察与标注（Step 1）

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

## T2 — 多图共性提取（可选 · 仅多图场景）

**用途**：用户提供 2 张及以上参考图时使用，从多张图中提炼"风格骨架"，而不是机械拼接。完整工作流见 [`multi-image.md`](multi-image.md)。

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

## T3 — 美学指纹 JSON（Step 2）

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

## T4 — 跨主体适配（Step 3，仅跨主体时）

**用途**：跨主题迁移前，逐项体检新主体与原美学的兼容性，生成"经过适配"的最终关键词集。

**调用指令**：

> 用户希望把上述美学指纹应用到新主体：
>
> **[NEW_SUBJECT_DESCRIPTION]**
>
> 请执行跨主体适配。**不要给 1-10 兼容性评分**——那是虚指标。直接给可执行的风险列表与处理方案。

### 1. L4 兼容性体检（硬门槛）

先看元艺术哲学是否本质冲突：

- **情绪基调** 是否兼容？（例：挽歌 ↔ 欢快派对、史诗 ↔ 私密日常）
- **叙事视角** 是否可移植？（例：第一人称内心独白 ↔ 远景广告大片）
- **时代/流派锚定** 是否可移植？

**如发现本质冲突**：

- 不要硬上。
- 显式向用户报警，并给出三选一建议：(a) 换参考图；(b) 接受气质转向（指出会损失什么）；(c) 选择性放弃部分 L4 属性（明示牺牲哪一块）。
- 用户拍板后再继续。

### 2. Subject-bound 属性逐条复核

对 JSON 中 `transfer_risk_tags.subject_bound` 的每一项，输出一行处理决策：

| 属性 | 是否真冲突 | 决策 | 替换方案（如选"替换"） |
|---|---|---|---|
| 例：厚涂油画笔触 | 是（油画 × 现代数码题材） | 替换 | 改为 cel-shaded + 轻 grain，保留笔触可见性 |
| 例：原主体 pose | 是 | 丢弃 | — |
| 例：特定材质暗示 | 否（巧合担忧） | 保留 | — |

### 3. Conditional 属性的主体适配

对 `transfer_risk_tags.conditional` 中的属性，给具体适配方案。**关键是保留底层逻辑，替换表层表达**：

| 原属性 | 与新主体的张力 | 适配后 | 保留的底层逻辑 |
|---|---|---|---|
| 例：warm golden hour palette | 新主体是深夜赛博都市 | cool neon-lit palette | low-key rim-lighting + split-toning 结构 |

### 4. 风险点汇总

把上面前三步的所有非"保留"决策汇总为一张风险表，按风险等级降序：

| 风险点 | 等级 | 处理 | 备注 |
|---|---|---|---|
| ... | High / Med / Low | 保留 / 降权 / 丢弃 / 替换 | ... |

> **铁律**：每条 High 风险都必须有明确处理方案。Medium 风险可以打包成"可选启用"。Low 风险可省略。

### 5. 最终可迁移视觉 DNA

输出一组英文关键词（8-15 个），按权重顺序公式排列：

```
[主体] → [流派锚定] → [构图] → [光照] → [镜头] → [色彩/调色] → [材质/技法] → [氛围] → [后期] → [排除项]
```

这些关键词将作为 Step 4 出 prompt 的输入。

---

## T5 — 对话式 LLM 自然语言提示词（Step 4）

**用途**：把美学指纹（+ T4 适配结果）转写成可直接喂给 ChatGPT、Claude、Gemini 等对话式 LLM 的自然语言 prompt。**这是默认的、唯一的出 prompt 路径**。

**调用指令**：

> 把美学指纹和（如有的话）T4 适配结果转写成自然语言 prompt，直接可贴进 ChatGPT/Claude/Gemini 的对话框。**不要使用 MJ/SD 风格的 CLI 参数或加权语法**。

### 模板结构

```
[NEW_SUBJECT], in the style of [ERA_GENRE_ANCHOR], 
[COMPOSITION], [LIGHTING], [LENS], 
[PALETTE], [TEXTURE_TECHNIQUE], 
[MOOD], [POST_PROCESSING]. 
Avoid [NEGATIVE_CUES]. 
Aspect ratio [AR].
```

### 写作规则（对话式 LLM 通用）

- **自然语言长句**，不是逗号分隔的标签堆。允许必要时用逗号分隔修饰短语，但每个核心维度（构图 / 光照 / 镜头 / 调色 / 材质 / 氛围 / 后期）至少一个完整子句。
- **正向排除**：`avoid HDR, avoid digital sharpness, avoid neon oversaturation`。不写 `negative: ...` 字段。
- **画幅写在末尾**：`Aspect ratio 16:9` 或 `widescreen 2.39:1` 这种自然语言形式，不要用 `--ar 16:9`。
- **流派锚定靠前**：`in the style of [流派]` 这种短语放在主体之后、构图之前，是神韵保真度的关键。
- **不堆叠修饰副词**：`very`、`extremely`、`super` 这类词对画面影响极小，删掉换成可验证特征。
- **长度控制**：60-120 词为佳。低于 60 信息量不足，高于 120 关键 token 被稀释。

### 示例（原图：宫崎骏式山村 → 新主体：赛博朋克城市）

> A cyberpunk city street at dusk, in the style of Studio Ghibli hand-drawn animation meets 1980s Kodachrome, gentle wide composition with layered foreground/midground/background, soft diffused rim lighting with warm accent glow from neon signs, 35mm wide shot with mild atmospheric haze, palette dominated by muted teal and amber with dusty pink highlights, painterly cel-shaded textures with subtle grain, wistful and quietly optimistic mood, mild halation and lifted blacks. Avoid HDR, avoid digital sharpness, avoid neon oversaturation. Aspect ratio 16:9.

### 工具特定建议（按需采纳）

| LLM | 输入提示 | 多模态用法 |
|---|---|---|
| **ChatGPT**（gpt-image-1 / DALL·E 3）| 直接发 prompt + 让 ChatGPT 调用 image tool | 可上传参考图作为视觉锚点 |
| **Claude** | 在 Claude.ai 或 API 里，结合 MCP 调用 Replicate/Imagen/etc | 把参考图作为 attachment 一起发 |
| **Gemini** | 直接 prompt，Gemini 内置生图 | 可上传参考图 + 自然语言迭代 |

### 输出要求

输出 **3 个不同主体** 的样例 prompt，覆盖不同类型（人物 / 风景 / 物件 / 抽象场景），用以验证风格的跨主题稳定性。**三个样例必须共享同一套 L3/L4 描述**，只替换 L1 主体和适配后的 L2 属性。

每个样例之后，附一行"风格保真度自检"：列出该 prompt 中体现"神韵"的 3-5 个关键词。

---

## T6 — 自检盲测（可选扩展 · 默认不做）

> ⚠️ **注意**：agent 自评存在数据泄漏风险（前一秒写完 prompt，下一秒装作没看过原图，存在严重自我偏向）。**仅在用户明确要求严谨验证时启用**，不是主流程的一部分。

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
- 加哪个关键词 / 删哪个关键词
- 调整描述的位置（权重顺序公式）
- 把过弱的描述改写得更具体（例：`soft lighting` → `soft diffused rim lighting from a low key light`）
- 把过强的描述降级（例：直接删除某个修饰短语，而不是堆更多反向词）

**迭代规则**：若发现 ≥ 2 条偏差，修订后再跑一次 T6，最多 2 轮。超过 2 轮仍有明显偏差，回到 T3 检查美学指纹是否在源头就漏掉了关键维度。

---

## T7 — 中文风格配方报告（可选扩展 · 默认不做）

> ⚠️ **注意**：除非用户要"沉淀到个人风格库"或"给非技术伙伴讲解"，否则是冗余产出。**默认输出关键词列表已经够用，不要主动给中文报告**。

**何时调用**：用户**明确要求**深入理解风格、沉淀到个人风格库、或向非技术伙伴解释时。

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
- **使用建议**：
  - 在 ChatGPT / Claude / Gemini 等对话式 LLM 里，直接贴 T5 自然语言 prompt
  - 多轮对话迭代时，按 §3 风险速查表的 conditional 项做局部调整（例：再压一些饱和度、去掉 grain）
  - 如有参考图，把它作为 attachment 一起发，能显著提升风格保真度

### 6. 英文提示词示例
给一个假想新主体的完整 prompt 示例（使用 T5 自然语言模板），体现"风格不变，主体可变"。

### 7. 沉淀建议（可选）
如果这个风格值得长期复用，建议用户：
- **保存 JSON 指纹**：作为风格库的可机读条目
- **保存参考图**：作为视觉锚点，配合多模态 LLM 使用时直接附上
- **保存 3-5 个样例 prompt**：覆盖不同主体（人/物/景），形成自己的"风格触发器"
