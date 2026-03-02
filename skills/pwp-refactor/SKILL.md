---
name: pwp-refactor
description: "Safe refactoring protocol — restructure code without breaking behavior. Use this skill whenever the user asks to refactor, restructure, reorganize, clean up, simplify, or decompose code. Also use when they say 'this file is too big', 'too much duplication', 'split this up', 'make this cleaner', 'reduce complexity', or 'extract this into its own thing'. Covers extract, rename, move, inline, and simplify patterns with safety verification."
---

# Refactoring Skill

This skill defines how to refactor code safely. The cardinal rule: **refactoring changes structure, never behavior.** If behavior changes, it's not a refactor — it's a feature or a fix.

## Refactoring Mindset

- **Refactoring is not cleaning.** It's restructuring code to make it easier to understand, extend, or maintain. It requires the same rigor as writing new features.
- **Never refactor and change behavior in the same commit.** Mixing structural changes with behavioral changes makes review impossible and debugging a nightmare.
- **Tests are your safety net.** If tests don't exist for the code you're refactoring, write them first.
- **Approval required.** Do not refactor code you weren't asked to touch. Flag ugly code, but don't fix it without explicit approval.

## When to Refactor

| Signal | Example | Action |
|--------|---------|--------|
| **Duplication** | Same logic in 3+ places | Extract to shared function/component |
| **Complexity** | Function > 50 lines, deeply nested conditionals | Decompose into smaller functions |
| **Naming** | Misleading or abbreviated names | Rename for clarity |
| **God files** | File > 500 lines mixing concerns | Split by responsibility |
| **Dead code** | Unused imports, unreachable branches | Remove |
| **Type weakness** | `any` types, missing interfaces | Add proper types |

## When NOT to Refactor

- During a bug fix (fix the bug, then propose a refactor separately)
- Without tests for the affected code
- Without explicit approval from the user/team
- When you're "just making it better" without a concrete improvement goal
- In the same commit as a feature change

## Refactoring Protocol

### Step 1: Justify
- State what you want to refactor and why
- Quantify the improvement: "Reduces duplication from 3 copies to 1"

### Step 2: Ensure Safety Net
- Verify tests exist for the code being refactored
- If tests are missing: write them first, commit them separately, then refactor

### Step 3: Plan the Changes
- List every file that will change
- Identify the refactoring pattern
- Declare the scope boundary: "I will only touch files X, Y, Z"

### Step 4: Execute
- One refactoring pattern per commit
- Run tests after each step
- If tests break, revert and investigate

### Step 5: Verify
```bash
# Before and after must match
npm test → all pass
npm run build → exits 0
```

## Common Refactoring Patterns

- **Extract Function:** Long function → smaller functions with clear names
- **Extract Component:** Large component → focused components composed together
- **Rename:** Misleading names → descriptive, consistent names
- **Move / Reorganize:** Code in wrong file → logical location
- **Inline:** Unnecessary abstraction → logic inlined where used
- **Simplify Conditionals:** Nested if/else → guard clauses, early returns

## Anti-Patterns

| Anti-Pattern | Why It's Wrong |
|-------------|---------------|
| Refactoring while fixing a bug | Can't tell if the bug fix works or the refactor broke something |
| Refactoring without tests | No safety net — behavior changes go undetected |
| "While I'm here" refactoring | Scope creep — stay within declared boundaries |
| Premature abstraction | Extracting a pattern seen only once adds complexity |
| Renaming everything at once | High blast radius — rename incrementally |
