# Style & Writing Guide

This skill uses a fixed visual template. When populating the JSON, follow these
guidelines so the output looks polished and is dense enough to fit 2 pages.

## Visual system (fixed; do not edit)

- **Palette**: Navy `#1e40af` accent, dark `#111827` for bold headings, body
  `#1f2937`, muted gray `#4b5563` for metadata.
- **Page**: A4, margins 10mm top/bottom × 12mm left/right.
- **Name**: 24pt bold navy, letter-spaced.
- **Title tag**: Solid navy pill, 9.5pt white bold.
- **Section titles**: 12pt bold navy with 1.5pt bottom border.
- **Project banners**: Solid navy, 10pt white bold.
- **Sub-section banners**: Light blue `#eff6ff` with 4px navy left bar, 9pt
  navy bold.
- **Bullets**: 8.8pt body, leading 1.5; navy `·` marker.
- **Company header**: 11pt bold dark + gray role + 8.5pt right-aligned meta.
- **Photo**: 80×108px top-right, optional.

## Writing rules

### 1. Lead with metrics, not responsibilities

Each bullet should end with a **bolded quantified result**. Structure:

> **Action verb** + what you built/designed + how + **metric → metric (Δ)**.

Good:
> 设计归因树 DSL 与基于状态机的 BFS 分层推进协议……**数据计算准确率 73% → 92%（↑19pp）**。

Weak (no metric, passive voice):
> 参与了系统的设计与开发，负责了部分模块的实现工作。

### 2. Use strong action verbs

Preferred first words for bullets: 主导 / 设计 / 构建 / 落地 / 搭建 / 沉淀 /
抽象 / 攻克 / 推动 / 协同. Avoid: 参与 / 协助 / 负责 (use 主导 or 独立负责
instead for senior roles).

### 3. Pyramid principle per section

- **Company intro** (1 paragraph): role scope → two-phase strategy → aggregate
  impact metric.
- **Project intro** (1 paragraph, optional): positioning → pain point →
  paradigm shift → adoption metric.
- **Sub-section bullets**: 3–5 per sub-section, grouped by theme (architecture,
  accuracy, reliability, efficiency, collaboration, etc.).

### 4. Quantify aggressively

Find numbers for:
- User scale: DAU / UV / 覆盖业务数 / 用户数
- Performance: latency / throughput / FCP / success rate
- Efficiency: 提效小时数 / 接入周期缩短 / 成本节省
- Quality: 准确率 / 召回率 / 采纳率 / NPS / 质量分
- Reuse: npm 下载量 / 跨业务复用数 / 接入 API 数

If a metric is an estimate, mark it as approximate ("约 X") — do not invent.

### 5. Density target

- 2 A4 pages total is the hard constraint.
- Page 1: header + 个人概述 + 技术栈 + 第一家公司 + primary project.
- Page 2: remaining project(s) of first company + second company + 教育 + 证书.
- Use `"page_break": true` on a project to manually push it to page 2 if the
  automatic flow spills the primary project onto page 2 early.

### 6. Bold sparingly

Over-bolding destroys the scanning pattern. Bold only:
- Core role scope in company intro
- Paradigm name ("AI 执行、人决策")
- Final quantified result at end of each bullet
- Key product/positioning phrases in project intro

Do NOT bold:
- Mid-sentence technical terms (let them be body weight)
- Conjunctions or punctuation
- More than ~30% of any paragraph

### 7. Photo

If a `photo.jpg` is supplied (35mm × 45mm portrait, white/blue background
recommended), pass it via `--photo photo.jpg`. The generator will embed it in
the DOCX and reference it via `file://` in the HTML. For best PDF rendering,
place the photo in the same directory as the HTML output (the default `--outdir`
if you don't specify an absolute path).

### 8. Tone

Professional, concise, self-confident but not boastful. Avoid internet slang,
emoji, and first person ("我"). Use third-person implicit subject ("主导构建"
not "我主导构建").
