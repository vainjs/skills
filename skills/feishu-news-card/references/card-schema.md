# 飞书卡片 JSON v2 结构参考

官方文档：https://open.feishu.cn/document/feishu-cards/card-json-v2-structure?lang=zh-CN

---

## 顶层结构

```json
{
  "config": {
    "enable_forward": true,       // 允许转发
    "update_multi": false         // 是否更新多端（默认 false）
  },
  "header": {
    "title": { "tag": "plain_text", "content": "标题" },
    "template": "blue"            // 标题栏颜色
  },
  "elements": [ ... ]             // 卡片正文元素数组
}
```

---

## elements 支持的 tag 类型

### markdown — 富文本段落

```json
{
  "tag": "markdown",
  "content": "**加粗** _斜体_ [链接](https://...)\n换行用 \\n"
}
```

支持：`**bold**`、`_italic_`、`[text](url)`、`\n` 换行、`<at id=xxx></at>` @人

---

### hr — 水平分割线

```json
{ "tag": "hr" }
```

---

### div — 文本+附件行

```json
{
  "tag": "div",
  "text": { "tag": "lark_md", "content": "正文内容" },
  "extra": { ... }   // 可选：右侧附加元素（按钮、图片等）
}
```

---

### action — 按钮组

```json
{
  "tag": "action",
  "actions": [
    {
      "tag": "button",
      "text": { "tag": "plain_text", "content": "点击查看" },
      "type": "default",          // default / primary / danger
      "url": "https://..."        // 跳转链接
    }
  ]
}
```

---

### img — 图片

```json
{
  "tag": "img",
  "img_key": "img_xxx",          // 通过飞书上传接口获取
  "alt": { "tag": "plain_text", "content": "图片描述" }
}
```

---

### column_set — 多列布局

```json
{
  "tag": "column_set",
  "flex_mode": "none",           // none / stretch / flow / bisect / trisect
  "columns": [
    {
      "tag": "column",
      "width": "weighted",
      "weight": 1,
      "elements": [ ... ]        // 列内子元素
    }
  ]
}
```

---

### note — 底部注释

```json
{
  "tag": "note",
  "elements": [
    { "tag": "plain_text", "content": "更新时间：2026-03-30" }
  ]
}
```

---

## header template 颜色值

`blue` / `red` / `green` / `yellow` / `purple` / `grey` / `orange` / `indigo` / `wathet` / `turquoise` / `lime` / `pink` / `carmine`
