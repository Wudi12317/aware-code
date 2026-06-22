# Coding Patterns and Conventions Checklist

## Pre-Write Checklist

- [ ] Read AGENTS.md / CLAUDE.md for project rules
- [ ] Checked package.json / Cargo.toml / requirements.txt for dependencies
- [ ] Reviewed directory structure for organizational patterns
- [ ] Found and read 2-3 similar files to understand conventions

## Convention Extraction

When reading existing files, extract:

| Aspect | Look For |
|--------|----------|
| **Naming** | camelCase (JS/TS/Java), snake_case (Python/Rust), PascalCase (classes/types), SCREAMING_CASE (constants) |
| **Imports** | Absolute vs relative, grouped by type (built-in, external, internal), sorted |
| **Exports** | Named exports vs default exports, barrel files (index.ts) |
| **Error Handling** | try/catch, Result/Option types, error returns, panic/throw, custom error classes |
| **Async** | async/await, .then/.catch, Promise.all vs sequential, callbacks, goroutines |
| **Types** | TypeScript strict mode, Python type hints, JSDoc, Go interfaces, Rust traits |
| **Testing** | jest/vitest/pytest/go test, file naming (<name>.test.ts), assertion style, mocking approach |
| **Comments** | JSDoc/docstrings (explain parameters and return), inline (explain "why"), or minimal |
| **Formatting** | Indentation (2 spaces, 4 spaces, tabs), semicolons, trailing commas, line length |

## Library Verification

```
grep -r "library-name" package.json
```

If not found, do not use it. Ask the user or use an alternative already in the project.

## File Modification Rules

1. **Read first** — read the full file before editing, not just the target area
2. **Understand context** — imports, exports, adjacent functions, type definitions
3. **Minimal change** — change only what's needed, don't refactor unrelated code
4. **Match style** — match the file's existing style exactly (don't reformat)
5. **Don't create new files unless necessary** — editing existing files is usually better

## Common Anti-Patterns

- Adding comments that explain "what" instead of "why" (the code already shows "what")
- Over-engineering — adding abstractions, patterns, or generics before they're needed
- Reformatting existing code (changes formatting, rearranges imports, renames variables) as part of a functional change
- Writing new files when editing an existing one is sufficient
- Insufficient context — editing without reading enough surrounding code
- Copy-paste without adaptation — duplicated logic that should be shared
