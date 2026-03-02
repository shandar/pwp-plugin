---
name: pwp-code-review
description: "Systematic code review protocol — review code with rigor and specificity. Use this skill whenever the user asks you to review code, check a PR, audit a file, look over changes, or give feedback on implementation quality. Also use when they say 'review this', 'is this code good', 'check my work', 'what do you think of this implementation', or 'any issues with this'. Covers correctness, security, readability, performance, maintainability, and testing."
---

# Code Review Skill

This skill defines how to perform a structured code review. Use it when reviewing your own output before presenting to the user, or when asked to review existing code.

## Review Mindset

- You are a reviewer, not an author. Your job is to find problems, not defend choices.
- Every comment must be actionable. "This could be better" is not a review comment. "Extract this into a utility function to reduce duplication with line 45" is.
- Prioritize correctness over style. A bug matters more than a naming preference.

## Review Checklist

For every code change, check the following in order:

### 1. Correctness
- Does the code do what it claims to do?
- Are all edge cases handled? (null, empty, boundary values, error states)
- Are async operations properly awaited? Error-handled?
- Are types correct and strict? (no `any` escapes, no type assertions without justification)

### 2. Security
- Is user input sanitized before rendering or database writes?
- Are secrets kept out of client code?
- Are permissions checked server-side, not just hidden in UI?
- Are dependencies from trusted sources?

### 3. Readability
- Can you understand the code without the author explaining it?
- Are variable and function names descriptive and consistent?
- Is the code structured logically? (related things together, clear flow)
- Are comments present where the *why* isn't obvious? Absent where it is?

### 4. Performance
- Are there unnecessary re-renders, re-fetches, or re-computations?
- Are large operations debounced or throttled?
- Are heavy modules lazy-loaded?
- Is data fetching efficient? (no N+1 queries, no over-fetching)

### 5. Maintainability
- Is the code DRY without being over-abstracted?
- Are there magic numbers or hardcoded values that should be constants?
- Is the change scoped correctly? (no mixed concerns in one commit)
- Will the next developer understand this in 6 months?

### 6. Testing
- Is the change tested? (new behavior has tests, bug fixes have regression tests)
- Are tests testing behavior, not implementation?
- Are test names descriptive?

## Severity Levels

| Level | Meaning | Action Required |
|-------|---------|----------------|
| **Blocker** | Bug, security flaw, or data loss risk | Must fix before merge |
| **Warning** | Logic concern, missing edge case, performance issue | Should fix; merge with caution if deferred |
| **Suggestion** | Readability improvement, naming preference, minor optimization | Optional; author decides |
| **Nit** | Formatting, whitespace, trivial style | Fix if easy; ignore if not |

## Comment Format

```
[SEVERITY] file:line — description

Example:
[Blocker] src/auth/login.ts:42 — Password comparison uses == instead of timing-safe comparison.
[Warning] src/hooks/useCart.ts:18 — Missing cleanup in useEffect return. Will cause memory leak.
[Suggestion] src/utils/format.ts:7 — Consider renaming `fmt` to `formatCurrency` for clarity.
```

## Self-Review Protocol

Before presenting any code to the user:

1. Re-read every line you changed
2. Run through the checklist above mentally
3. Flag anything you're less than 90% confident about
4. Verify the change compiles and builds
5. Confirm you haven't modified files outside the declared scope

## Anti-Patterns

- **Rubber-stamp reviews:** Approving without actually reading. Never.
- **Style-only reviews:** Catching semicolons but missing null pointer exceptions.
- **Ego reviews:** Rewriting working code to match personal preference.
- **Drive-by comments:** Pointing out issues without suggesting fixes.
