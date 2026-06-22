---
name: aware-code
description: "Use when writing or modifying code. Scans project context and conventions before changes, learns from errors, reviews quality (correctness, performance, security, tests), syncs docs, and outputs structured summaries. Do NOT use for purely conversational tasks or quick drafts where review is explicitly skipped."
license: MIT
compatibility: opencode
metadata:
  audience: developers
  category: workflow
  version: 1.0.0
allowed-tools: read write edit bash glob grep skill
---

# AwareCode — Coding Awareness Protocol

## Core Principle

**Aware before you act. Learn from every mistake.**

AwareCode is a meta-skill that governs how code is written, not what code is written. It enforces a five-phase protocol that runs before, during, and after every code change.

### When to use

- Writing new code or modifying existing code
- Debugging and fixing errors
- Refactoring or restructuring code
- Reviewing code quality

### When NOT to use

- Purely conversational questions (e.g., "explain concept X")
- Quick drafts where the user explicitly says "no review needed"
- Tasks that are entirely about reading/exploring without changing code
- When the user asks for a brainstorming session without implementation

---

## The Five Phases

| Phase | When | What |
|-------|------|------|
| **1. Context Scan** | Before writing code | Read project docs, search existing patterns, verify dependencies |
| **2. Error Memory** | When errors are reported | Analyze root cause, log it, avoid repetition |
| **3. Quality Review** | After writing code | Check correctness, performance, security, test coverage |
| **4. Doc Sync** | After quality review | Update comments, README, CHANGELOG |
| **5. Summary** | End of each change | Output structured summary in clear language |

---

## Phase 1 — Context Scan

Execute this phase before writing or modifying any code.

### Step 1: Project Context

Read the following files (in priority order):

```
HIGH PRIORITY (always check):
  AGENTS.md / CLAUDE.md          — Project rules, architecture, conventions
  opencode.json / opencode.jsonc — Provider, model, custom commands
  README.md                      — Project overview and setup

MEDIUM PRIORITY (check when relevant):
  package.json / Cargo.toml / requirements.txt / go.mod — Dependencies
  tsconfig.json / .editorconfig / .eslintrc / .prettierrc — Tool config
  Makefile / Justfile / Taskfile — Build commands
  Dockerfile / docker-compose.yml — Container setup

LOW PRIORITY (check for large/team projects):
  CONTRIBUTING.md — Contribution guidelines
  docs/ directory — Additional documentation
  .github/ — CI/CD workflows
```

### Step 2: Pattern Search

Find and read 2-3 existing files related to your task:

- Use `glob` to locate similar files by name or extension
- Use `grep` to find existing implementations of related features
- Extract these conventions:
  - **Naming**: camelCase / snake_case / PascalCase
  - **Imports**: absolute vs relative, grouped vs flat
  - **Error handling**: try/catch, Result types, panic, error returns
  - **Types**: TypeScript, Python hints, JSDoc, or none
  - **Async**: async/await, promises, callbacks, threads
  - **Testing**: framework used, file naming, assertion style
  - **Comments**: docstrings, inline, or minimal

### Step 3: Dependency Verification

Never assume a library exists. Verify:

```
grep for <library-name> in package.json / Cargo.toml / imports in existing files
```

If the library is not found, do not use it. Use what is already in the project, or ask the user before adding a new dependency.

### Step 4: Design Awareness

Before writing, ask yourself:

- [ ] Is this on a hot path? (frequent calls, large loops, per-frame)
- [ ] Does this touch I/O? (database, network, filesystem)
- [ ] Does this handle user input? (security: injection, XSS, validation)
- [ ] Is this a public API? (backward compatibility, versioning)
- [ ] Is there an existing pattern I should follow?
- [ ] Can this be done with a smaller change?

---

## Phase 2 — Error Memory

Execute this phase when the user reports an error or unexpected behavior.

### Step 1: Root Cause Analysis

```
ERROR:
  Message: <exact error text>
  File: <file path>
  Line: <relevant line>
  Code: <relevant snippet>

ROOT CAUSE:
  Category: <see table below>
  Details: <what went wrong>
  Why: <why I wrote it this way — false assumption? missed context?>
```

#### Root Cause Categories

| Category | Description | Prevention |
|----------|-------------|------------|
| API Misuse | Wrong method signature, parameters, or return type | Read API docs before calling |
| Type Mismatch | Wrong type, missing type, incorrect casting | Check type signatures |
| Missing Import | Using an undefined symbol | Verify imports before writing |
| Wrong Convention | Using wrong naming, pattern, or idiom | Check AGENTS.md and existing code |
| Logic Error | Wrong algorithm, off-by-one, edge case | Trace through edge cases |
| Library Assumption | Assuming a library exists without checking | Verify in package.json first |
| Path Mistake | Wrong import path, wrong file reference | Verify path before editing |
| Silent Failure | Missing error handling, unhandled rejection | Always handle error returns |
| Overwrite | Replacing needed code during edit | Read full file before edit |
| Environment | Wrong version, missing tool, wrong OS | Check project prerequisites |
| False Assumption | Assuming behavior without verifying | Test the assumption first |

