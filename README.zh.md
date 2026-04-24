# AI Frontend Design Kit

**[English](./README.md) · 中文**

> **约束先于生成。** 一个给 Claude Code 和 Codex 用的 10-skill 插件——把 AI 前端设计从"生成四个方案让你选"升级为有品味级设计契约的 7 阶段严格流程。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-blue)](https://docs.claude.com/en/docs/claude-code)
[![Codex](https://img.shields.io/badge/Codex-plugin-green)](https://github.com/openai/codex)

---

## 这个工具解决什么

市面上大多数"AI 前端"工具的逻辑是：生成 2-4 个页面方案，让你挑一个。这对扔掉就扔掉的 demo 够用，但**当你真在乎结果时就不够用了**——因为 AI 没法替你形成审美判断。

本 kit 把这个回路反过来：

1. **先形成约束**（主语言、六维品味映射、双轮深度采访、Moodboard 锚定、MOTION-SPEC）
2. **在约束内生成**（视觉参考、代码、迭代）
3. **对照契约做 review**，改写契约，再循环

最终产出是一份常驻的 `design.md`（kun 15 段格式），后续每一个 AI agent 在动一行 JSX 之前都要先读它。

---

## 安装

### Claude Code

```bash
# 把本仓库注册为 marketplace
claude plugin marketplace add https://github.com/kkunkunya/ai-frontend-design-kit

# 安装插件
claude plugin install ai-frontend-design-kit@ai-frontend-design-kit
```

### Codex

```bash
# 克隆到任意位置
git clone https://github.com/kkunkunya/ai-frontend-design-kit.git ~/codex-plugins/ai-frontend-design-kit

# 在 ~/.codex/config.toml 里注册
[[plugins]]
source = "~/codex-plugins/ai-frontend-design-kit"
enabled = true
```

### 验证

安装后，对 Claude Code 或 Codex 说：

> 启动一个新的前端项目，走 7 阶段流程。

Agent 应该会路由到 `frontend-interview-dualround`（阶段 1）。如果是这样，就装好了。

---

## 10 个 skill

| Skill | 阶段定位 | 一句话职责 |
|---|---|---|
| `frontend-iteration-planner` | 场景 C 入口 | 二开 / 增量路由器——读老资产 + 三档 tier 裁剪（T1 新页面 / T2 新 section / T3 微改） |
| `frontend-interview-dualround` | 横切（阶段 1 + 3） | 产品设计师视角五类提问，前轮建边界 + 后轮拔细节 |
| `frontend-design-research` | 阶段 2 | 六维机械骨架试吃 4-5 参考站 → `design.md` 初稿 |
| `frontend-visual-reference` | 阶段 4 | 视觉锚定三件套（Moodboard 探索 + 页面示意图 + 动效帧序列），双路径出图 |
| `frontend-motion-prompt-writer` | 阶段 4C + 6 | 把 MOTION-SPEC + 帧序列翻译为 LoadingScreen 风格代码级 prompt |
| `frontend-design-writer` | 贯穿 1-7 | **主写手**。Kun 15 段四轮写作；维护 `design.md` + `MOTION-SPEC.md` + `DESIGN-LANGUAGE.md` |
| `frontend-design-review` | 阶段 7 | 代码后三段式追问 + Tweaks 参数外显 + 反写 `design.md` |
| `frontend-anti-slop-gate` | 阶段 6 横切 | 十条作者硬判断红线扫描（Inter 字 / Lila 色 / 3 列栅格 / `h-screen` / 占位数据等） |
| `system-design` | 正交 | 系统架构决策：Design Doc + ADR + 结构影响分析 |
| `frontend-i18n-essentials` | 正交 | 国际化三层正交模型（运行时 / 翻译内容 / 视觉）+ subagent 并行翻译流水线 |

---

## 7 阶段流程

```
阶段 1  需求 + 前轮深度采访              ── frontend-interview-dualround (前轮)
阶段 2  六维试吃 × 4-5 站                ── frontend-design-research
阶段 3  后轮深度采访                     ── frontend-interview-dualround (后轮)
阶段 4  视觉锚定三件套                   ── frontend-visual-reference (A Moodboard + B 示意图 + C 帧序列)
阶段 5  资产生成（双路径）                ── baoyu-imagine 主 + 手动兜底
阶段 6  代码实现                         ── 正常开发流程 + 反 slop 横切
阶段 7  迭代微调                         ── frontend-design-review

横切：
├─ 反 slop 硬禁令（阶段 6 常驻）          ── frontend-anti-slop-gate
└─ 深度采访双轮（阶段 1 + 3）            ── frontend-interview-dualround

正交：
├─ 系统架构决策                          ── system-design
└─ 国际化规划（多语言网站）               ── frontend-i18n-essentials

贯穿：design.md 持续维护                 ── frontend-design-writer（kun 15 段）
```

---

## 三种场景覆盖

| 场景 | 触发 | 流程 |
|---|---|---|
| **A · 全新项目** | 没有 `design.md` 或只有空骨架 | 完整阶段 1-7 |
| **B · 整站视觉重写** | `design.md` 有但要换主 Moodboard 方向 | 回阶段 2-4 重跑 |
| **C · 二开 / 增量** | `design.md` 定稿 + 加新页面 / 新 section / 微改 | ★ `frontend-iteration-planner` 入口路由器 |

---

## 快速开始：一个 10 行对话

装好后，触发完整 7 阶段流程：

```
User：我要做一个 fintech 创业公司的 dashboard，先做 landing page。
      从 7 阶段前端设计流程开始。

Claude Code：
  [路由到 frontend-interview-dualround 前轮]
  Q1：主用户是个人交易者、机构 PM 还是 CFO？
  Q2：主语言是数据密度型、还是决策清晰型？
  Q3：...
```

三轮对话后，你会拿到一份 15 段的 `design.md` commit 到项目里。之后每个 AI agent 开始写代码前都会先读它。

---

## 这套东西和别家不同在哪

1. **15 段 DESIGN.md 契约**——不是 README、不是样式指南，而是一份 Agent 可执行的设计契约，覆盖 Primary Language → 六维 Token → 视觉锚定 → MOTION-SPEC → 采访日志。Kun 格式，四轮顺序写。
2. **六维机械骨架**——形 / 色 / 字 / 构 / 质 / 动。每一个参考站都按六维试吃；`design.md` 里的每一个 token 都挂在六维的某一维下。
3. **Moodboard 探索前移**——"AI 生成四方案"这一步发生在 Moodboard 阶段（阶段 4A），不是代码阶段（阶段 6）。代码开始时审美契约已经锁死。
4. **十条反 slop 横切**——十条硬禁令拦住 AI 默认审美（Inter 全套 / 3 列栅格 / `h-screen` 首屏 / 占位数据 / emoji CTA 等），在用户看到结果之前拦下。
5. **认知层自带**——每个 skill 附带 `references/packaged-knowledge/`——原本住在作者 Obsidian 笔记库的完整认知方法论，已打包嵌入。零外部依赖。

---

## 运行要求

- **Claude Code** ≥ 2.0（要支持 plugin marketplace），**或**
- **Codex**（通过 `~/.codex/config.toml` 加载 plugin）
- skill 本身不需要 Python / Node 依赖
- （可选）`baoyu-imagine` 或类似图像生成 skill 用于阶段 5 出图——支持手动兜底

---

## 项目状态

**v0.1.0 · 首个公开版本。** 从作者私有的 `.agents` 插件工作区抽出并脱敏——这套方法论自 2026 年 4 月起就在生产前端设计工作中日常使用。

Kit 本身稳定；认知层（`references/packaged-knowledge/` 下的 .md）是 2026-04-24 的快照。未来更新走标签 release。

---

## 设计哲学

> AI 不负责替你形成审美判断。AI 负责在你划定的判断边界内，快速探索、实现、回看、迭代。

这句共鸣了，你大概率用得顺手。如果你在找"一个 prompt 直接出漂亮网站"的魔法盒——这个 kit 不是那种东西，别家的也不是。

---

## 致谢

- 7 阶段流程从约 40 次 Claude Code 和 Codex 生产级前端对话中结晶出来
- 六维骨架的词汇借鉴自工业设计分类学
- Kun 15 段 DESIGN.md 格式是和 `alexpate/awesome-design-systems` 模板 + Stitch 风格组件契约长期磨合演化出来的

---

## License

MIT © 2026 Kunkun ([@kkunkunya](https://github.com/kkunkunya))
