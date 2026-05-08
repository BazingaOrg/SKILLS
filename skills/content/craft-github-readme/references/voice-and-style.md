# Voice & Style Guide

写作的核心：信息密度 / 可扫描性 / 量化代替修辞。

---

## 1. Slogan 公式

**强动词 + 名词目标 + 可量化承诺**。一句话讲完。**不超过 12 个英文词 / 20 个汉字**。

| 类型 | 公式 | 例 |
|---|---|---|
| 转换型工具 | `Turn` X `into` Y | Turn any webpage into a desktop app with one command |
| 优化型工具 | `Deep clean` / `Optimize` X | Deep clean and optimize your Mac |
| 角色型工具 | A [量化形容词] X built for Y | A fast, out-of-the-box terminal built for AI coding |
| 价值主张型 | 直白讲意图 | Good content deserves good paper |
| 库 / SDK | X `for` Y | A type-safe HTTP client for TypeScript |

**避坑**：

- ❌ 修饰词 ≥ 3 个："A modern, beautiful, easy-to-use library for…"
- ❌ "next-generation / revolutionary / ultimate / all-in-one"
- ❌ "The most X" 最高级未经证明就是空话

---

## 2. Features bullet 量化清单

**每条 bullet 必须能被一个数字、一个对比、或一个具体动作验证**。

### 空形容词替换表

| 空形容词 | 替换为可验证描述 |
|---|---|
| easy to use | One-command install, no config file |
| lightweight | 5MB binary, 20× smaller than Electron |
| fast | Cold start ≤ 100ms / 40% faster than X |
| beautiful | Warm parchment canvas, ink-blue accent only |
| powerful | Removes caches across 12 locations / Handles 10K req/s |
| flexible | Lua config, 100% WezTerm API compatible |
| modern | Built with Rust / Native Apple Silicon |
| comprehensive | Covers 8 document types with 14 chart variants |
| robust | 95% test coverage, fuzz-tested for 10K hours |
| seamless | Auto-detects environment, falls back to X on failure |

### Bullet 写法模板

```markdown
- <emoji?> **<关键词>**: <一句话量化说明>
```

emoji **可选**：整篇要么都用要么都不用，别一半用一半不用。库 / SDK 类项目不建议用 emoji。

---

## 3. Background 段第一人称模板

骨架：**当时的处境 → 试过什么 → 不满意的点 → 自己做的过程 → 现在的形态**。

```markdown
## Background

I <在做什么>, and <遇到什么具体问题>. Tools like <竞品 A> and <竞品 B>
were <为什么不够>. <我试了 X 方案>, but <X 的局限>. So I started <做什么>,
<具体过程，可以带数据>, until it became <现在的形态>.
```

**正例（可量化的细节是关键）**：

> Built from patterns across real projects, refined through actual use. Every gotcha traces to a real failure: a wrong code path that took four rounds to find, a release posted before artifacts were uploaded, a server restarted eight times without reading the error. **30 days, 300+ sessions, 7 projects, 500 hours**.

**反例**：

> ❌ "We built X to solve the common problem of Y in modern development workflows."

**没真实故事就不写这段**。Background 不是必备段，硬编出来读起来是自我感动。

---

## 4. 终端代码块模拟规范

CLI / TUI 项目把"它真的工作"做实的最高性价比方式。

**要素**：
- 命令行提示符 `$` 开头
- 进度反馈：`✓` / `▶` / `→`
- ASCII 进度条 `█████░░░░░` 或 Unicode 块字符
- 分隔线 `=====` / `─────`
- 结果汇总单独一行，含具体数字

**Unicode 字符备选**：
- 进度条：`█░` / `▮▯` / `▰▱`
- 状态符号：`✓ ✗ ▶ ⚙ ▦ ⇅ ●`
- 分隔线：`─ ═ ━`
- TUI 元素：`⌘ Cmd ⌫ Q ↑↓←→`

**坑**：模拟数据要真实合理，单位别离谱（不要写 "Cleaned 9999 PB"）；命令格式要符合该 CLI 的真实风格。

---

## 5. 信息架构原则（三梯度）

按读者获取信息的深度组织：

| 时长 | 读者要回答的问题 | 在 README 哪里 |
|---|---|---|
| **3 秒** | 这是什么？给谁用？要不要继续看？ | Header + Slogan |
| **30 秒** | 它能干什么？怎么装？大概值不值？ | What it does + Features + Install |
| **5 分钟** | 怎么真的用起来？有什么坑？ | Usage + Demo + FAQ |

**长内容必须外链**：超过 5 分钟阅读量的内容（详细 API、配置全集、架构说明）拆到 `docs/`。主 README 不要尝试 self-contained。

---

## 6. 反模式速查

| 反例 | 修正 |
|---|---|
| 形容词堆砌 "amazing, powerful, beautiful" | 删掉所有形容词，加一个数字 |
| 用 We（"We built", "We provide"） | 第三人称 "X exposes Y" 或第一人称 "I built X" |
| 抽象动词 "leverages, utilizes, empowers" | 换成 "uses, runs, sends" |
| 空 emoji 装饰 "✨ Awesome features ✨" | 删掉 emoji，或保留并加具体能力 |
| 整篇没有代码块或截图 | 至少加一个最小可跑示例 |
| 复制别人 README 的赞助 / 致谢链接 | 替换成自己的渠道，没有就删 |
| 段落多就是好 | 重点突出 5-7 段，比凑齐 12 段每段两句空话好 |
| 中英混杂随意切换 | 选一种主语言，必要时用 README_CN.md 分文件 |
| 顶部一定要 `<div align="center">` | 看气质选；库 / SDK 左对齐更专业 |
