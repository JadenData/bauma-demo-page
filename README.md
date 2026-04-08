# bauma-demo-page

Static GitHub Pages site used as the staging iframe parent for **Excavator Scout** during MVP development.

The legacy `bauma` repo prefix is intentional and kept for historical reasons. The actual product is **Excavator Scout** (`https://app.excavatorscout.com/` once production launches).

## Purpose

This repository hosts a single `index.html` that loads the embeddable Excavator Scout chat (`/embed/chat`) inside an iframe so the iframe security path can be exercised end-to-end against a real cross-origin parent before the supplier (Hydraulikdichtungen24) pastes the snippet into their JTL shop.

## How it ties into the MVP

- **STORY-004** (Supplier workspace + embed config) seeds the first supplier workspace's `allowed_origins` with this site's GitHub Pages URL so the embed bootstrap accepts it as a valid parent origin.
- **STORY-009** (Iframe security) runs its Playwright E2E coverage against this page. The page is the parent; the Excavator Scout app is the iframe child.

## Configuration

The demo loads the iframe against an app origin you can set in the UI. There are two supported app origins for the iframe child during MVP development:

- `https://chere-prowessed-uncustomarily.ngrok-free.dev` — the project's pinned ngrok tunnel pointing at local `localhost:3000`. Default. Use when running the app locally with `ngrok http --url=chere-prowessed-uncustomarily.ngrok-free.dev 3000`.
- `https://excavatorscout.vercel.app/` — Vercel preview deployment. Use when ngrok is not running.

The page exposes one-click "Use ngrok" / "Use Vercel" preset buttons. Tenant id defaults to `default`; shop id is optional. Values are persisted to `localStorage` (key `exsc-demo-config-v1`) so refreshes do not lose state.

## Ralph permissions

Ralph (the autonomous agent driving the MVP) is **explicitly allowed to modify this page**. This includes:

- editing `index.html` (HTML, CSS, JavaScript, presets, debug controls)
- changing the default app origin or tenant id
- adding new debug surfaces (origin echo, console mirror, network log, etc.) that help iframe E2E coverage in STORY-009
- pushing directly to `main` — GitHub Pages auto-deploys from the `main` branch root

The only constraint is that this repo intentionally retains the legacy `bauma` prefix; do not rename it.

## Updating the page

Edit `index.html` and push to `main`. GitHub Pages serves the `main` branch root. The `.nojekyll` file disables Jekyll processing so all paths are served verbatim.

## Why a separate repo

Keeping the staging parent in its own repo makes the cross-origin requirement honest: the Excavator Scout app and the parent page have different origins (`*.vercel.app` / ngrok vs `*.github.io`), which is what the iframe security path needs to validate. The parent origin (always `https://jadendata.github.io`) is what STORY-004 must seed into the supplier workspace `allowed_origins`.
