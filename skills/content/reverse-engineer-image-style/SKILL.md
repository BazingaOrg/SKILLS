---
name: reverse-engineer-image-style
description: Use when the user provides reference images and wants a reusable prompt that captures the visual style (not the subject) for Midjourney, Stable Diffusion, or Flux—including swapping in a new subject.
version: 1.0.0
author: Bazinga
---

# Reverse Engineer Image Style

把参考图的"神韵"反推成**可复用提示词**。目标是换主体后风格依然成立——**不是复制原图**。

## Quick Path（90% 场景，照这 4 步走）

| 步骤 | 做什么 | 模板 |
|---|---|---|
| 1. 观察 | 把图拆成 L1 主体 / L2 表层 / L3 美学框架 / L4 元艺术哲学 | T1 |
| 2. 指纹 | 输出结构化的美学指纹 JSON | T3 |
| 3. 适配 | （仅跨主体时）标 universal/conditional/subject-bound，列风险与方案 | T4 |
| 4. 出 prompt | 按目标工具（MJ / SD / Flux / 通用）生成提示词 | T5a-d |

> 多图共性提炼、自检盲测、中文风格配方报告 —— 三项是**可选扩展**，默认不做，见 §6。

模板正文都在 [`references/prompts.md`](references/prompts.md)；执行前/中/后的硬性检查在 [`references/checklists.md`](references/checklists.md)。

---

## 1. 核心理念：L1-L4 解耦模型

风格迁移的本质是识别并分离四个耦合层级。这是全流程的理论基础：

| 层级 | 内容 | 在跨主题迁移中的处理 |
|---|---|---|
| **L1 主体内容** | 人物、物体、动作、场景 | **必须替换**，由用户指定新主体 |
| **L2 表层耦合** | 具体配色、服装道具、特定时辰 | **按需保留**，看新主体兼容性 |
| **L3 美学框架** | 构图、光影、镜头、材质、技法、后期 | **核心保留**——真正的"神韵" |
| **L4 元艺术哲学** | 情绪、叙事视角、时代/流派锚定 | **强制保留**，气质一致性靠它 |

**铁律**：尽可能把 L3/L4 说清楚；L2 标"可选条件迁移"；彻底剔除 L1。

---

## 2. 主线步骤详解

### Step 1 — 分层观察（→ T1）

按 L1-L4 输出结构化观察笔记。L3/L4 详写，L1/L2 简短即可。**所有场景都从这里开始。**

### Step 2 — 输出美学指纹（→ T3）

把观察结果固化成 JSON。该 JSON 是后续所有产出的"单一真相源"，三种输出形态都从它派生：

| 输出形态 | 用途 | 何时给用户 |
|---|---|---|
| **JSON 指纹** | 机器可读，便于二次处理或入库 | 用户要"沉淀风格"或要喂给脚本 |
| **关键词列表** | 直接喂给 AI 绘图工具 | 用户要立刻出图（默认输出）|
| **中文风格报告** | 给人阅读的解释 + 应用指南 | 用户明确要求时（默认不出）|

> 三种形态是**同一份信息的不同载体**，不要包装成三个独立概念让用户记三个词。

### Step 3 — 跨主体适配（→ T4，仅跨主体时）

**不用 1-10 兼容性评分**（虚指标，没有阈值意义）。直接列风险点 + 处理方案：

1. 对 JSON 中 `transfer_risk_tags.subject_bound` 每一项判定：**保留 / 降权 / 丢弃 / 替换**
2. 对 `conditional` 属性给具体适配方案
   - 例：原图"暖金调色" + 新主体"深夜赛博" → 替换为冷调霓虹，但**保留底层逻辑**（"低调 + 高光偏暖暗部偏冷"的 split-toning 结构不变）
3. **L4 兼容性体检**：如出现"挽歌气质 × 欢快主题"等本质冲突 → **显式向用户报警**，不要硬上
   - 此时建议用户：换参考图 / 接受气质转向 / 选择性放弃部分 L4 属性
4. 输出最终可迁移视觉 DNA：8-15 个英文关键词，按权重顺序公式排列

### Step 4 — 按工具出 prompt（→ T5a-d）

**工具差异速查表**（共性见 §3，差异在各自模板内）：

| 工具 | 提示词风格 | 关键参数/语法 | 模板 |
|---|---|---|---|
| **Midjourney** (v6+) | 标签 + CLI 参数 | `--ar` / `--sref` / `--sw` / `--stylize` / `--style raw` | T5a |
| **Stable Diffusion** (含 SDXL/Pony 等) | 加权标签 + Negative | `(keyword:1.3)`，权重词 ≤ 8 | T5b |
| **Flux** (dev/pro/schnell) | 自然语言长句（60-120 词）| `guidance_scale`，禁用括号堆 | T5c |
| **通用 / 未指定** | 自然语言模板，跨工具兼容 | — | T5d |

**默认行为**：用户没指定工具时，先走 T5d 给通用版，再询问是否需要工具特化。

