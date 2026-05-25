# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

This repo is a static web arcade deployed to `cobaltmeadow.work` (see `CNAME`) via GitHub Pages. Each game is a **single self-contained HTML file** at the repo root with inline CSS and inline JavaScript — there is no build step, no package manager, no bundler, and no shared module file. `index.html` is the arcade landing page; each tile links to one game file.

To run locally, serve the directory over a static HTTP server (e.g. `python3 -m http.server`) and open `http://localhost:8000/`. Opening files via `file://` works for most games but will break the few that use ES modules / importmaps.

There are no tests, no linter, and no CI in this repo.

## Adding or modifying a game

- Each game owns its CSS and JS — there is no shared stylesheet or helper file, so changes are local to one HTML file. When adding a new game, copy the structure of an existing one rather than trying to share code across files.
- After creating a new game file, wire it into the arcade by adding a `<a class="tile ..." href="new-game.html">` tile in `index.html` matching the style of the existing tiles.
- Mobile/iOS standalone-app behavior matters: most games include `apple-mobile-web-app-capable`, `viewport-fit=cover`, and `env(safe-area-inset-*)` padding. Preserve these meta tags and safe-area handling when editing layouts.

## Game-specific architecture notes

- **`race-cars.html`, `marble-run.html`, `city-marbles.html`, `orb-rush.html`** use Three.js loaded via `<script type="importmap">` pointing at `unpkg.com/three@0.160.0`. Their main script is `type="module"`. These games require an HTTP server (importmaps don't resolve from `file://`).
- **`maximum-race-cars.html`** is the only networked game: two-player realtime over WebRTC via PeerJS (loaded from `unpkg.com/peerjs@1.5.4`). It uses Google STUN + a public TURN relay for NAT traversal. Two URL params override defaults:
  - `?relay=1` forces TURN-only mode (for client-isolated WiFi / strict NATs)
  - `?turn=user:pass@host:port` supplies a custom TURN server
  When debugging multiplayer issues, the "connection test" diagnostic in the UI is the right starting point.
- **Several games persist nothing**; `improvements.md` is a playtest punch-list noting that progress persistence (localStorage), a unified SFX library, and OG/Twitter share-card meta tags are still open work items across the arcade. Treat that file as the canonical TODO list when picking up open-ended polish work.
