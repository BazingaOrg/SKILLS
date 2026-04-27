---
name: reverse-engineer-image-style
description: Use when the user provides one or more reference images and wants to extract the underlying visual style, aesthetic framework, reusable image-generation prompt, or cross-subject style-transfer recipe for tools like Midjourney, Stable Diffusion, or Flux.
---

# Reverse Engineer Image Style

把任意参考图（单图或多图）反向解构成可复用、可跨主题迁移的"美学配方"，服务于 Midjourney / Stable Diffusion / Flux 等生成工具。核心目标是提取"神韵"——不依赖具体主体的底层视觉逻辑。

## 0. 核心理念：风格四层解耦模型 (L1-L4)

风格迁移的本质，是识别并分离四个耦合层级。这是全流程的理论基础，所有后续操作都围绕它展开。

| 层级 | 内容 | 在跨主题迁移中的处理策略 |
|---|---|---|
| **L1 主体内容** | 人物、物体、主体场景、具体动作 | **必须替换** —— 由用户指定新主体 |
| **L2 表层耦合属性** | 具体配色、服装道具、局部环境、特定时辰 | **按需保留** —— 视新主体兼容性而定 |
| **L3 美学框架** | 构图、光影逻辑、镜头语言、材质、笔触、技法、后期 | **核心保留** —— 真正的"神韵"所在 |
| **L4 元艺术哲学** | 情绪基调、叙事视角、文化语境、时代/流派锚定 | **强制保留** —— 保证气质一致性 |

**提取铁律**：尽可能把 L3/L4 说清楚；把 L2 标注为"可选条件迁移"；彻底剔除 L1。

### 术语约定（全文遵循）

| 术语 | 形态 | 用途 |
|---|---|---|
| **美学指纹 (Aesthetic Fingerprint)** | 结构化 JSON | 机器可读的底层参数，供二次处理或沉淀 |
| **视觉 DNA (Visual DNA)** | 英文关键词列表 | 直接喂给 AI 绘图工具 |
| **风格配方 (Style Recipe)** | 中文报告 | 给人看的解释 + 应用指南 |

---

## 1. 工作流：原子操作 + 场景路由

每次任务按"用户意图"选择一条场景路径，组合调用下面的原子操作。每个原子对应 `references/prompts.md` 中的一个模板。

### 1.1 原子操作清单

| ID | 操作 | 对应模板 | 何时使用 |
|---|---|---|---|
| **A1** | L1-L4 分层观察与标注 | T1 | 所有场景的第一步，强制 |
| **A2** | 多图共性提取（求交集、去异集） | T2 | 仅多图输入时启用 |
| **A3** | 美学指纹 JSON 输出 | T3 | 需要结构化参数或供二次处理 |
| **A4** | 迁移适配度检查 | T4 | 跨主题迁移前的风险体检 |
| **A5** | 目标工具提示词生成（T5a-d） | T5a / T5b / T5c / T5d | 根据目标工具选对应模板 |
| **A6** | 自检盲测 | T6 | 迁移前的最后一道验证 |
| **A7** | 中文风格配方报告 | T7 | 用户需要深入理解或沉淀到个人风格库 |

### 1.2 场景路由

| 场景 | 用户意图 | 原子组合 |
|---|---|---|
| **S1 单图深度分析** | 理解一张图的风格本质 | A1 → A3 → A7 |
| **S2 跨主题风格迁移** | 把 A 的风格套到 B 主体 | A1 → A3 → **A4** → A5 → **A6** |
| **S3 多图共性提炼** | 作品集 → 提炼共性风格 | A1 → **A2** → A3 → A4 → A5 |
| **S4 Midjourney SREF** | 利用 `--sref` 工作流 | A1 → A3(精简) → A5a |
| **S5 SD/Flux 精调** | 本地/云端模型精控 | A1 → A3 → A5b 或 A5c |

### 1.3 执行前检查

在启动任何场景前，先过 `references/checklists.md` 中的「提取前 checklist」。S2/S3 场景在 A4 之后、A5 之前还需过「迁移前 checklist」。A5/A7 产出后过「输出前 checklist」。

---

## 2. 提示词编写规范

### 2.1 权重顺序公式（核心经验）

生成提示词时严格按以下顺序排列，越靠前权重越高：

```
[主体] → [艺术流派/时代锚定] → [构图] → [光照] → [镜头] →
[色彩/调色] → [材质/技法] → [氛围/情绪] → [后期/胶片特征] →
[排除项/negative cues] → [工具参数 --ar / --sref / --stylize 等]
```

这个顺序来自实战经验：生成模型对前 30% 的 token 最敏感，流派锚定放在主体之后能最大化"神韵"保真度。

### 2.2 四项基本原则

- **深度抽象**：提 L3/L4，避 L1/L2。例如不要 "golden hour"（L2 具体时辰），要 "natural rim light with warm temperature bias"（L3 光影逻辑）。
- **禁用空词**：不用 `beautiful` / `amazing` / `masterpiece` / `best quality`。用 `2.39:1 anamorphic, natural rim light, split-toning` 这种可验证描述。
- **排除法**：显式列出 negative cues（如 `no HDR, no digital sharpness, no oversaturation`）。
- **工具对齐**：
  - **Midjourney** → `--sref <url> --sw 100`、`--stylize`、`--chaos`
  - **Stable Diffusion** → `(keyword:1.3)` 权重语法 + negative prompt
  - **Flux** → 自然语言长句，避免短词堆砌

---

## 3. 迁移风险速查表

每个 L3/L4 属性在跨主题迁移时的风险等级。A4 步骤中必须对每个关键词标注下述等级。

| 风险等级 | 典型属性 | 迁移策略 |
|---|---|---|
| **Universal（通用）** | 三分法构图、rim light、2.39:1 画幅、散景、广角透视、高光羽化 | 直接迁移，无需调整 |
| **Conditional（条件）** | 暖金调色、胶片颗粒、浅景深、specific era palette | 按新主体适配；若冲突则替换为等效属性 |
| **Subject-bound（强绑定）** | 厚涂油画笔触 × 现代数码题材、特定人物 pose、原主体专属的材质暗示 | 降权或丢弃；强套会破坏"神韵" |

---

## 4. 多图模式说明

当用户提供 2 张及以上参考图时：

- **禁止**对每张图单独分析后简单拼接（会放大噪声、稀释共性）。
- 必须执行 A2，输出三类特征：
  - **共性特征**（所有图都具备）→ 风格库的"骨架"，必进最终 prompt
  - **高频特征**（≥ 60% 图具备）→ 作为次级特征，可选进 prompt
  - **异常特征**（仅单张具备）→ 判定是偶然（排除）还是"变体标签"（保留为可选扰动）

多图模式下，A3 的美学指纹 JSON 只反映"共性 + 高频"，不含异常特征。

---

## 5. 参考资源

- `references/prompts.md`：所有原子操作的提示词模板（T1-T7）
- `references/checklists.md`：提取前 / 迁移前 / 输出前三张检查清单
