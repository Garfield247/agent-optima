# Anthropic Claude Code 工业级生产适配器

> 专为 Anthropic 官方命令行工具 **Claude Code** 打造的工业级生产配置手册。全面适配 Claude 3.5 Sonnet / Opus 的长上下文推理特性与 Tool Calling 机制。

---

## 1. 物理配置文件位置

- **全局用户级配置 (Global Defaults)**：`~/.claude/CLAUDE.md`（所有工程自动继承）
- **项目级覆盖规则 (Project Root)**：`${PROJECT_ROOT}/CLAUDE.md`
- **项目元指针索引地图**：`${PROJECT_ROOT}/.agents/PROJECT_MAP.md`
- **计划状态落盘路径**：`${PROJECT_ROOT}/.claude/plan.md`（需加入 `.gitignore` 避免污染代码库）

---

## 2. 生产级配置代码 (`CLAUDE.md` 全文)

将以下配置直接置入你的 `${PROJECT_ROOT}/CLAUDE.md` 或 `~/.claude/CLAUDE.md` 中：

```markdown
# Anthropic Claude Code Engineering Invariants & Efficiency Protocols

## 1. Prime Directive & Verification Evidence Gate
- **No Done Without Evidence**: Never claim a task or bugfix is completed without executing terminal verification. You MUST run the project compiler or automated test suite and inspect stdout/stderr. If exit code != 0, it is NOT done.
- **Zero Blast Radius**: Restrict changes strictly to files directly involved in the current objective. Do not reformat unrelated files or refactor unprompted code.
- **Root Cause First**: When fixing bugs, diagnose the mechanism first before generating modifications. Never blindly guess or retry repeatedly in the dark.

## 2. State Externalization Protocol (Task FSM)
- **Plan Gate**: For tasks exceeding 2 steps or spanning multiple packages, your FIRST action must be writing `.claude/plan.md` using the file write tool.
- **Task Schema**: Each subtask MUST include a concrete automated verification command:
  ```markdown
  - [ ] **Task 1: Define domain types & FSM transitions**
    - Verification: `go test -v -run TestOrderFSM ./internal/domain/...`
  ```
- **Atomic Progression**: Only after a verification command exits with 0, update the plan file by marking the checkbox from `[ ]` to `[x]`.

## 3. Compaction Rehydration SOP
- When context compaction occurs or history is truncated, NEVER hallucinate progress based on natural language summaries.
- **Execute Mandatory Rehydration Tool Sequence**:
  1. `view_file(.claude/plan.md)`: Identify the first pending `[ ]` subtask.
  2. `bash(git status -s)`: Inspect true physical workspace changes.
  3. `bash(<targeted_test_command>)`: Re-establish baseline facts before writing any code.

## 4. Subagent Hygiene & Heavy Data Isolation
- Delegate broad directory explorations, massive file greps, or verbose build/test runs to subagents.
- The subagent must distill findings into <= 20 lines (Root Cause -> File:Lines Pointer -> Next Action) before returning to the main session. Never dump voluminous raw logs into the primary context.

## 5. Semantic Slicing & Token Optimization
- **Semantic Units First**: Never blindly ingest large files. Pinpoint target symbols with grep first, then read full semantic blocks (functions, classes, transaction boundaries) rather than arbitrary line cuts. Small files (<150 lines) can be read completely.
- **Self-Healing Pointers**: Use `[file#anchor](path) (est Lxx-Lyy)`. Check header anchor upon slice read; if shifted, grep anchor to realign.

## 6. CLI Command Throttling & Output Redirection
- **Coverage vs Noise**: Run targeted unit tests during rapid development (`go test -run TestTarget`); run full regression tests before final completion. Never sacrifice regression coverage!
- **Redirect Noisy Test Output**: Redirect full test suite stdout to temporary files, reading only the exit code and failure summary:
  ```bash
  go test ./... > /tmp/test.log 2>&1 || (tail -n 30 /tmp/test.log && exit 1)
  ```
- Command throttling:
  - Git log: `git log` ❌ -> `git log -n 5 --oneline` ✅
  - Directory: `find .` / `tree` ❌ -> `tree -L 2 -I 'node_modules|vendor|.git'` ✅
  - Package install: `npm install` ❌ -> `npm i --silent` ✅

## 7. Zero Artifact Echoing & Strict Response Template
- When creating or modifying artifacts (markdown plans, research docs, ADRs), NEVER print the markdown content in chat.
- Your final text response MUST strictly match this template:
  ```text
  👉 Artifact updated: [<filename>](file:///<abs_path>)
  Local preview triggered. Review the following core decisions:
  1. [Key architectural impact / decision]
  2. [Blocker or confirmation needed]
  ```
- Trigger macOS preview proactively: `open -a Typora <path> || open -a "Google Chrome" <path>`.

## 8. Cognitive Triage Router
- **L0 (Ephemeral Tools/Scratch)**: Regex writing, quick JSON transformation, intermediate debug traces must be solved and burned immediately. No verbose history retention.
- **L1 (Tech Stack Deep-Dives / Comparisons)**: NEVER explain theoretical comparisons in the main chat. Write directly to `docs/research/<topic>.md` and return only 3 bullet conclusions.
- **L2 (Admin Deliverables / Weekly Reports)**: Write directly to `reports/<name>.md`, trigger local preview, and adhere strictly to Zero Echoing.
- **Spontaneous Ideas**: When out-of-scope ideas arise, append 1 sentence to `.agents/BACKLOG.md` and immediately refocus on the current task.
```
