---
id: INS-2026-04-21-005
title: "动效文档两层：MOTION-SPEC vs 实现 prompt 集"
maturity: 0.70
tags: [AI设计, 前端, 动效, MOTION-SPEC, 实现prompt, 帧序列, 双层文档]
source: conversation/2026-04-21-cognition-intake-align
related:
  - "[[AI前端设计认知框架]]"
  - "[[AI 前端动效盲区与采访补偿]]"
  - "[[视觉锚定三件套：代价结构与资产生成双路径]]"
  - "[[六维：形色字构质动是网站试吃的机械骨架]]"
  - "[[DESIGN.md 格式与七层框架的位置]]"
created: 2026-04-21
updated: 2026-04-21
type: asset
status: active
---

# 动效文档两层：MOTION-SPEC vs 实现 prompt 集

## 一句话

==动效采访的产出==要写成两层文档：**宏观的 MOTION-SPEC.md**（给人读的量化语法 + 双向性约定）+ **微观的实现 prompt 集**（逐组件 code-level 规格，直接喂 Agent 出代码），两层互补，缺一不可。

## 为什么一层不够

早期只用 MOTION-SPEC（量化语法 `[触发]→[主体]→[变化]→[时序]`）有两个问题：

1. **量化语法还是"描述"**——"按钮 hover 时轻微放大 1.02x，300ms ease-out"给人看能懂，给 Agent 看会生成各种风格的实现
2. **跨元素协调缺失**——单个动效描述清楚了，但"Loading 页面的 4 个元素合起来的时序"没地方住

另一方面，只用实现 prompt 也不够：

1. **人读不动**——一份 500 行的 Framer Motion 规格，用户审不了"这个方向对不对"
2. **采访阶段过早落代码**——在没定组件架构时就逐元素写 Tailwind class，会锁死后续灵活性

## 两层的定义

| 层 | 给谁读 | 粒度 | 何时写 | 产出形态 |
|---|---|---|---|---|
| **宏观层（MOTION-SPEC.md）** | 人 + Agent 并读 | 整站所有核心动效的清单 + 单条量化语法 | 调研 + 动效采访阶段 | 一份 markdown 清单，按类型分组 |
| **微观层（实现 prompt 集）** | Agent 直接读 → 写代码 | 单个关键组件的完整实现规格 | 代码实现阶段触发时 | 多份 prompt 文档，一组件一份 |

## 宏观层结构（MOTION-SPEC.md）

对应 `DESIGN.md` §10，或独立成文件：

```markdown
## 1. 核心动效清单

### 1.1 Hero 入场
- 触发：页面 mount
- 主体：大标题 + 副标题 + CTA
- 变化：从下向上 20px translate + opacity 0→1
- 时序：stagger 80ms，单个 duration 600ms，ease [0.4, 0, 0.2, 1]
- 双向性：单次触发（一次性）

### 1.2 Scroll-driven 背景变化
- 触发：scrollY 0 → 1000px
- 主体：背景 gradient 角度 + 噪点密度
- 变化：角度 90deg → 180deg，噪点 5% → 2%
- 时序：scroll-linked 双向连续
- 双向性：跟随 scrollY 实时反向（禁止 `whileInView: once: true`）

## 2. 双向性约定
核心动效默认 scroll-driven 双向连续；单次触发仅用于装饰

## 3. 层级关系
多动效共存时的优先级：...

## 4. 反例清单
- 禁止：通用 fade-in on whileInView
- 禁止：无触发条件的循环动画
```

## 微观层结构（实现 prompt 集）

当代码实现阶段触达核心组件时，为该组件==现场生成==一份完整的实现 prompt。参考范式（LoadingScreen 风格）：

```markdown
# LoadingScreen 动效实现 prompt

## Theme Tokens
--bg: #0a0a0a
--text: #f5f5f5
--muted: #888888
--stroke: #1f1f1f

## Fonts
font-display: Instrument Serif (Google Fonts, italic, weight 400)

## Component 签名
props: { onComplete: () => void }

## Container
<motion.div> fixed inset-0 z-[9999] bg-bg
exit: { opacity: 0 }, duration 0.6s, ease [0.4, 0, 0.2, 1]
wrap in <AnimatePresence mode="wait">

## Element 1: "Portfolio" Label
位置：absolute top-8 left-8 md:top-12 md:left-12
文本：Portfolio
class：text-xs md:text-sm text-muted uppercase tracking-[0.3em]
entrance：initial={{ opacity: 0, y: -20 }}, animate={{ opacity: 1, y: 0 }}
duration 0.6s, delay 0.1s

## Element 2: Rotating Words (Center)
...（每个元素逐一展开）

## Element N: Progress Bar
...

## Parent Wrapper Behavior
state: isLoading starts true
render <LoadingScreen /> inside <AnimatePresence mode="wait"> only when isLoading
main content: style={{ opacity: isLoading ? 0 : 1, transition: "opacity 0.5s ease-out" }}

## Timing Summary
0.0s — Loader appears, "Portfolio" slides in, counter starts at 000
0.0s — "Design" appears
0.9s — "Create" replaces "Design"
1.8s — "Inspire" replaces "Create"
2.7s — Counter hits 100, progress bar full
3.1s — onComplete fires (400ms delay)
3.1s — Loader fades out (0.6s exit animation)
3.7s — Page content fades in (0.5s opacity transition)
```

