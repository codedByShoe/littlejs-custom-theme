# Shared Layouts

## `layout/theme.liquid`

Root storefront document. Loads CSS variables, the critical stylesheet, metadata, Shopify scripts, header group, page content, and footer group.

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
    {% sections 'header-group' %}
    {{ content_for_layout }}
    {% sections 'footer-group' %}
  </body>
</html>
```

## `sections/header.liquid`

Current global header: shop name, a single-level menu, optional account icon, and cart icon/count.

```liquid
<header>
  <h2 class="header__title">
    {{ shop.name | link_to: routes.root_url }}
  </h2>

  <div class="header__menu">
    {% for link in section.settings.menu.links %}
      {{ link.title | link_to: link.url }}
    {% endfor %}
  </div>

  <div class="header__icons">
    {% if shop.customer_accounts_enabled %}
      <shopify-account menu="{{ section.settings.customer_account_menu }}">
        {{ 'icon-account.svg' | inline_asset_content }}
      </shopify-account>
    {% endif %}

    <a href="{{ routes.cart_url }}">
      {% if cart.item_count > 0 %}
        <sup>{{ cart.item_count }}</sup>
      {% endif %}
      {{ 'icon-cart.svg' | inline_asset_content }}
    </a>
  </div>
</header>

{% stylesheet %}
  header {
    height: 5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  header a {
    position: relative;
    text-decoration: none;
    color: var(--color-foreground);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  header a sup {
    position: absolute;
    left: 100%;
    overflow: hidden;
    max-width: var(--page-margin);
  }
  header svg {
    width: 2rem;
  }
  header .header__menu,
  header .header__icons {
    display: flex;
    gap: 1rem;
  }
{% endstylesheet %}
```

## `sections/footer.liquid`

Current global footer: copyright, navigation, and payment icons.

```liquid
<footer>
  <div class="footer__copyright">
    &copy;
    {{ 'now' | date: '%Y' }}
    {{ shop.name | link_to: routes.root_url }}, {{ powered_by_link }}
  </div>

  <div class="footer__links">
    {% for link in section.settings.menu.links %}
      {{ link.title | link_to: link.url }}
    {% endfor %}
  </div>

  <div class="footer__payment">
    {% if section.settings.show_payment_icons %}
      {% for type in shop.enabled_payment_types %}
        {{ type | payment_type_svg_tag }}
      {% endfor %}
    {% endif %}
  </div>
</footer>

{% stylesheet %}
  footer {
    display: flex;
    justify-content: space-between;
    margin-top: 2rem;
  }
  footer a {
    text-decoration: none;
    color: var(--color-foreground);
  }
  footer .footer__links,
  footer .footer__payment {
    display: flex;
    gap: 1rem;
  }
{% endstylesheet %}
```

