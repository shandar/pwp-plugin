---
name: pwp-debug
description: "Systematic debugging protocol — diagnose, isolate, and fix bugs with discipline. Use this skill whenever the user reports a bug, error, crash, or unexpected behavior. Also use when they say things like 'this is broken', 'not working', 'something's wrong', 'fix this', 'why is this failing', or share an error message or stack trace. Covers reproduce → isolate → root cause → fix → verify workflow."
---

# Debugging Skill

This skill defines how to approach bugs systematically. No guessing. No random changes. Reproduce, isolate, fix, verify.

## Debugging Mindset

- **Reproduce first.** Never propose a fix for a bug you haven't seen. If you can't reproduce it, you don't understand it.
- **Read the error.** The full error message. The stack trace. The line number. Not just the first line.
- **One change at a time.** Change one thing, test, observe. Never change three things and hope one of them fixes it.
- **The bug is in your code.** Until proven otherwise, assume the framework, library, and runtime are correct. Your code is the most likely source.

## Debugging Protocol

### Step 1: Reproduce
- Get the exact steps that trigger the bug
- Reproduce it yourself (or see the error output)
- If you can't reproduce it: ask for more context, check environment differences

### Step 2: Read the Error
- Read the **full** error message and stack trace
- Identify the **file and line** where the error originates
- Trace the **call chain** — where was this function called from?
- Check if the error message tells you exactly what's wrong (it usually does)

### Step 3: Isolate
Use binary search to narrow the problem:

```
Layer 1: Is it a UI issue or a data issue?
  → Check the data: is it correct at the source?
  → If data is correct, the bug is in rendering
  → If data is wrong, trace it backward

Layer 2: Is it a client issue or a server issue?
  → Check the network request/response
  → If response is wrong, debug the server
  → If response is correct, debug the client

Layer 3: Is it this commit or a previous one?
  → Use git log and git diff to find when it broke
  → git bisect for harder cases
```

### Step 4: Understand the Root Cause
- Don't just find *what* broke — understand *why* it broke
- Is this a one-off mistake or a systemic pattern?
- Are there other places in the codebase with the same issue?

### Step 5: Fix
- Fix the **root cause**, not just the symptom
- Make the **smallest effective change**
- If the fix is complex, write a plan before implementing

### Step 6: Verify
- Confirm the original bug is fixed
- Confirm no new bugs were introduced
- Run the full verification suite (tsc, build, tests)
- If the bug was in a critical path, add a regression test

### Step 7: Document
- Write a clear commit message explaining what broke and why
- If the bug was subtle, add a code comment explaining the fix
- If the bug was a P0/P1, write a postmortem

## Common Bug Categories

| Category | Symptoms | Where to Look |
|----------|----------|--------------|
| **Null/undefined** | "Cannot read property of undefined" | Data flow — what's returning null? |
| **Async timing** | Intermittent failures, race conditions | Missing await, missing cleanup, stale closures |
| **Type mismatch** | Wrong data rendered, silent failures | API response shape vs. expected type |
| **State management** | UI out of sync, stale data | State updates, re-render triggers, cache invalidation |
| **Environment** | Works locally, fails in production | Environment variables, build config, API endpoints |
| **Dependency** | Broke after update | Check changelogs, run `git diff package-lock.json` |

## Anti-Patterns

| Anti-Pattern | Why It's Wrong | Do This Instead |
|-------------|---------------|----------------|
| "Try this and see if it works" | Guessing wastes time and erodes trust | Reproduce, isolate, understand, then fix |
| Changing multiple things at once | Can't tell which change fixed (or broke) it | One change at a time |
| Ignoring the error message | The answer is usually in the error | Read the full message and stack trace |
| Blaming the framework | 99% of the time, it's your code | Check your code first |
| Patch fixing without understanding | Creates new bugs and tech debt | Fix the root cause |
| Not adding a regression test | Same bug will return | Write a test that fails without the fix |

## Reporting Format

When presenting a bug fix:

```
## Bug: {brief description}

**Symptom:** {what the user saw}
**Root Cause:** {what was actually wrong and why}
**Fix:** {what was changed — file:line references}
**Verification:** {how to confirm the fix works}
**Regression Risk:** {what else could be affected — "Low/Medium/High" with explanation}
```
