# CLAUDE.md: gotdle-vue3 (frontend)

Gotdle is a daily Game of Thrones guessing game (in the spirit of loldle / narutodle).
This repo is the Vue frontend. The API lives in a sibling repo:
`../gotdle-api-fast-api` (FastAPI + PostgreSQL).

**Read [`ROADMAP.md`](ROADMAP.md) first.** It holds the MVP goal, the design rules,
the testing policy, the decisions and the current phase. The backend has its own
roadmap with the same phase numbers. Build UI only after the Phase 0.2 design
system exists, and use its tokens rather than ad-hoc colors and sizes.

## Stack

- Vue 3.5 (`<script setup>` + TypeScript), Vite 8, Vue Router 5, Pinia 4
- Tailwind CSS v4 via `@tailwindcss/vite`. Design tokens go in `@theme` in `src/assets/main.css`
- TanStack Query (`@tanstack/vue-query`) for **all** server state
- Vitest + @vue/test-utils (jsdom) for unit tests; Playwright (Chromium) for e2e
- oxlint + ESLint + Prettier
- pnpm 12, Node `^22.18 || >=24.12`

Decisions and their reasons are in `ROADMAP.md` → "Decisions". Don't add a
dependency when a few lines of code would do.

## Commands

```sh
pnpm install
pnpm dev          # dev server
pnpm test:unit    # vitest (watch; add --run for one shot)
pnpm test:e2e     # playwright (first time: pnpm exec playwright install chromium)
pnpm type-check   # vue-tsc
pnpm lint         # oxlint + eslint (auto-fix)
pnpm format       # prettier, whole repo
pnpm build        # type-check + production build
```

Run lint, type-check, unit tests and build before calling work done. CI
(`.github/workflows/ci.yml`) also runs `prettier --check` and e2e.

## Rules of the game (domain)

- Each mode (Classic, Quote, …) has **one answer per day**. It's the same for every
  player and resets at **00:00 UTC**.
- **The backend checks guesses.** The frontend never knows today's answer before the
  player has won. Don't add an endpoint or a store that leaks it.
- MVP has no accounts. Game progress, streaks and stats live in `localStorage`, keyed
  by mode + UTC date.

## Conventions

- Put components in `src/components`, route pages in `src/views`, stores in `src/stores`
  and composables in `src/composables`.
- Server data goes through TanStack Query (`useQuery`/`useMutation` in a composable
  per endpoint). Never copy it into Pinia. Pinia is only for client state shared
  across views (game progress, streaks). Local UI state stays in the component.
- Use the API types generated from the backend OpenAPI schema. Don't hand-write
  response types.
- Keep it small. No abstraction until there's a second real use of it.

## Testing

TDD for logic that can silently be wrong: game state transitions, streak/day
rollover and guess-feedback rendering rules. No snapshot tests and no tests for
static markup or framework wiring. See `ROADMAP.md` → "Testing policy".
Keep the unit test files next to the code in `__tests__/`.

## Workflow

- Branch off `main`, one roadmap item per PR, and tick its checkbox in `ROADMAP.md`
  in the same PR.
- Write commit messages in Conventional Commits style (`feat:`, `fix:`, `chore:`, `docs:`).
