# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

NeuroKids — an AI school landing page for Almaty, Kazakhstan. Single-file static HTML (`neurokids-landing.html`). No build system, no npm, no framework. All CSS and JS are inline in the HTML file.

**Deploy:** Vercel auto-deploys from GitHub (`github.com/iagadilov/neurokids`, branch `main`) to `neurokids-lemon.vercel.app`. `vercel.json` rewrites `/` → `/neurokids-landing.html`.

**Preview locally:** open `neurokids-landing.html` directly in a browser, or run `python3 -m http.server 8080` in this directory.

**Test on mobile:** use Playwright at 390×844 viewport; always navigate AFTER `browser_resize` to get correct `window.innerWidth`.

## Architecture

Everything lives in `neurokids-landing.html` (~2400 lines):

1. **`<head>` / CSS** (lines ~10–1380) — all styles inline in `<style>`. Structured in comment blocks: `RESET & BASE → CSS VARIABLES → TYPOGRAPHY → LAYOUT → component sections → TABLET (≤1024px) → MOBILE (≤768px) → SMALL (≤480px)`.
2. **`<body>` / HTML** (lines ~1383–2300) — section order: NAV → HERO → TRUST BAR → FOR WHOM → STUDENT WORKS → COURSES → WHY AI → HOW IT WORKS → PRICING → DIPLOMA → TEACHERS → REVIEWS → CTA → FOOTER.
3. **`<script>`** (lines ~2300–2400) — vanilla JS: scroll reveal (`IntersectionObserver`), navbar scroll effect, hamburger menu, teachers carousel drag-scroll, CTA form → WhatsApp redirect, phone mask.

## CSS Conventions

**Variables** (`:root`):
- `--bg: #080C15`, `--purple: #7C3AED`, `--amber: #F59E0B`
- `--text: #E8EDF5`, `--text-2: #94A3B8`
- `--radius: 20px`, `--card: #111827`

**Typography classes:** `.t-hero` (Unbounded 800, clamp 2.2–3.8rem), `.t-h2`, `.t-h3`, `.t-label`.

**Mobile media queries require `!important`** on all overrides because some sections use inline `style=""` attributes that CSS can't override without it.

**CSS Grid + `min-width: 0`:** grid children must have `min-width: 0` on mobile to prevent `1fr` columns from expanding beyond the viewport (Unbounded font's min-content is wide enough to trigger this).

## Key Patterns

**Scroll reveal:** add class `reveal` (and optionally `reveal-delay-1` through `reveal-delay-4`) to any element. The `IntersectionObserver` in JS adds `visible` when it enters the viewport.

**Hero on mobile:**
- Floating cards (`.hero-floating-card`) are `display: none !important` at ≤768px.
- Replaced by `.hero-proj-strip` — a horizontal scroll strip of YouTube thumbnails.
- `.hero-visual` must be `flex-direction: column` on mobile (default `row` would place the strip beside the robot, not below it).

**YouTube thumbnails** (no API key): `https://img.youtube.com/vi/{VIDEO_ID}/mqdefault.jpg`

**CTA form** collects name/phone/course/format and opens WhatsApp: `https://wa.me/77027501407?text=...`

## Assets

Teacher photos: `https://www.neurokids.kz/images/teachers/{filename}`  
Robot images, module illustrations: Tilda CDN (`static.tildacdn.pro/...`)  
Logo: `https://www.neurokids.kz/attached-logo.png`

Full asset inventory with verified URLs: `neurokids-assets.md`  
Business context, copy, pricing, social links: `neurokids-business-context.md`
