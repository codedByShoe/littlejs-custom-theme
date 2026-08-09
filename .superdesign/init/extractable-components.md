# Extractable Components

## ProductCard

- Source: `snippets/product-card.liquid`
- Category: commerce
- Description: Shared product tile — 4:5 media, status badge, title, price with sale/compare treatment.
- Extractable props: product (object), showBadge (boolean), imageSizes (string)
- Hardcoded: badge vocabulary ("Sold out" / "Preorder" / "Ready to ship") and its tag-matching rules; responsive width list; 4:5 aspect ratio

## ProductPlaceholder

- Source: `snippets/product-placeholder.liquid`
- Category: commerce
- Description: Branded 4:5 fallback for products with no media.
- Extractable props: className (string)
- Hardcoded: inline ornament SVG; reads `settings.product_placeholder_logo` and `settings.product_placeholder_message` from global theme settings

## Pagination

- Source: `snippets/pagination.liquid`
- Category: navigation
- Description: Numbered pagination with previous/next controls and a page summary.
- Extractable props: paginate (object), anchor (string)
- Hardcoded: English control labels; ellipsis rendering; hidden-until-`pages > 1` behavior

## StorefrontHeader

- Source: `sections/header.liquid`
- Category: layout
- Description: Sticky header with wordmark/logo, three-level mega menu, mobile drawer, search trigger, account, and cart drawer.
- Extractable props: logo (image), logoWidth (number), wordmark (string), menu (linklist), customerAccountMenu (menu)
- Hardcoded: checkbox+label drawer mechanism; "ready"/"preorder" title-substring priority-link rule and its mega-panel copy; `<shopify-account>` element; cart drawer form posting to `routes.cart_url`

## CartDrawer

- Source: `sections/header.liquid` (sibling of the `<header>` element)
- Category: commerce
- Description: Slide-in bag with line items, quantity inputs, remove links, subtotal, and checkout.
- Extractable props: none — reads the global `cart` object
- Hardcoded: toggle is bound to the header's `CartDrawerToggle-{{ section.id }}` checkbox, so it cannot be lifted out of the header section as-is; non-AJAX form submission

## SearchOverlay

- Source: `sections/search-overlay.liquid`
- Category: navigation
- Description: Full-screen `<dialog>` search with debounced predictive results.
- Extractable props: none
- Hardcoded: `<search-overlay>` custom element; `[data-search-open]` global trigger contract; fetch of `/search/suggest?…&section_id=predictive-search`

## PredictiveSearchResults

- Source: `sections/predictive-search.liquid`
- Category: navigation
- Description: Server-rendered results fragment for the search overlay.
- Extractable props: none — reads `predictive_search`
- Hardcoded: must remain a section (not a snippet) because it is fetched by `section_id`

## StorefrontFooter

- Source: `sections/footer.liquid`
- Category: layout
- Description: Four-column footer band with brand, menu blocks, newsletter form, copyright, and payment icons.
- Extractable props: logo (image), logoWidth (number), wordmark (string), tagline (string), showSocial (boolean), showPaymentIcons (boolean)
- Hardcoded: `{% form 'customer' %}` newsletter tagging; `shop.enabled_payment_types` loop; social links read settings that do not exist yet (see `layouts.md`)

## PageHeader

- Source: `sections/page.liquid`
- Category: layout
- Description: Reusable page title block with eyebrow, intro, width, alignment, and content-style options; shared by every `page.*` template.
- Extractable props: eyebrow (string), introduction (richtext), contentWidth (string), headerAlignment (string), contentStyle (string)
- Hardcoded: renders `page.content` directly

## ContactForm

- Source: `sections/contact-form.liquid`
- Category: forms
- Description: Dual-purpose contact/wholesale form with success state and merchant contact details.
- Extractable props: formType (string), eyebrow/heading/text (string), email (string), phone (string), wholesaleNote (string), buttonLabel (string), successHeading/successText (string), background (string)
- Hardcoded: which fields appear per `form_type`; `{% form 'contact' %}` field names

## ShippingFeature

- Source: `sections/shipping-feature.liquid`
- Category: content
- Description: Image-left / copy-right feature explaining local pickup and shipping, with repeating delivery-option blocks.
- Extractable props: image (image), eyebrow (string), heading (string), text (richtext), linkLabel (string), link (url), options (block list of title + text)
- Hardcoded: 5:4 media ratio; rose-tinted media backdrop; `data-reveal` markup

## ScrollReveal

- Source: `snippets/scroll-reveal.liquid` + the `[data-reveal-ready]` rules in `assets/critical.css`
- Category: behavior
- Description: IntersectionObserver fade-and-lift used across the storefront.
- Extractable props: none — driven entirely by `data-reveal` / `data-reveal-stagger` attributes
- Hardcoded: 700ms duration, 1rem offset, 80ms stagger step capped at 400ms, `-8%` bottom root margin

## TextBlock

- Source: `blocks/text.liquid`
- Category: basic
- Description: Theme-editor text with title, subtitle, and normal variants plus alignment.
- Extractable props: text (string), textStyle (string), alignment (string)
- Hardcoded: CSS scale for title and subtitle

## Group

- Source: `blocks/group.liquid`
- Category: basic
- Description: Nestable horizontal or vertical content group rendering `{% content_for 'blocks' %}`.
- Extractable props: layoutDirection (string), alignment (string), padding (number)
- Hardcoded: flexbox implementation; single-dimension settings

## Homepage narrative sections

`editorial-hero`, `category-strip`, `featured-products`, `fulfillment-split`, `sibling-feature`, `trust-bar`, `testimonials`, and `newsletter-band` are each single-purpose and fully settings-driven with presets. They are composable in the theme editor but are not general-purpose components — treat them as page content, not a component library.
