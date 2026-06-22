---
name: aware-code
description: "Use when writing or modifying code. Scans project context and conventions before changes, auto-detects project type (Godot/Rust/Node/Python/etc.), learns from errors with cross-session persistence, reviews quality (correctness, performance, security, tests), syncs docs, and outputs structured summaries. Supports lightweight mode for small changes and rollback for recovery. Do NOT use for purely conversational tasks or quick drafts where review is explicitly skipped."
license: MIT
compatibility: opencode
metadata:
  audience: developers
  category: workflow
  version: 1.0.0
allowed-tools: read write edit bash glob grep skill
---

# AwareCode 鈥?Coding Awareness Protocol

## Core Principle

**Aware before you act. Learn from every mistake.**

AwareCode is a meta-skill that governs how code is written, not what code is written. It enforces a five-phase protocol that runs before, during, and after every code change. Small changes (typos, comments, config tweaks) use a lightweight mode that skips heavy review steps.

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
| **0. Classify** | Before everything | Small change 鈫?lightweight mode; Large change 鈫?full protocol |
| **1. Context Scan** | Before writing code | Detect project type, read docs, search patterns, verify dependencies |
| **2. Error Memory** | When errors are reported | Analyze root cause, log it (session + file), avoid repetition |
| **3. Quality Review** | After writing code | Check correctness, performance, security, tests (with timeout) |
| **4. Doc Sync** | After quality review | Update comments, README, CHANGELOG |
| **5. Summary** | End of each change | Output structured summary in clear language |

---

## Phase 1 鈥?Context Scan

Execute this phase before writing or modifying any code.

### Step 0: Classify the Change

Determine the scope to choose full or lightweight mode:

```
Small change (lightweight mode):
  - Typo fixes, comment updates, config tweaks
  - Single-line changes
  - Skip: pattern search, design awareness, quality review
  - Do: quick scan of AGENTS.md + README.md 鈫?write 鈫?doc sync 鈫?summary

Large change (full mode):
  - New features, bug fixes, refactors
  - Cross-file modifications, API changes
  - Execute all steps below
```

### Step 1: Detect Project Type

Auto-detect the project type to apply relevant conventions:

```
Check references/preferences.md first for manual override.
If not set, detect from project root:

  project.godot / *.tscn          鈫?Godot
  Cargo.toml                      鈫?Rust
  go.mod                          鈫?Go
  package.json                    鈫?Node / TypeScript (check for React/Vue/Solid)
  pubspec.yaml                    鈫?Flutter / Dart
  CMakeLists.txt                  鈫?C / C++ (CMake)
  pyproject.toml / requirements.txt 鈫?Python
  Gemfile                         鈫?Ruby
  mix.exs                         鈫?Elixir
  *.sln / *.csproj                鈫?.NET / C#
  pom.xml / build.gradle          鈫?Java (Maven/Gradle)
```

### Step 2: Project Context

Read the following files (in priority order):

```
HIGH PRIORITY (always check):
  AGENTS.md / CLAUDE.md          鈥?Project rules, architecture, conventions
  opencode.json / opencode.jsonc 鈥?Provider, model, custom commands
  README.md                      鈥?Project overview and setup

MEDIUM PRIORITY (check when relevant):
  package.json / Cargo.toml / requirements.txt / go.mod 鈥?Dependencies
  tsconfig.json / .editorconfig / .eslintrc / .prettierrc 鈥?Tool config
  Makefile / Justfile / Taskfile 鈥?Build commands
  Dockerfile / docker-compose.yml 鈥?Container setup

LOW PRIORITY (check for large/team projects):
  CONTRIBUTING.md 鈥?Contribution guidelines
  docs/ directory 鈥?Additional documentation
  .github/ 鈥?CI/CD workflows
```

### Step 3: Pattern Search

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

### Step 4: Dependency Verification

Never assume a library exists. Verify:

```
grep for <library-name> in package.json / Cargo.toml / imports in existing files
```

If the library is not found, do not use it. Use what is already in the project, or ask the user before adding a new dependency.

### Step 5: Design Awareness

Before writing, ask yourself:

- [ ] Is this on a hot path? (frequent calls, large loops, per-frame)
- [ ] Does this touch I/O? (database, network, filesystem)
- [ ] Does this handle user input? (security: injection, XSS, validation)
- [ ] Is this a public API? (backward compatibility, versioning)
- [ ] Is there an existing pattern I should follow?
- [ ] Can this be done with a smaller change?

---

## Phase 2 鈥?Error Memory

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
  Why: <why I wrote it this way 鈥?false assumption? missed context?>
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
#1 [TypeMismatch] src/utils/format.ts:15 鈥?passed string to calculateTotal which expects number 鈫?cast with Number()
#2 [LibraryAssumption] src/server.ts 鈥?assumed express is installed, project uses hono 鈫?rewrote with hono routing
```

Before each subsequent code generation, scan this log for relevant past mistakes.

### Step 2.5: Persist to File (Cross-Session)

If `references/errors.md` exists, append the error in YAML format:

```yaml
- date: 2026-06-22
  category: TypeMismatch
  file: src/utils/format.ts:15
  description: passed string to calculateTotal which expects number
  fix: cast with Number()
