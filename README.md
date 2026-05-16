# AI Frontend Design Kit

## Portfolio positioning

This is a portfolio-facing Agent/Plugin project for frontend design direction, design constraints, and Claude Code/Codex handoff workflows. It demonstrates how I package reusable AI engineering capability as installable project tooling.


**English · [中文](./README.zh.md)**

> **Constraints before generation.** A 10-skill plugin for Claude Code and Codex that upgrades AI-assisted frontend design from "generate four options, pick one" to a rigorous 7-stage flow with taste-level design contracts.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-blue)](https://docs.claude.com/en/docs/claude-code)
[![Codex](https://img.shields.io/badge/Codex-plugin-green)](https://github.com/openai/codex)

---

## Why this exists

Most "AI frontend" tooling generates 2–4 page variants and asks you to pick one. That works for throwaway demos. It does not work when you actually care about the result — because AI cannot form your aesthetic judgment for you.

This kit inverts the loop:

1. **Form constraints first** (primary language, six-dimension taste mapping, dualround interview, moodboard anchor, motion spec)
2. **Generate inside those constraints** (visual references, code, iterations)
3. **Review against the contract**, rewrite the contract, loop

The result is a persistent `design.md` (kun 15-segment format) that every subsequent AI agent reads before touching a single line of JSX.

---

## Install

### Claude Code

```bash
# Add this repo as a marketplace
claude plugin marketplace add https://github.com/kkunkunya/ai-frontend-design-kit

# Install the plugin
claude plugin install ai-frontend-design-kit@ai-frontend-design-kit
```

### Codex

```bash
# Clone anywhere
git clone https://github.com/kkunkunya/ai-frontend-design-kit.git ~/codex-plugins/ai-frontend-design-kit

# Register in ~/.codex/config.toml
[[plugins]]
source = "~/codex-plugins/ai-frontend-design-kit"
enabled = true
```

### Verify

After install, ask Claude Code or Codex:

> Start a new frontend project, use the 7-stage flow.

The agent should route to `frontend-interview-dualround` (stage 1). If it does, you're set.

---

## The 10 skills

| Skill | Stage | One-line job |
|---|---|---|
| `frontend-iteration-planner` | Scenario C entry | Increment router — read existing `design.md` + assets, then tier the change into T1 new-page / T2 new-section / T3 micro-tweak |
| `frontend-interview-dualround` | Cross-cut (stage 1 + 3) | Product-designer perspective five-class interview, pre-round to set boundaries + post-round to extract details |
| `frontend-design-research` | Stage 2 | Six-dimension tasting (form / color / type / layout / material / motion) across 4–5 reference sites → `design.md` draft |
| `frontend-visual-reference` | Stage 4 | Visual anchor triptych (moodboard exploration + page mockup + motion frame sequence) via `baoyu-imagine` + manual fallback |
| `frontend-motion-prompt-writer` | Stage 4C + 6 | Translates MOTION-SPEC + frame sequence into LoadingScreen-style code-level prompts |
| `frontend-design-writer` | Stages 1–7 | **The main writer.** Kun 15-segment 4-round workflow; maintains `design.md` + `MOTION-SPEC.md` + `DESIGN-LANGUAGE.md` |
| `frontend-design-review` | Stage 7 | Post-code three-stage interrogation + Tweaks parameter externalization + write-back to `design.md` |
| `frontend-anti-slop-gate` | Stage 6 cross-cut | Ten hard red-lines scan (Inter font / Lila palette / 3-column grid / h-screen / placeholder data / etc.) |
| `system-design` | Orthogonal | System architecture: Design Doc + ADR + structural-impact analysis |
| `frontend-i18n-essentials` | Orthogonal | i18n three-layer orthogonal model (runtime / translation-content / visual) + subagent-parallel translation pipeline |

---

## The 7-stage flow

```
Stage 1  Requirement + pre-round interview      ── frontend-interview-dualround (pre)
Stage 2  Six-dimension tasting × 4-5 sites       ── frontend-design-research
Stage 3  Post-round interview                    ── frontend-interview-dualround (post)
Stage 4  Visual anchor triptych                  ── frontend-visual-reference (A moodboard + B mockup + C motion frames)
Stage 5  Asset generation (dual path)            ── baoyu-imagine primary + manual fallback
Stage 6  Code implementation                     ── your usual dev flow + anti-slop gate
Stage 7  Iteration and micro-tuning              ── frontend-design-review

Cross-cuts:
├─ Anti-slop hard bans (stage 6 resident)        ── frontend-anti-slop-gate
└─ Dualround interview (stages 1 + 3)            ── frontend-interview-dualround

Orthogonal:
├─ System architecture decisions                 ── system-design
└─ i18n planning (multilingual sites)            ── frontend-i18n-essentials

Throughline: design.md is continuously maintained ── frontend-design-writer (kun 15 segments)
```

---

## Three scenario coverage

| Scenario | Trigger | Flow |
|---|---|---|
| **A · Greenfield** | No `design.md` or empty skeleton | Full stages 1–7 |
| **B · Full visual rewrite** | `design.md` exists but main moodboard direction changes | Replay stages 2–4 |
| **C · Increment / extension** | `design.md` finalized + new page / new section / micro-fix | ★ `frontend-iteration-planner` entry router |

---

## Quick start: a 10-line call

Once installed, trigger the full 7-stage flow:

```
User: I want to build a dashboard for a fintech startup. Landing page first. 
      Start from the 7-stage frontend design flow.

Claude Code:
  [routes to frontend-interview-dualround pre-round]
  Q1: Who is the primary user — individual traders, institutional PMs, or CFOs?
  Q2: Is the primary language stat-dense density, or decisive-action clarity?
  Q3: ...
```

Three rounds later you'll have a 15-segment `design.md` committed to your project. From there, every future AI agent reads it before generating code.

---

## What makes this different

1. **15-segment DESIGN.md contract** — Not a README, not a style guide. It's a machine-executable design contract that covers Primary Language → Six-Dimension Tokens → Visual Anchors → Motion Spec → Interview Log. Written in kun format, four-round sequence.
2. **Six-dimension mechanical skeleton** — Form / Color / Typography / Layout / Material / Motion. Every reference site gets tasted across all six; every token in `design.md` hangs under one of these.
3. **Moodboard exploration moved forward** — The "AI generates four variants" step happens at the moodboard (stage 4A), not at the code (stage 6). By the time code starts, the aesthetic contract is locked.
4. **Ten-line anti-slop gate** — Ten hard bans that catch the AI's default aesthetic (Inter everything / 3-column grid / `h-screen` hero / placeholder data / emoji CTAs / etc.) before the user sees the result.
5. **Packaged cognitive knowledge** — Each skill ships with `references/packaged-knowledge/` — the full cognitive methodology that originally lived in the author's Obsidian vault, snapshotted and embedded. Zero external dependency.

---

## Requirements

- **Claude Code** ≥ 2.0 with plugin marketplace support, **or**
- **Codex** with plugin loading via `~/.codex/config.toml`
- No Python / Node dependencies for the skills themselves
- (Optional) `baoyu-imagine` or similar image-generation skill for stage 5 asset generation — manual fallback supported

---

## Project status

**v0.1.0 · Initial release.** Extracted and desensitized from the author's private `.agents` plugin workspace, where it has been in daily use since 2026-04 for production frontend design work.

The kit is stable; the cognitive layer (in `references/packaged-knowledge/`) is a snapshot taken on 2026-04-24. Future updates will be tagged releases.

---

## Philosophy

> AI does not form aesthetic judgment for you. AI delivers fast exploration, implementation, retrospection, and iteration *inside* the judgment boundary you draw.

If this resonates, you'll feel at home. If you're looking for a "one prompt, beautiful site" magic box — this is not that, and nothing else is either.

---

## Acknowledgements

- The 7-stage flow crystallized from ~40 production frontend sessions with Claude Code and Codex
- The six-dimension skeleton borrows vocabulary from industrial design taxonomies
- The kun 15-segment DESIGN.md format evolved out of friction with `alexpate/awesome-design-systems` templates and Stitch-style component contracts

---

## License

MIT © 2026 Kunkun ([@kkunkunya](https://github.com/kkunkunya))
