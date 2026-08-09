# Theme Tokens

## Compact token summary

- Framework: Shopify Online Store 2.0 Liquid theme (Little J's children's boutique).
- CSS approach: global reset and primitives in `assets/critical.css` — the only CSS asset; all component CSS lives in `{% stylesheet %}` tags inside the section/snippet/block that uses it.
- JavaScript: minimal and progressive. `sections/header.liquid` (closes open `<details>` dropdowns) and `sections/search-overlay.liquid` (the `<search-overlay>` custom element) use `{% javascript %}`; `snippets/scroll-reveal.liquid` is the one inline head script. Navigation, drawers, cart, and search all function without any of it.
- Scroll reveal: `data-reveal` fades and lifts an element in once; `data-reveal-stagger` on a parent staggers its children by 80ms (max 400ms). Hidden state is gated on `[data-reveal-ready]`, set before first paint only when IntersectionObserver exists and motion is not reduced.
- Brand name: `settings.brand_name` ("Brand → Business name", default `Little J's Children's Boutique`) is the customer-facing name everywhere. `shop.name` is stale and is only ever a fallback — see `CLAUDE.md`.
- Primary font: merchant-selected Shopify font; default Work Sans 400. Base and bold variants are emitted with `font-display: swap`; only the base variant is preloaded.
- Palette (defaults, all merchant-editable): background `#FBF8F3`, surface `#FFFFFF`, soft cream `#F2ECE4`, ink `#292421`, muted `#6F6761`, line `#DED6CD`, dusty rose `#D9A3A5`, rose dark `#A96469`, dusty blue `#AFC5D5`, sage `#B9C4AD`, sale `#A33A32`. Role definitions live in `.superdesign/design-system.md`.
- Page max width: merchant-selectable `90rem` (Standard) or `110rem` (Wide); default `90rem`.
- Page gutter: `clamp(1rem, 3vw, {{ settings.min_page_margin }}px)`; the setting is a 16–64px range, default 48px.
- Input radius: merchant-selectable 0–16px, default 8px. Media radius: 0–24px, default 16px.
- Motion: single shared easing token `--motion-ease: cubic-bezier(0.16, 1, 0.3, 1)`; transitions are ~180ms and disabled under `prefers-reduced-motion`.
- Type scale: `h1` `clamp(2.75rem, 7vw, 6.75rem)` / lh `0.94` / ls `-0.055em`; `h2` `clamp(2rem, 4vw, 4.25rem)` / lh `1` / ls `-0.04em`; `h3` `clamp(1.2rem, 2vw, 1.75rem)` / lh `1.15` / ls `-0.02em`; body `1rem` / lh `1.6`; headings are weight 600.
- Section grid: every `.shopify-section` is `gutter | content | gutter`; direct children land in the content column automatically, `.full-width` bleeds edge to edge. Vertical rhythm comes from `.section-spacing` (`padding-block: clamp(4rem, 9vw, 8rem)`).
- Global primitives available to every component: `.button` / `.button--secondary`, `.eyebrow`, `.price--sale`, `.price--compare`, `.rte`, `.visually-hidden`, `.skip-link`, `.section-spacing`, `.full-width`.
- Breakpoints in use (max-width, descending frequency): `760px` (primary layout collapse), `990px`, `900px`, `700px`, `800px`, `560px`, `520px`, `1100px`. `760px` is the de facto mobile breakpoint — prefer it for new work.
- Scroll locking is declarative: `html:has(dialog[scroll-lock][open], details[scroll-lock][open])`.
- Shadows: used sparingly; only the mobile search overlay panel carries one.

## Raw source: `snippets/css-variables.liquid`

```liquid
{% doc %}
  Emits the global font faces and theme design tokens.
{% enddoc %}

{% style %}
  {{ settings.type_primary_font | font_face: font_display: 'swap' }}
  {{ settings.type_primary_font | font_modify: 'weight', 'bold' | font_face: font_display: 'swap' }}

  :root {
    --font-primary--family: {{ settings.type_primary_font.family }}, {{ settings.type_primary_font.fallback_families }};
    --font-primary--style: {{ settings.type_primary_font.style }};
    --font-primary--weight: {{ settings.type_primary_font.weight }};
    --page-width: {{ settings.max_page_width }};
    --page-margin: clamp(1rem, 3vw, {{ settings.min_page_margin }}px);
    --color-background: {{ settings.background_color }};
    --color-surface: {{ settings.surface_color }};
    --color-soft-cream: {{ settings.soft_cream_color }};
    --color-foreground: {{ settings.foreground_color }};
    --color-muted: {{ settings.muted_color }};
    --color-line: {{ settings.line_color }};
    --color-rose: {{ settings.rose_color }};
    --color-rose-dark: {{ settings.rose_dark_color }};
    --color-blue: {{ settings.blue_color }};
    --color-sage: {{ settings.sage_color }};
    --color-sale: {{ settings.sale_color }};
    --style-border-radius-inputs: {{ settings.input_corner_radius }}px;
    --style-border-radius-media: {{ settings.media_corner_radius }}px;
    --motion-ease: cubic-bezier(0.16, 1, 0.3, 1);
  }
{% endstyle %}

```

