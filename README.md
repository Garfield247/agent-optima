# Agent Optima (⚡ AI Agent 架构优化与多生态生产工程手册)

> 🚀 专为解决大语言模型 Agent 研发痛点而生：**终结长会话上下文腐化、杜绝思维混乱与幻觉造假、实现 Token 极致能效与跨平台多 Agent 生产落地**。

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Ecosystem](https://img.shields.io/badge/Agents-Gemini%20|%20Claude%20|%20Codex%20|%20Cursor-orange.svg)](#)

---

## 📖 为什么需要 Agent Optima？

随着 LLM 编程助手（Antigravity/agy、Claude Code、OpenAI Codex、Cursor 等）深入核心微服务与大型遗留系统开发，工程师普遍遭遇三大深水区痛点：

1. **上下文腐化 (Context Rot) 与失忆**：长会话进行到中后期，Agent 出现严重的“前言不搭后语”、遗忘早期核心设计原则、逻辑相互打架；
2. **凭感觉写代码与幻觉造假 (Hallucination)**：Agent 盲目假设任务成功，闭门造车杜撰不存在的 API 或自圆其说；
3. **Token 账单爆炸与延迟恶化 (Token Waste)**：无脑裸读整个大文件、全量运行输出数千行无用日志、聊天框大段复述方案文档，导致单会话边际成本指数级攀升。

`Agent Optima` 汇聚了业界一线全栈架构师的实战经验，从**底层机理推导**到**四大主流生态落地**，建立了一套工业级的防御与治理体系。

---

## 🏛️ 核心手册体系 (Handbook Modules)

每篇指南严格遵循统一的四步工程叙事基调：  
`现象诊断与痛点表征 ➔ 机理剖析与根因解构 ➔ 架构防御与治理策略 ➔ 多生态跨端落地实现`

| 模块序号 | 指南文档 | 核心聚焦与解决的问题 |
| :--- | :--- | :--- |
| **01** | [**长会话抗腐化与思维防混乱指南**](./01-context-architecture-and-anti-rot.md) | 外部状态强制落盘、压缩重锚机制、海量脏数据 Subagent 隔离、单一数据源 (SSOT) 与章节锚点自愈。 |
| **02** | [**Token 极致能效与零损耗降本手册**](./02-token-efficiency-and-cost-optimization.md) | 外科手术式行号切片、终端命令管道限流、工件零复述、Prompt Caching 缓存最大化与 Epic 会话生命周期管理。 |

---

## 🌐 多 Agent 生态实装适配矩阵 (Adapters)

本项目不仅提供理论与架构，更提供针对当前一线主流 AI Agent 平台的即插即用配置文件：

- 🔷 [**Google Gemini / agy (Antigravity)**](./adapters/gemini/README.md) - `GEMINI.md` 宪法级红线、`.agents/PROJECT_MAP.md` 元指针与 Typora/Chrome 自动唤起；
- 🟣 [**Anthropic Claude Code**](./adapters/claude/README.md) - `CLAUDE.md` 规则注入、Subagent 派生隔离参数与行号切片约束；
- 🟢 [**OpenAI Codex / Operator**](./adapters/codex/README.md) - System Instructions 结构化约束与 Stateful Sandbox 防幻觉断言；
- ⚡ [**Cursor / Windsurf**](./adapters/cursor/README.md) - `.cursor/rules/*.mdc` 现代规则规范、Globs 按需激活与 Composer 上下文控制。

---

## 📜 开源协议

本项目基于 [Apache 2.0 License](./LICENSE) 开源。
