# bauma-demo-page

Static GitHub Pages site used as the staging iframe parent for **Excavator Scout** during MVP development.

The legacy `bauma` repo prefix is intentional and kept for historical reasons. The actual product is **Excavator Scout** (`https://app.excavatorscout.com/` once production launches).

## Purpose

This repository hosts a single `index.html` that loads the embeddable Excavator Scout chat (`/embed/chat`) inside an iframe so the iframe security path can be exercised end-to-end against a real cross-origin parent before the supplier (Hydraulikdichtungen24) pastes the snippet into their JTL shop.

## How it ties into the MVP

- **STORY-004** (Supplier workspace + embed config) seeds the first supplier workspace's `allowed_origins` with this site's GitHub Pages URL so the embed bootstrap accepts it as a valid parent origin.
- **STORY-009** (Iframe security) runs its Playwright E2E coverage against this page. The page is the parent; the Excavator Scout app is the iframe child.

## Configuration

The demo loads the iframe against an app origin you can set in the UI. Defaults:

- App origin: `https://chere-prowessed-uncustomarily.ngrok-free.dev` (the project's pinned ngrok tunnel pointing at local `localhost:3000`)
- Tenant id: `default`
- Shop id: empty

Values are persisted to `localStorage` so refreshes do not lose state.

## Updating the page

Edit `index.html` and push to `main`. GitHub Pages serves the `main` branch root.

## Why a separate repo

Keeping the staging parent in its own repo makes the cross-origin requirement honest: the Excavator Scout app and the parent page have different origins (`*.vercel.app` / ngrok vs `*.github.io`), which is what the iframe security path needs to validate.
