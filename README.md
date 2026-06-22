# AwareCode

**A coding awareness protocol for AI agents.** Before writing code, scan project context and existing patterns. When errors occur, learn from them. After changes, review quality, sync docs, and summarize.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-6A4EFF)](https://agentskills.io)

AwareCode is a meta-skill: it governs *how* code is written, not *what* code is written about. It works alongside domain-specific skills (React, Godot, SQL, etc.) to ensure every code change is context-aware, learns from mistakes, and leaves documentation in a healthy state.

## Features

- **Context Scan** — Reads AGENTS.md, package.json, existing files, and project conventions before writing a single line
- **Error Memory** — Analyzes root causes, logs them for the session, and avoids repeating the same mistake
- **Quality Review** — Checks correctness, performance, security, and test coverage after every change
- **Doc Sync** — Updates inline comments, README, and CHANGELOG when code changes
- **Action Summary** — Outputs a structured summary after each change (what, why, trade-offs)

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
├── SKILL.md                  # Main protocol (five phases)
├── LICENSE                   # MIT
├── README.md                 # This file
├── CHANGELOG.md              # Release history
├── references/
│   ├── patterns.md           # Code patterns and conventions checklist
│   ├── errors.md             # Error memory templates and root cause catalog
│   ├── performance.md        # Performance optimization by domain
│   ├── security.md           # Security checklist by vulnerability class
│   └── workflow.md           # Advanced workflow patterns with OpenCode
└── examples/
    └── basic.md              # Usage scenarios
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
