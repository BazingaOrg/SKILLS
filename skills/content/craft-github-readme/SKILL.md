---
name: craft-github-readme
description: Use when creating or rewriting a GitHub project's top-level README.md to produce a clean, scannable structure that fits the project type (CLI / library / app / design-system / skill-set / awesome list) and replaces marketing rhetoric with quantified specifics.
version: 0.1.0
author: Bazinga
---

# Craft GitHub README

把一份能让读者**在 3 秒、30 秒、5 分钟三个梯度**都获取到对应深度信息的 README 写出来。核心是**信息架构**：固定的扫描入口 + 按项目类型挑选合适段落 + 量化代替修辞。

不是套一份固定模板，是按项目类型组合段落、按统一标准把每段写实在。

## When to use

- 用户给一个 GitHub 项目，要写或重写它的根目录 `README.md`
- 现有 README 是脚手架默认产物，或堆满空形容词需要重做
- 用户希望 README 看起来"专业且克制"，不是 marketing landing page

## When NOT to use

- 只改某一段（直接编辑原文，不需要套整套流程）
- `docs/` 子文档、API reference、CONTRIBUTING、AGENTS.md、CHANGELOG
- 不是给 GitHub 公开仓库看的（内部 wiki / Notion 文档）
- 用户要写营销长文 / 产品发布稿

## Workflow

### Step 1 — Intake（搞清楚再动笔）

下列字段每项必须明确，缺就先问用户，不要脑补：

| 字段 | 用途 |
|---|---|
| **项目类型** | CLI / 库 / 桌面应用 / 框架 / 设计系统 / Skill 集合 / Awesome 列表 / 其他。决定段落组合 |
| **主要受众** | 终端用户 / 应用开发者 / 库使用者 / 贡献者。一份 README 别同时讨好所有人 |
| **一句话定位** | 强动词 + 名词目标 + 可量化承诺，写 Hero slogan |
| **主语言 / 平台** | Quick Start 命令 + 适用徽章 |
| **安装渠道** | brew / npm / curl / GitHub Release / 商店 / Marketplace，每个独立成段 |
| **关键卖点 3-5 条** | 必须可量化或具体动作，写 Features / Why 段 |
| **是否有 docs/ 子目录** | 决定主 README 多薄、要不要 Docs index 段 |
| **是否多语言** | 决定要不要顶部多语言切换条 |
| **作者句柄 / 赞助渠道**（可选） | 徽章 + Support 段（**只有用户主动要才加**）|

### Step 2 — Pick the sections

从段落工具箱挑组合。**5 段必备**任何项目都要；**可选段**按项目类型勾选。详见 [`references/section-library.md`](references/section-library.md)。

| 必备段 | 作用 |
|---|---|
| Header | 项目名 + slogan + 必要徽章 |
| What it does | 一句到一段话讲清做什么、给谁用、不做什么 |
| Install / Quick Start | 最快上手路径，代码可直接复制运行 |
| Usage | 至少一个能跑的示例（命令 / 代码片段 / 截图）|
| License | 一句话 + 特殊资产说明（如有）|

可选段（按需开启）：Features、Why / Motivation、Demo / Screenshots、API / Configuration、Background、FAQ、Docs index、Contributing、Acknowledgements、Support。

`section-library.md` 给出每段的：何时用 / 写法 / 反模式 / 范例片段。**段落顺序参考**也在那份文件里按项目类型列了 7 种推荐排列。

### Step 3 — Draft section by section

按 Step 2 选定的顺序写。每段对照 [`references/voice-and-style.md`](references/voice-and-style.md) 的写作规范：slogan 公式、Features 量化清单、第一人称 Background 模板、空形容词替换表、终端代码块模拟规范。

写之前查 [`references/examples.md`](references/examples.md)，找跟你项目类型最像的范例对照灵感。**借鉴结构思路，不要照抄链接 / 人名 / 赞助 / 系列归属**。

### Step 4 — QA pass（交付前自检）

- [ ] Header 让访客**3 秒内**知道这是什么、给谁用
- [ ] 至少一段里有**代码块、截图或表格**，禁止纯文字 README
- [ ] Install 段命令可直接复制运行；多渠道分别独立成段
- [ ] Features / 卖点 bullet **每条带数字或具体动作**，没有 awesome / amazing / beautiful / powerful / easy-to-use 等空词
- [ ] Background 段（如有）是第一人称、讲个人故事，不是 marketing 口吻；没真实故事就别写这段
- [ ] 长内容已外链 `docs/`，主 README 保持轻
- [ ] License 段一句话讲清；特殊资产 / 双协议单独说明
- [ ] **没有照抄第三方模板里的链接 / 人名 / 赞助 / 系列归属**
- [ ] emoji / 多语言切换条 / Support 段都是用户主动选择，没默认套上

任一项 fail 回到 Step 3 改完再过 Step 4。

## Hard constraints

- **段落取舍服从项目类型**：CLI 工具该有 Features 和 Demo，库该有 Quick Example 和 API，不要互相错位
- **量化代替修辞**：每个卖点必须能被验证（数字 / 对比 / 具体动作），不能只能"被相信"
- **借鉴结构、不抄人名链接**：参考范例（包括 tw93 系列）只用来对照风格；赞助 / cats / sponsors / trilogy 这类个人 IP 链接绝对不要照搬
- **可读性高于完整性**：宁可少 2 段每段写实在，也别每段都写一点空话凑齐 N 段
- **3 秒原则**：Header 不能让人读完还不知道项目是什么

## Anti-patterns

- 不区分项目类型套同一份模板（CLI 套库的骨架、库套 marketing 页）
- Features 段全是 "easy / powerful / flexible / modern" 占位词
- 整篇没有任何代码块、截图、表格
- Background 用 "We built X to solve Y"（应是 "I built X because…" 或干脆删掉）
- 复制别人 README 的赞助 / 致谢段连人名一起留下
- emoji / 多语言切换条 / 居中 hero 当默认装饰套上去（按需选用）
- 主 README 把 `docs/` 该写的全塞进来，violate progressive disclosure

## References

- [`references/section-library.md`](references/section-library.md)：5 段必备 + 全部可选段清单，每段含何时用 / 写法 / 反模式 + 7 种项目类型的推荐顺序
- [`references/voice-and-style.md`](references/voice-and-style.md)：slogan 公式 / Features 量化清单 / 第一人称 Background 模板 / 空形容词替换表 / 终端代码块规范 / 信息架构三梯度
- [`references/examples.md`](references/examples.md)：按项目类型分类的优秀 README 案例（含 tw93 系列），用于对照灵感
