---
name: job-application-processor
description: Use when the user says “处理最新投递”, “整理最新投递记录”, “处理这条投递记录”, or asks to clean up a Web Clipper job application note in Obsidian. Finds or reads a 投递记录 note, extracts company/position/JD details, fixes YAML, renames the file according to the resume workflow, formats the JD, updates Dataview-compatible fields, and optionally generates a tailored resume.
---

# Job Application Processor

## Purpose

Automate the post-processing of job application notes clipped by Obsidian Web Clipper.

The user wants the workflow to be as simple as:

1. Clip a JD page with Web Clipper.
2. Say “处理最新投递” or “处理最新投递并生成简历”.
3. The assistant handles cleanup, naming, YAML, JD formatting, Dataview compatibility, and optionally resume generation.

## Source of Truth

- Resume material library: `01实习/简历素材库.md`
- Naming/workflow rules: `01实习/简历命名与投递记录规范.md`
- Application records folder: `01实习/投递记录/`
- Resume output folder: `01实习/简历输出/`
- Dataview summary: `01实习/JD投递汇总.md`

Read the naming/workflow rules before changing file names or YAML.

## Triggered Tasks

### If the user says “处理最新投递”

Process the latest clipped application record only. Do not generate a resume unless the user explicitly asks.

### If the user says “处理最新投递并生成简历”

Process the latest application record, then generate a JD-tailored one-page resume using `resume-bullet-tailor` principles, save it to `01实习/简历输出/`, and link it back in the record YAML.

### If the user references a specific record note

Process that note instead of searching for the latest one.

## Workflow: Process Latest Application Record

1. **Find target record**
   - If the user references a file, use that file.
   - Otherwise, find the most recently modified Markdown file under `01实习/投递记录/`.
   - Prefer files whose name contains `待整理` or whose YAML fields `company`, `direction`, or `city` are blank.

2. **Read and parse**
   - Read YAML and body.
   - Extract JD text from `## 原始JD`.
   - Extract source URL from YAML `source` or the body.

3. **Infer fields**
   - Infer `company`, `position`, `city`, `channel`, `direction`, and `priority` from title, JD text, URL, hashtags, and delivery method.
   - Keep uncertain fields blank rather than hallucinating.
   - If company is unclear but hashtags or email domain indicate it, infer conservatively.
   - For 小红书内推 posts, set `channel: 小红书内推` when applicable.

4. **Format JD body**
   - Clean the raw clipped text into readable sections:
     - `## 原始JD`
     - `### 职位描述`
     - `### 职位要求`
     - `### 加分项 / 特别偏好`
     - `### 投递方式`
   - Preserve the original source URL.
   - Remove noisy hashtag URLs unless they contain useful signals.
   - Keep the original meaning. Do not invent requirements.

5. **Update YAML**
   Required fields:
   ```yaml
   ---
   type: 投递记录
   company:
   position:
   status:
   direction:
   apply_date:
   priority:
   resume:
   source:
   channel:
   city:
   clipped_at:
   ---
   ```
   - If a resume has not been generated, keep `resume:` blank.
   - If only processed but not applied, keep or set `status: 待生成简历` unless the current status is more advanced.

6. **Rename file**
   Use:
   ```text
   YYYY-MM-DD｜投递记录｜公司｜岗位.md
   ```
   - Use `apply_date` as the date.
   - Remove `待整理` after fields are inferred.
   - Keep names concise but recognizable.
   - Avoid changing names if company or position is still too uncertain.

7. **Add/refresh strategy section**
   Add or update:
   ```md
   ## 简历策略
   ```
   Include 3-5 concise bullets about which resume materials to emphasize for this JD.

8. **Verify Dataview compatibility**
   - Confirm YAML has `type: 投递记录`.
   - Confirm the file is under `01实习/投递记录/`.
   - Confirm `apply_date` is a valid date.
   - Confirm no YAML syntax is broken by colons, pipes, or unquoted wikilinks.

## Optional Workflow: Generate Resume

If the user asks to generate a resume:

1. Read `01实习/简历素材库.md`.
2. Use `resume-bullet-tailor` principles.
3. Generate a one-page Chinese resume tailored to the JD.
4. Save as:
   ```text
   01实习/简历输出/YYYY-MM-DD｜简历输出｜公司｜岗位.md
   ```
5. Add YAML to the resume:
   ```yaml
   ---
   type: 简历输出
   company:
   position:
   direction:
   created_date:
   record: "[[01实习/投递记录/YYYY-MM-DD｜投递记录｜公司｜岗位]]"
   status: 可投递
   ---
   ```
6. Update the application record YAML `resume` field with a wikilink to the generated resume.
7. Add a line to `## 投递记录` noting resume generation.

## Output to User

After processing, respond briefly with:

- The processed record link.
- Fields inferred.
- Any fields still needing manual confirmation.
- If generated, the resume link.

Do not paste the full file content unless asked.

## Safety Rules

- Do not delete the source JD content.
- Do not overwrite advanced statuses such as `已投递`, `面试中`, or `Offer/完成` unless the user asks.
- Do not fabricate company, position, city, email, or deadlines.
- Do not imply the user has applied unless there is explicit evidence.
- If a filename collision exists, append `｜v2` rather than overwriting.
