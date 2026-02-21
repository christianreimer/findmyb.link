# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Find My B.link (findmyb.link) is a lightweight single-page web app that synchronizes screen color flashes across mobile devices. Users share a URL-encoded configuration (colors, morse pattern, time offset) so two phones can blink in sync to help people find each other.

## Build & Development Commands

```bash
npm run build          # Build CSS + TypeScript
npm run build:css      # Tailwind: app.css → styles.css
npm run build:ts       # TypeScript: code.ts → code.js
npm run dev            # Watch mode for both CSS and TS
npm run watch:css      # Watch Tailwind only
npm run watch:ts       # Watch TypeScript only
./serve.sh             # Python HTTP server on 0.0.0.0:8000
```

No test framework or linter is configured.

## Tech Stack

- **TypeScript** (strict mode, ES2020 target) — single source file `code.ts` compiles to `code.js`
- **Tailwind CSS v4** with **DaisyUI v5** — `app.css` (source with custom animations) compiles to `styles.css`
- **No framework** — vanilla TS/JS, no bundler, no backend
- Deployed to **GitHub Pages** via GitHub Actions on release (`.github/workflows/release-pages.yml`)

## Architecture

**Entry point:** `index.html` loads `styles.css` and `code.js` directly (no bundler).

**State machine with 3 states:** `WELCOME` → `COUNTDOWN` → `RUNNING`

**Two roles:** `INITIATOR` (creates and shares a link) and `RECEIVER` (opens the shared link).

**Core config (`BlinkConfig`):** Encodes color pair, morse pattern index, and time offset as URL query params (`c1`, `c2`, `p`, `t`). The receiver's URL contains all config needed to synchronize.

**Time synchronization:** Uses wall-clock seconds with a configurable time offset (0-9) so both devices start their blink pattern at the same second boundary.

**Morse patterns:** 10 predefined dot/dash patterns control flash timing. Dots = 250ms, dashes = 750ms, 100ms between elements, 1000ms between repeats.

**Colors:** 6 DaisyUI theme colors (secondary, accent, info, success, warning, error) used as CSS variable references.

**Custom CSS animations** in `app.css`: phone rotation (`.rotate10`), word fade (`.word-fade`), and fade in/out keyframes.
