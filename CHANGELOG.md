# Changelog

## 1.1.0 (2026-06-22)

### Features

- **Change Classification (Phase 0)**: Auto-detects small vs large changes 鈥?lightweight mode skips heavy review for typos, comments, and config tweaks
- **Project Type Detection**: Auto-detects Godot, Rust, Node, Python, Go, Flutter, C#, Java, and more from project root files
- **Persistent Error Memory**: Errors now save to `references/errors.md` for cross-session recall
- **Test Timeout Handling**: Asks user if tests run over 30s, recommends manual run at 2min
- **Rollback Protocol**: Structured recovery flow 鈥?revert code, clean residuals, log cause

## 1.0.0 (2026-06-22)

Initial release.

### Features

- **Phase 1 鈥?Context Scan**: Reads AGENTS.md, package.json, existing patterns, and project conventions before writing code
- **Phase 2 鈥?Error Memory**: Root cause analysis with 11 categories, session error logging, prevention strategies
- **Phase 3 鈥?Quality Review**: Correctness, performance, security, and testing checks after every change
- **Phase 4 鈥?Doc Sync**: Automatic identification and update of affected documentation (comments, README, CHANGELOG)
- **Phase 5 鈥?Summary**: Structured output after each change with what, why, trade-offs
- **References**: 5 reference files covering patterns, errors, performance, security, and workflow
- **Compatibility**: Follows Agent Skills standard, works with OpenCode, Claude Code, Codex, Cursor, Windsurf
