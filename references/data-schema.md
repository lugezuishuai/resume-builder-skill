# Resume JSON Schema

The generator expects a single JSON file describing the candidate. The visual
template is fixed (navy-blue consulting style, A4, 2-page target), so data
must be curated for density — aim for 1500–2200 CJK characters total across
summary + skills + experience bullets.

## Top-level fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | yes | Full name. Renders at 24pt navy. |
| `title` | string | yes | Title tag under the name (e.g. "全栈工程师（AI Agent 方向）"). |
| `contact` | object | yes | See Contact below. |
| `summary` | string | yes | 个人概述 paragraph. `**bold**` markers supported. |
| `skills` | array | yes | List of `{label, desc}` rows (3–5 rows recommended). |
| `experience` | array | yes | Chronological list of companies, newest first. |
| `education` | array | yes | List of `{school, degree, meta}`. |
| `certs` | array | yes | Mix of strings or `{text, bold}` objects; first item auto-bolds if plain string. |

## Contact object

```jsonc
{
  "age": "27",                 // optional
  "phone": "13800000000",      // optional
  "email": "x@y.com",          // optional
  "wechat": "wxid",            // optional
  "blog": "https://x.com/",    // optional, rendered as clickable link
  "extra": [                   // optional, additional contact entries
    {"label": "GitHub", "url": "https://github.com/x"}
  ]
}
```

## Skills array

```jsonc
[
  {"label": "AI Agent 工程化：", "desc": "熟悉 Agent Loop、Harness Engineering …"},
  {"label": "全栈开发：",        "desc": "精通 TypeScript、React …"}
]
```

Each skill renders as a single line: **label** in bold, `desc` in regular.
Keep `desc` to ~60–90 CJK chars for 2-page fit.

## Experience array

```jsonc
[
  {
    "company": "星河云智科技（虚构）· 企业智能产品部",
    "role":    "全栈开发工程师",
    "meta":    "2024.06 – 至今 · 杭州",
    "intro":   "公司/角色概述 paragraph。支持 **bold**。",
    "projects": [
      {
    "title":     "星河运营助手",                 // dark navy banner
        "intro":     "项目概述（可选）。支持 **bold**。",
        "page_break": false,                       // set true to push to next page
        "sections": [
          {
    "subtitle": "Agent 架构与工程体系",              // light-blue sub-banner
            "bullets": [
              "第一条 bullet。支持 **bold** 高亮核心指标。",
              "第二条 bullet。量化数据如 **准确率 73% → 92%（↑19pp）** 请加粗。"
            ]
          }
        ],
        "bullets": [          // optional: bullets directly under project (no sub-banner)
          "无 subsection 的 bullet 内容。"
        ]
      }
    ]
  }
]
```

### Notes on projects

- Each project can use either `sections` (with subtitles) or top-level `bullets`.
- Mixing both is fine — top-level `bullets` render after all `sections`.
- Set `page_break: true` on a project to push it to the next page (useful when
  the last project of a company needs to land on page 2 to keep the 2-page
  balance).
- Bold your metrics. The template uses bold dark text (`#111827`) for emphasis,
  so wrap numbers and key achievements in `**…**`.

## Education array

```jsonc
[
  {"school": "海州理工大学（虚构）", "degree": "软件工程 本科", "meta": "2016.09 – 2020.06 · 南京"}
]
```

## Certs array

```jsonc
[
  {"text": "系统架构设计师（示例）", "bold": true},
  {"text": "云原生应用开发认证（示例）"},
  "大学英语六级（示例）"
]
```

Entries render as ` · `-separated items on a single line.

## Inline formatting

Only one inline marker is supported in all text fields:

- `**bold**` → bold weight, darker color (`#111827`). Use for:
  - Quantified metrics: `**↑19pp**`, `**↓52%**`
  - Key role titles/scope: `**主导构建 …**`
  - Product names where they first appear in a project intro

Do not nest markers; do not use other markdown (no italics, no links in body
text — hyperlinks are only supported in the contact/blog section).

## Length guidance

| Section | Target length |
|---------|---------------|
| summary | 80–130 CJK chars, 1 paragraph |
| skills  | 4 items × ~70 chars |
| experience[0].intro | ~120–150 chars |
| experience[0].projects — hero project | 4–6 bullets |
| experience[0].projects — other | 1–2 bullets per project |
| experience[1+] | 2–3 projects, 1 bullet each |

If the rendered PDF spills to 3 pages, prune weaker bullets first before
shrinking font sizes — the template is tuned for readability and the font
sizes should not be reduced below 8.5pt body / 8.8pt bullet.

## See also

- `examples/resume_data_example.json` — full working example
- `references/style-guide.md` — visual and content-writing rules enforced by
  the template
