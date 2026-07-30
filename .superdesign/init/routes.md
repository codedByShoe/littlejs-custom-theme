# Storefront Routes

This is a Shopify Online Store 2.0 Liquid theme. Route resolution is controlled by Shopify template conventions rather than an application router.

| URL type | Template | Main section |
| --- | --- | --- |
| `/` | `templates/index.json` | `sections/hello-world.liquid` |
| `/collections` | `templates/list-collections.json` | `sections/collections.liquid` |
| `/collections/:handle` | `templates/collection.json` | `sections/collection.liquid` |
| `/products/:handle` | `templates/product.json` | `sections/product.liquid` |
| `/cart` | `templates/cart.json` | `sections/cart.liquid` |
| `/search` | `templates/search.json` | `sections/search.liquid` |
| `/blogs/:handle` | `templates/blog.json` | `sections/blog.liquid` |
| `/blogs/:blog/:article` | `templates/article.json` | `sections/article.liquid` |
| `/pages/:handle` | `templates/page.json` | `sections/page.liquid` |
| `/password` | `templates/password.json` | `sections/password.liquid`, `layout/password.liquid` |
| `/404` | `templates/404.json` | `sections/404.liquid` |
| Gift card | `templates/gift_card.liquid` | standalone Liquid template |

All standard storefront pages use `layout/theme.liquid`, `sections/header-group.json`, and `sections/footer-group.json`.

## Current home template

```json
{
  "sections": {
    "main": {
      "type": "hello-world",
      "settings": {}
    }
  },
  "order": ["main"]
}
```

