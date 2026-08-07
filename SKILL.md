---
name: resume-builder
description: 专业技术简历生成器，输出固定海军蓝咨询风的 PDF/DOCX/HTML 三格式简历（A4 2 页）。支持用户说"帮我写简历"、"生成简历"、"更新简历"、"导出简历 PDF/DOCX"、"把这份简历做成好看的 PDF"、"排版我的简历"、"重新生成简历"、"按这个风格生成简历"等场景时使用，即使用户没有明确要求某种格式。工作流：(1) 通过引导式对话收集候选人信息并填充到标准 JSON 数据文件；(2) 调用 scripts/generate_resume.py 一次产出 HTML/DOCX/PDF；(3) 用户可迭代修改 JSON 内容并重跑脚本。当用户需要一份排版专业、可直接投递的中文 tech resume 时都应触发本技能，尤其是互联网/大厂技术岗跳槽、晋升材料、求职简历制作场景。
---

# 简历制作 Skill (resume-builder)

为中文技术岗位生成一份排版专业的简历，一次性输出 **HTML / DOCX / PDF** 三种格式。
视觉风格为**海军蓝（navy）咨询风**——深蓝标题条、浅蓝副标题条、结构化 project banner、
量化指标加粗——已锁定为固定模板，用户无需也不应调样式，专注内容即可。

## 核心原则

1. **不要自己写排版代码**。所有文件生成都必须通过 `scripts/generate_resume.py` 完成。
2. **不要改样式**。颜色、字号、间距、banner 形态是固定的，改了就破坏了风格的一致性。
3. **内容为王**。把精力花在帮用户挖掘经历亮点、提炼量化指标、把 STAR 转成 bullet 上。
4. **目标 2 页 A4**。超过 2 页就删次要内容，而不是缩小字号。

---

## 环境准备

首次运行前确认环境为 **Python 3.8+**，并安装依赖（用户环境没有就装）：

```bash
python3 -m pip install --user --break-system-packages python-docx weasyprint
```

