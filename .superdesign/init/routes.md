# Storefront Routes

This is a Shopify Online Store 2.0 Liquid theme. Route resolution is controlled by Shopify template conventions rather than an application router.

| URL type | Template | Sections (in order) |
| --- | --- | --- |
| `/` | `templates/index.json` | `editorial-hero`, `category-strip`, `featured-products`, `fulfillment-split`, `sibling-feature`, `shipping-feature`, `trust-bar`, `testimonials`, `newsletter-band` |
| `/collections` | `templates/list-collections.json` | `collections` |
| `/collections/:handle` | `templates/collection.json` | `collection` |
| `/products/:handle` | `templates/product.json` | `product` |
| `/cart` | `templates/cart.json` | `cart` |
| `/search` | `templates/search.json` | `search` |
| `/blogs/:handle` | `templates/blog.json` | `blog` |
| `/blogs/:blog/:article` | `templates/article.json` | `article` |
| `/pages/:handle` | `templates/page.json` | `page` |
| `/pages/contact` | `templates/page.contact.json` | `page`, `contact-form` (`form_type: contact`) |
| `/pages/faq` | `templates/page.faq.json` | `page`, `faq` |
| `/pages/wholesale` | `templates/page.wholesale.json` | `page`, `contact-form` (`form_type: wholesale`) |
| `/password` | `templates/password.json` | `password` (uses `layout/password.liquid`) |
| `/404` | `templates/404.json` | `404` |
| Gift card | `templates/gift_card.liquid` | standalone Liquid template |

Alternate page templates (`page.contact`, `page.faq`, `page.wholesale`) are assigned per-page in the Shopify admin. Each pairs the shared `page` section — which renders the page title, optional eyebrow/intro, and `page.content` — with a purpose-built section beneath it.

All standard storefront pages use `layout/theme.liquid`, `sections/header-group.json`, and `sections/footer-group.json`.

## Section groups

`sections/header-group.json` (rendered above `content_for_layout`):

| Section id | Type | Purpose |
| --- | --- | --- |
| `announcement` | `announcement-bar` | Shipping/promo message with merchant-set colors |
| `header` | `header` | Wordmark, mega menu, search trigger, account, cart drawer |
| `search` | `search-overlay` | `<search-overlay>` dialog; opened by any `[data-search-open]` trigger |

`sections/footer-group.json` (rendered below `content_for_layout`):

| Section id | Type | Blocks |
| --- | --- | --- |
| `footer` | `footer` | `menu` ("Shop"), `menu` ("Help"), `newsletter` |

## Non-routed sections

- `sections/predictive-search.liquid` — never placed in a template. It is fetched by the search overlay via `/search/suggest?…&section_id=predictive-search` and renders only the results fragment.
- `sections/custom-section.liquid` — merchant-composable section accepting `@theme` blocks; available in the theme editor with a preset.
- `sections/hello-world.liquid` — leftover Skeleton Theme demo section. Not referenced by any template; kept only because it still has a preset.
