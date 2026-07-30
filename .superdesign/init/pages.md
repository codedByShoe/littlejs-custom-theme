# Page Dependency Trees

Every standard route is wrapped by:

- `layout/theme.liquid`
  - `snippets/css-variables.liquid`
  - `assets/critical.css`
  - `snippets/meta-tags.liquid`
  - `sections/header-group.json`
    - `sections/header.liquid`
    - `assets/icon-account.svg`
    - `assets/icon-cart.svg`
  - `sections/footer-group.json`
    - `sections/footer.liquid`

## `/` — Home

- `templates/index.json`
  - `sections/hello-world.liquid`
    - `assets/shoppy-x-ray.svg`

## `/collections` — Collection index

- `templates/list-collections.json`
  - `sections/collections.liquid`
    - `snippets/image.liquid`

## `/collections/:handle` — Collection

- `templates/collection.json`
  - `sections/collection.liquid`
    - `snippets/image.liquid`

## `/products/:handle` — Product

- `templates/product.json`
  - `sections/product.liquid`
    - `snippets/image.liquid`

## `/cart` — Cart

- `templates/cart.json`
  - `sections/cart.liquid`
    - `snippets/image.liquid`

## `/search` — Search

- `templates/search.json`
  - `sections/search.liquid`
    - `snippets/image.liquid`

## `/blogs/:handle` — Blog

- `templates/blog.json`
  - `sections/blog.liquid`
    - `snippets/image.liquid`

## `/blogs/:blog/:article` — Article

- `templates/article.json`
  - `sections/article.liquid`
    - `snippets/image.liquid`

## `/pages/:handle` — Content page

- `templates/page.json`
  - `sections/page.liquid`

## `/404` — Not found

- `templates/404.json`
  - `sections/404.liquid`

