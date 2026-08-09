# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A custom Shopify Online Store 2.0 Liquid theme for **Little J's**, a children's boutique selling children's and women's clothing (sibling sets, occasion collections, ready-to-ship vs. preorder items, activewear, swim, accessories). It started from Shopify's Skeleton Theme (see `README.md`) and has been built out with real storefront sections.

**The shop no longer sells smocked clothing and does not currently offer free shipping over $100.** Both claims were left over from earlier copy and have been removed; don't reintroduce either. The word "smocks" survives only in the store handle (`little-j-s-smocks`), the stale `shop.name`, and one real customer testimonial — all three are correct as-is.

There is no build step, no `package.json`, and no test suite. Shopify CLI serves the `.liquid`/`.json` files directly.

## Commands

```bash
make dev                                    # shopify theme dev --store little-j-s-smocks
shopify theme check                         # lint (extends theme-check:recommended)
shopify theme push / pull                   # deploy / sync from the store
```

`.shopifyignore` controls what CLI operations touch.

## Reference docs in this repo

- **`AGENTS.md`** (1500 lines) — Shopify's canonical Liquid/theme authoring guidance: schema rules, Liquid tags/filters/objects, LiquidDoc, translation and localization standards, worked examples per asset kind. Consult it before writing Liquid you're unsure about; don't duplicate its content here.
- **`.superdesign/design-system.md`** — the brand spec (palette roles, typography scale, layout, component rules, voice). This is the authority for visual decisions.
- **`.superdesign/init/*.md`** — generated snapshots of the current theme: `routes.md` (templates → sections), `pages.md` (per-route dependency trees), `layouts.md` (theme/password layouts, header, footer), `components.md` and `extractable-components.md` (shared snippets and blocks), `theme.md` (token summary plus verbatim `css-variables.liquid`, `critical.css`, `settings_schema.json`). These are snapshots — if you change a section's structure or a token, update the matching file or it will drift.
- `.shopify/metafields.json` — metafield definitions available on the store (e.g. `product.metafields.reviews.rating`).

## Architecture

### Styling: one global sheet, everything else component-scoped

`assets/critical.css` is the *only* stylesheet asset. It holds the reset, base typography (the `h1`–`h3` clamp scale), form/button/`.eyebrow`/`.price--*` primitives, and the section layout grid. Everything else lives in a `{% stylesheet %}` block inside the component that uses it. Do not add new files to `assets/` for CSS — `{% stylesheet %}` output is concatenated by Shopify.

Design tokens flow: `config/settings_schema.json` (merchant settings) → `snippets/css-variables.liquid` (inlined `{% style %}` in `<head>`) → `--color-*`, `--page-width`, `--page-margin`, `--style-border-radius-*`, `--motion-ease` CSS variables. **Never hardcode a brand color or radius in component CSS** — read the variable. Adding a new token means adding a setting in `settings_schema.json` *and* emitting it in `css-variables.liquid`.

### Section layout grid

Every `.shopify-section` is a 3-column grid: gutter / content / gutter. Direct children land in column 2 automatically; add `class="full-width"` to a direct child to bleed edge-to-edge. Vertical rhythm comes from `.section-spacing` (`padding-block: clamp(4rem, 9vw, 8rem)`). Section markup should therefore start with a plain `<section class="… section-spacing">` and let the grid do the centering — no inner wrapper divs.

### JavaScript: minimal and progressive

Core browsing and purchasing must work with JS disabled. Three files ship JS:

- `sections/header.liquid` (`{% javascript %}`) — mobile nav drawer and cart drawer are **CSS-only**, driven by hidden checkbox inputs + `<label for>` triggers. JS only adds the nicety of closing open `<details>` dropdowns on outside-click/Escape.
- `sections/search-overlay.liquid` (`{% javascript %}`) — a `<search-overlay>` custom element wrapping a native `<dialog>`; it debounces input and fetches `/search/suggest?…&section_id=predictive-search`, which renders `sections/predictive-search.liquid` server-side. `<noscript>` links to `routes.search_url` are present as the fallback.
- `snippets/scroll-reveal.liquid` — the only inline `<script>`, rendered in `<head>` by `layout/theme.liquid`. See below.