## 微观层的核心要素

一份合格的实现 prompt 必须覆盖：

| 要素 | 作用 | 例子 |
|---|---|---|
| **Theme tokens** | 色板量化到像素 | `--bg: #0a0a0a` |
| **字体引用** | 字体量化到 Google Fonts / 本地字体 | Instrument Serif italic 400 |
| **组件签名** | Props / TS 类型 / 依赖 | `onComplete: () => void` |
| **逐元素规格** | 位置 / class / initial / animate / exit / duration / ease | Tailwind + framer motion 字段 |
| **父组件行为** | 状态管理 / AnimatePresence / 可见性 | isLoading state + mode wait |
| **时序总表** | 0.0s → N.0s 每个时间点谁在动 | 全景时间轴 |

==时序总表是关键==——让 AI 看到"所有动效合起来是什么节奏"，而不是孤立看单个元素。

## 两层的触发关系

```
动效采访阶段
   ↓
 写 MOTION-SPEC.md（宏观层）
   ↓
视觉锚定阶段
   ↓
 生成动效帧序列图（视觉锚点）
   ↓
代码实现阶段
   ↓
 核心动效组件逐一写实现 prompt（微观层）
   ↓
 喂给 Agent 生成代码
```

==微观层是按需生成的==——不是一次性把所有组件的 prompt 都写完，而是代码实现时每触达一个核心动效组件，现场生成一份。

## 核心动效组件判定

哪些组件需要微观层 prompt？三条标准：

1. **跨元素协调**——多个子元素有时序依赖（如 LoadingScreen 4 元素联动）
2. **非标准动效**——不是简单 fade/translate，涉及 stagger / scroll-linked / gesture
3. **品牌承载**——Hero / 转场 / 标志性交互，直接决定产品气质

次要动效（普通 hover / 按钮 scale / 单 fade）只在 MOTION-SPEC 宏观层登记，不单写实现 prompt。

## 和视觉锚定三件套的关系

三件套的阶段 C（动效帧序列）和动效文档的关系：

| 产出物 | 角色 | 给谁用 |
|---|---|---|
| **动效帧序列图** | 视觉锚点（从左到右帧变化） | 给用户看确认方向 + 给 Agent 视觉参考 |
| **MOTION-SPEC（宏观）** | 整站动效清单 + 双向性约定 | 人 Agent 并读 |
| **实现 prompt（微观）** | 单组件代码级规格 | Agent 写代码直接读 |

三者互补，==帧序列 + 实现 prompt 对应同一个动效==——帧序列是图像锚点，实现 prompt 是代码规格，双产出。

详见 [[视觉锚定三件套：代价结构与资产生成双路径]]。

## 样板住在哪里

==笔记库（Obsidian）只存方法论结构==（上面展示的 LoadingScreen 范例是**范式参考**，不是可复用模板）。

**实际的实现 prompt 住在具体项目的 `knowledge/design-docs/motion-prompts/` 目录下**，一组件一份，命名 `<component>.md`。Agent 按项目 context 现场生成，用完归档。

跨项目的稳定模式（比如"Loading 页通用结构"）如果沉淀出来，可以升级为 skill 级模板（`frontend-motion-interview` 或类似），但前期都是项目级 reference。

## 升级路径

两层文档的成熟度演化：

1. **初期**：手写 MOTION-SPEC + 手写实现 prompt
2. **中期**：做专用 skill（`frontend-motion-interview` + `frontend-motion-prompt-generator`），从采访输出自动生成两层
3. **未来**：帧序列图直接生成实现 prompt（多模态模型读图出代码规格）

## deletion-spec

- **触发删除条件**：当 AI 默认能从"动效采访结果"一次到位生成代码（不需要两层文档中转）时，本笔记可降级；或者当前端实现栈变化（比如 framer motion 被新库取代）使得实现 prompt 结构需要重写时，本笔记的"实现 prompt 要素"段需要更新。
- **禁用方式**：frontmatter 改 `status: deprecated`。
- **卸载清单**：本笔记被 [[AI前端设计认知框架]] 的阶段流程引用、被 [[视觉锚定三件套：代价结构与资产生成双路径]] 的阶段 C 引用、被 [[AI 前端动效盲区与采访补偿]] 的"升级到实现 prompt"段引用（后者将在组 2.2 大改中加入）、被 [[DESIGN.md 格式与七层框架的位置]] 的 §10 定位引用。拆除前需在上述位置替换。
