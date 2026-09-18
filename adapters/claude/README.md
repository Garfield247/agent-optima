# Anthropic Claude Code 适配器

## 核心配置文件清单
- `CLAUDE.md`: 存放于项目根目录；
- `~/.claude/skills/`: 软链接或同步我们沉淀的通用 Skills。

## 核心特性
- 强制使用 Subagent 派生隔离大范围勘测；
- 限制 `view` 工具调用行号切片（避免 context 快速打满 200K）；
- 计划任务落盘至 `.claude/plan.md`。
