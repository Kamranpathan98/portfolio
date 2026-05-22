# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Stack

Single-file portfolio: pure HTML + CSS + vanilla JS in `index.html`. No build tools, no npm, no frameworks. Open the file directly in a browser.

## Fonts

Loaded from Google Fonts — **do not change**:
- `Syne` → all headings (h1–h4), section titles, project titles
- `Inter` → all body text, paragraphs, descriptions
- `JetBrains Mono` → eyebrow labels, tags, stack chips, metric labels, timestamps

This combination (Syne + Inter + JetBrains Mono) is intentional — it creates the Linear/Vercel aesthetic that differentiates the site.

## Design tokens

All colours, spacing, and typography live in `:root` CSS custom properties at the top of `<style>`. Edit tokens there; do not hardcode values elsewhere.

## Architecture

All content is in one file in this order:
1. `<style>` block — tokens → global → navbar → hero/metrics panel → projects → principles → skills → experience → writing → contact → footer → responsive breakpoints
2. HTML sections in the same order, each preceded by a `<div class="section-divider">`
3. `<script>` block at end of `<body>` — navbar scroll, active-section detection, scroll reveal (IntersectionObserver), smooth scroll, email clipboard copy, and the live metrics panel (counters, sparkline SVG, infra bars, event stream feed)

## Live metrics panel

The hero right column hosts a fake-live telemetry panel:
- `S` object holds mutable state (rps, lat, err, dau, cpu, mem, io, p95[], p50[])
- `walk(v, vol, lo, hi)` random-walks each value every 1100ms
- `tween(el, target, dec)` animates counter DOM updates via `requestAnimationFrame`
- `drawSparkline()` recomputes SVG path `d` attributes from `S.p95` / `S.p50` arrays
- Event feed pushes a new row every 1800ms, keeping the last 7 visible

## Responsive breakpoints

| Breakpoint | Change |
|---|---|
| 1100px | skills grid → 2 columns |
| 960px | hero → 1 column, contact → 1 column, principles → 2 columns |
| 820px | section padding → 70px, metrics body → 1 column, hamburger nav |
| 600px | h1 → 36px, skills/principles/writing → 1 column, contact card padding → 28px |
