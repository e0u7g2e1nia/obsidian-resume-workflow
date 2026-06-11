# Web Clipper 投递记录模板导入说明

## 文件

模板文件：[[01实习/Web Clipper配置/投递记录-web-clipper-template.json]]

## 导入 / 更新步骤

1. 浏览器打开 Obsidian Web Clipper 扩展。
2. 点击设置齿轮。
3. 进入 Templates。
4. 如果已经有旧的 `投递记录` 模板，建议先删除或改名。
5. 点击 Import，选择本文件：`01实习/Web Clipper配置/投递记录-web-clipper-template.json`。
6. 导入后确认模板名称为：`投递记录`。

## 重要：自动整理区需要 Interpreter

模板里的 `职位描述`、`职位要求`、`加分项`、`简历匹配提示` 使用了 Web Clipper 的 prompt variables。

如果你没有在 Web Clipper 里启用 Interpreter，这些区域可能不会自动生成，只会保留变量文本或为空。此时仍然可以正常使用 `原始JD` 区域。

## 图片规则

模板会自动记录：

- `source_image: {{image}}`：网页社交分享图。
- `页面图片`：页面中图片的远程链接。
- `![分享图]({{image}})`：在正文中嵌入分享图。

注意：这些默认是远程图片链接，不是本地附件。如果以后需要离线保存图片，可以再让 AI 执行“下载投递记录中的远程图片并替换为 Obsidian wiki-link”的后处理。

## 使用方式

1. 打开招聘 JD 网页。
2. 点击 Web Clipper。
3. 选择模板：`投递记录`。
4. 保存到 Obsidian。
5. 回到 Obsidian，将新文件名从：

```text
YYYY-MM-DD｜投递记录｜待整理｜网页标题.md
```

改成：

```text
YYYY-MM-DD｜投递记录｜公司｜岗位.md
```

6. 补充 YAML：`company`、`position`、`direction`、`priority`、`city`、`channel`。
7. `[[01实习/JD投递汇总]]` 会自动读取。
