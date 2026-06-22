# Usage Examples

## Example 1: Adding a New Feature

**User:** "Add a sorting option to the user list"

**AwareCode Phase 1 — Context Scan:**
- Reads AGENTS.md: project uses React + TypeScript + Tailwind
- Greps for existing sort implementations
- Reads the UserList component to understand current patterns
- Checks dependencies: lodash? No. Use native Array.sort

**AwareCode Phase 3 — Quality Review:**
- Correctness: handles empty list, single item, already-sorted list
- Performance: O(n log n), acceptable for client-side sorting of <10k items
- Security: no user input involved, no injection risk
- Testing: existing tests pass, adds new test case

**AwareCode Phase 4 — Doc Sync:**
- Updates any inline comments if sort direction or behavior needs explanation

**AwareCode Phase 5 — Summary:**

```
## Change Summary

### What
- Added ascending/descending sort toggle to the user list via column header click

### References
- Project uses native Array.sort, no external dependency added
- Sorting pattern follows the existing ContactsList component

### Notes
- Client-side sorting only; consider server-side sorting if data exceeds 10000 rows

### Performance
- O(n log n), imperceptible for <1000 items
- Result memoized with useMemo to avoid unnecessary re-renders

### Security
- No user input involved, no security risk
```

## Example 2: Fixing a Bug

**User:** "The login page crashes when I enter an invalid email"

**AwareCode Phase 2 — Error Memory:**

```
ERROR:
  Message: Cannot read properties of null (reading 'status')
  File: src/pages/Login.tsx
  Line: 42
  Code: if (response.status === 200)

ROOT CAUSE:
  Category: Silent Failure
  Details: API returns null on network error, code assumes response is always valid
  Why: Did not handle the error case for API failure

FIX:
  Added null check before accessing response
  Prevention: Always check API responses for null/undefined before use
```

**AwareCode Phase 5 — Summary:**

```
## Change Summary

### What
- Fixed login page crash when API returns null
- Added null check before accessing response.status

### References
- Other API calls in the same file already had similar checks; this one was missed

### Notes
- Consider adding a global response interceptor to the API client to handle this uniformly
```

## Example 3: Refactoring

**User:** "Extract the authentication logic into a custom hook"

**AwareCode Phase 1 — Context Scan:**
- Reads existing hooks in src/hooks/ for naming patterns (`useAuth` matches `useUser`, `useSettings`)
- Checks the existing auth logic for side effects and dependencies

**AwareCode Phase 3 — Quality Review:**
- Correctness: preserves all existing behavior
- Performance: no additional re-renders (useCallback/useMemo for stable references)
- Security: no exposure of tokens or sensitive data in the hook API
- Testing: hooks are now testable in isolation

**AwareCode Phase 5 — Summary:**

```
## Change Summary

### What
- Extracted authentication logic from Login.tsx into src/hooks/useAuth.ts
- Returns { user, login, logout, isLoading, error }

### References
- Follows naming and return patterns from useUser and useSettings hooks
- Error handling style matches existing hooks

### Notes
- Login.tsx updated to use the new hook; other pages can reuse it later

### Performance
- login/logout have stable references (useCallback), won't trigger unnecessary child re-renders
```
