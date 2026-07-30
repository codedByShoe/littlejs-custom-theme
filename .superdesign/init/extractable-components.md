# Extractable Components

## StorefrontHeader

- Source: `sections/header.liquid`
- Category: layout
- Description: Global header with storefront identity, primary menu, account action, and cart count.
- Extractable props: activeItem (string), cartCount (number), showAccount (boolean)
- Hardcoded: Shopify account element, account/cart icons, typography and layout CSS

## StorefrontFooter

- Source: `sections/footer.liquid`
- Category: layout
- Description: Global footer with copyright, merchant menu, and payment icons.
- Extractable props: showPaymentIcons (boolean)
- Hardcoded: copyright structure and Shopify payment icon loop

## ResponsiveImage

- Source: `snippets/image.liquid`
- Category: basic
- Description: Responsive image wrapper that optionally links and supports size/crop controls.
- Extractable props: url (string), className (string), width (number), height (number), crop (string)
- Hardcoded: responsive image sizing CSS

## TextBlock

- Source: `blocks/text.liquid`
- Category: basic
- Description: Theme-editor text with title, subtitle, and body variants.
- Extractable props: text (string), textStyle (string), alignment (string)
- Hardcoded: CSS scale for title and subtitle

## Group

- Source: `blocks/group.liquid`
- Category: basic
- Description: Nestable horizontal or vertical content group.
- Extractable props: layoutDirection (string), alignment (string), padding (number)
- Hardcoded: flexbox implementation

