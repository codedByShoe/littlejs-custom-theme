# Theme Tokens

## Compact token summary

- Framework: Shopify Online Store 2.0 Liquid theme.
- CSS approach: native CSS reset in `assets/critical.css`; component CSS lives in `{% stylesheet %}` tags.
- JavaScript: none in the current storefront shell.
- Primary font: merchant-selected Shopify font; default Work Sans 400. Bold and italic variants are emitted with `font-display: swap`.
- Background: `#FFFFFF` by default.
- Foreground: `#333333` by default.
- Page max width: merchant-selectable `90rem` or `110rem`; default `90rem`.
- Minimum page gutter: merchant-selectable 10–100px; default `20px`.
- Input radius: merchant-selectable 0–10px; default `4px`.
- Current section grid: full-bleed outer column plus centered content column.
- Current breakpoints: only the demo homepage uses `1100px`.
- Current shadows: used only by the demo homepage button.

## Raw source: `snippets/css-variables.liquid`

```liquid
{% style %}
  {{ settings.type_primary_font | font_face: font_display: 'swap' }}
  {{ settings.type_primary_font | font_modify: 'weight', 'bold' | font_face: font_display: 'swap' }}
  {{ settings.type_primary_font | font_modify: 'weight', 'bold' | font_modify: 'style', 'italic' | font_face: font_display: 'swap' }}
  {{ settings.type_primary_font | font_modify: 'style', 'italic' | font_face: font_display: 'swap' }}

  :root {
    --font-primary--family: {{ settings.type_primary_font.family }}, {{ settings.type_primary_font.fallback_families }};
    --font-primary--style: {{ settings.type_primary_font.style }};
    --font-primary--weight: {{ settings.type_primary_font.weight }};
    --page-width: {{ settings.max_page_width }};
    --page-margin: {{ settings.min_page_margin }}px;
    --color-background: {{ settings.background_color }};
    --color-foreground: {{ settings.foreground_color }};
    --style-border-radius-inputs: {{ settings.input_corner_radius }}px;
  }
{% endstyle %}
```

## Raw source: `assets/critical.css`

```css
* {
  box-sizing: border-box;
  margin: 0;
}

body {
  display: flex;
  flex-direction: column;
  margin: 0;
  min-height: 100svh;
}

html:has(dialog[scroll-lock][open], details[scroll-lock][open]) {
  overflow: hidden;
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
  height: auto;
}

input,
textarea,
select {
  font: inherit;
  border-radius: var(--style-border-radius-inputs);
}

select {
  background-color: var(--color-background);
  color: currentcolor;
}

dialog {
  background-color: var(--color-background);
  color: var(--color-foreground);
}

p {
  text-wrap: pretty;
}

p,
h1,
h2,
h3,
h4,
h5,
h6 {
  overflow-wrap: break-word;
}

p:empty {
  display: none;
}

body {
  font-family: var(--font-primary--family);
  background-color: var(--color-background);
  color: var(--color-foreground);
}

.shopify-section {
  --content-width: min(
    calc(var(--page-width) - var(--page-margin) * 2),
    calc(100% - var(--page-margin) * 2)
  );
  --content-margin: minmax(var(--page-margin), 1fr);
  --content-grid: var(--content-margin) var(--content-width) var(--content-margin);
  position: relative;
  grid-template-columns: var(--content-grid);
  display: grid;
  width: 100%;
}

.shopify-section > * {
  grid-column: 2;
}

.shopify-section > .full-width {
  grid-column: 1 / -1;
}
```

## Raw source: `config/settings_schema.json`

```json
[
  {
    "name": "theme_info",
    "theme_name": "Skeleton",
    "theme_version": "0.1.0",
    "theme_author": "Shopify"
  },
  {
    "name": "Typography",
    "settings": [
      {
        "type": "font_picker",
        "id": "type_primary_font",
        "default": "work_sans_n4",
        "label": "Primary"
      }
    ]
  },
  {
    "name": "Layout",
    "settings": [
      {
        "type": "select",
        "id": "max_page_width",
        "options": [
          { "value": "90rem", "label": "Narrow" },
          { "value": "110rem", "label": "Wide" }
        ],
        "default": "90rem"
      },
      {
        "type": "range",
        "id": "min_page_margin",
        "min": 10,
        "max": 100,
        "step": 1,
        "unit": "px",
        "default": 20
      }
    ]
  },
  {
    "name": "Colors",
    "settings": [
      { "type": "color", "id": "background_color", "default": "#FFFFFF" },
      { "type": "color", "id": "foreground_color", "default": "#333333" },
      {
        "type": "range",
        "id": "input_corner_radius",
        "min": 0,
        "max": 10,
        "step": 1,
        "unit": "px",
        "default": 4
      }
    ]
  }
]
```

