---
name: pwp-perf
description: "Performance optimization protocol — measure first, optimize second, verify always. Use this skill whenever the user reports something is slow, asks about performance, wants to optimize, or mentions load times, bundle size, rendering speed, memory usage, or database query performance. Also use when they say 'this is slow', 'speed this up', 'optimize this', 'reduce bundle size', 'improve load time', or 'why is this taking so long'. Covers web vitals, profiling, bottleneck identification, and before/after verification."
---

# Performance Optimization Skill

The cardinal rule: **never optimize on vibes.** Measure, identify the bottleneck, fix it, prove it's faster.

## Performance Mindset

- **Measure before optimizing.** Intuition about performance is wrong more often than right. Profile first.
- **Optimize the bottleneck.** Making a fast operation faster doesn't help.
- **Premature optimization is real.** Only optimize measured problems or known-critical paths.
- **Verify the improvement.** Before/after numbers or it didn't happen.

## Investigation Protocol

### Step 1: Define the Problem
- What is slow? (Page load, API, render, build)
- How slow? (Actual numbers: "3.2 seconds")
- What is the target? ("Under 1 second")

### Step 2: Measure Current State

| Area | How to Measure |
|------|---------------|
| **Page load** | Lighthouse, DevTools Performance tab |
| **Core Web Vitals** | LCP, INP, CLS via Lighthouse |
| **API response** | Network tab timing, server-side logging |
| **Render** | React DevTools Profiler |
| **Bundle size** | Build output, bundlephobia |
| **Memory** | DevTools Memory tab, heap snapshots |
| **Database** | Query EXPLAIN plans |

### Step 3: Identify the Bottleneck

1. **Network** — too many requests, large payloads, no caching
2. **Data** — slow queries, over-fetching, N+1 patterns
3. **Rendering** — unnecessary re-renders, layout thrashing
4. **Compute** — expensive calculations on main thread
5. **Assets** — uncompressed images, unminified JS

### Step 4: Fix the Bottleneck

| Bottleneck | Common Fixes |
|-----------|-------------|
| **Large bundle** | Code splitting, dynamic imports, tree shaking |
| **Slow page load** | Lazy loading, preload critical resources |
| **Re-renders** | useMemo, useCallback, React.memo (only where profiled) |
| **Slow API** | Caching, pagination, field selection |
| **N+1 queries** | Batch queries, eager loading, DataLoader |
| **Large images** | WebP/AVIF, responsive srcset, lazy loading |
| **Heavy computation** | Web Workers, caching results |

### Step 5: Verify

```markdown
**Before:** {metric} = {value}
**After:** {metric} = {value}
**Improvement:** {percentage}%
**Target met:** Yes / No
**Method:** {tool, conditions}
```

## Web Performance Targets

| Metric | Good | Needs Work | Poor |
|--------|------|-----------|------|
| **LCP** | < 2.5s | 2.5-4.0s | > 4.0s |
| **INP** | < 200ms | 200-500ms | > 500ms |
| **CLS** | < 0.1 | 0.1-0.25 | > 0.25 |
| **TTFB** | < 800ms | 800-1800ms | > 1800ms |
| **Bundle (gzip)** | < 100KB | 100-300KB | > 300KB |

## Anti-Patterns

| Anti-Pattern | Do This Instead |
|-------------|----------------|
| Memoizing everything | Profile first, memoize measured bottlenecks |
| Optimizing render count | Profile actual paint/layout cost |
| Cache without TTL | Always set expiration |
| Lazy loading everything | Only lazy load below-fold content |
| Compressing at runtime | Compress at build time |
