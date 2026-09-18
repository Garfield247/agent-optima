# OpenAI Codex / Operator 适配器

## 核心配置文件清单
- `system_instructions.md`: 自定义 GPTs / Operator 的 System Instructions；
- `instructions.json`: API 调用时的静态前置 Prompt。

## 核心特性
- 状态沙箱（Sandbox）内物理断言验证；
- 结构化 JSON 返回与工具限流；
- 杜绝聊天流复述长篇 Markdown。