## Raw source: `assets/critical.css`

```css
/** Critical storefront styles. Component-specific styles live beside each Liquid component. */
*,
*::before,
*::after {
  box-sizing: border-box;
}

* {
  margin: 0;
}

html {
  scroll-behavior: smooth;
}

body {
  min-height: 100svh;
  display: flex;
  flex-direction: column;
  color: var(--color-foreground);
  background: var(--color-background);
  font-family: var(--font-primary--family);
  font-size: 1rem;
  font-style: var(--font-primary--style);
  font-weight: var(--font-primary--weight);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

main {
  flex: 1;
}

html:has(dialog[scroll-lock][open], details[scroll-lock][open]) {
  overflow: hidden;
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
  height: auto;
}

button,
input,
textarea,
select {
  font: inherit;
}

button,
[type='button'],
[type='submit'] {
  cursor: pointer;
}

input,
textarea,
select {
  min-height: 3rem;
  padding: 0.75rem 1rem;
  color: var(--color-foreground);
  background: var(--color-surface);
  border: 1px solid var(--color-line);
  border-radius: var(--style-border-radius-inputs);
}

textarea {
  min-height: 8rem;
  resize: vertical;
}

dialog {
  color: var(--color-foreground);
  background: var(--color-background);
}

a {
  color: currentcolor;
  text-decoration-thickness: 1px;
  text-underline-offset: 0.2em;
}

p {
  text-wrap: pretty;
}

p,
h1,
h2,
h3,
h4,
h5,
h6 {
  overflow-wrap: break-word;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: 600;
}

h1 {
  font-size: clamp(2.75rem, 7vw, 6.75rem);
  line-height: 0.94;
  letter-spacing: -0.055em;
}

h2 {
  font-size: clamp(2rem, 4vw, 4.25rem);
  line-height: 1;
  letter-spacing: -0.04em;
}

h3 {
  font-size: clamp(1.2rem, 2vw, 1.75rem);
  line-height: 1.15;
  letter-spacing: -0.02em;
}

p:empty {
  display: none;
}

:focus-visible {
  outline: 2px solid var(--color-foreground);
  outline-offset: 3px;
}

.visually-hidden {
  position: absolute !important;
  width: 1px !important;
  height: 1px !important;
  padding: 0 !important;
  margin: -1px !important;
  overflow: hidden !important;
  clip: rect(0, 0, 0, 0) !important;
  white-space: nowrap !important;
  border: 0 !important;
}

.skip-link {
  position: fixed;
  z-index: 100;
  top: 0.75rem;
  left: 0.75rem;
  padding: 0.75rem 1rem;
  color: var(--color-background);
  background: var(--color-foreground);
  transform: translateY(-200%);
}

.skip-link:focus {
  transform: translateY(0);
}

.button,
.shopify-payment-button__button--unbranded {
  min-height: 3rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.7rem 1.5rem;
  color: var(--color-background);
  background: var(--color-foreground);
  border: 1px solid var(--color-foreground);
  border-radius: var(--style-border-radius-inputs);
  font-weight: 600;
  line-height: 1.2;
  text-decoration: none;
  transition: color 180ms var(--motion-ease), background-color 180ms var(--motion-ease);
}

.button:hover,
.shopify-payment-button__button--unbranded:hover {
  color: var(--color-foreground);
  background: transparent;
}

.button--secondary {
  color: var(--color-foreground);
  background: transparent;
  border-color: var(--color-line);
}

.button--secondary:hover {
  background: var(--color-soft-cream);
}

.eyebrow {
  color: var(--color-muted);
  font-size: 0.75rem;
  font-weight: 600;
  line-height: 1.3;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.price--sale {
  color: var(--color-sale);
}

.price--compare {
  color: var(--color-muted);
  text-decoration: line-through;
}

.shopify-section {
  --content-width: min(
    calc(var(--page-width) - var(--page-margin) * 2),
    calc(100% - var(--page-margin) * 2)
  );
  --content-margin: minmax(var(--page-margin), 1fr);
  --content-grid: var(--content-margin) var(--content-width) var(--content-margin);
  position: relative;
  display: grid;
  grid-template-columns: var(--content-grid);
  width: 100%;
}

.shopify-section > * {
  grid-column: 2;
}

.shopify-section > .full-width {
  grid-column: 1 / -1;
}

.section-spacing {
  padding-block: clamp(4rem, 9vw, 8rem);
}

.rte {
  max-width: 72ch;
}

.rte > * + * {
  margin-block-start: 1em;
}

/**
 * Scroll reveal. The hidden state is scoped to [data-reveal-ready], which
 * snippets/scroll-reveal.liquid sets on <html> before first paint only when
 * the animation can actually run. Without it these rules never match, so
 * content renders normally with no JavaScript.
 */
[data-reveal-ready] [data-reveal] {
  opacity: 0;
  transform: translateY(1rem);
  transition: opacity 700ms var(--motion-ease), transform 700ms var(--motion-ease);
}

[data-reveal-ready] [data-reveal].is-revealed {
  opacity: 1;
  transform: none;
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
  }

  [data-reveal] {
    opacity: 1 !important;
    transform: none !important;
  }
}
```