中文字体由系统提供（WeasyPrint 会自动 PingFang SC / 微软雅黑 / Noto Sans CJK SC 回退）。
如 WeasyPrint 启动报错缺少系统库（常见于 Linux 最小化环境），参考
[Troubleshooting](#troubleshooting)。

---

## 工作流

### Step 1：收集信息

如果用户已经直接给出简历正文/Markdown/旧 PDF/飞书文档，直接从里面提取信息填充 JSON，
**不要**反复追问。如果用户只说"帮我写简历"没有提供内容，按下面的清单引导（可以一次
问完，不要逐条追问）：

```
📋 请提供以下信息（可直接粘贴已有简历文本）：

1. 基本信息：姓名、目标职位、年龄/电话/邮箱/微信/博客（可选）
2. 个人概述：1-2 句话描述工作年限、当前岗位、核心方向、代表性能力（80-130字）
3. 技术栈：3-5 条核心能力（方向 + 熟练度 + 代表性技术/落地经验）
4. 工作经历（按时间倒序，每家公司）：
   - 公司/部门｜岗位｜时间·地点
   - 一段整体介绍（做什么、核心职责、整体 impact）
   - 代表性项目（2-4 个）：
     * 项目名、定位一句话
     * 子模块（如有）：架构/技术攻坚/协作沉淀
     * 每条 bullet 尽量带数字（DAU/准确率/性能提升/节省人天）
5. 教育经历：学校｜专业·学位｜时间·地点
6. 证书/荣誉：2-4 条
7. （可选）证件照：给一张 jpg，放到工作目录命名为 photo.jpg
```

引导时提醒用户：**优先给数字**——用户规模、性能提升、提效小时、下载量、业务覆盖数等。
没有精确数字的可以写"约 X"，但不要编造。

### Step 2：写入 JSON 数据文件

按 `references/data-schema.md` 的结构把收集到的信息整理成 `resume.json`，
放在**当前工作目录**（也就是用户 cwd，或者你选的一个输出目录）。

写作要点（详见 `references/style-guide.md`）：

- **每条 bullet 末尾用 `**加粗**` 标出量化结果**，比如 `**准确率 73% → 92%（↑19pp）**`。
- **动词前置**：用"主导/设计/构建/落地/沉淀/抽象/攻克/推动"而不是"参与/协助/负责"。
- **公司 intro 一段**写清角色 + 两阶段路径 + 整体 impact 数字。
- **技术栈每条一行**：`**方向：**简述（70 字以内）`。
- **个人概述**不要把所有经历都堆进去，只写最硬的 1-2 个标签（年限/职级/核心方向/差异化能力）。
- 如果某项目需要强制换页到第二页，在项目对象里加 `"page_break": true`（典型用法：
  把最后一个非核心项目推到第二页，让第一页在主项目处自然收尾）。
- 加粗只给核心 scope/范式名/数字结果，不要给中间术语加粗，粗体占比控制在段落 30% 以内。

可以参考 `examples/resume_data_example.json` 作为模板起点（其中人物、公司、项目和指标均为
虚构数据；内容密度和加粗模式可直接对照）。

### Step 3：生成三种格式

```bash
# 基本用法——在 JSON 所在目录执行
python3 <SKILL_DIR>/scripts/generate_resume.py --data resume.json --outdir ./
# 带照片
python3 <SKILL_DIR>/scripts/generate_resume.py --data resume.json --outdir ./ --photo photo.jpg
# 只要 HTML/DOCX（本地无 WeasyPrint 时的 fallback）
python3 <SKILL_DIR>/scripts/generate_resume.py --data resume.json --outdir ./ --no-pdf
```

其中 `<SKILL_DIR>` 是本 Skill 所在目录。脚本会在 `--outdir` 下生成：

- `resume.html`（单文件 HTML，可浏览器打开打印为 PDF）
- `resume.docx`（可编辑 Word 文档）
- `resume.pdf`（WeasyPrint 渲染的最终投递版，**推荐使用**）

### Step 4：交付与迭代

1. 上传生成的 PDF（必要时同时上传 DOCX/HTML）给用户，用一句话说明生成了什么。
2. 如果用户说"太长/超 2 页"——**删内容**，不要调字号。优先删：
   - 第二家公司以后非核心项目的 bullet
   - 技术栈里最弱的一条
   - 证书里含金量最低的一条
3. 如果用户说"某部分要加粗/要改掉/要新增"——直接改 JSON 对应字段，重跑脚本。
4. 如果用户想要 DOCX 里手动微调——交付 DOCX，告知其样式已锁定，建议只改内容不改字号颜色。

---

## JSON 结构速查

完整字段说明见 `references/data-schema.md`，这里给出最小可用骨架：

```json
{
  "name": "张三",
  "title": "高级前端工程师",
  "contact": {"age":"28","phone":"138…","email":"z@x.com","wechat":"zs","blog":"https://..."},
  "summary": "N 年前端经验，现任……核心方向……。",
  "skills": [
    {"label":"前端工程化：","desc":"精通 React/TypeScript/……"},
    {"label":"……：","desc":"……"}
  ],
  "experience": [
    {
      "company": "X 公司 · Y 部门",
      "role": "高级前端工程师",
      "meta": "2022.01 – 至今 · 北京",
      "intro": "担任……，**主导构建……**，整体 **impact 数字**。",
      "projects": [
        {
          "title": "项目名",
          "intro": "定位、痛点、范式、adoption 数字。支持 **bold**。",
          "sections": [
            {"subtitle": "架构设计", "bullets": ["bullet 1……**数字**。","bullet 2……**数字**。"]},
            {"subtitle": "核心攻坚", "bullets": ["……"]}
          ]
        }
      ]
    }
  ],
  "education": [{"school":"XX 大学","degree":"计算机 本科","meta":"2016.09 – 2020.06 · 北京"}],
  "certs": [{"text":"证书 1","bold":true},"证书 2","证书 3"]
}
```

所有文本字段支持 `**加粗**` Markdown 语法，会被渲染成深黑色粗体。

---

## Troubleshooting

**WeasyPrint 报错缺少 lib 库**（Ubuntu/Debian 常见）：
```bash
sudo apt-get install -y libpango-1.0-0 libpangoft2-1.0-0 libharfbuzz0b libffi-dev
python3 -m pip install --user --break-system-packages weasyprint
```

**PDF 里中文字体显示异常（方框/乱码）**：
- 安装 Noto Sans CJK：`sudo apt-get install fonts-noto-cjk`（Linux）或确认系统有 PingFang SC/微软雅黑。
- macOS 自带 PingFang SC；Windows 自带微软雅黑；大多数 Linux 服务器装 fonts-noto-cjk 即可。

**PDF 变成 3 页**：
- 不要改 CSS/字号！删内容。优先级：弱 bullet → 弱技能 → 弱证书 → 收紧公司 intro。
- 检查是否有项目可以加 `"page_break": true` 让第一页更饱满地收在主项目末尾。

**DOCX 打开后布局和 PDF 不完全一致**：
- Word 渲染引擎差异属正常现象，PDF 是最终投递版，DOCX 用于二次编辑。
- 如对 DOCX 布局一致性有强需求，以 HTML 浏览器打印为 PDF 作为备用方案。

**照片显示不出**：
- 确认传给 `--photo` 的路径存在，且图片为 JPG/PNG。建议 35×45mm 证件照比例，宽高比 0.74 左右。

---

## 不要做的事

- ❌ 不要自己写 HTML/CSS/PDF 生成代码——统一用 `scripts/generate_resume.py`。
- ❌ 不要修改 `scripts/generate_resume.py` 里的颜色/字号/间距去迎合某个用户的审美偏好；
      模板是产品化的，调一个坏一个。
- ❌ 不要把简历写成段落散文——一律用 bullet + 数字。
- ❌ 不要编造指标，没有数字就问用户，用户也没有就写"约"或干脆不写。
- ❌ 不要生成英文简历——本模板专为中文技术简历优化。
- ❌ 不要生成超过 2 页的 PDF。
- ❌ 不要用 emoji、网络用语、第一人称"我"。

---

## 参考文件

- `references/data-schema.md` — JSON 字段完整说明
- `references/style-guide.md` — 视觉系统与写作规范（加粗规则、数字指标、动词清单）
- `examples/resume_data_example.json` — 完整可跑的样例数据（高级全栈/AI Agent 方向）
- `scripts/generate_resume.py` — 三格式生成脚本（维护者可改，使用者不要改）
