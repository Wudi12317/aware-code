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
## 变更总结

### 做了什么
- 为用户列表添加了升序/降序排序切换，通过点击表头触发

### 参考依据
- 项目使用原生 Array.sort，无额外依赖
- 排序逻辑参考了项目中 ContactsList 的现有实现

### 注意事项
- 目前是客户端排序，数据量大（>10000条）时建议改服务端排序

### 性能考量
- O(n log n)，1000条以下无感知
- 排序结果已用 useMemo 缓存，避免不必要的重排

### 安全考量
- 不涉及用户输入，无安全风险
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
  Why: Didn't handle the error case for API failure

FIX:
  Added null check before accessing response
  Prevention: Always check API responses for null/undefined before use
```

**AwareCode Phase 5 — Summary:**

```
## 变更总结

### 做了什么
- 修复了登录页在 API 返回 null 时的崩溃问题
- 在访问 response.status 前增加了空值检查

### 参考依据
- 同一文件中的其他 API 调用已有类似检查，这次遗漏了

### 注意事项
- 建议给 API 客户端加全局响应拦截器，统一处理此类问题
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
## 变更总结

### 做了什么
- 将 Login.tsx 中的认证逻辑提取到 src/hooks/useAuth.ts
- useAuth 返回 { user, login, logout, isLoading, error }
- 保持了与现有代码完全兼容的行为

### 参考依据
- 遵循了项目中 useUser 和 useSettings 的 hook 命名和返回模式
- 函数签名和错误处理方式与现有 hooks 一致

### 注意事项
- Login.tsx 已更新使用新 hook，其他页面后续可复用

### 性能考量
- login/logout 引用稳定（useCallback），不会触发子组件不必要重渲染
```