## Raw source: `config/settings_schema.json`

```json
[
  {
    "name": "theme_info",
    "theme_name": "Little J's",
    "theme_version": "1.0.0",
    "theme_author": "Little J's",
    "theme_documentation_url": "https://help.shopify.com/manual/online-store/themes",
    "theme_support_url": "https://support.shopify.com/"
  },
  {
    "name": "Brand",
    "settings": [
      {
        "type": "text",
        "id": "brand_name",
        "label": "Business name",
        "default": "Little J's Children's Boutique",
        "info": "Used for the browser tab title, social sharing, search engine data, and the footer copyright. Set this instead of relying on the Shopify store name."
      }
    ]
  },
  {
    "name": "Typography",
    "settings": [
      {
        "type": "font_picker",
        "id": "type_primary_font",
        "default": "work_sans_n4",
        "label": "Primary font"
      }
    ]
  },
  {
    "name": "Social sharing",
    "settings": [
      {
        "type": "image_picker",
        "id": "social_sharing_image",
        "label": "Default social sharing image",
        "info": "Used when a page does not have its own featured image. A 1200 × 630 px image works best."
      }
    ]
  },
  {
    "name": "Product placeholders",
    "settings": [
      {
        "type": "image_picker",
        "id": "product_placeholder_logo",
        "label": "Logo",
        "info": "Shown when a product does not have an image. A transparent PNG or SVG works best."
      },
      {
        "type": "text",
        "id": "product_placeholder_message",
        "label": "Message",
        "default": "Image coming soon"
      }
    ]
  },
  {
    "name": "Layout",
    "settings": [
      {
        "type": "select",
        "id": "max_page_width",
        "label": "Maximum page width",
        "options": [
          { "value": "90rem", "label": "Standard" },
          { "value": "110rem", "label": "Wide" }
        ],
        "default": "90rem"
      },
      {
        "type": "range",
        "id": "min_page_margin",
        "min": 16,
        "max": 64,
        "step": 4,
        "unit": "px",
        "label": "Maximum page gutter",
        "default": 48
      },
      {
        "type": "range",
        "id": "input_corner_radius",
        "min": 0,
        "max": 16,
        "step": 2,
        "unit": "px",
        "label": "Control corner radius",
        "default": 8
      },
      {
        "type": "range",
        "id": "media_corner_radius",
        "min": 0,
        "max": 24,
        "step": 2,
        "unit": "px",
        "label": "Media corner radius",
        "default": 16
      }
    ]
  },
  {
    "name": "Colors",
    "settings": [
      { "type": "color", "id": "background_color", "default": "#FBF8F3", "label": "Background" },
      { "type": "color", "id": "surface_color", "default": "#FFFFFF", "label": "Surface" },
      { "type": "color", "id": "soft_cream_color", "default": "#F2ECE4", "label": "Soft cream" },
      { "type": "color", "id": "foreground_color", "default": "#292421", "label": "Text" },
      { "type": "color", "id": "muted_color", "default": "#6F6761", "label": "Muted text" },
      { "type": "color", "id": "line_color", "default": "#DED6CD", "label": "Borders" },
      { "type": "color", "id": "rose_color", "default": "#D9A3A5", "label": "Dusty rose" },
      { "type": "color", "id": "rose_dark_color", "default": "#A96469", "label": "Dusty rose dark" },
      { "type": "color", "id": "blue_color", "default": "#AFC5D5", "label": "Dusty blue" },
      { "type": "color", "id": "sage_color", "default": "#B9C4AD", "label": "Sage" },
      { "type": "color", "id": "sale_color", "default": "#A33A32", "label": "Sale" }
    ]
  }
]
```