---

## 3. 提示词编写规范

### 3.1 权重顺序公式（核心经验）

```
[主体] → [艺术流派/时代锚定] → [构图] → [光照] → [镜头] →
[色彩/调色] → [材质/技法] → [氛围/情绪] → [后期/胶片特征] →
[排除项 negative] → [工具参数 --ar / --sref 等]
```

> 经验法则：生成模型对前 30% 的 token 最敏感。**流派锚定放在主体之后**能最大化"神韵"保真度。

### 3.2 四项基本原则

- **深度抽象**：提 L3/L4，避 L1/L2。例：不写 `golden hour`（L2 时辰），写 `natural rim light with warm temperature bias`（L3 光影逻辑）。
- **禁用空词**：不用 `beautiful` / `amazing` / `masterpiece` / `best quality`。用 `2.39:1 anamorphic, natural rim light, split-toning` 这种**可验证**描述。
- **排除法**：显式列 negative cues，例如 `no HDR, no digital sharpness, no oversaturation`。
- **工具对齐**：见 §2 Step 4 速查表。

---

## 4. 迁移风险速查表

每个 L3/L4 属性在跨主题迁移时的风险等级。Step 3 中对每个关键词标注下列等级：

| 风险等级 | 典型属性 | 迁移策略 |
|---|---|---|
| **Universal（通用）** | 三分法构图、rim light、2.39:1 画幅、散景、广角透视、高光羽化 | 直接迁移，无需调整 |
| **Conditional（条件）** | 暖金调色、胶片颗粒、浅景深、特定时代色板 | 按新主体适配；冲突时换等效属性 |
| **Subject-bound（强绑定）** | 厚涂油画 × 现代数码、特定 pose、原主体专属材质 | 降权或丢弃；强套会破坏神韵 |

---

## 5. Mini End-to-End 示例

让 agent 看一个完整跑完的例子，比抽象描述更可靠。

**输入**：宫崎骏式日本山村黄昏 + 新主体"赛博朋克城市夜景"。

**Step 1（L1-L4 摘要）**

- L1：山村、黄昏、农人归家
- L2：暖金 `#E8B86A` 主调，浅蓝 `#7BA8C9` 点缀，秋季傍晚
- L3：广角低水平线、柔光漫射、cel-shaded 笔触、轻 grain、浅景深
- L4：田园挽歌、第三人称旁观、Studio Ghibli 流派、温情中带怅惘

**Step 3 适配**

| 风险标签 | 原属性 | 适配后 |
|---|---|---|
| `subject_bound` | 暖金 + 田园 | 替换为霓虹冷光 + 都市，但保留"低调 + 暗部偏冷高光偏暖"的底层 split-toning |
| `conditional` | 秋季傍晚 | 替换为夜雨初停 |
| `universal` | 广角低水平线 / cel-shaded / grain | 直接保留 |

**L4 兼容性体检**：田园挽歌 ↔ 赛博朋克 → 都属"静谧 + 人文"基调，**兼容**。

**Step 4 输出（T5d 通用模板）**

> A cyberpunk city street at dusk, **in the style of Studio Ghibli hand-drawn animation meets 1980s Kodachrome**, gentle wide composition with layered foreground/midground/background, soft diffused rim lighting with warm accent glow from neon signs, 35mm wide shot with mild atmospheric haze, palette dominated by muted teal and amber with dusty pink highlights, painterly cel-shaded textures with subtle grain, wistful and quietly optimistic mood, mild halation and lifted blacks. Avoid HDR, avoid digital sharpness, avoid neon oversaturation. Aspect ratio 16:9.

**关键观察**：主体（cyberpunk city）变了，但 Studio Ghibli + 1980s Kodachrome + cel-shaded + halation 这套美学 DNA 整体迁移，气质保持一致。

---

## 6. 可选扩展（按需启用，默认不做）

| 扩展 | 何时启用 | 入口 |
|---|---|---|
| **多图共性提炼** | 用户提供 ≥ 2 张图，需要从中抽共性骨架 | [`references/multi-image.md`](references/multi-image.md) |
| **自检盲测** | 用户要求严谨验证，或对结果有疑虑 | `references/prompts.md` 中的 T6 |
| **中文风格配方报告** | 用户要沉淀风格库 / 给非技术伙伴讲解 | `references/prompts.md` 中的 T7 |

> 这三项默认**不**进主流程，原因：
> - 多图：90% 单图场景下放进来是噪声
> - 盲测：agent 自评存在数据泄漏，自我偏向严重，价值有限
> - 中文报告：除非要长期复用，否则是冗余产出

---

## 7. 参考资源

- [`references/prompts.md`](references/prompts.md)：T1-T7 全套模板（T6/T7 标记为可选扩展）
- [`references/checklists.md`](references/checklists.md)：提取前 / 迁移前 / 输出前三张硬性检查清单
- [`references/multi-image.md`](references/multi-image.md)：多图共性提炼专题
