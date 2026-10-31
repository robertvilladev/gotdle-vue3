# Gotdle: frontend

A daily guessing game set in the Game of Thrones world, inspired by
[loldle.net](https://loldle.net/) and [narutodle.net](https://narutodle.net/).
Every day there's a new character to guess in each mode, and it's the same for
every player.

> Status: **Phase 0 (foundations)**. Nothing is playable yet. See [ROADMAP.md](ROADMAP.md).

## Game modes

| Mode    | Guess                                                                      | Status        |
| ------- | -------------------------------------------------------------------------- | ------------- |
| Classic | A character, from per-attribute feedback (house, status, region, seasons…) | planned (MVP) |
| Quote   | Who said this quote                                                        | planned (MVP) |
| Scene   | Which season a scene is from                                               | post-MVP      |

## Repos

- **Frontend** (this repo): Vue 3 + Vite + TypeScript, Tailwind CSS v4, TanStack Query, Pinia
- **Backend**: [gotdle-api-fast-api](https://github.com/robertvilladev/gotdle-api-fast-api), FastAPI + PostgreSQL

## Getting started

Requires Node `^22.18` or `>=24.12` and [pnpm](https://pnpm.io/).

```sh
pnpm install
pnpm exec playwright install chromium   # once, for e2e tests
pnpm dev
```

The app expects the backend at `http://localhost:8000`. See the backend README to
start it.

## Scripts

| Command           | What it does                  |
| ----------------- | ----------------------------- |
| `pnpm dev`        | Dev server with hot reload    |
| `pnpm build`      | Type-check + production build |
| `pnpm preview`    | Serve the production build    |
| `pnpm test:unit`  | Unit tests (Vitest)           |
| `pnpm test:e2e`   | e2e tests (Playwright)        |
| `pnpm type-check` | `vue-tsc`                     |
| `pnpm lint`       | oxlint + ESLint, auto-fix     |
| `pnpm format`     | Prettier on the whole repo    |

## Docs

- [ROADMAP.md](ROADMAP.md): MVP goal, design rules, decisions, phases
- [CLAUDE.md](CLAUDE.md): working conventions for AI agents (and humans)
