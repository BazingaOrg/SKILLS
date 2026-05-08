# Examples: 优秀 README 的结构特征案例

收录一组结构清晰、信息密度高的 README，按**项目类型**分组。

> **借鉴它们的结构思路、不要复制它们的赞助 / 致谢 / 个人 IP / 系列归属**。这些案例帮你看懂"同类型项目长什么样"，不是模板。

---

## 类型 → 推荐参考 速查

| 你的项目是… | 推荐参考 |
|---|---|
| CLI / 系统工具 | tw93/Mole（终端代码块模拟做到极致）|
| 跨平台 CLI / 桌面打包工具 | tw93/Pake（多语言切换 + 多平台下载表）|
| 终端 / 桌面应用（fork 优化版）| tw93/Kaku（Performance 量化对比表）|
| 设计系统 / 内容生成模板 | tw93/Kami（Why 段三段式 + 范例展示表）|
| Skill / Plugin 集合 | tw93/Waza（总览表 "When + What it does" 双列）|

> 当前案例都来自 tw93 一组优秀样本。后续可继续收录其他类型的范例（例如 charmbracelet 系列的 TUI、sindresorhus 系列的小工具、Anthropic / OpenAI 官方仓库的库 README、Next.js / Astro 的框架 README）。

---

## tw93/Kami — 设计系统 / 内容模板

**结构亮点**：
- Why 段三段式：解名字寓意 → 本质问题 → 系列归属
- "See it" 段用 `<table>` 横向 4 列展示成品（每列：图 + 类型标签 + 语言标签 + 描述）
- Design 段单列规则表，把约束系统讲清楚

**可借鉴**：成品展示表的写法、约束规则的表格化呈现、Why 段如何把抽象项目讲实在。

**不要照抄**：trilogy 系列归属（你项目可能没系列）、cats 赞助链接、阿里云 OSS 图床 URL、特定字体（TsangerJinKai02）的授权说明。

---

## tw93/Waza — Skill 集合

**结构亮点**：
- `## Skills` 段用 3 列大表（Skill / When / What it does），每行链 `skills/<name>/SKILL.md`
- Chaining 段讲 skill 之间的组合工作流（用箭头表示链路）
- Project Context 段把"通用骨架 vs 项目定制"边界讲清

**可借鉴**：Skill / Plugin 集合类项目的总览表入口；多渠道安装（Claude Code / Codex / Marketplace / Desktop）分段写法。

**不要照抄**：cats、sponsors、family（Kaku/Waza/Kami trilogy）相关文案。

---

## tw93/Kaku — 终端 / 桌面应用 fork

**结构亮点**：Performance 段是 fork 类项目的范本 —— 4 列对比表（Metric / Upstream / Kaku / Methodology），把"我的版本 vs 上游"用具体指标量化。

**可借鉴**：Fork / 优化版项目都该有这个段。模板：

```markdown
| Metric | Upstream | YourFork | Methodology |
| :--- | :--- | :--- | :--- |
| **Executable Size** | 67 MB | 40 MB | Symbol stripping & feature pruning |
| **Launch Latency** | Standard | Instant | Just-in-time initialization |
```

**不要照抄**：family 系列归属、特定厂商图床。

---

## tw93/Pake — 跨平台 CLI / 桌面打包工具

**结构亮点**：
- 顶部多语言切换条（`<h4 align="right">`）—— 仅当确实有 README_CN.md 时才用
- "Popular Packages" 表：每行一个产品 + 三平台下载链接 + 截图，密度极高
- Getting Started 按"用户角色"分（Beginners / Developers / Advanced / Troubleshooting）

**可借鉴**：多用户群项目（小白 / 开发者 / 高级用户）的入口分流写法；多平台多 SKU 的展示表。

**不要照抄**：具体产品列表、tw93/static 仓库的截图 URL。

---

## tw93/Mole — macOS CLI / TUI 工具

**结构亮点**：终端代码块模拟做到极致，每个子命令独立一段，全是高保真模拟输出。

**可借鉴 Unicode 字符选择**：
- 进度条：`█░`
- 状态符号：`✓ ▶ ⚙ ▦ ⇅`
- 分隔线：`─ ═`
- TUI 元素：`⌘ Cmd ⌫ Q`

CLI / TUI 项目把这一点学过来，README 的"产品质感"立刻提升一个档次。

**不要照抄**：cats 赞助、特定厂商图床、quick launchers 的 setup-quick-launchers.sh 脚本（那是 tw93 自己的实现）。

---

## 通用借鉴清单（5 个项目都做对的事）

无论你的项目类型，这几条普适：

1. **Slogan 是一句话强动词** — 不是段落
2. **量化代替修辞** — 每个卖点带数字
3. **多渠道安装独立成段** — 不要混成一段
4. **终端代码块或截图至少有一个** — 禁止纯文字 README
5. **Background（如有）是第一人称** — 不是 marketing 口吻
6. **License 段简短清晰** — 特殊资产单独说明

---

## 选范例的快速决策

1. 找一个**项目类型最像**的范例
2. 看它的**段落顺序**和**段落取舍**
3. 在 [`section-library.md`](section-library.md) 里查每段写法
4. 写完用 [`voice-and-style.md`](voice-and-style.md) 做风格质检

**不要**：
- 把范例项目的链接 / 人名 / 赞助 / 系列归属直接复制
- 不区分类型硬套同一个范例骨架
- 把范例当模板逐字仿写（要"学结构"不是"抄文案"）
