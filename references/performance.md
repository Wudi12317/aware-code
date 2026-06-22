# Performance Optimization Reference

## Pre-Write: Identifying Hot Paths

Ask before writing:

- [ ] Is this code in a loop, callback, or recursive function?
- [ ] Is this called per-frame, per-request, or per-user-action?
- [ ] Does this touch disk, network, or database?
- [ ] Does this process a large amount of data?
- [ ] Does this create or destroy objects frequently?

## General Optimization Principles

### 1. Avoid Repeated Work

```python
# ❌ Repeated calculation in loop
for item in items:
    result = expensive_lookup(item.category)  # same category each time
    ...

# ✅ Hoist invariant
lookup_cache = {cat: expensive_lookup(cat) for cat in categories}
for item in items:
    result = lookup_cache[item.category]
```

### 2. Choose the Right Data Structure

| Operation | Fast | Slow |
|-----------|------|------|
| Lookup by key | Map / Dict / Set | Array / List |
| Insert/delete at ends | ArrayDeque / VecDeque | Array (shift/unshift) |
| Insert/delete in middle | LinkedList | Array (splice) |
| Ordered unique items | SortedSet / TreeSet | Array + manual dedup |
| Numeric array | TypedArray / numpy / Vec | Plain objects |

### 3. Cache Expensive Results

- Pure functions → memoization (React.useMemo, Python functools.lru_cache)
- Database queries → query cache with TTL or LRU
- Computed values → compute once, store, invalidate when inputs change

### 4. Batch I/O

```javascript
// ❌ N+1 queries
for (const id of ids) {
  const user = await db.findUser(id);
}

// ✅ Batch query
const users = await db.findUsers(ids);
```

### 5. Avoid Hot-Path Allocation

Create objects once, reuse them. Use object pools for frequently created/destroyed objects (bullets, particles, connection handlers).

### 6. Lazy / Deferred Work

- Load expensive resources when needed, not at startup
- Use pagination, virtual scrolling, infinite scroll for large lists
- Defer non-critical work to idle callbacks or background workers

## Domain-Specific Guides

### Web (JavaScript/TypeScript)

| Concern | Recommendation |
|---------|---------------|
| React re-renders | useMemo, useCallback, React.memo, stable keys |
| Large lists | Virtual scrolling (react-window, @tanstack/virtual) |
| Bundle size | Tree-shaking, code splitting, dynamic imports |
| Images | Lazy loading, WebP/AVIF, responsive srcset |
| Animations | CSS transform/opacity (GPU-composited), requestAnimationFrame |

### Backend (Node.js / Python / Go / Rust)

| Concern | Recommendation |
|---------|---------------|
| Database | N+1 → JOIN or batch, add indexes, connection pooling |
| API responses | Pagination, field selection, response caching |
| Concurrency | Worker pool, connection pool, async I/O |
| Logging | Async logging, structured, avoid hot-path log writes |

### Game Development (Godot / Unity)

| Concern | Recommendation |
|---------|---------------|
| Per-frame logic | Disable _process when not needed (ProcessMode.Disabled) |
| Object creation | Object pooling for bullets, enemies, particles |
| Resources | Preload with ResourcePreloader, avoid runtime load() |
| Physics | Reduce RigidBody count, use Area for proximity checks |
| UI | Minimize Control nesting, use CanvasLayer layering |

## Performance Mindset

1. **Don't optimize prematurely** — write correct code first, profile to find bottlenecks
2. **Pareto principle** — 80% of performance issues are in 20% of the code
3. **Measure, don't guess** — always benchmark before and after optimization
4. **Document trade-offs** — if you pick a faster-but-less-readable approach, explain why
