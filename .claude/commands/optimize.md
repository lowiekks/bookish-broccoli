---
description: "Optimize component/function for performance"
argument-hint: [filepath]
---

Analyze @$1 for performance optimizations while maintaining functionality.

**React Component Optimizations:**
- [ ] Unnecessary re-renders (use React.memo if props rarely change)
- [ ] Heavy computations in render (move to useMemo)
- [ ] Event handlers recreated on render (use useCallback)
- [ ] Context updates causing cascading re-renders (split contexts?)
- [ ] Large lists without virtualization (use react-virtual?)

**Data Fetching:**
- [ ] Multiple serial requests (can they be parallel?)
- [ ] Missing React Query caching
- [ ] Over-fetching data (select only needed fields?)
- [ ] Real-time listeners on large datasets (add pagination?)

**Bundle Size:**
- [ ] Large dependencies (check with dynamic imports)
- [ ] Unused imports (tree-shaking working?)
- [ ] Heavy libraries for simple tasks (can use lighter alternative?)

**Firestore Optimizations:**
- [ ] Queries properly indexed?
- [ ] Using pagination/limits on large collections?
- [ ] Unnecessary snapshot listeners (use get() if data doesn't need to be real-time)
- [ ] Batch writes where possible

**Rendering Optimizations:**
- [ ] Images using next/image?
- [ ] Fonts optimized (next/font)?
- [ ] Lazy loading for off-screen content?
- [ ] Suspense boundaries for code splitting?

Implement optimizations and provide:
1. What was optimized
2. Expected performance improvement
3. Trade-offs (if any)
4. Benchmarks (before/after if measurable)
