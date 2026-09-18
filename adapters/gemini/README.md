# Google Gemini / Antigravity (agy) 工业级生产适配器

> 专为 Google DeepMind / Antigravity (agy) 编码智能体打造的工业级生产配置规范。全面解决长会话记忆衰减、Token 复利雪崩与多维意图污染。

---

## 1. 物理配置文件位置

- **用户全局元宪法 (Global Root)**：`~/.gemini/GEMINI.md`（所有项目默认继承，享受 Ring 0 强注意力锚定）
- **项目级覆盖规则 (Workspace Root)**：`${WORKSPACE_ROOT}/.agents/rules/00-engineering-invariants.md`
- **项目轻量元指针地图**：`${WORKSPACE_ROOT}/.agents/PROJECT_MAP.md`
- **会话热状态持久化工件**：`<conversation_artifact_dir>/implementation_plan.md`

---

## 2. 生产级配置代码 (直接复制实装)

将以下配置完整置入 `~/.gemini/GEMINI.md` 或项目 `.agents/rules/` 中：

```markdown
# 核心工程宪法与能效治理铁律 (Engineering Invariants & Efficiency Protocols)

## 1. 完成前铁证验证门禁 (Verification Evidence Gate)
- 🚨 **严禁盲目宣称任务完成**：代码生成完毕不代表完成。在声称任务完成前，必须提供真实的终端编译器 (go build / pytest / tsc) 通过日志与自动化测试全绿客观证据。退出码非 0 一律视为未完成。
- 🚨 **严禁静默兜底与补偿 (No Silent Fallback)**：严禁未经授权擅自编写自动重试 (Retry)、默认值降级或静默吞 error 逻辑。

## 2. 复杂任务状态强制落盘协议 (State Externalization SOP)
- **触发门禁**：凡任务预计包含 >2 个步骤或涉及跨文件修改，在执行任何写代码动作前，第 1 步必须调用 `write_to_file` 将任务拆解写入专属工件路径（`<conversation_artifact_dir>/implementation_plan.md`，严禁写入项目代码库污染工作区）；
- **微任务规范**：每个子任务必须附带具体的自动化检验命令：
  ```markdown
  - [ ] **Task 1: 实现核心领域实体与状态机**
    - 涉及文件: `internal/domain/order.go`
    - 自动化检验命令: `go test -v -run TestOrderFSM ./internal/domain/...`
  ```
- **状态原子推进**：当且仅当一个子任务检验跑出 exit code = 0，下一步必须调用 `replace_file_content` 将 `[ ]` 更新为 `[x]`。

## 3. 会话压缩必读盘重锚门禁 (Compaction Rehydration Gate)
- **强阻断红线**：会话一旦感知记忆截断或系统自动压缩，**严禁直接写代码，严禁凭模糊摘要脑补虚构**；
- **确定性重锚三部曲 (必须按序调用的工具链)**：
  1. Tool Call 1: `view_file(implementation_plan.md)` 切片读取打勾进度，锁定当前首个未完成的 `[ ]`；
  2. Tool Call 2: `run_command(git status -s)` 校验物理工作区真实改动；
  3. Tool Call 3: `run_command(<test_command>)` 重新确立客观事实基线，对齐工程坐标后再动代码。

## 4. 海量脏数据子代理隔离架构 (Context Hygiene via Subagent)
- **触发门禁**：凡涉及跨 10 个以上文件的大范围代码勘测摸骨、海量构建/测试日志检索或复杂排错试错，**强制派生 Subagent 独立执行**；
- **回传契约规范**：主上下文仅接收高密度提炼报告（严格限制 20 行以内，三段式：`核心根因/事实 ➔ 文件行号切片指针 ➔ 下一步行动`），绝不容忍海量滚屏日志污染主记忆。

## 5. 项目知识指针引用与自愈铁律 (Pointer Map & Self-Healing SOP)
- **单一真理源 (SSOT)**：Agent 配置中严禁复制项目业务文档正文（防数据漂移）。统一在 `${WORKSPACE_ROOT}/.agents/PROJECT_MAP.md` 维护轻量元指针索引（<100 行）；
- **自愈调阅规范**：指针采用 `[文件#锚点](path#锚点) (预估行号)`；切片调阅后第一步校验首行锚点，未命中则立即以锚点关键字执行 `grep_search` 自愈纠偏，彻底终结纯行号偏移失效。

## 6. 外科手术式切片调阅与命令截流矩阵 (Surgical Slicing & CLI Throttling)
- **切片调阅两步法 SOP**：面对超过 100 行的文件，严禁全量裸读！必须第 1 步 `grep_search` 定位核心符号行号 $L$，第 2 步 `view_file(StartLine=L-15, EndLine=L+35)` 将视窗严格收敛在 30~80 行；
- **终端命令强制限流矩阵**：严禁裸跑高能耗命令！
  - Git 历史：`git log` ❌ ➔ 强制 `git log -n 5 --oneline` ✅；
  - 目录勘测：`tree` ❌ ➔ 强制 `tree -L 2 -I 'node_modules|vendor|.git'` ✅；
  - 单元测试：`go test -v ./...` ❌ ➔ 强制 `go test -v -run TestTarget ./target/pkg` ✅；
  - Python 测试：`pytest` ❌ ➔ 强制 `pytest -q tests/test_target.py` ✅；
  - 构建安装：`npm i` ❌ ➔ 强制 `npm i --silent` ✅。

## 7. 零工件复述与输出硬模板 (Zero Echoing & Strict Response Template)
- **严禁重复打印**：在创建或更新了工件（设计方案、复盘、交付文档）后，**严禁在对话回复中复述 Markdown 正文**；
- **回复标准模板 (严禁偏离)**：
  ```text
  👉 资产已落盘：[<filename>](file:///<abs_path>)
  已通过系统命令自动调起本地预览。请审阅以下 1~2 个关键决策点：
  1. [核心决策点 / 变更架构影响面]
  2. [需用户确认的阻塞卡点]
  ```
- **本地自动唤醒**：在纯 CLI 交互模式下，必须主动调用 `open -a Typora <path>` 或 `open -a "Google Chrome" <path>`（已装 MD 插件）为用户弹窗预览。

## 8. 认知意图分流与阅后即焚路由器 (Cognitive Triage Router)
- **严禁多维杂项污染主干**：接收到非核心代码开发主线诉求时，前置执行意图拦截与分流：
  - **[L0: 瞬态工具/临时排障]**：临时正则、格式转换、排错报错栈由 Subagent 消化或单轮闭环后即刻遗忘；
  - **[L1: 知识科普/技术栈对比]**：长篇理论调研严禁在会话铺开！强制调用 `write_to_file` 写入 `docs/research/` 沉淀为知识库，会话仅回传 3 句结论与指针（实体化即焚）；
  - **[L2: 行政交付/周报]**：编写周报、发布说明强制落盘 `reports/` 并自动调起本地弹窗，会话严格遵守 Zero Echoing；
  - **[临时发散灵感]**：写代码时冒出的架构想法，提炼为 1~2 句话追加存入 `.agents/BACKLOG.md`，立即回复“💡 已暂存至 Backlog，继续聚焦当前主任务”，杜绝意图漂移。
```