```

At the start of each new session, read `references/errors.md` to load historical error patterns.

### Step 3: Fix with Awareness

- Re-read the affected file(s) before fixing
- Address the root cause, not the symptom
- Check if the same mistake exists elsewhere in the codebase
- Verify the fix, then verify it didn't break anything else

### Step 4: Update Mental Model

If the error reveals a pattern (e.g., "this project uses Result types not exceptions"), update your understanding and apply it to ALL subsequent code in the session.

---

## Phase 3 鈥?Quality Review

Execute this phase after writing or modifying code, before syncing documentation.

### Section A 鈥?Correctness

- [ ] Does the code handle the "happy path" correctly?
- [ ] Does the code handle edge cases? (empty input, null, max values)
- [ ] Does the code handle error cases? (network failure, invalid input, auth failure)
- [ ] Are there any unhandled rejections or uncaught exceptions?
- [ ] Are there any race conditions or timing issues?

### Section B 鈥?Performance

- [ ] Hot path check: is this code called frequently? If so:
  - Avoid unnecessary allocations in loops
  - Hoist invariant calculations out of loops
  - Use appropriate data structures (Set vs Array.includes)
- [ ] I/O check: does this touch database/network/filesystem? If so:
  - Batch operations instead of individual calls
  - Consider caching for repeated reads
  - Use connection pooling
- [ ] Algorithm check: is this O(n虏) or worse for potentially large inputs?
- [ ] Memory check: are there leaks from closures, listeners, or large retained objects?

### Section C 鈥?Security

- [ ] User input: is it validated, sanitized, and escaped?
  - SQL: use parameterized queries, not string concatenation
  - HTML: escape output, use safe rendering
  - Shell: avoid passing user input to shell commands
  - Paths: prevent directory traversal
- [ ] Authentication: are protected routes/operations properly guarded?
- [ ] Secrets: no API keys, tokens, or passwords in code or logs
- [ ] Dependencies: is the code using known-unsafe patterns?

### Section D 鈥?Testing

- [ ] Does the project have tests, and do they pass?
- [ ] Have I run the full test suite?
- [ ] New feature 鈫?added at least one happy-path test
- [ ] Bug fix 鈫?added a regression test to prevent recurrence
- [ ] Refactor 鈫?existing tests cover the change
- [ ] Hot path change 鈫?consider adding a benchmark
- [ ] Did I run the linter if the project has one?

**Test command reference (check which applies):**
- Node: `npm test` / `pnpm test` / `yarn test`
- Python: `pytest` / `unittest`
- Rust: `cargo test`
- Go: `go test ./...`
- Java: `./gradlew test` / `mvn test`
- Godot: check project for test scene or GUT setup

**Timeout handling:**
- If tests run longer than 30 seconds with no output 鈫?ask user if they want to wait
- If tests run longer than 2 minutes 鈫?recommend manual test run, proceed with caution
- Note the test status in the summary so the user knows verification is pending

---

## Phase 4 鈥?Doc Sync

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

## Phase 5 鈥?Summary

Output a structured summary after every code change.

### Template (English 鈥?default)

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

### Template (涓枃)

```
## 鍙樻洿鎬荤粨

### 鍋氫簡浠€涔?- <绠€杩版湰娆℃敼鍔ㄧ殑鏍稿績鍐呭>

### 鍙傝€冧緷鎹?- <鍙傝€冧簡鍝簺鏂囦欢/妯″紡/鏂囨。>

### 娉ㄦ剰浜嬮」
- <闇€瑕佺暀鎰忕殑鐐广€佸悗缁楠ゃ€佸凡鐭ラ檺鍒?

### 鎬ц兘鑰冮噺
- <鎬ц兘浼樺寲鐐规垨鏉冭　锛堝閫傜敤锛?

### 瀹夊叏鑰冮噺
- <瀹夊叏鐩稿叧鐨勬敞鎰忎簨椤癸紙濡傞€傜敤锛?
```

### Rules

1. Use clear, concise language 鈥?the user should understand the change in 10 seconds
2. Be honest about trade-offs and limitations
3. If the change affects other parts of the system, mention it
4. Use the template that matches the user's language preference (default: English)

---

## Rollback (Recovery Protocol)

If a change causes problems, use this recovery flow:

### Step 1: Revert Code

```
git checkout -- <file-path>          # Revert single file
git revert <commit-hash> --no-edit   # Revert a specific commit
git reset HEAD~1 --hard              # Undo last commit (destructive)
```

- Check if the project uses Git; if not, suggest manual backup before future changes
- Confirm no other work is lost before reverting

### Step 2: Clean Up Residuals

- Remove newly created files that are no longer needed
- Restore modified config files to their original state

### Step 3: Log the Cause

- Write the failure reason to `references/errors.md` to prevent recurrence
- Consult this log before making similar changes in the future

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
