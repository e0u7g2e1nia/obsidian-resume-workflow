# Obsidian Resume Workflow

一个基于 Obsidian 的求职投递与定制简历工作流，用于把分散的 JD、投递记录、简历素材、AI 改写规则、LaTeX/PDF 输出串成一个可追踪、可复用、可迭代的系统。

## 核心目标

- 用 Web Clipper 快速剪藏岗位 JD。
- 用 YAML 和 Dataview 管理投递状态、岗位方向、优先级和对应简历。
- 用结构化素材库沉淀个人经历，而不是每次从零写简历。
- 用 Codex Skill 根据 JD 筛选素材、合并能力点、生成一页中文简历。
- 用 LaTeX 模板把 Markdown 简历转成可投递的一页 PDF。

## 主要流程

### 1. 剪藏 JD

使用 Obsidian Web Clipper 模板保存岗位网页，生成一篇投递记录。

输出位置示例：

```text
01实习/投递记录/YYYY-MM-DD｜投递记录｜待整理｜网页标题.md
```

投递记录包含：

- YAML 字段：公司、岗位、状态、方向、日期、优先级、简历链接、来源、渠道、城市等。
- 原始 JD。
- 自动整理区。
- 投递记录、简历策略和后续进展。

相关文件：

- `templates/web-clipper/job-application-web-clipper-template.json`
- `templates/web-clipper/README.md`

### 2. 清洗投递记录

调用 `job-application-processor` Skill，整理最新剪藏记录：

- 识别公司、岗位、城市、渠道、方向、优先级。
- 清洗原始 JD，拆成职位描述、职位要求、加分项、投递方式。
- 更新 YAML，使 Dataview 可以读取。
- 按统一规则重命名文件。
- 生成 3-5 条简历策略。

相关文件：

- `skills/job-application-processor/SKILL.md`
- `docs/naming-and-yaml-rules.md`

### 3. 汇总投递状态

Dataview 汇总页不存数据，只读取每篇投递记录的 YAML。

可查看：

- 投递总表。
- 按状态分组。
- 公司、岗位、方向、日期、优先级、渠道、城市、关联简历。

相关文件：

- `templates/dataview/job-application-dashboard.md`

### 4. 生成定制简历

调用 `resume-bullet-tailor` Skill，根据 JD 从简历素材库中筛选内容。

核心原则：

- 简历素材库是原材料，不是最终稿。
- 先判断目标 JD 需要证明什么能力。
- 把碎片内容按“证明同一能力”合并成 bullet。
- 与 JD 高相关的能力完整保留，部分相关压缩，不相关删除。
- 少用项目内部术语，多用外部可理解的表达和数据。
- Markdown 简历是内容母版，LaTeX/PDF 是排版输出。

相关文件：

- `skills/resume-bullet-tailor/SKILL.md`

### 5. 输出 LaTeX/PDF

用 LaTeX 模板生成一页 PDF 简历。

模板特点：

- 教育背景固定多行结构。
- 项目标题采用三段式：左侧项目名称与角色，中间工具标签，右侧时间。
- 工具标签使用浅蓝色块，类似 `985`、`双一流` 标签。
- 支持证件照、分区横线、项目简介、bullet points、个人总结。

相关文件：

- `templates/latex/chinese-one-page-resume-template.tex`

## 文件命名规则

统一格式：

```text
YYYY-MM-DD｜类型｜公司｜岗位｜可选版本.md
```

常见类型：

- `投递记录`
- `简历输出`

详细规则见：

- `docs/naming-and-yaml-rules.md`

## 不建议同步的内容

以下内容通常包含隐私，不建议上传公开仓库：

- 真实简历素材库。
- 真实简历输出。
- 真实投递记录。
- 证件照、PDF、联系方式。
- 公司投递邮箱、内推信息、私人链接。

本仓库建议只保存可复用的模板、规则和 Skill。
