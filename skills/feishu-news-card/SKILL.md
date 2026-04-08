---
name: feishu-news-card
description: 发送飞书卡片消息，用于新闻推送、通知提醒、数据报告等场景。Make sure to use this skill whenever the user mentions 推送新闻、发飞书消息、发送通知卡片、发飞书通知、拉取飞书消息、飞书卡片、Feishu card、Lark card，或任何与飞书/飞书消息推送相关的场景 — even if they don't explicitly say "skill"。
---

# Feishu News Card

## 步骤

1. 构建卡片 JSON（见下方两种路径）
2. 用 Write 工具写入临时文件
3. 发送

**Mac/Linux** — 写入 `/tmp/feishu_card.json`：

```bash
openclaw message send --channel feishu --account main \
  --card "$(cat /tmp/feishu_card.json)" \
  --target '<target>'
```

**Windows** — 写入 `%TEMP%\feishu_card.json`：

```powershell
openclaw message send --channel feishu --account main `
  --card (Get-Content -Raw "$env:TEMP\feishu_card.json") `
  --target '<target>'
```

---

## 路径一：固定模板（新闻/通知推送）

直接使用此模板，按需填入内容，`hr` 分隔每条条目：

```json
{
  "config": { "enable_forward": true },
  "header": {
    "title": { "tag": "plain_text", "content": "卡片标题" },
    "template": "blue"
  },
  "elements": [
    {
      "tag": "markdown",
      "content": "**1. 新闻标题**\n摘要内容\n来源：[来源名](链接)"
    },
    { "tag": "hr" },
    {
      "tag": "markdown",
      "content": "**2. 新闻标题**\n摘要内容\n来源：[来源名](链接)"
    }
  ]
}
```

颜色（`template` 字段）：

| 场景     | 值     |
| -------- | ------ |
| 常规新闻 | blue   |
| 紧急告警 | red    |
| 成功报告 | green  |
| 警告提醒 | yellow |

---

## 路径二：自定义模板

需要按钮、多列、图片等复杂布局时，读取 `references/card-schema.md` 了解完整 element 类型和字段说明，再自行构建 JSON。

无论自定义成何种结构，最终 JSON **必须**满足：

```
{
  "config": { ... },          // 必须
  "header": {
    "title": { "tag": "plain_text", "content": "..." },  // 必须
    "template": "<颜色>"      // 必须
  },
  "elements": [ ... ]         // 必须，至少一个元素
}
```

---

## target 格式

- 个人：`user:ou_xxx`
- 群组：`openchat_id:oc_xxx`
