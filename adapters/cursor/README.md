# Cursor / Windsurf 工业级生产适配器

> 专为 **Cursor (Composer / Agent Mode)** 与 **Windsurf (Cascade)** 打造的工业级生产规则规范（`.cursor/rules/*.mdc` 体系）。

---

## 1. 物理配置文件位置与发现机制

不同 IDE 的规则加载体系存在差异：
- **Cursor (Composer / Agent)**：采用 `.cursor/rules/*.mdc` 目录体系（支持 YAML frontmatter 与 glob 匹配）；
- **Windsurf (Cascade)**：采用项目根目录的 `.windsurfrules`（纯 Markdown 格式）；
- **项目轻量元指针地图**：`.agents/PROJECT_MAP.md`。

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
  - 严禁盲目声称任务完成。在声称任务成功前，必须在终端执行真实测试或编译（`go test`、`pytest`、`tsc --noEmit`）。
  - **覆盖率与噪声平衡**：开发迭代阶段跑定向测试以实现秒级反馈；在交付提交前，必须运行影响范围的完整回归测试。严禁为了省 Token 阉割全量回归测试！通过重定向或管道截流日志。
  - 没有终端客观证据（测试真实执行且退出码为 0），严禁宣称完成。
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
