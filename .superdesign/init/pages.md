# Page Dependency Trees

Every standard route is wrapped by:

- `layout/theme.liquid`
  - `snippets/css-variables.liquid` — inlined `{% style %}` design tokens
  - `assets/critical.css` — preloaded global stylesheet
  - `snippets/meta-tags.liquid` — title, canonical, OG/Twitter cards, JSON-LD
  - `snippets/scroll-reveal.liquid` — inline head script enabling `data-reveal`
  - `sections/header-group.json`
    - `sections/announcement-bar.liquid`
    - `sections/header.liquid`
      - `assets/icon-search.svg`, `assets/icon-account.svg`, `assets/icon-cart.svg` (via `inline_asset_content`)
    - `sections/search-overlay.liquid`
      - `assets/icon-search.svg`
      - fetches `sections/predictive-search.liquid` at runtime
        - `snippets/product-card.liquid`
  - `sections/footer-group.json`
    - `sections/footer.liquid`

`/password` is the exception: it uses `layout/password.liquid`, which loads the same CSS variables, critical CSS, and meta tags but renders no header or footer group and no scroll-reveal script.

## `/` — Home

- `templates/index.json`
  - `sections/editorial-hero.liquid`
  - `sections/category-strip.liquid` — `link` blocks
  - `sections/featured-products.liquid`
    - `snippets/product-card.liquid`
      - `snippets/product-placeholder.liquid`
  - `sections/fulfillment-split.liquid`
  - `sections/sibling-feature.liquid`
  - `sections/shipping-feature.liquid` — `option` blocks
  - `sections/trust-bar.liquid` — `item` blocks
  - `sections/testimonials.liquid` — `testimonial` blocks
  - `sections/newsletter-band.liquid`

## `/collections` — Collection index

- `templates/list-collections.json`
  - `sections/collections.liquid`

## `/collections/:handle` — Collection

- `templates/collection.json`
  - `sections/collection.liquid`
    - `snippets/product-card.liquid`
      - `snippets/product-placeholder.liquid`
    - `snippets/pagination.liquid`

## `/products/:handle` — Product

- `templates/product.json`
  - `sections/product.liquid`
    - `snippets/product-placeholder.liquid`

## `/cart` — Cart

- `templates/cart.json`
  - `sections/cart.liquid`

## `/search` — Search

- `templates/search.json`
  - `sections/search.liquid`
    - `snippets/product-card.liquid`
    - `snippets/pagination.liquid`

## `/blogs/:handle` — Blog

- `templates/blog.json`
  - `sections/blog.liquid`
    - `snippets/pagination.liquid`

## `/blogs/:blog/:article` — Article

- `templates/article.json`
  - `sections/article.liquid`
    - `snippets/pagination.liquid` (comments, anchored to `#comments`)

## `/pages/:handle` — Content page

- `templates/page.json`
  - `sections/page.liquid`

## `/pages/contact` and `/pages/wholesale`

- `templates/page.contact.json` / `templates/page.wholesale.json`
  - `sections/page.liquid`
  - `sections/contact-form.liquid`

## `/pages/faq`

- `templates/page.faq.json`
  - `sections/page.liquid`
  - `sections/faq.liquid` — `question` blocks

## `/password` — Password page

- `layout/password.liquid`
  - `templates/password.json`
    - `sections/password.liquid`

## `/404` — Not found

- `templates/404.json`
  - `sections/404.liquid`
