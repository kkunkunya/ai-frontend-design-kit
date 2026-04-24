---
id: INS-2026-04-18-001
title: "反 slop 硬禁令：AI 前端的十条视觉红线"
maturity: 0.55
tags: [AI设计, 前端, 反slop, 硬禁令, Taste-Skill, 吸收]
source: Leonxlnx/taste-skill clone 2026-04-18, /Users/kunkun/.agents/前端内容优化/repos/taste-skill/
related:
  - "[[AI前端设计认知框架]]"
  - "[[Taste-Skill 吸收记：作者字典 vs 访谈式采访]]"
  - "[[AI 偷懒是训练产物，完整性验收是补偿机制]]"
  - "[[审美是隐性认知，追问是唯一的解压工具]]"
created: 2026-04-18
updated: 2026-04-20
type: asset
status: active
对应skill: frontend-anti-slop-gate（规划中，无状态门禁 skill）
---

# 反 slop 硬禁令：AI 前端的十条视觉红线

## 一句话

==反 slop 硬禁令是"作者硬判断"型约束==，独立于主语言（"项目软诉求"型）之外，打底贯穿所有 AI 前端项目。主语言会随项目变，反 slop 不会。

## 为什么必须独立成条

==七层框架解决的是"这个项目该是什么样"==（项目特异性），但有一批更底层的问题是"所有 AI 生成前端都会踩的坑"——模型训练语料里的统计偏差导致的默认输出。这些坑和项目无关：

- Inter 字体默认、紫/蓝渐变默认、3 列等宽卡片默认、Lucide 图标默认——这是 AI 的**签名**
- 主语言再好，不过硬门禁，每次生成都会回到默认

两者耦合在一起会丢失清晰度。Taste-Skill 把这些硬规抽成独立"Anti-pattern 段"，我的体系应该抽成独立 skill。

## 十条硬禁令清单

按影响层分组。每条附"为什么禁"与"替代方案"。

### 排版层

**1. 禁用 Inter 字体（"高级"语境）**
- 为什么：Inter 是 LLM 默认的"安全"字体，几乎每个 AI 生成站都用它
- 替代：Geist、Outfit、Cabinet Grotesk、Satoshi、Switzer
- 工具型仪表盘允许用无衬线对（Geist + Geist Mono）

**2. 禁用通用衬线字体（Times / Georgia / Garamond / Palatino）**
- 为什么：Web 默认栈，零性格
- 替代：现代衬线 Fraunces、Gambarino、Editorial New、Instrument Serif、Newsreader
- 硬约束：**仪表盘和软件 UI 严格禁用衬线**（仅创意/编辑类项目可用）

### 色彩层

**3. 禁用紫/蓝 AI 渐变（Lila Ban）**
- 为什么：`purple-600`、`indigo-500`、`from-purple-to-blue` 是 AI 生成 UI 的签名
- 替代：Zinc/Slate 中性基础 + 单一高对比度重音（Emerald、Electric Blue、Deep Rose）
- 硬约束：饱和度 < 80%，最多 1 个重音色

**4. 禁用纯黑 `#000000`**
- 为什么：视觉上过于刺眼，无纹理感
- 替代：Off-black（`#0a0a0a`、`#121212`、Zinc-950、炭灰、深海军蓝）

### 布局层

**5. 禁用 3 列等宽卡片特征行**
- 为什么：LLM 在无具体方向时的**默认布局**，最通用的 AI 签名
- 替代：2 列 Zig-Zag、非对称 CSS Grid（`2fr 1fr 1fr`）、水平滚动、砌体（Masonry）

**6. 禁用居中对称 Hero（variance > 4 时）**
- 为什么：展示型项目的主语言通常要求不对称打破视觉惯性
- 替代：Split Screen（50/50）、左对齐文本+右对齐资产、非对称留白

**7. 禁用 `h-screen` 全屏部分**
- 为什么：iOS Safari 地址栏收起时会跳动，灾难级体验
- 替代：`min-h-[100dvh]`

### 资源层

**8. 禁用 Lucide / Feather 默认图标独占**
- 为什么："默认" AI 图标选择，零差异化
- 替代：Phosphor（Bold/Fill）、Heroicons、Radix UI Icons、自定义集
- 硬约束：审计所有图标的描边宽度一致

**9. 禁用通用占位数据**
- 为什么：AI 生成的"John Doe / Jane Smith"、"99.99%"、"Acme Corp"是签名
- 替代：
  - 人名：多样、逼真的真实名字
  - 数字：有机混乱数据（`47.2%`、`$99.00`、`+1 (312) 847-1928`）
  - 公司：发明背景文脉、可信品牌名（不是 Acme / Nexus / SmartFlow）
- 硬约束：**禁用 AI 文案词**——Elevate、Seamless、Unleash、Next-Gen、Game-changer、Delve、Tapestry、"In the world of..."

### 代码层

**10. 禁用 `window.addEventListener('scroll')` 和 React `useState` 动画**
- 为什么：触发连续回流，移动端性能崩溃
- 替代：
  - 滚动：`IntersectionObserver` 或 Framer Motion `whileInView`
  - 连续动画：`useMotionValue` + `useTransform`（React 渲染周期外）
