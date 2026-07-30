# Little J’s Children’s Boutique Design System

## Product context

Little J’s is a Shopify children’s boutique specializing in smocked and custom children’s clothing, sibling sets, seasonal collections, ready-to-ship items, preorders, swim, and accessories. The storefront should help a parent quickly understand what is available now, shop by child/category, discover coordinated outfits, and distinguish ready-to-ship merchandise from preorders.

The theme is intentionally lightweight. Prefer server-rendered Liquid, native HTML, component-scoped CSS, responsive Shopify images, and progressive enhancement. Core browsing and purchasing must work without custom JavaScript.

## Key storefront architecture

- Global announcement bar for shipping, preorder, and promotion messages.
- Header with Little J’s wordmark, primary collections, search, account, and cart.
- Home: hero, high-priority category navigation, new arrivals or featured collection, sibling-set/preorder story, trust/reviews, newsletter.
- Collection: clear title and description, compact filters/sort, responsive product grid.
- Product: gallery, title/price, availability and preorder status, variants, add to cart, shipping notes, complementary products.
- Cart: editable lines, order note, subtotal, checkout, shipping/preorder expectations.
- Search, content pages, blog/article, collection index, and 404.
- Footer: shop links, help/policies, social links, newsletter, payment methods.

## Brand principles

- Modern and warm, never fussy.
- Child-friendly without cartoon styling.
- Boutique polish with straightforward shopping behavior.
- Product photography carries most of the visual energy.
- Copy is concise, specific, and helpful to busy parents.
- No gradients, glassmorphism, heavy shadows, parallax, autoplay video, or decorative animation loops.

## Color palette

Use only these colors and their specified roles.

- `Canvas / warm ivory`: `#FBF8F3` — primary page background.
- `Surface / white`: `#FFFFFF` — product cards, forms, and high-clarity content areas.
- `Soft cream`: `#F2ECE4` — alternate section background and subtle separators.
- `Ink`: `#292421` — primary text and dark buttons.
- `Muted ink`: `#6F6761` — secondary copy and metadata.
- `Line`: `#DED6CD` — borders and rules.
- `Dusty rose`: `#D9A3A5` — primary brand accent and small highlights.
- `Dusty rose dark`: `#A96469` — accessible accent text and hover state.
- `Dusty blue`: `#AFC5D5` — boys/category accent and informational callouts.
- `Sage`: `#B9C4AD` — seasonal/story accent and calm supporting surfaces.
- `Sale`: `#A33A32` — sale prices and urgent inventory states only.
- Text on colored pastel surfaces remains `#292421`.

## Typography

- Use the Shopify-hosted merchant font configured by the theme; default to Work Sans.
- Do not load third-party font files or remote font services.
- Body: Work Sans 400, `1rem`, line-height `1.6`.
- Small/meta: Work Sans 600, `0.75rem`, line-height `1.3`, letter-spacing `0.08em`, uppercase only for short labels.
- Navigation: Work Sans 600, `0.875rem`, line-height `1.2`.
- H1: Work Sans 600, `clamp(2.75rem, 7vw, 6.75rem)`, line-height `0.94`, letter-spacing `-0.055em`.
- H2: Work Sans 600, `clamp(2rem, 4vw, 4.25rem)`, line-height `1`, letter-spacing `-0.04em`.
- H3: Work Sans 600, `clamp(1.2rem, 2vw, 1.75rem)`, line-height `1.15`, letter-spacing `-0.02em`.
- Product title: Work Sans 500, `0.95rem`, line-height `1.35`.
- Never use decorative scripts or extra typefaces.

## Layout and spacing

- Maximum content width: `90rem`.
- Page gutters: `clamp(1rem, 3vw, 3rem)`.
- Section spacing: `clamp(4rem, 9vw, 8rem)`.
- Use a 12-column desktop grid, 6-column tablet grid, and 2-column mobile grid.
- Base spacing units: `4, 8, 12, 16, 24, 32, 48, 64, 96, 128px`.
- Product imagery uses consistent `4 / 5` aspect ratio and `object-fit: cover`.
- Hero imagery may use `4 / 5`, `3 / 4`, or wide `16 / 10` depending on the chosen layout.
- Prefer whitespace and 1px rules over nested cards.

## Shape, borders, and shadows

- Standard radius: `8px`.
- Large media radius: `16px`.
- Pills: `999px` only for status badges and compact filters.
- Border: `1px solid #DED6CD`.
- Shadow is exceptional: `0 12px 30px rgba(41, 36, 33, 0.08)` for overlays only.
- Product imagery and ordinary sections have no shadow.

## Components

### Announcement bar

- 32–40px tall, ink or dusty-rose background.
- One short centered message with optional link.
- No carousel by default.

### Header

- 72–84px tall on desktop, 64px mobile.
- Warm ivory or white background with a single bottom rule.
- Wordmark uses text or a merchant-uploaded logo; do not invent a decorative logo.
- Desktop navigation is horizontal and concise.
- Search, account, and cart use simple line icons and at least 44px tap targets.
- Mobile navigation uses native `details`/`summary` where practical.
- Sticky behavior is allowed, but avoid blur and transparency effects.

### Buttons

- Primary: ink background, ivory text, 1px ink border.
- Secondary: transparent background, ink text, 1px line border.
- Accent: dusty rose background, ink text; use sparingly.
- Height: 44–48px; padding-inline 20–28px; radius 8px.
- Hover: color or border change only; no scaling.
- Focus: 2px ink outline with 3px offset.

### Product cards

- Image first, then optional status badge, title, price, compare-at price, and color count.
- Avoid enclosing cards in visible boxes.
- Image hover may crossfade to the second product image only when implemented without layout shift.
- Sold-out and preorder states are always text, never color alone.

### Category cards

- Strong photography with concise labels.
- May use image overlays only with a solid readability scrim.
- Mobile remains at least two columns for small category tiles or one column for editorial feature cards.

### Forms

- 48px minimum input height, white background, ink text, line border.
- Labels remain visible; placeholders do not replace labels.
- Error text uses Sale red and appears adjacent to the field.

### Reviews and trust

- Use real testimonial names and concise quotes.
- Trust statements should be concrete: satisfaction guarantee, secure checkout, shipping or preorder expectations.
- Avoid generic icon rows when a short text statement is clearer.

## Motion

- Motion is optional and never required to understand the UI.
- Duration: 140–220ms.
- Easing: `cubic-bezier(0.16, 1, 0.3, 1)`.
- Allowed: color, border-color, opacity, and simple menu disclosure.
- Respect `prefers-reduced-motion`.
- No scroll-triggered entrance animations, bounce loops, parallax, or grayscale image effects.

## Accessibility and performance

- Target WCAG AA contrast for all text and controls.
- Semantic headings, landmarks, lists, buttons, and forms.
- Visible keyboard focus on every interactive element.
- Minimum 44×44px touch targets.
- Responsive `image_url`/`image_tag`, explicit image dimensions, lazy loading below the fold, and high fetch priority only for the primary hero image.
- Avoid custom JavaScript for sliders, tabs, and mobile navigation when native HTML works.
- Avoid external UI frameworks and icon libraries.
- Keep global critical CSS compact; component rules belong with snippets, blocks, and sections.

