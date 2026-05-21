# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Speed is not a goal

Take the time to do this right. Don't optimize for finishing fast — optimize for getting it correct, readable, and consistent with the rest of the mock. If a change needs a careful pass over the palette tokens, accessibility attributes, or responsive breakpoints, do that pass.

## What this repo is

Lumora is currently a **design mock only** — there is no application, no build, no tests. The deliverable is a set of self-contained HTML files under `docs/mocks/`, each with inline CSS and inline SVG. The product is a teeth-whitening / smile-journey app; the mocks depict the post-login experience.

| File | Screen | Marked as current in nav |
|------|--------|--------------------------|
| `docs/mocks/index.html` | Dashboard — hero, Smile Health Index, journey strip, daily-overview grid, milestone | Dashboard |
| `docs/mocks/routine.html` | Today's routine — session hero, ordered steps, weekly intensity strip, reminders + sensitivity settings | Routine |
| `docs/mocks/progress.html` | Progress — KPI hero, 21-day shade chart, daily-photo grid, streak calendar, milestone list, insights | Progress |
| `docs/mocks/shop.html` | Shop — member-perks banner, category chips, recommended row, product grid, auto-deliver + rewards | Shop |

The dashboard also has rendered screenshots at three breakpoints (mobile/tablet/desktop) committed as PNGs next to the mock. The other three pages do not currently have committed screenshots.

All four pages share the same chrome (app bar with brand mark, primary nav, avatar) and link to each other via the nav. Each page is intentionally self-contained — CSS is **not** extracted to a shared stylesheet because designers iterate per-screen and the base block is small.

## Working on the mock

- Open `docs/mocks/index.html` directly in a browser — there is no dev server, no bundler, no package manager. Just reload the file after edits.
- All styles are in one `<style>` block at the top of the file. There is no external CSS, no JS framework, no dependencies.
- Icons are inline SVG. Fonts use the system stack with Inter as the first preference (no webfont is loaded).
- The "screenshots" in `docs/mocks/` (`screenshot-mobile.png`, `screenshot-tablet.png`, `screenshot-desktop.png`, plus the legacy `mock-page-screenshot.png`) are committed artifacts — regenerate them by capturing the rendered page at the corresponding breakpoint when the design changes meaningfully.

## Design system rules (do not violate)

The palette is **strictly five tokens**, defined as CSS custom properties at the top of `index.html` and named after an "ice" theme:

| Token     | Hex       | Role |
|-----------|-----------|------|
| `--ink`   | `#002E99` | deep indigo — primary text, key strokes |
| `--blue`  | `#5276CC` | medium blue — secondary, interactive |
| `--ice`   | `#80E9FF` | ice cyan — accent, glow, milestone |
| `--snow`  | `#FFFCFB` | snow white — surface, background |
| `--stone` | `#CCC8C7` | warm gray — borders, muted, neutrals |

Any additional color values **must** be alpha tints of one of these five (see the `--ink-08`, `--blue-15`, `--ice-25`, `--stone-30`, etc. helpers already in the file). Do not introduce new hex literals outside these tokens — the `8110d01` refactor explicitly tightened this and PRs have been reviewed for compliance.

Other invariants in use:
- Radii live on `--radius-sm|md|lg|xl`; shadows on `--shadow-sm|md|lg`; focus ring on `--ring`.
- Responsive breakpoints in this file are `640px`, `768px`, `1024px`.
- Animations respect `prefers-reduced-motion` via a global override at the bottom of the stylesheet — keep new animations under that umbrella.

## Accessibility expectations

Prior PRs (`d1b6307`, `4b0ccd0`) specifically improved semantics. When editing, preserve:
- `<button type="button">` for actions (not `<div>` or `<a>`).
- `aria-label` / `aria-current` on the nav and avatar.
- `role="img"` + `aria-label` on decorative SVG composites that convey data (e.g. the Smile Health Index halo, shade row).
- Decorative-only SVGs marked `aria-hidden="true"`.
