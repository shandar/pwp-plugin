---
name: pwp-test
description: "Testing strategy and implementation protocol — structured quality assurance that catches real bugs. Use this skill whenever the user asks to write tests, add test coverage, set up a testing strategy, or asks 'how should I test this'. Also use when they say 'add tests', 'write tests for this', 'test coverage', 'what should I test', 'this needs tests', or 'set up testing'. Covers test pyramid, naming conventions, coverage targets, test data factories, and when to test what."
---

# PWP Testing Protocol

You are now operating under the Project Workflow Protocol's testing discipline. Follow this structured approach for every testing task.

## Context: $ARGUMENTS

## Phase 1: Assess Testing Needs

Before writing any test, understand:
1. **What is being tested?** — Business logic, API endpoint, UI component, integration?
2. **What layer of the test pyramid?** — Unit (fast, many), Integration (medium, some), E2E (slow, few)
3. **What already exists?** — Check for existing test files, test utils, and coverage reports
4. **What's the testing stack?** — Jest, Vitest, Playwright, Cypress, React Testing Library, etc.

## Phase 2: Apply the Test Pyramid

Distribute testing effort intentionally:

| Layer | What It Tests | Speed | Quantity |
|-------|--------------|-------|----------|
| **Unit** | Pure functions, utilities, isolated logic | Fast (ms) | Many |
| **Integration** | Component interactions, API endpoints, DB queries | Medium (s) | Some |
| **E2E** | Full user journeys through real UI | Slow (min) | Few |

### Always Test
- Business logic and calculations
- Data transformations and parsing
- Edge cases: empty inputs, null values, boundary conditions
- Error paths: what happens when things fail
- API contracts: request/response shapes
- Auth and permission logic

### Test With Judgment
- Component rendering (test behavior, not snapshot every div)
- Form validation rules
- State transitions
- Navigation flows

### Rarely Test
- Third-party library internals (trust the library, mock the boundary)
- Pure styling (visual regression tools handle this better)
- One-line getters or trivial pass-through functions
- Framework boilerplate

## Phase 3: Write Tests

### File Naming
Co-locate test files with source:
```
src/utils/formatCurrency.ts
src/utils/formatCurrency.test.ts      ← co-located
src/components/InvoiceTable.tsx
src/components/InvoiceTable.test.tsx   ← co-located
__tests__/integration/checkout.test.ts ← integration in dedicated folder
```

### Test Case Naming
Pattern: `it('should {expected behavior} when {condition}')`

```
describe('formatCurrency')
  it('should format positive numbers with two decimals')
  it('should return "$0.00" when given zero')
  it('should handle negative numbers with a minus prefix')
  it('should throw when given NaN')
```

### Test Data Rules
- **Use factories, not fixtures.** Generate test data with overridable defaults.
- **No production data.** Use realistic but synthetic data.
- **Isolate state.** Each test sets up and tears down its own state.
- **Name clearly.** `validUser`, `expiredToken`, `emptyCart` — not `testData1`.

## Phase 4: When to Write Tests

| Scenario | Approach |
|----------|----------|
| New business logic | Write tests alongside implementation |
| Bug fix | Write failing test first, then fix (TDD for bugs) |
| Refactoring | Ensure tests exist BEFORE refactoring (safety net) |
| Exploratory/prototype | Skip tests, but flag as untested |
| Performance-critical path | Add benchmark tests with baseline thresholds |

## Phase 5: Coverage Targets

| Context | Target | Rationale |
|---------|--------|-----------|
| Business logic / utils | 90%+ | Most testable and most critical |
| API routes / controllers | 80%+ | Integration tests cover important paths |
| UI components | 60%+ | Test behavior and interactions |
| Overall project | 70%+ | Meaningful floor that compounds confidence |

Coverage is a compass, not a destination.

## Philosophy (non-negotiable)

- **Test behavior, not implementation.** Tests should survive refactors.
- **Tests are documentation.** A well-named test suite tells the next dev what the code does.
- **Diminishing returns are real.** 80% meaningful coverage beats 100% checkbox coverage.
- **Flaky tests are worse than no tests.** A flaky test erodes trust in the entire suite. Fix or delete.

## Verification Checklist

After writing tests, confirm:
- [ ] New business logic has corresponding tests
- [ ] Bug fixes include a regression test
- [ ] Tests are named descriptively (behavior + condition)
- [ ] Test data is synthetic, isolated, and clearly named
- [ ] No test depends on another test's state
- [ ] Flaky tests are fixed or removed
- [ ] Coverage targets met for changed code
- [ ] Test suite passes with exit code 0