Scroll locking is declarative: `html:has(dialog[scroll-lock][open], details[scroll-lock][open])` in `critical.css`.

### Scroll reveal

Add `data-reveal` to fade-and-lift an element into view once; add `data-reveal-stagger` to its **parent** to stagger the reveal of sibling `data-reveal` children (80ms steps, capped at 400ms).

The hidden state in `critical.css` is scoped to `[data-reveal-ready]`, an attribute `scroll-reveal.liquid` sets on `<html>` **before first paint** — which is why it is an inline head script rather than `{% javascript %}` (deferred bundle output would let content paint, then hide, then animate). The attribute is only set when `IntersectionObserver` exists and the visitor has not requested reduced motion, so with no JS, an old browser, or reduced motion the rules never match and everything renders normally.

Two rules when adding it:

- **Never put `data-reveal` inside `snippets/product-card.liquid`.** Predictive search injects cards via `innerHTML` after the observer has already run, so they would stay permanently invisible. Mark the static grid container instead.
- Don't mark the hero or anything above the fold — `editorial-hero` holds the LCP image and is deliberately excluded.

### Sections, groups, templates

`layout/theme.liquid` renders `header-group` → `content_for_layout` → `footer-group`. The header group stacks `announcement-bar`, `header`, and `search-overlay` (the overlay is a section so its markup is globally available; any `[data-search-open]` button anywhere opens it).

Templates are JSON and carry real merchant content (`templates/index.json`, `page.faq.json`, `page.wholesale.json` are pre-populated). Section/block content changed in the theme editor lands in these JSON files, so treat them as data that can be overwritten from the admin.

### `shop.name` is the wrong name — do not use it

The Shopify store name is still **"Little J's Smocks"** (the old business name). The business is now **Little J's Children's Boutique**, but Shopify won't let the merchant rename the store while it carries a credit balance, so `shop.name` returns stale branding on every page.

**`settings.brand_name` is the source of truth for the customer-facing business name** (theme setting "Brand → Business name", default `Little J's Children's Boutique`). Every customer-visible name reads `settings.brand_name | default: shop.name` — the fallback exists only so the theme degrades sanely, never because `shop.name` is correct.

Where it is consumed: `<title>`, `og:site_name`, and the JSON-LD `Organization`/`WebSite` names in `snippets/meta-tags.liquid`; the footer copyright and both logo `alt`s in `sections/footer.liquid` and `sections/header.liquid`; the wordmark fallback in `snippets/product-placeholder.liquid`; the heading and logo `alt` in `templates/gift_card.liquid`. Sections that need it assign a local `brand_name` at the top of the file.

Two things not to "fix":

- The `full_title` capture in `meta-tags.liquid` is **intentionally unused** — theme-check reports it as `UnusedAssign` and that warning is expected. The merchant does not want the tag/page-number title composition. Leave `<title>` as `{{ brand_name }} | {{ page_title }}`.
- When Shopify eventually allows the store rename, clear the `brand_name` setting rather than editing Liquid; every consumer falls back to `shop.name` automatically.

Note that a filter chain inside a *filter argument* does not bind the way it reads — `image_tag: alt: settings.brand_name | default: shop.name` applies `default` to the `image_tag` output, not to `alt`. Assign the value first, then pass the variable.

### Domain conventions

- Product status badges derive from **tags**: `Preorder` and `Ready to ship`, plus `product.available == false` → "Sold out". See `snippets/product-card.liquid`, which is the shared card used by collection, search, and featured-products grids.
- `snippets/product-placeholder.liquid` renders the merchant's logo + "Image coming soon" message for products without images.

### Localization

Partially localized: `locales/en.default.json` exists and some sections use `{{ 'key' | t }}` / `"t:…"` schema labels, but most of the newer custom sections (header, footer, hero, product, cart, faq…) hardcode English strings. When editing an existing section, follow whichever convention that file already uses rather than half-converting it. `AGENTS.md` has the full translation standards if you do move a section to `t:` keys.
