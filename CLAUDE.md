!# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` — start Vite dev server at http://localhost:5173
- `npm run build` — production build to `dist/`
- `npm run preview` — serve the production build locally
- `npm run lint` — run ESLint over the repo (flat config in `eslint.config.js`)

There is no test runner configured in this project.

## Architecture

Single-page React 19 app bootstrapped with Vite. No router, no context, no backend, no persistence — refreshing the page resets to the hard-coded seed transactions.

- `src/main.jsx` — React entry point; mounts `<App />` in `StrictMode`
- `src/App.jsx` — holds the `transactions` array (the single source of truth) and the shared `categories` list, and wires the three child components. Exposes `addTransaction` to the form.
- `src/Summary.jsx` — receives `transactions`, derives `totalIncome` / `totalExpenses` / `balance` inline, renders the three summary cards.
- `src/TransactionForm.jsx` — owns its own form-field state (description/amount/type/category) and calls `onAdd(transaction)` on submit; new rows are appended by `App`.
- `src/TransactionList.jsx` — owns its own `filterType` / `filterCategory` state and renders the filtered table.
- `src/App.css` / `src/index.css` — all styling, plain CSS (no Tailwind, CSS-in-JS, or modules).

Form state and filter state each live inside the component that uses them; only `transactions` is lifted into `App`. `amount` is still stored as a string on each transaction (both in the seed data and from `<input type="number">`), so anywhere it is summed it must be coerced — see `Summary.jsx` where the reducers call `Number(t.amount)` before adding. Keep that coercion whenever you introduce new math over `amount`.

### Starter-project background

This repo was the starting point for a Claude Code course (see `README.md`) and originally shipped with an intentional bug: the summary reducers used `sum + t.amount` on string amounts, so the totals concatenated instead of adding. That bug has since been fixed by wrapping each `amount` in `Number(...)` in `Summary.jsx`.

## Conventions

- ESLint rule `no-unused-vars` ignores identifiers matching `^[A-Z_]` (allows unused `PascalCase`/`CONSTANT` imports).
- `dist` is globally ignored by ESLint.
