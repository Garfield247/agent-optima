# Cursor / Windsurf 工业级生产适配器

> 专为 **Cursor (Composer / Agent Mode)** 与 **Windsurf (Cascade)** 打造的工业级生产规则规范（`.cursor/rules/*.mdc` 体系）。

---

## 1. 物理配置文件位置

Cursor 采用基于文件前缀和 Glob 模式的 `.cursor/rules/` 架构：
- `.cursor/rules/00-architecture-invariants.mdc`: 最高特权级工程宪法与验证硬门禁（`alwaysApply: true`）
- `.cursor/rules/01-token-optima.mdc`: Token 能效、切片调阅与命令限流规则（`alwaysApply: true`）
- `.cursor/rules/02-cognitive-triage.mdc`: 意图分流与阅后即焚路由器（`alwaysApply: true`）
- 项目元指针地图：`.agents/PROJECT_MAP.md`

---

## 2. 生产级规则代码 (直接复制实装)

### 2.1 `00-architecture-invariants.mdc` (核心宪法与验证门禁)

```yaml
---
description: 核心工程宪法、物理编译验证硬门禁与状态落盘协议
globs: ["*"]
alwaysApply: true
---

# Cursor Core Architecture Invariants

- **完成前铁证验证门禁 (Evidence Gate)**：
  - 严禁盲目声称任务完成。在提交修改前，必须在终端执行真实编译或自动化测试（`go build`、`pytest -q`、`tsc --noEmit`）。
  - 没有终端绿灯客观证据，严禁在回答中说“已经成功实现”。退出码非 0 视为未完成。
- **复杂任务强制落盘 (State Externalization)**：
  - 任务预计超过 2 个步骤时，必须先在本地写入任务清单（如 `.cursor/plan.md` 或专属工件路径）。
  - 每个任务项必须包含明确的自动化检验命令，验证通过后打勾。
- **会话截断逆向重锚**：
  - 一旦上下文被截断，严禁凭模糊记忆推断后续代码。第一步读取计划文件对齐物理进度，第二步执行 `git status` 确认真实变动。
```

### 2.2 `01-token-optima.mdc` (Token 能效与切片调阅)

```yaml
---
description: Token 能效极致优化、外科手术切片调阅与终端强制限流
globs: ["*"]
alwaysApply: true
---

# Cursor Token Optimization Invariants

- **外科手术式切片调阅 (Surgical Slicing)**：
  - 严禁全量裸读超过 100 行的文件。必须先通过代码搜索定位目标符号行号 $L$，再按范围读取 `[L-15, L+35]`，视窗严格限制在 30~80 行。
- **终端强制限流矩阵 (CLI Throttling)**：
  - Git 日志必须带条数限制：`git log -n 5 --oneline`；
  - 目录查看必须限制层级：`tree -L 2`；
  - 运行单测必须指定目标测试用例，严禁全量测试导致海量日志滚屏。
- **零工件复述 (Zero Artifact Echoing)**：
  - 将方案或文档写入文件后，严禁在聊天对话框中把正文重新敲一遍。仅输出指针链接与 1~2 个关键决策点，并在 macOS 下调用 `open -a Typora <path>` 自动弹窗。
```

### 2.3 `02-cognitive-triage.mdc` (认知意图分流与阅后即焚)

```yaml
---
description: 多维意图拦截、非代码任务分流与阅后即焚路由器
globs: ["*"]
alwaysApply: true
---

# Cursor Cognitive Triage Router

- **L0 (瞬态工具/排错)**：临时正则、格式转换、排错过程日志在沙盒内消化，单轮闭环即焚，不向主会话堆积过程垃圾。
- **L1 (技术栈科普/深度对比)**：严禁在主聊天流长篇大论！字数预估 >300 字的技术理论调研，强制写入 `docs/research/<topic>.md`，会话中仅返回 3 句结论。
- **L2 (行政交付/周报)**：周报、发布说明直接写入 `reports/` 目录并触发本地弹窗预览，会话内不打印正文。
- **发散灵感暂存**：用户临时冒出的非当前任务架构想法，提炼 1~2 句话追加存入 `.agents/BACKLOG.md`，主干保持绝对聚焦。
```
