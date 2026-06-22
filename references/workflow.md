# Advanced Workflow Patterns

This reference shows how AwareCode integrates with OpenCode's built-in features for a complete development workflow.

## Plan-Build Separation

OpenCode's Plan agent (read-only) and Build agent (full access) complement AwareCode's Phase 1 (Context Scan):

```
Tab to Plan agent → "Analyze the current architecture for [task]"
  ── AwareCode Phase 1 runs: scans AGENTS.md, patterns, dependencies
  ── Plan produces analysis without changing files

Tab to Build agent → "Implement the solution"
  ── AwareCode Phase 1 re-runs (new context)
  ── Phase 3-5 run after implementation
```

## Using with Subagents

AwareCode works alongside specialized subagents (@mention):

```
@explorer Search for all existing API route patterns in the project
  ── AwareCode Phase 1 guides what to look for

@reviewer Review the just-implemented code for security issues
  ── AwareCode Phase 3 Section C (Security) guides the review
```

## Error Recovery Loop

When the user reports an error:

```
1. User: "This doesn't work"
   ── AwareCode Phase 2: Analyze root cause, log it

2. Fix the code
   ── AwareCode Phase 3: Verify fix, check for regression

3. User: "Still broken"
   ── AwareCode Phase 2 again: new analysis, compare with previous attempt
   ── Log accumulates: avoid repeating the same failed approach

4. Successful fix
   ── AwareCode Phase 4 + 5: Sync docs, output summary
```

## Context Management

In long sessions, use OpenCode's context management commands:

| When | Command |
|------|---------|
| Context getting bloated | `/compact` to condense |
| Need fresh start | `/clear` to reset conversation |
| Check token usage | `/tokens` |
| Preview risky changes | `/dry-run` |

## Custom Commands Integration

Define AwareCode-related commands in `opencode.json`:

```json
{
  "commands": {
    "review": {
      "description": "Run AwareCode quality review on the last change",
      "command": "Use the aware-code skill to review my last code change"
    },
    "why": {
      "description": "Ask AwareCode to explain a design decision",
      "command": "Use the aware-code skill to explain the reasoning behind [file]"
    }
  }
}
```

## Combining with Other Skills

AwareCode is designed to work alongside domain-specific skills:

- **SQL Expert** → AwareCode provides the protocol, SQL Expert provides the database knowledge
- **Vercel React Best Practices** → AwareCode provides the lifecycle, React skill provides the rules
- **Superpowers** → AwareCode's quality review complements Superpowers' TDD workflow

Skills auto-discover and the agent loads the relevant ones based on the task.
