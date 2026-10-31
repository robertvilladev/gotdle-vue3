# Gotdle Roadmap: frontend

Gotdle is a daily guessing game set in the Game of Thrones world, inspired by
[loldle.net](https://loldle.net/) and [narutodle.net](https://narutodle.net/).
Every day each game mode has one answer. It's the same answer for every player,
and it resets at 00:00 UTC.

Each repo has its own roadmap with the **same phase numbers**:

- Frontend: this file
- Backend: [gotdle-api-fast-api/ROADMAP.md](https://github.com/robertvilladev/gotdle-api-fast-api/blob/main/ROADMAP.md)

When a task depends on the other repo, it says so (`needs BE x.y`). Tick the
checkbox in the same PR that finishes the work.

## MVP goal

> A player opens Gotdle, plays today's **Classic** (guess the character) and
> **Quote** challenges, gets per-guess feedback, and their streak persists
> in their browser. New content and new modes can be added **by adding data,
> not by redesigning**.

**In MVP:** Classic mode, Quote mode, daily rotation, local streaks/stats, deployment.
**After MVP:** Scene → season mode (images), accounts, global stats, more modes.

## Frontend design rules

1. **The backend checks guesses.** The client never receives today's answer
   before the player has won or given up.
2. **No accounts in MVP.** Game progress, streaks and stats live in `localStorage`,
   keyed by mode + UTC date.
3. **Modes share one game flow.** Guess input → feedback → win/lose → countdown.
   A new mode should mean a new feedback view, not a new game engine.
4. **Server state goes through TanStack Query, client state through Pinia.** Never
   copy query data into a store.

## Testing policy (TDD, without slop)

Write the test first **for logic that can silently be wrong**:

- game state transitions (guessing, win, lose, already played today);
- streak and day rollover at 00:00 UTC;
- rules that map feedback to visuals (correct / partial / wrong, ↑↓ arrows);
- e2e: **one** happy path per mode (Playwright).

**Don't** write tests for static markup, framework wiring, snapshots or
plain stores. If deleting a test wouldn't let a real bug ship, the test shouldn't
exist. CI target: **< 2 min**.

## Decisions

| #   | Decision        | Outcome                                                                   |
| --- | --------------- | ------------------------------------------------------------------------- |
| F1  | Scaffold        | Fresh `create-vue` (Vue 3.5, Vite 8, TS, Router, Pinia)                   |
| F2  | Package manager | pnpm                                                                      |
| F3  | Styling         | Tailwind CSS v4 (`@tailwindcss/vite`)                                     |
| F4  | Server state    | TanStack Query (`@tanstack/vue-query`)                                    |
| F5  | Unit tests      | Vitest + @vue/test-utils (jsdom)                                          |
| F6  | e2e             | Playwright, one happy path per mode                                       |
| F7  | Lint / format   | oxlint + ESLint + Prettier                                                |
| F8  | API types       | `openapi-typescript` from the backend `/openapi.json` (set up in Phase 2) |
| F9  | Hosting         | open, decide in Phase 5                                                   |

---

## Phase 0: Foundations

### 0.1 Tooling (setup PR)

- [x] Audit the current state; write `CLAUDE.md`, `README.md`, `ROADMAP.md`
- [x] Fresh `create-vue` scaffold with no demo code
- [x] Tailwind v4, TanStack Query, Playwright wired in
- [x] CI: lint, type-check, unit tests, build, e2e

### 0.2 Look and feel (Claude Design)

Goal: a small design system to build the UI on, so it isn't improvised per screen.

- [ ] Mood board / art direction. GoT-inspired, but original: no HBO logos or
      fonts. Try parchment + iron + dark, plus a high-contrast option.
- [ ] Design tokens: palette (incl. feedback colors that work for colorblind
      players), type scale, spacing, radius
- [ ] Key screens: home/mode picker, Classic board, Quote screen, win modal,
      stats modal (mobile first)
- [ ] Core components: guess input with autocomplete, feedback cell/row,
      mode card, countdown, modal
- [ ] Turn the tokens into Tailwind `@theme` variables in `src/assets/main.css`

## Phase 1: App shell

- [ ] Layout, header, mode picker, routing per mode (`/classic`, `/quote`)
- [ ] API client + `VueQueryPlugin` defaults (base URL from `VITE_API_URL`)
- [ ] Local persistence composable for game state + streaks (TDD: day rollover)

## Phase 2: Classic mode (end to end)

- [ ] Generate API types with `openapi-typescript` (needs BE 2.x endpoints)
- [ ] Guess input with autocomplete (needs BE `GET /characters/search`)
- [ ] Feedback grid: green / yellow / red, ↑↓ for seasons (TDD: mapping rules)
- [ ] Win state, "come back tomorrow" countdown to 00:00 UTC
- [ ] e2e happy path

## Phase 3: Quote mode

- [ ] Quote screen that reuses the guess flow (needs BE quote mode)
- [ ] Progressive hints after N wrong guesses
- [ ] e2e happy path

## Phase 4: Polish and launch

- [ ] Share result (emoji grid to the clipboard)
- [ ] Stats modal (played, win %, current/max streak, guess distribution)
- [ ] Accessibility pass (keyboard, contrast, screen-reader labels on feedback)
- [ ] Deploy (static hosting) + error monitoring + privacy-friendly analytics

## Post-MVP

- Scene → season mode (image viewer, zoom/blur hints)
- Accounts + cross-device streaks
- i18n (ES/EN)
