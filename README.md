# resume-builder

为中文技术岗求职者生成**专业排版简历**的 Agent Skill。海军蓝（navy）咨询风固定模板，
一次产出 HTML / DOCX / PDF 三种格式，目标控制在 A4 2 页。

![style](assets/badges/style.svg)
![format](assets/badges/format.svg)
![license](assets/badges/license.svg)

## ✨ 特点

- **固定视觉系统**：海军蓝 #1e40af 主色、深蓝 project banner、浅蓝子模块横幅、量化指标加粗，
  复刻咨询/大厂简历的专业观感——用户专注内容即可，不用操心排版。
- **三格式一次产出**：同一份 JSON 数据同时渲染 HTML（单文件）、DOCX（可编辑 Word）、
  PDF（WeasyPrint 渲染，投递首选）。
- **结构化数据驱动**：一份 `resume.json` 描述姓名/概述/技术栈/工作经历/项目/教育/证书，
  所有文本支持 `**加粗**` Markdown 标记。
- **精确分页控制**：可对任意项目设置 `"page_break": true` 强制换页，保证 2 页平衡。
- **证件照可选**：传 `--photo photo.jpg` 即可在右上角嵌入证件照。
- **指标导向**：内建写作规范引导用户为每条 bullet 补量化数据（DAU、性能提升、提效小时、
  准确率 delta 等）。

## 📦 安装

使用 `npx skills add` 全局安装：

```bash
npx skills add lugezuishuai/resume-builder-skill -g -y
```

如果需要指定安装到 Codex：

```bash
npx skills add lugezuishuai/resume-builder-skill -g -y -a codex
```

安装完成后，在新的会话中即可直接使用 `resume-builder`。

### Python 依赖

```bash
python3 -m pip install python-docx weasyprint
```

Linux 最小化环境如 WeasyPrint 报错，补装系统库：

```bash
# Debian/Ubuntu
sudo apt-get install -y libpango-1.0-0 libpangoft2-1.0-0 libharfbuzz0b fonts-noto-cjk
```

macOS / Windows 自带 CJK 字体，一般开箱即用。

## 🚀 快速开始

安装后，在 Codex、Claude Code 等已安装该 Skill 的 Agent 的新会话中直接说即可：

```
帮我用 resume-builder 生成一份简历。我是 5 年前端经验，现在在 XX 公司做 XX……
```

Agent 会引导你补充信息（或直接从你粘贴的旧简历里提取），生成 `resume.json`，
再调用脚本一次产出三个文件。若需手动运行，将 `<SKILL_DIR>` 替换为安装后的
`resume-builder` 目录：

```bash
python3 <SKILL_DIR>/scripts/generate_resume.py --data resume.json --outdir ./ --photo photo.jpg
# 输出：resume.html / resume.docx / resume.pdf
```

## 📁 目录结构

```
resume-builder/
├── SKILL.md                        # Skill 入口与工作流（供 Agent 读取）
├── skillmap.json                   # Skill 元数据
├── LICENSE                         # MIT License
├── README.md                       # 本文件
├── scripts/
│   └── generate_resume.py          # 三格式生成脚本（核心）
├── references/
│   ├── data-schema.md              # JSON 字段完整说明
│   └── style-guide.md              # 视觉系统 + 写作规范（加粗/动词/数字规则）
└── examples/
    └── resume_data_example.json    # 完整样例数据（高级全栈/AI Agent 方向）
```

## 📝 JSON 数据结构（最小示例）

```json
{
  "name": "张三",
  "title": "高级前端工程师",
  "contact": {
    "phone": "13800000000",
    "email": "zhang@example.com"
  },
  "summary": "5 年前端研发经验，现任 XX 公司高级前端工程师，主导 XX 平台建设……",
  "skills": [
    {"label": "前端工程化：", "desc": "精通 React/TypeScript/Vite，主导建设 XX 组件库，周下载 500+。"}
  ],
  "experience": [
    {
      "company": "XX 公司 · YY 部门",
      "role": "高级前端工程师",
      "meta": "2022.01 – 至今 · 北京",
      "intro": "担任……，**主导构建 XX 平台**，**已覆盖 50+ 业务线**。",
      "projects": [
        {
          "title": "项目名称",
          "intro": "项目定位与 impact。",
          "sections": [
            {
              "subtitle": "架构设计",
              "bullets": ["设计 XX 架构……**FCP ↓52%**。"]
            }
          ]
        }
      ]
    }
  ],
  "education": [
    {"school": "XX 大学", "degree": "计算机 本科", "meta": "2016.09 – 2020.06 · 北京"}
  ],
  "certs": [{"text": "系统架构设计师（软考高级）", "bold": true}]
}
```

完整字段、长度建议、写作要点见 [references/data-schema.md](references/data-schema.md)
与 [references/style-guide.md](references/style-guide.md)。可直接以
[examples/resume_data_example.json](examples/resume_data_example.json) 为模板改。

## 🎨 视觉规范（固定，勿改）

| 元素 | 规格 |
|------|------|
| 页面 | A4，margin 10mm × 12mm |
| 姓名 | 24pt 加粗 #1e40af，字距 3px |
| 职位标签 | 9.5pt 白字 #1e40af 背景 |
| 章节标题 | 12pt 加粗 #1e40af，下划 1.5pt |
| 公司名 | 11pt 加粗 #111827 |
| Project banner | #1e40af 背景，10pt 白字加粗 |
| 子模块 banner | #eff6ff 背景 + 4px 左条，9pt #1e40af 加粗 |
| 正文 | 9pt / 8.8pt bullet，#1f2937，行距 1.45–1.5 |
| 加粗 | 700 weight #111827，用于 scope/范式/数字结果 |

## 🛠 命令行用法

```
python3 <SKILL_DIR>/scripts/generate_resume.py \
  --data resume.json \
  --outdir ./output \
  [--photo photo.jpg] \
  [--html-only] [--no-pdf]
```

- `--data`（必填）：JSON 数据文件路径
- `--outdir`：输出目录，默认当前目录
- `--photo`：可选证件照（建议 35×45mm 白底/蓝底 JPG）
- `--html-only`：只输出 HTML（跳过 DOCX/PDF）
- `--no-pdf`：输出 HTML + DOCX，不渲染 PDF

## 💡 写作要点（节选自 Style Guide）

1. **每条 bullet 以动词开头，以加粗的数字结尾**：
   > 设计归因树 DSL 与 BFS 分层推进协议……**数据计算准确率 73% → 92%（↑19pp）**。
2. **动词清单**：主导 / 设计 / 构建 / 落地 / 搭建 / 沉淀 / 抽象 / 攻克 / 推动 / 协同。
   避免"参与 / 协助 / 负责了"。
3. **数字维度**：用户规模（DAU/UV）、性能（latency/FCP/success rate）、效率（提效小时/
   接入周期）、质量（准确率/召回率/NPS）、复用（下载量/业务覆盖数）。
4. **粗体占比 < 30%**：只给核心 scope、范式名、最终数字加粗，不给中间术语加粗。
5. **目标 2 页**：超出时删内容，不要缩字号。用 `"page_break": true` 控制分页。
6. **不用第一人称**，不用 emoji，不用网络用语。

## 📄 License

MIT © 2026 黄东璐 (Huang Donglu). 详见 [LICENSE](LICENSE)。

## 🔗 相关

- 样例数据参考了作者本人的真实简历打磨过程，内容密度和加粗模式可直接对照复用。
- 如有问题或改进建议，欢迎提 issue / PR。
