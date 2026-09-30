# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

E-Agent workspace — a Node.js project (CommonJS) with TypeScript and Vue 3. Currently in early bootstrapping phase.

## Tech Stack

- **Language**: TypeScript
- **Frontend**: Vue 3 (Composition API, `<script setup lang="ts">`)
- **State management**: Pinia (setup stores)
- **Routing**: vue-router (lazy-loaded)
- **Server cache**: @tanstack/vue-query
- **Validation**: Zod
- **Testing**: Vitest + @vue/test-utils (unit/component), Playwright (E2E)
- **Lint/Format**: ESLint flat config with `vue/vue3-recommended`, Prettier
- **Typecheck**: `vue-tsc --noEmit` (do not use plain `tsc` — it cannot read `.vue` SFCs)

## Rules System

This repo contains an ECC rules framework (`rules/`) that defines layered coding standards:

- **`rules/common/`** — Language-agnostic principles: coding style, testing (TDD, 80% coverage), patterns, security, performance, agent orchestration, git workflow, code review
- **`rules/typescript/`** — TypeScript/JS extensions (types, immutability, Zod validation)
- **`rules/vue/`** — Vue 3 extensions (SFC structure, composables, Pinia, vue-query, XSS vectors)

Language-specific rules override common rules where idioms differ. See `rules/README.md` for full structure and installation.

## Key Conventions

- **Immutability is critical**: always create new objects, never mutate. Use spread operators.
- **TDD mandatory**: write test first (RED → GREEN → REFACTOR), verify 80%+ coverage
- **Small files**: 200–400 lines typical, 800 lines soft ceiling for source files
- **Small functions**: <50 lines
- **Max nesting depth**: 4 levels
- **No `any`**: use `unknown` + narrowing, `interface` for objects, `type` for unions/intersections
- **No `console.log`** in production code; use a proper logger
- **Conventional Commits**: `<type>: <description>` — types: feat, fix, refactor, docs, test, chore, perf, ci
- **API responses**: use a consistent envelope (`ApiResponse<T>` with `success`, `data`, `error`, `meta`)

## Agent-Driven Workflow

Development follows this pipeline for all features:

1. **Research first**: `gh search` → primary docs → package registries — prefer existing solutions over new code
2. **Plan**: use planner agent to generate PRD, architecture, task list
3. **TDD**: use tdd-guide agent — write tests first, implement, refactor
4. **Code review**: use code-reviewer agent immediately after writing code
5. **Commit**: conventional commits format, then open PR

For security-sensitive code (auth, payments, user data, DB queries, crypto), stop and use the security-reviewer agent before committing.

When delegating to agents: always wait for results and integrate them — never fire-and-forget.