- 硬约束：**动画仅 `transform` 和 `opacity`**，绝不 `top`、`left`、`width`、`height`

## Scale 地板（数字侧硬判断）

十条红线是"禁止的方向"，Scale 地板是"==最小的量=="——两类都是作者硬判断，但前者靠 taste，后者靠 absolute minimum。地板没踩到的项无论多好看都是失格。

来源：吸收自 Anthropic Design 系统提示词的 "Use appropriate scales" 段（2026-04-20 对齐）。

### 字号 / 字距地板

| 场景 | 最小值 | 说明 |
|---|---|---|
| 1920×1080 幻灯片正文 | ==24px== | 低于此值投屏不可读；大多数应显著更大（40-64px 常见） |
| 打印 / PDF 交付正文 | ==12pt== | 印刷最小可读阈 |
| Web 正文（桌面展示型） | 16px | 低于此值用户需放大 |
| Web 正文（工具型稠密界面） | 13px | 仅工具型可下探，展示型不允许 |
| 行高 / line-height | ==1.5-1.7==（正文） | 密度 < 1.4 的大段正文视同失格 |

### 点击 / 触达地板

| 场景 | 最小值 | 说明 |
|---|---|---|
| 移动端 hit target | ==44px × 44px== | 苹果 HIG + Google Material 共识线 |
| 桌面可点击元素 | 32px × 32px | 低于此值在笔记本触控板上失准 |
| 相邻交互元素间隙 | 8px | 低于此值误触率陡升 |

### 对比度地板

| 场景 | 最小值 | 依据 |
|---|---|---|
| 正文文本 / 背景 | ==4.5:1== | WCAG AA |
| 18pt+ 或 14pt 粗体 | 3:1 | WCAG AA Large |
| 非文字 UI 元素（边框、图标） | 3:1 | WCAG 2.1 |
| 禁用态文本 | 3:1 下限 | 可低于 4.5，但不得低于 3 |

### 执行方式

- 这四张表**不参与创意决策**——Agent 写完代码后当成 CI 清单过一遍
- 低于地板时：回去改，不是"看起来差不多就行"
- 工具型 UI 可微调少量地板（比如正文 13px），展示型严格按表执行
- 和十条红线的关系：==红线管"别做什么"，地板管"至少做到什么"==

## 附加次级禁令（可选择启用）

- 禁用圆形 loading spinner（改骨骼屏匹配布局形状）
- 禁用霓虹外部发光（`filter: drop-shadow` / 纯色 glow）
- 禁用自定义鼠标光标
- 禁用标签 `SECTION 01` / `QUESTION 05` / `ABOUT US`（廉价感）
- 禁用"Scroll to explore" / 滚动箭头 / 弹跳 v 形（填充 UI 噪音）

## 和七层框架的关系

==反 slop 不是七层之一==，是七层的**横切前置硬规**——类似 CI 里的 lint：

```
反 slop 硬门禁  ←← 横切前置，每次代码生成前必过
    ↓
[层 0] 主语言  ←→ 层 1 目标 ←→ 层 2 调研 ←→ 层 2.5 动效采访
    ↓
[层 3] 方向（DESIGN.md 契约）
    ↓
[层 4] 生成（Codex 写代码） ←← 反 slop 门禁常驻
    ↓
[层 5] 评审（三段式追问）
    ↓
[层 6] 沉淀
```

## 如何应用

### 作为独立 skill（推荐）
- skill 名：`frontend-anti-slop-gate`
- **无状态**——不读取项目 knowledge/，纯规则匹配
- 触发：Agent 写前端代码前 / 评审代码时 / 显式调用"走一遍 anti-slop"
- 产出：违规项清单 + 必须修正项（P0）

### 作为 DESIGN.md 的 §12 Anti-pattern 段
- 契约里显式引用本笔记
- 每个项目可定制补充（某些项目允许纯黑 / 某些项目允许 Inter）

## 抗腐化机制

硬禁令的风险是**随潮流过期**（比如 Inter 再度变好、某配色从俗套翻身）。每条禁令必须带：

1. **为什么禁**（rationale）—— 见上清单
2. **过期条件**—— 什么情况下这条可以撤销
3. **上下文评估**—— 某个项目的主语言和这条冲突时，主语言优先

## 来源

本清单吸收自 Taste-Skill 的 `taste-skill/SKILL.md` + `redesign-skill/SKILL.md` + `soft-skill/SKILL.md` 的 Anti-pattern 段交集。完整 Taste 拆解见 [[Taste-Skill 吸收记：作者字典 vs 访谈式采访]]。

## 关联

- [[AI前端设计认知框架]]：七层框架总图，本笔记是其横切前置硬规
- [[Taste-Skill 吸收记：作者字典 vs 访谈式采访]]：来源与对齐分析
- [[AI 偷懒是训练产物，完整性验收是补偿机制]]：姊妹笔记——硬禁令防视觉 slop，完整性验收防工程 slop
- [[审美是隐性认知，追问是唯一的解压工具]]：互补——审美是用户侧软约束，反 slop 是作者侧硬约束
