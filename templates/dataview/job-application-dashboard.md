# JD投递汇总

> 这是自动汇总页。每个岗位的信息来自 `[[01实习/投递记录]]` 下单独的投递记录笔记。Dataview 不存数据，只读取每篇记录的 YAML。

## 投递总表

```dataview
TABLE WITHOUT ID
  file.link AS 记录,
  company AS 公司,
  position AS 岗位,
  status AS 状态,
  direction AS 方向,
  apply_date AS 日期,
  priority AS 优先级,
  resume AS 简历,
  channel AS 渠道,
  city AS 城市
FROM "01实习/投递记录"
WHERE type = "投递记录"
SORT apply_date DESC, file.name ASC
```

## 按状态查看

```dataview
TABLE WITHOUT ID
  file.link AS 记录,
  company AS 公司,
  position AS 岗位,
  apply_date AS 日期,
  priority AS 优先级,
  resume AS 简历
FROM "01实习/投递记录"
WHERE type = "投递记录"
GROUP BY status AS 状态
SORT 状态 ASC
```

## 状态说明

- 待分析：刚看到岗位，还没判断是否适合。
- 待生成简历：已经决定投，还没生成/修改简历。
- 已生成简历：已生成定制简历，还没投出或待确认。
- 已投递：已经投出。
- 笔试/测评：进入笔试或测评环节。
- 面试中：进入面试流程。
- 已拒/无回应：暂时无后续。
- Offer/完成：拿到结果或流程结束。

## 投递记录 YAML 模板

```yaml
---
type: 投递记录
company: 公司名
position: 岗位名
status: 待分析
direction: AI产品 / 内容产品 / AI内容运营
apply_date: 2026-06-09
priority: 高 / 中 / 低
resume: 
source: 
channel: 
city: 
---
```

## 工作流

1. 用 Web Clipper 或手动方式为每个岗位创建一篇 `投递记录`。
2. 在投递记录 YAML 里填写公司、岗位、状态、方向、日期、优先级、简历链接。
3. 从 `[[01实习/简历素材库]]` 生成定制简历，保存到 `[[01实习/简历输出]]`。
4. 在投递记录里补充投递渠道、账号、后续进展。
5. 本页会通过 Dataview 自动汇总。

## 命名规则

详见 [[01实习/简历命名与投递记录规范]]。
