# Shared UI Components

Reusable pieces live in `snippets/` (rendered with `{% render %}`, not merchant-editable) and `blocks/` (merchant-editable in the theme editor). Every snippet carries a `{% doc %}` header declaring its parameters, and component CSS lives in the file's own `{% stylesheet %}` block.

## `snippets/product-card.liquid` — ProductCard

The shared product tile used by the collection grid, search results, predictive search, and featured products. There is no generic image snippet in this theme — cards render `image_tag` directly.

```liquid
{% doc %}
  Renders a lightweight product card.

  @param {product} product - Product to render.
  @param {boolean} [show_badge] - Whether to show availability or preorder status.
  @param {string} [image_sizes] - Responsive sizes attribute.

  @example
  {% render 'product-card', product: product, show_badge: true %}
{% enddoc %}
```

- Badge logic, when `show_badge` is truthy, in priority order: `product.available == false` → "Sold out"; tag `Preorder` → "Preorder"; tag `Ready to ship` → "Ready to ship". These are **tag-driven conventions** shared with merchandising.
- Media is a `4 / 5` aspect box on `--color-soft-cream` with `--style-border-radius-media`, `object-fit: cover`, `loading: 'lazy'`, and widths `240–900`. Callers pass `image_sizes` matching their grid (e.g. `'(min-width: 990px) 25vw, 50vw'`).
- Falls back to `snippets/product-placeholder.liquid` when the product has no featured image.
- Price uses the global `.price--sale` / `.price--compare` primitives from `critical.css` and prefixes `From` when `product.price_varies`.

## `snippets/product-placeholder.liquid` — ProductPlaceholder

Branded fallback for products without media. Matches the card's `4 / 5` box so grids stay aligned.

```liquid
{% doc %}
  Renders the branded fallback shown when a product has no media.

  @param {string} [class] - Optional additional CSS class.
{% enddoc %}
```

Renders the merchant's `settings.product_placeholder_logo` (or `settings.brand_name` as a wordmark), a small inline SVG ornament, and `settings.product_placeholder_message` (default "Image coming soon"). The wrapper is `role="img"` with the message as its `aria-label`, and the decorative children are `aria-hidden`.

## `snippets/pagination.liquid` — Pagination

```liquid
{% doc %}
  Renders pagination controls for a Shopify paginate object.

  @param {object} paginate - The paginate object available inside a paginate tag.
  @param {string} [anchor] - Optional fragment identifier appended to pagination URLs.
{% enddoc %}
```

Renders nothing when `paginate.pages <= 1`. Emits a "Page X of Y" summary, previous/next controls, numbered `paginate.parts`, and an ellipsis span for gaps. The current page is a `<span aria-current="page">` with a `.visually-hidden` "Current page" label. `anchor` is appended to every href — `sections/article.liquid` passes `'#comments'`.

## `snippets/meta-tags.liquid` — MetaTags

Charset, viewport, and `theme-color` (from `settings.background_color`), then the canonical URL, an Open Graph/Twitter card set, and page-appropriate JSON-LD. Title composition appends current tags, page number, and the shop name when not already present. `page_image` falls back to `settings.social_sharing_image`. Rendered by both layouts.

## `snippets/css-variables.liquid` — DesignTokens

Emits the `@font-face` declarations (base + bold, `font-display: swap`) and the `:root` custom properties consumed by every component. See `theme.md` for the full token list. This is the only place merchant settings become CSS.

## `snippets/scroll-reveal.liquid` — ScrollReveal

The site-wide scroll animation. Rendered in `<head>` by `layout/theme.liquid`; the only inline `<script>` in the theme.

Sets `data-reveal-ready` on `<html>` before first paint, but only when `IntersectionObserver` is supported and the visitor has not requested reduced motion. The hidden state in `critical.css` is scoped to that attribute, so with no JS, an old browser, or reduced motion nothing is ever hidden.

- `data-reveal` on an element fades and lifts it into view once, then unobserves it.
- `data-reveal-stagger` on a parent staggers its `data-reveal` children by 80ms each, capped at 400ms.
- Do not add `data-reveal` inside `product-card.liquid` — predictive search injects cards after the observer has run and they would never be revealed. Mark the static grid container instead.
- The hero is deliberately excluded; it carries the LCP image.

## `blocks/text.liquid` — Text

Theme-editor text block with title/subtitle/normal styles and alignment. Rendered inside `@theme`-accepting containers (`blocks/group.liquid`, `sections/custom-section.liquid`).

Settings: `text`, `text_style` (`text--title` | `text--subtitle` | `text--normal`), `alignment`.

## `blocks/group.liquid` — Group

Nestable horizontal or vertical flex container that renders `{% content_for 'blocks' %}`. Settings apply to a single dimension each to keep configuration simple.

Settings: `layout_direction` (`group--horizontal` | `group--vertical`), `alignment`, `padding`.

Both blocks are inherited from the Skeleton Theme, are labelled with `t:` translation keys, and are the only components in the theme that use them consistently.

## Repeating section blocks

These are declared inline in their section's `{% schema %}` rather than as files in `blocks/`:

| Section | Block type | Settings |
| --- | --- | --- |
| `category-strip` | `link` | `label`, `link` |
| `trust-bar` | `item` | heading/text pair |
| `testimonials` | `testimonial` | quote and attribution |
| `faq` | `question` | `question`, `answer` (richtext) |
| `shipping-feature` | `option` | `title`, `text` |
| `footer` | `menu`, `newsletter` | `heading` + `menu`; `heading`, `text`, `button_label` |

## Asset icons

Inlined with `inline_asset_content`, not `asset_url`:

- `assets/icon-search.svg` — header and search overlay
- `assets/icon-account.svg` — header customer account
- `assets/icon-cart.svg` — header cart trigger
- `assets/shoppy-x-ray.svg` — Skeleton Theme artwork; used only by the unused `hello-world` section and the README
