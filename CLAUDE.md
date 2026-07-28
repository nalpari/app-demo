# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## This is NOT the Next.js you know

Next 16.2 + React 19.2. APIs, conventions, and file structure differ from your training data.
Read the relevant guide in `node_modules/next/dist/docs/` (App Router docs live under `01-app/`)
before writing any code. Heed deprecation notices.

## Commands

This repo uses **bun** (pinned via `packageManager`, lockfile `bun.lock`). Never run
`npm install` here — it would regenerate a `package-lock.json` alongside `bun.lock`.

```bash
bun install
bun dev        # dev server on :3000 (Turbopack is the default bundler; --webpack to opt out)
bun run build  # note: `bun build` is bun's own bundler, not this script
bun start      # serve the production build
bun lint       # eslint (flat config)
```

No test runner is configured.

bun blocks dependency lifecycle scripts unless the package is in its default-trusted list or
in `trustedDependencies` in `package.json` (currently `unrs-resolver`). If an install prints
"Blocked N postinstall", run `bun pm untrusted` to see what, then `bun pm trust <pkg>`.

## Architecture

Bare `create-next-app` scaffold — App Router only, no API routes, no state library, no data layer yet.

- `src/app/` — routes. `@/*` maps to `./src/*` (tsconfig paths).
- **React Compiler is on** (`reactCompiler: true` in `next.config.ts`, via `babel-plugin-react-compiler`).
  Don't hand-add `useMemo`/`useCallback`/`memo`; the compiler handles memoization. Do keep components
  rules-of-React clean, since the compiler bails out on violations.
- **Tailwind v4, CSS-first config** — there is no `tailwind.config.*`. Theme tokens are declared in
  `src/app/globals.css` under `@theme inline`; add design tokens there, not in a JS config.
- Geist fonts are loaded in `layout.tsx` and exposed as `--font-geist-sans` / `--font-geist-mono`,
  wired to Tailwind's `--font-sans` / `--font-mono`.

`AGENTS.md` just re-exports this file.

## Always Do it

- 모든 결과와 추론과정은 한국어로 한다.
- 실제 앱을 구현하기 전에 디자인 먼저 적용한다.
- 디비는 추후 구성할 예정이라 dummy data로 대체한다.(json)