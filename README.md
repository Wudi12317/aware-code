# AwareCode

**A coding awareness protocol for AI agents.** Before writing code, scan project context and existing patterns. When errors occur, learn from them. After changes, review quality, sync docs, and summarize.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-6A4EFF)](https://agentskills.io)

AwareCode is a meta-skill: it governs *how* code is written, not *what* code is written about. It works alongside domain-specific skills (React, Godot, SQL, etc.) to ensure every code change is context-aware, learns from mistakes, and leaves documentation in a healthy state.

## Features

- **Change Classification** 鈥?Auto-detects small vs large changes; lightweight mode skips heavy review for trivial edits
- **Project Type Detection** 鈥?Automatically identifies project type (Godot, Rust, Node, Python, Go, etc.) from root files
- **Context Scan** 鈥?Reads AGENTS.md, package.json, existing files, and project conventions before writing a single line
- **Error Memory** 鈥?Analyzes root causes, logs them for the session, and persists to `references/errors.md` for cross-session recall
- **Quality Review** 鈥?Checks correctness, performance, security, and test coverage after every change
- **Doc Sync** 鈥?Updates inline comments, README, and CHANGELOG when code changes
- **Rollback** 鈥?Structured recovery protocol when things go wrong
- **Action Summary** 鈥?Outputs a structured summary after each change (what, why, trade-offs)

## Installation

### Via npx skills (easiest)

```bash
npx skills add Wudi12317/aware-code
```

### Global (manual)

```bash
git clone https://github.com/Wudi12317/aware-code.git
cp -r aware-code ~/.config/opencode/skills/aware-code
```

### Per-project

```bash
cp -r aware-code .opencode/skills/aware-code
```

### Windows

```powershell
git clone https://github.com/Wudi12317/aware-code.git
Copy-Item -Recursive aware-code "$env:USERPROFILE\.config\opencode\skills\aware-code"
```

### Verify

Start opencode and ask:

```
What skills are available?
```

You should see `aware-code` in the list.

## Usage

AwareCode activates automatically on coding tasks. You can also invoke it directly:

```
Use the aware-code skill to review my last change
```

The skill speaks in clear, structured summaries after each change. If you prefer a different language or format, fork the skill and adjust Phase 5.

## Structure

```
aware-code/
鈹溾攢鈹€ SKILL.md                  # Main protocol (five phases)
鈹溾攢鈹€ LICENSE                   # MIT
鈹溾攢鈹€ README.md                 # This file
鈹溾攢鈹€ CHANGELOG.md              # Release history
鈹溾攢鈹€ references/
鈹?  鈹溾攢鈹€ patterns.md           # Code patterns and conventions checklist
鈹?  鈹溾攢鈹€ errors.md             # Error memory templates and root cause catalog
鈹?  鈹溾攢鈹€ performance.md        # Performance optimization by domain
鈹?  鈹溾攢鈹€ security.md           # Security checklist by vulnerability class
鈹?  鈹斺攢鈹€ workflow.md           # Advanced workflow patterns with OpenCode
鈹斺攢鈹€ examples/
    鈹斺攢鈹€ basic.md              # Usage scenarios
```

## Compatibility

AwareCode follows the [Agent Skills](https://agentskills.io) standard and is compatible with any tool that supports `SKILL.md` discovery:

- OpenCode
- Claude Code
- OpenAI Codex
- Cursor
- Windsurf
- GitHub Copilot
- Any other tool supporting the Agent Skills format

## License

MIT
