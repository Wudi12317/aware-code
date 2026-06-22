# Error Memory — Templates and Root Cause Catalog

## Session Error Log Format

```
#N [Category] file:line — root cause → fix applied
```

## Root Cause Analysis Template

```
ERROR:
  Message: <exact error message>
  File: <file path>
  Line: <line number>
  Code: <relevant snippet>

ROOT CAUSE:
  Category: <from catalog below>
  Details: <what went wrong>
  Why: <why was it written this way — assumption? missed context? haste?>

FIX:
  Applied: <what was changed>
  Prevention: <how to avoid this in the future>
```

## Root Cause Catalog

### API Misuse
Calling a method/function with wrong parameters, wrong types, or wrong calling convention.

```
Example: fs.writeFile(path, data, callback) — forgot the callback
Fix: Read the API signature before calling
```

### Type Mismatch
Passing a value of the wrong type.

```
Example: "42" instead of 42 for a numeric parameter
Fix: Check expected types. Use parseInt/Number for conversions.
```

### Missing Import / Reference
Using a symbol that hasn't been imported or defined.

```
Example: using useState without importing it from React
Fix: Check imports at the top of the file before writing the body.
```

### Convention Violation
Writing code that doesn't match the project's established patterns.

```
Example: using class components in a functional-component-only project
Fix: Read 2-3 existing files to understand the project's style.
```

### Logic Error
Correct syntax, wrong algorithm.

```
Example: off-by-one in loop boundary, incorrect sort comparator
Fix: Trace through edge cases. Write a minimal test.
```

### Library Assumption
Assuming a library is installed when it isn't.

```
Example: using lodash.debounce when the project doesn't have lodash
Fix: Always grep package.json before using external libraries.
```

### Path Mistake
Wrong import path, wrong file reference.

```
Example: import from '../../utils/helpers' when the correct path is '../utils/helpers'
Fix: Use IDE autocomplete or verify paths before writing imports.
```

### Silent Failure
Not handling an error return, promise rejection, or null case.

```
Example: JSON.parse(input) without try/catch when input could be invalid
Fix: Always handle error returns and null/undefined cases.
```

### Overwrite During Edit
Replacing existing code that was needed.

```
Example: replacing a function body without noticing the existing logic
Fix: Read the full function/class before making changes.
```

### Environment Mismatch
Wrong runtime version, missing system dependency, wrong OS.

```
Example: using fs.promise API on Node 10
Fix: Check engines field in package.json.
```

### False Assumption
Believing something about the codebase or API that isn't true.

```
Example: assuming fetchUser always returns a User when it returns User | null
Fix: Check the actual return type before using the value.
```

## Prevention Strategies

| Category | Prevention |
|----------|------------|
| API Misuse | Read docs before calling unfamiliar APIs |
| Type Mismatch | Check type signatures / TypeScript types first |
| Missing Import | Write imports first, then the body |
| Convention Violation | Read 2-3 existing files before writing |
| Logic Error | Test with edge cases mentally or with code |
| Library Assumption | Grep package.json before using |
| Path Mistake | Verify path with read/glob before importing |
| Silent Failure | Always check error returns and null cases |
| Overwrite | Read the full target file before editing |
| False Assumption | Verify the assumption with a quick read or test |

## Golden Rules

1. Never repeat the same error twice in a session
2. Log every new error pattern immediately
3. Scan the error log before each subsequent code generation
4. A mistake in one file means the same pattern may be wrong elsewhere — check
