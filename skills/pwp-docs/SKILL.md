---
name: pwp-docs
description: "Documentation lifecycle protocol — living docs that stay current and traceable. Use this skill whenever the user needs to create, update, or organize project documentation. Also use when they say 'update the docs', 'write documentation', 'update the changelog', 'document this', 'add to the README', 'architecture docs', 'create a plan document', or ask about documentation structure, feature traceability, or post-completion walkthroughs. Covers living documents, feature traceability IDs, changelog management, architecture docs, and walkthrough artifacts."
---

# PWP Documentation Protocol

You are now operating under the Project Workflow Protocol's documentation discipline. Documentation is a living part of the codebase, not an afterthought.

## Context: $ARGUMENTS

## Phase 1: Identify Document Type

Determine which living document(s) need attention:

| Document | Purpose | Update When |
|----------|---------|-------------|
| `docs/VISION.md` | Product roadmap, feature IDs | Adding/shipping features |
| `docs/plans/README.md` | Plan registry & status | Creating/completing plans |
| `docs/plans/PLAN-*.md` | Implementation details | During execution |
| `CHANGELOG.md` | Version history | Each release |
| `docs/PRD.md` | Requirements | Scope changes |
| `docs/ARCHITECTURE.md` | Tech stack & diagrams | System changes |
| `README.md` | Project overview | Setup/usage changes |
| `CLAUDE.md` | AI agent instructions | Pattern/convention changes |

## Phase 2: Feature Traceability

Use unique IDs to trace intent from vision to code:

```
Vision IDs:    A1, S1              (in VISION.md)
Plan IDs:      PLAN-001/P0         (in plan files)
Commits:       feat(PLAN-001/P0): description
Code Comments: // See PLAN-001/P2  (where non-obvious)
```

### Traceability Chain
```
VISION.md (feature A1)
  → PLAN-003.md (implements A1)
    → commit: feat(PLAN-003/P1): add payment form
      → code comment: // See PLAN-003/P2 for validation rules
```

Every feature should be traceable from "why we're building it" to "where the code lives."

## Phase 3: CHANGELOG Management

Follow Keep a Changelog format:

```markdown
## [Unreleased]
### Added
- New payment processing module (PLAN-003)

### Changed
- Updated auth flow to support OAuth2 (PLAN-005)

### Fixed
- Cart total calculation rounding error (#142)

### Removed
- Legacy XML export endpoint (deprecated since v2.1)
```

### Rules
- Group by: Added, Changed, Deprecated, Removed, Fixed, Security
- Reference plan IDs or issue numbers
- Write for humans, not machines
- Update with every meaningful PR, not just releases

## Phase 4: Architecture Documentation

`ARCHITECTURE.md` should cover:

1. **System Overview** — High-level diagram of components
2. **Tech Stack** — Languages, frameworks, databases, infrastructure
3. **Directory Structure** — What lives where and why
4. **Data Flow** — How requests move through the system
5. **Key Decisions** — ADRs (Architecture Decision Records) for major choices
6. **External Services** — APIs, third-party integrations, auth providers

### When to Update
- Adding a new service or database
- Changing the deployment architecture
- Introducing a new major dependency
- Modifying the authentication flow
- Restructuring the directory layout

## Phase 5: Post-Completion Walkthroughs

After completing a major feature or plan, create a **Walkthrough Artifact**:

```markdown
## Walkthrough: {Feature Name}

### What Changed
- {Design decisions made and rationale}
- {Key files added or modified}

### Verification
- {What was tested and how}
- {Edge cases covered}

### Evidence
- {Screenshots for UI changes}
- {Test output / coverage report}

### Follow-up
- {Known limitations}
- {Future work identified}
- {Technical debt introduced}
```

## Phase 6: README Standards

A project README must include:

1. **What it is** — One sentence description
2. **Quick start** — How to run it locally in <5 commands
3. **Prerequisites** — Node version, env vars, services needed
4. **Project structure** — Brief directory overview
5. **Available scripts** — npm commands and what they do
6. **Contributing** — Link to CONTRIBUTING.md
7. **License** — Link to LICENSE

## Documentation Anti-Patterns

| Anti-Pattern | Why It's Wrong | Do This Instead |
|-------------|---------------|-----------------|
| "Self-documenting code" as excuse | Complex systems need context code can't provide | Write docs for the *why*, code for the *what* |
| Stale docs | Worse than no docs — actively misleading | Update docs in the same PR as code changes |
| Documenting everything | Noise drowns signal | Document decisions, architecture, and non-obvious patterns |
| Separate doc repo | Falls out of sync immediately | Docs live next to the code they describe |

## Quick Checklist

- [ ] Correct document identified for the change
- [ ] Feature IDs trace from vision to code
- [ ] CHANGELOG updated with this release's changes
- [ ] Architecture docs reflect current system state
- [ ] README is accurate for new contributors
- [ ] No stale documentation left behind
- [ ] Walkthrough created for major features
