---
name: pwp-bootstrap
description: "New project initialization protocol — from empty directory to production-ready foundation in one session. Use this skill whenever the user wants to start a new project, initialize a repo, scaffold an application, or set up a codebase from scratch. Also use when they say 'new project', 'start fresh', 'initialize a repo', 'scaffold this', 'set up the project', or 'create a new app'. Covers repo setup, tooling config, folder structure, hello world verification, and CI/CD."
---

# Project Bootstrap Skill

This skill defines how to start a new project from scratch. Covers repository setup, folder structure, configuration, and the first commit.

## Bootstrap Mindset

- **Foundation before features.** A well-initialized project saves hundreds of hours later.
- **Conventions from day one.** It's 10x harder to adopt conventions after code exists.
- **Ship the skeleton first.** Get the project building and deploying before writing features.
- **Document decisions early.** The first ADR is the cheapest to write.

## Bootstrap Checklist

### Phase 1: Repository Foundation (5 min)
- [ ] Git initialized
- [ ] Package manager configured
- [ ] .gitignore created
- [ ] First commit: `chore: initialize repository`

### Phase 2: Governance Files (5 min)
- [ ] README.md — name, description, quick start
- [ ] LICENSE
- [ ] .env.example with placeholder values

### Phase 3: Tooling Configuration (10 min)
- [ ] TypeScript (strict mode)
- [ ] Linter (ESLint / Biome)
- [ ] Formatter (Prettier or equivalent)
- [ ] Build tool (Vite, Next.js, etc.)

### Phase 4: Project Structure (5 min)
```
{project}/
├── src/
│   ├── components/
│   ├── lib/
│   ├── types/
│   ├── hooks/
│   ├── api/
│   └── styles/
├── public/
├── docs/plans/
├── tests/
├── .env.example
├── .gitignore
├── README.md
├── tsconfig.json
└── package.json
```

### Phase 5: Hello World (5 min)
- [ ] Entry point created
- [ ] Builds successfully
- [ ] Runs locally
- [ ] TypeScript compiles cleanly

### Phase 6: CI/CD Setup (optional, 10 min)
- [ ] GitHub Actions for build + type check
- [ ] Deploy pipeline configured
- [ ] Branch protection on main

## CLAUDE.md Template

```markdown
# Project: {name}

## Stack
- {Framework}: {version}
- {Language}: {version}

## Conventions
- Components: PascalCase, co-located with styles/tests
- Utilities: camelCase, in src/lib/
- Tests: co-located as {file}.test.ts

## Commands
- `npm run dev` — development server
- `npm run build` — production build
- `npm test` — run tests
- `npx tsc --noEmit` — type check
```

## Anti-Patterns

| Anti-Pattern | Do This Instead |
|-------------|----------------|
| Starting with features, adding structure later | Structure first, features second |
| No .gitignore from start | Create in first commit |
| Copying config from another project blindly | Configure intentionally |
| Skipping "Hello World" verification | Verify build/run before features |
| No CI from start | Set up CI before first feature |
