!# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` — start Vite dev server at http://localhost:5173
- `npm run build` — production build to `dist/`
- `npm run preview` — serve the production build locally
- `npm run lint` — run ESLint over the repo (flat config in `eslint.config.js`)

There is no test runner configured in this project.

## Architecture

Single-page React 19 app bootstrapped with Vite. The entire application lives in one component:

- `src/main.jsx` — React entry point; mounts `<App />` in `StrictMode`
- `src/App.jsx` — the whole app: transaction state, add-transaction form, filters, and summary cards. All state is held in `useState` hooks inside `App`; there is no router, no context, no backend, and no persistence (refreshing the page resets to the hard-coded seed transactions).
- `src/App.css` / `src/index.css` — all styling, plain CSS (no Tailwind, CSS-in-JS, or modules)

The `transactions` array is the single source of truth; derived values (`totalIncome`, `totalExpenses`, `balance`, `filteredTransactions`) are recomputed inline on each render.

### Known starter-project quirks

This repo is the starting point for a Claude Code course (see `README.md`) and the author has **intentionally** left a bug and messy code to be fixed during the course. Notably, `amount` is stored as a string on each transaction, so `reduce((sum, t) => sum + t.amount, 0)` in `App.jsx` concatenates instead of summing. Treat this as expected starter state unless the user asks to fix it.

## Conventions

- ESLint rule `no-unused-vars` ignores identifiers matching `^[A-Z_]` (allows unused `PascalCase`/`CONSTANT` imports).
- `dist` is globally ignored by ESLint.