### Step 2: Log for Session Memory

Maintain an error log for the duration of the session:

```
[ERROR LOG]
#1 [TypeMismatch] src/utils/format.ts:15 — passed string to calculateTotal which expects number → cast with Number()
#2 [LibraryAssumption] src/server.ts — assumed express is installed, project uses hono → rewrote with hono routing
```

Before each subsequent code generation, scan this log for relevant past mistakes.

### Step 3: Fix with Awareness

- Re-read the affected file(s) before fixing
- Address the root cause, not the symptom
- Check if the same mistake exists elsewhere in the codebase
- Verify the fix, then verify it didn't break anything else

### Step 4: Update Mental Model

If the error reveals a pattern (e.g., "this project uses Result types not exceptions"), update your understanding and apply it to ALL subsequent code in the session.

---

## Phase 3 — Quality Review

Execute this phase after writing or modifying code, before syncing documentation.

### Section A — Correctness

- [ ] Does the code handle the "happy path" correctly?
- [ ] Does the code handle edge cases? (empty input, null, max values)
- [ ] Does the code handle error cases? (network failure, invalid input, auth failure)
- [ ] Are there any unhandled rejections or uncaught exceptions?
- [ ] Are there any race conditions or timing issues?

### Section B — Performance

- [ ] Hot path check: is this code called frequently? If so:
  - Avoid unnecessary allocations in loops
  - Hoist invariant calculations out of loops
  - Use appropriate data structures (Set vs Array.includes)
- [ ] I/O check: does this touch database/network/filesystem? If so:
  - Batch operations instead of individual calls
  - Consider caching for repeated reads
  - Use connection pooling
- [ ] Algorithm check: is this O(n²) or worse for potentially large inputs?
- [ ] Memory check: are there leaks from closures, listeners, or large retained objects?

### Section C — Security

- [ ] User input: is it validated, sanitized, and escaped?
  - SQL: use parameterized queries, not string concatenation
  - HTML: escape output, use safe rendering
  - Shell: avoid passing user input to shell commands
  - Paths: prevent directory traversal
- [ ] Authentication: are protected routes/operations properly guarded?
- [ ] Secrets: no API keys, tokens, or passwords in code or logs
- [ ] Dependencies: is the code using known-unsafe patterns?

### Section D — Testing

- [ ] Does the project have tests, and do they pass?
- [ ] Should I add tests for this change?
- [ ] Have I run the tests to verify nothing is broken?

---

## Phase 4 — Doc Sync

Execute this phase after quality review.

### Step 1: Identify Affected Documentation

| If you changed... | Update... |
|-------------------|-----------|
| Function/API signature | JSDoc / docstring / type signature comments |
| Public interface | README, API docs, usage examples |
| Configuration/behavior | Config docs, migration guide |
| Project setup or build | README setup section, CI/CD config |
| Breaking changes | CHANGELOG, migration guide, deprecation notices |

### Step 2: Sync Inline Comments

- Update existing comments that are now incorrect
- Add comments for non-obvious logic (explain "why", not "what")
- Remove comments that are redundant with clear code

### Step 3: Sync External Docs

- Check if README.md references the changed area
- Check if docs/ directory has relevant pages
- Update CHANGELOG.md if the change is user-facing

---

## Phase 5 — Summary

Output a structured summary after every code change.

### Template (English — default)

```
## Change Summary

### What
- <brief description of what changed>

### References
- <files, patterns, or docs consulted>

### Notes
- <caveats, follow-up steps, known limitations>

### Performance
- <optimizations or trade-offs, if applicable>

### Security
- <security considerations, if applicable>
```

### Template (中文)

```
## 变更总结

### 做了什么
- <简述本次改动的核心内容>

### 参考依据
- <参考了哪些文件/模式/文档>

### 注意事项
- <需要留意的点、后续步骤、已知限制>

### 性能考量
- <性能优化点或权衡（如适用）>

### 安全考量
- <安全相关的注意事项（如适用）>
```

### Rules

1. Use clear, concise language — the user should understand the change in 10 seconds
2. Be honest about trade-offs and limitations
3. If the change affects other parts of the system, mention it
4. Use the template that matches the user's language preference (default: English)

---

## Reference Files

See the `references/` directory for detailed guides:

| File | Content |
|------|---------|
| `references/patterns.md` | Coding patterns and conventions checklist |
| `references/errors.md` | Error memory templates and root cause catalog |
| `references/performance.md` | Performance optimization patterns by domain |
| `references/security.md` | Security checklist by common vulnerability class |
| `references/workflow.md` | Advanced workflow patterns with OpenCode features |

See `examples/` for usage scenarios.
