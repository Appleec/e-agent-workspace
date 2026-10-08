# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

E-Agent workspace — a Node.js project (CommonJS) with TypeScript and Vue 3. Currently in early bootstrapping phase with no application source code yet. This repository primarily contains the ECC rules framework and agent skills.

## Current State

- **No source code**: No `.ts`, `.vue`, or `.js` application files exist yet
- **No build tools configured**: TypeScript, Vite, Vitest, ESLint, and Prettier are planned but not yet installed or configured
- **Rules framework ready**: The `rules/` directory contains the ECC rules system
- **Skills installed**: git-workflow and dev-team skills are available in `.claude/skills/`

## Tech Stack (Planned)

- **Language**: TypeScript
- **Frontend**: Vue 3 (Composition API, `<script setup lang="ts">`)
- **State management**: Pinia (setup stores)
- **Routing**: vue-router (lazy-loaded)
- **Server cache**: @tanstack/vue-query
- **Validation**: Zod
- **Testing**: Vitest + @vue/test-utils (unit/component), Playwright (E2E)
- **Lint/Format**: ESLint flat config with `vue/vue3-recommended`, Prettier
- **Typecheck**: `vue-tsc --noEmit` (do not use plain `tsc` — it cannot read `.vue` SFCs)

## Commands (To Be Configured)

These commands will be available once the project is bootstrapped. Currently, no build tools are installed.

```bash
# Install dependencies (when configured)
npm install

# Development server (when configured)
npm run dev

# Type checking (when configured)
npm run typecheck  # vue-tsc --noEmit

# Linting (when configured)
npm run lint

# Formatting (when configured)
npm run format

# Testing (when configured)
npm run test           # Run all tests
npm run test:unit      # Unit tests only
npm run test:e2e       # E2E tests only
npm run test:coverage  # With coverage report

# Build for production (when configured)
npm run build
```

## Rules System

This repo contains an ECC rules framework (`rules/`) that defines layered coding standards. See `rules/README.md` for full documentation.

### Structure

Rules are organized into a **common** layer plus **language-specific** directories:

- **`rules/common/`** — Language-agnostic principles: coding style, testing (TDD, 80% coverage), patterns, security, performance, agent orchestration, git workflow, code review
- **`rules/typescript/`** — TypeScript/JavaScript extensions (types, immutability, Zod validation)
- **`rules/vue/`** — Vue 3 extensions (SFC structure, composables, Pinia, vue-query, XSS vectors)
- Additional language-specific rules available: angular, nuxt, python, golang, web, react-native, swift, php, ruby, arkts

### Installing Rules

**Option 1: Install Script (Recommended)**

```bash
# Install common + one or more language-specific rule sets
./install.sh typescript
./install.sh vue
./install.sh typescript vue  # Multiple at once
```

**Option 2: Manual Installation**

```bash
# Create the ECC rule namespace
mkdir -p ~/.claude/rules/ecc

# Install common rules (required for all projects)
cp -r rules/common ~/.claude/rules/ecc/

# Install language-specific rules based on your project's tech stack
cp -r rules/typescript ~/.claude/rules/ecc/
cp -r rules/vue ~/.claude/rules/ecc/
```

For project-local rules:

```bash
mkdir -p .claude/rules/ecc
cp -r rules/common .claude/rules/ecc/
cp -r rules/typescript .claude/rules/ecc/
```

### Rule Priority

Language-specific rules override common rules where idioms differ. Rules in `rules/common/` that may be overridden are marked with language notes.

## Skills System

Custom skills are installed in `.claude/skills/` and available via the Skill tool:

- **git-workflow**: Git workflow patterns, branching strategies, commit conventions, merge vs rebase, conflict resolution
- **dev-team**: Multi-persona session (PM, Architect, Developer, QA) for collaborative design and planning

Skills provide deep reference material for specific tasks, while rules define standards and conventions.

## Agent-Driven Workflow

Development follows this pipeline for all features:

1. **Research first**: `gh search` → primary docs → package registries — prefer existing solutions over new code
2. **Plan**: use planner agent to generate PRD, architecture, task list
3. **TDD**: use tdd-guide agent — write tests first, implement, refactor
4. **Code review**: use code-reviewer agent immediately after writing code
5. **Commit**: conventional commits format, then open PR

For security-sensitive code (auth, payments, user data, DB queries, crypto), stop and use the security-reviewer agent before committing.

When delegating to agents: always wait for results and integrate them — never fire-and-forget.

## Key Conventions

These conventions are enforced by the rules system. The most critical ones:

- **Immutability is critical**: always create new objects, never mutate. Use spread operators.
- **TDD mandatory**: write test first (RED → GREEN → REFACTOR), verify 80%+ coverage
- **Small files**: 200–400 lines typical, 800 lines soft ceiling for source files
- **Small functions**: <50 lines
- **Max nesting depth**: 4 levels
- **No `any`**: use `unknown` + narrowing, `interface` for objects, `type` for unions/intersections
- **No `console.log`** in production code; use a proper logger
- **Conventional Commits**: `<type>: <description>` — types: feat, fix, refactor, docs, test, chore, perf, ci
- **API responses**: use a consistent envelope (`ApiResponse<T>` with `success`, `data`, `error`, `meta`)

For complete coding standards, refer to the rules in `rules/common/`, `rules/typescript/`, and `rules/vue/`.