# Shared Layouts

Header and footer have grown well past the point where inlining their source here is useful. This file documents structure and contracts; the `.liquid` files are the source of truth.

## `layout/theme.liquid`

Root storefront document. Loads design tokens, preconnects/preloads the merchant font, preloads the critical stylesheet, renders metadata and Shopify scripts, then the header group, main content, and footer group.

```liquid
<!doctype html>
<html lang="{{ request.locale.iso_code }}">
  <head>
    {% render 'css-variables' %}

    {% unless settings.type_primary_font.system? %}
      <link rel="preconnect" href="https://fonts.shopifycdn.com" crossorigin>
      {{ settings.type_primary_font | font_url | preload_tag: as: 'font', crossorigin: 'anonymous' }}
    {% endunless %}

    {{ 'critical.css' | asset_url | stylesheet_tag: preload: true }}
    {% render 'meta-tags' %}
    {{ content_for_header }}
  </head>

  <body>
    <a class="skip-link" href="#MainContent">Skip to content</a>
    {% sections 'header-group' %}

    <main id="MainContent" tabindex="-1">
      {{ content_for_layout }}
    </main>

    {% sections 'footer-group' %}
  </body>
</html>
```

Notes:

- Only the base and bold font variants are emitted as `@font-face` (see `css-variables`); only the base variant is preloaded.
- `body` is a flex column and `main` is `flex: 1`, so the footer stays at the bottom on short pages.
- A `.skip-link` precedes the header and targets `#MainContent`.

## `layout/password.liquid`

Same `<head>` as `theme.liquid` minus the font preload, with a bare `<body>` containing only `{{ content_for_layout }}` — no header or footer group.

## `sections/header.liquid` — StorefrontHeader

Sticky global header. Roughly 1,100 lines, the large majority of which is the `{% stylesheet %}` block.

Structure:

- Two `visually-hidden` checkbox inputs (`MobileMenuToggle-{{ section.id }}`, `CartDrawerToggle-{{ section.id }}`) drive the mobile nav drawer and the cart drawer. Every open/close affordance is a `<label for>` — **the drawers work with JavaScript disabled.**
- `.site-header__drawer` — mobile navigation. Nested menus render as `<details>`/`<summary>` down to three levels, each with a "View all …" link when the parent link is not `#`.
- `.site-header__brand` — merchant logo (eager-loaded, `logo_width` setting) or the `wordmark` text setting, falling back to `shop.name`.
- `.site-header__nav` — desktop mega menu. Each top-level link with children renders a `<details>` whose panel has an intro column (eyebrow, heading, blurb, "View all") plus a grid of child/grandchild link groups.
  - **Priority links**: a top-level link whose downcased title contains `ready` or `preorder` gets `--priority` styling and swaps the mega-panel copy to fulfillment-oriented text. This is a title-substring convention, not a setting.
- `.site-header__actions` — search trigger (`[data-search-open]`, opens the search overlay section), `<shopify-account>` when customer accounts are enabled, and the cart drawer label with item count. Each has a `<noscript>` link fallback to `routes.search_url` / `routes.cart_url`.
- `.cart-drawer` — sibling of `<header>`; backdrop label, line items with quantity `updates[]` inputs and `url_to_remove` links, subtotal, and a checkout submit. It posts to `routes.cart_url`, so it is a plain form, not an AJAX cart.

The only JavaScript is a progressive enhancement that closes other open `<details>` dropdowns on toggle, outside-click, and Escape.

Settings: `logo`, `logo_width`, `wordmark`, `menu`, `customer_account_menu`.

## `sections/footer.liquid` — StorefrontFooter

Four-column grid on a `--color-soft-cream` band, collapsing at the small breakpoints.

- Brand column: logo or wordmark, tagline, optional social links.
- Repeating blocks:
  - `menu` — heading plus a linklist.
  - `newsletter` — heading, text, and a `{% form 'customer' %}` that tags submissions `newsletter` and shows a `role="status"` confirmation plus `{{ form.errors | default_errors }}`.
- Bottom bar: copyright (`settings.brand_name`, not `shop.name` — see `CLAUDE.md`) and, when `show_payment_icons` is on, `shop.enabled_payment_types | payment_type_svg_tag`.

Settings: `logo`, `logo_width`, `wordmark`, `tagline`, `show_social`, `show_payment_icons`.

> **Known gap:** the `show_social` branch reads `settings.social_facebook_link`, `settings.social_instagram_link`, and `settings.social_tiktok_link`, but no such settings exist in `config/settings_schema.json`. Turning `show_social` on renders an empty container until those settings are added.
