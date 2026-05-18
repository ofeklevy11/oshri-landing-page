# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Single-page Hebrew (RTL) lead-generation landing page for **טלרן תקשורת** — fiber internet
availability checks in the Golan region. No build system, no dependencies, no backend.
Each `.html` file is fully self-contained: open it directly in a browser to preview.

## Files

- `index.html` — the current / canonical landing page (~1265 lines, most complete).
- `v1.html`, `v2.html`, `v3.html` — earlier design variants kept for A/B comparison.
  Treat `index.html` as the source of truth; only touch `vN.html` if explicitly asked.

## Architecture

Everything (markup, Tailwind theme config, CSS, JS) lives inline in one HTML file:

- **Styling**: Tailwind via CDN (`cdn.tailwindcss.com`). The brand palette and design
  tokens are defined in the inline `tailwind.config` `<script>` in `<head>` — extend
  colors/fonts there, not with arbitrary hex values in markup.
- **Icons**: Lucide via CDN; rendered by `lucide.createIcons()`. Use `<i data-lucide="name">`.
- **Fonts**: Heebo + Assistant from Google Fonts (Hebrew-first).
- **Sections**: `<main id="main">` holds anchored `<section>` blocks (`#benefits`, `#local`,
  `#how`, `#lead-form`, `#faq`). The header nav and mobile sticky CTA link to these anchors.
- **Inline JS** (bottom `<script>`) handles: sticky-header scroll state, mobile menu toggle,
  mobile CTA bar reveal, `IntersectionObserver` scroll reveals (`.reveal` / `.reveal-stagger`
  → `.in`), FAQ `<details>` aria sync, and form handling.

## Forms — important

Forms are identified by `data-form="hero"` / `data-form="main"`, paired with a matching
`[data-success="<key>"]` block that is unhidden on submit. **Submission is client-side only**:
it validates, swaps in the success state, and `console.log`s the lead. There is a
`// TODO: send to backend / webhook` — no data is actually sent anywhere yet. If asked to
"make the form work," that means wiring this handler to a real endpoint/webhook.

Phone inputs validate against `pattern="^0\d{8,9}$"` (Israeli format). Keep RTL (`dir="rtl"`,
`lang="he"`) and Hebrew copy intact when editing.

## Conventions

- Preserve the existing Tailwind utility style and the custom `bg-hero` / `bg-mesh` /
  `bg-cta` / `input-field` / `shadow-soft` classes defined in the inline config & `<style>`.
- No package manager or test suite exists — verify changes by opening the file in a browser
  and checking the responsive (mobile/desktop) layout and form success flow.
