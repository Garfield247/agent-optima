# OpenAI Codex / Operator 工业级生产适配器

> 专为 OpenAI Codex / Operator / Assistant 体系打造的工业级生产配置规范。全面适配 GPT-4o / o1 / o3 架构的长会话注意力机制与工具调用状态机。

---

## 1. 物理配置文件位置

- **全局用户级配置**：`~/.openai/AGENTS.md`
- **项目级规则文件**：`${PROJECT_ROOT}/AGENTS.md` 或 `.openai/rules.md`
- **项目元指针索引地图**：`${PROJECT_ROOT}/.agents/PROJECT_MAP.md`
- **任务状态机持久化文件**：`${PROJECT_ROOT}/.openai/task_state.json` 或 `implementation_plan.md`

---

## 2. 生产级配置代码 (`AGENTS.md` 全文)

将以下规范置入工程根目录的 `AGENTS.md` 中：

```markdown
# OpenAI Agent Operational Contract & Resource Efficiency Invariants

## 1. Verification Evidence Gate (Non-Negotiable)
- Execution cannot transition to COMPLETED without zero-exit-code evidence from the native project compiler or test runner.
- Never output speculative success statements ("Everything has been successfully implemented"). You MUST inspect terminal output lines for verification proof.
- If a compilation or test fails, do NOT blindly tweak the same broken code. Perform root cause analysis first.

## 2. State Externalization & Atomic Transitions
- For tasks requiring >2 distinct steps, initialize an external state tracker before generating code changes (`task_state.json` or `implementation_plan.md`).
- Each task unit must define:
  1. `Target Files`: Explicit file paths.
  2. `Verification Command`: Command line string yielding 0 on success.
- Progress updates must be synchronized to disk immediately after each step is verified.

## 3. Compaction Rehydration Protocol
- When token limits trigger automatic context truncation:
  - Step 1: Read the state tracker file to locate the exact in-progress step.
  - Step 2: Run `git status -s` to verify modified files on disk.
  - Step 3: Run the last passing test command to re-establish environmental facts.

## 4. Subagent Sandbox & Log Isolation
- Execute wide file system traverses or noisy test runs in an isolated subagent runtime.
- The subagent must return a strictly capped summary (<= 20 lines) containing:
  - Root cause / finding
  - Pointers with line numbers (`file.py:L20-L45`)
  - Proposed atomic modification
- Raw stdout from multi-package test runs must NEVER be ingested directly into the primary context.

## 5. Surgical Slicing Protocol
- Reading files >100 lines requires two-phase slicing:
  - Phase 1: Call search/grep tool to locate target identifier line $L$.
  - Phase 2: Call file read with `[L - 15, L + 35]` boundaries (<= 80 lines).
- Never ingest whole modules to inspect single function signatures.

## 6. CLI Execution Throttling
Always apply noise-filtering arguments to shell tool calls:
- Git: `git log -n 5 --oneline`, `git diff --stat`
- File tree: `tree -L 2 -I 'vendor|node_modules|.git'`
- Unit tests: Target single test functions (`go test -run`, `pytest -k`) instead of full package scans.

## 7. Zero Artifact Echoing & Strict Response Template
- Do NOT output file contents in the conversational stream after writing files to disk.
- Response format is strictly capped to:
  ```text
  👉 Artifact persisted: [<filename>](file:///<abs_path>)
  Local preview initiated. Review these key decision points:
  1. [Decision point / Impact]
  2. [Blocker / Verification question]
  ```
- Proactively call local preview on macOS: `open -a Typora <path> || open -a "Google Chrome" <path>`.

## 8. Cognitive Triage & Burn-After-Reading Router
- **L0 (Transient / Scratchpad)**: Format conversions and debugging stacktraces are solved and discarded immediately.
- **L1 (Architectural Deep-Dives)**: Theoretical technology comparisons must be written directly to `docs/research/<topic>.md`. Only return 3-bullet executive takeaways in conversation.
- **L2 (Administrative Reports)**: Weekly reports and release notes must be written to `reports/` with Zero Echoing applied.
- **Spontaneous Ideas**: Divergent ideas must be appended to `.agents/BACKLOG.md` without derailing current implementation focus.
```
