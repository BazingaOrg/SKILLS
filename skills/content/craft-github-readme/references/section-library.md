# Section Library

把 README 的每一段当成可选积木。**5 段必备**，其余按项目类型挑。每段写法、何时用、反模式都列在下面。

---

## 必备段（任何项目都要有）

### 1. Header

3 秒内回答"这是什么、给谁用"。

**包含**：项目名 + 一句话 slogan + 必要徽章（可选 logo / hero 图）。

**徽章按场景选**，不是越多越好：

| 项目场景 | 推荐徽章 |
|---|---|
| 严肃产品 / 应用 | stars、version、license、CI 状态、downloads |
| 库 / SDK | 包管理器版本（npm / crates.io / PyPI）、license、CI、覆盖率 |
| 社区 / 文档项目 | stars、license、贡献者数 |
| 个人小工具 | 1-2 个就够，太多徽章像装修过度 |

**居中 vs 左对齐**：纯结构性 README（库 / SDK / 文档项目）左对齐更清爽；面向终端用户的产品（应用 / CLI / 设计系统）居中更有"产品感"。两种都对，看气质选，**不要为了对齐 tw93 强制居中**。

### 2. What it does

紧跟 Header，1 段话回答：解决什么问题、做了什么、不做什么。

**长度**：50-150 字。超过就该拆出 Why / Motivation 段独立写。

**反模式**：抄 slogan 重复一遍；用 We / Our team；纯抽象词没有动作。

### 3. Install / Quick Start

最快上手路径。多渠道分别独立成段（brew / npm / curl / 下载 / Marketplace）。命令必须可直接复制运行，**不要写"Step 1 / Step 2 / Step 3"**罗嗦流程，命令本身就是步骤。

### 4. Usage

至少一个能跑的示例。

| 项目类型 | Usage 内容形式 |
|---|---|
| CLI 工具 | 命令 + 终端输出（代码块模拟）|
| 库 / SDK | 最小可跑代码片段，5-15 行 |
| 应用 | 截图 + 操作说明 |
| 框架 | "Hello World" 级最小示例 |

### 5. License

1 行说清。特殊字体 / 第三方资产 / 双协议另起一段说明。

---

## 可选段（按项目类型选）

### Features

**何时用**：实用工具、应用、CLI 工具。3-6 条 bullet，每条加粗关键词 + 量化或具体动作。详见 [`voice-and-style.md`](voice-and-style.md) §2。

**不适合**：纯库 / 框架（用 "Quick Example" 代替更直接）；设计系统（用 Why 段更合适）。

### Why / Motivation

**何时用**：哲学型项目、设计系统、抽象工具、fork 项目。讲存在的根本理由 + 为什么不用现有方案。

**不适合**：实用工具（Features 更直接）；理由不充分时（硬编 Why 段读起来是"自我感动"）。

### Demo / Screenshots

**何时用**：应用、TUI、设计系统**强烈推荐**。形式：截图、GIF、终端代码块模拟、`<table>` 多列展示。

**形式选择**：

- 桌面应用 / Web 应用 → 截图 / GIF
- CLI / TUI → 终端代码块模拟（含 ASCII 进度条 / 分隔线 / 表情符号）
- 设计系统 / 内容工具 → `<table>` 多列展示成品

### API / Configuration

**何时用**：库 / SDK 必备，应用按需。最小可用 API 放 README，详细查 `docs/`。

### Background

**何时用**：个人项目、有真实缘起故事的项目、非显而易见的设计选择需要解释时。

第一人称讲缘起。**没真实故事就别写这段**。模板见 [`voice-and-style.md`](voice-and-style.md) §3。

### FAQ

3-5 个高频问，更多链 `docs/faq.md`。问题用粗体或 `?` 提问句式。

### Docs index

**何时用**：项目大、子文档多时。Bullet 列子文档链接，每条 1 行说明。

### Contributing

链 `CONTRIBUTING.md`，README 内只放一两行邀请。

### Acknowledgements

致谢具名前辈 / 灵感来源 / 重度依赖的开源项目。**只列真正受益的**，不要凑数。

### Support / Sponsorship

**只在用户明确想要赞助渠道时才加**。形式：

- Buy Me a Coffee / GitHub Sponsors / 爱发电 / Open Collective
- 不要复制任何模板里现成的链接（包括 tw93 的 `cats.tw93.fun`）

---

## 段落顺序参考（按项目类型）

不是强制顺序，是常见组合。挑一个最像你项目的，再按需调整：

| 类型 | 推荐顺序 |
|---|---|
| **CLI 工具** | Header → What it does → Features → Install → Usage → Demo → FAQ → License |
| **桌面应用** | Header → Screenshots → Features → Download → Usage / Shortcuts → FAQ → License |
| **库 / SDK** | Header → What it does → Install → Quick Example → API → Advanced Usage → License |
| **框架** | Header → What it does → Install → Quick Example → 概念地图 → Ecosystem → License |
| **设计系统 / 模板** | Header → Why → Examples → Usage → Customization → License |
| **Skill / Plugin 集合** | Header → What it does → 列表（总览表）→ Install → 各项用法 → License |
| **Awesome 列表** | Header → What it does → 分类目录 → Contributing → License |

---

## 反模式

- 不区分类型套同一套段落（CLI 套库的骨架、库套 marketing 页）
- 每段都写一点凑齐 N 段，导致整篇没有重点
- 复制范例项目的赞助 / 致谢段连人名一起留下
- 主 README 把 `docs/` 该写的全塞进来（违反 progressive disclosure）
- 没故事时硬写 Background
- emoji / 多语言切换条 / Support 当默认装饰套上去
