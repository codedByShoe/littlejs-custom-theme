# Shared UI Components

## `snippets/image.liquid` — ResponsiveImage

Responsive image wrapper with optional link, size, and crop parameters.

```liquid
{% doc %}
  Renders a responsive image that might be wrapped in a link.

  @param {image} image - The image to be rendered
  @param {string} [url] - An optional destination URL for the image
  @param {string} [class] - Optional class to be added to the image wrapper
  @param {number} [width] - The highest resolution width of the image to be rendered
  @param {number} [height] - The highest resolution height of the image to be rendered
  @param {string} [crop] - The crop position of the image
{% enddoc %}

{% liquid
  unless height
    assign width = width | default: image.width
  endunless

  if url
    assign wrapper = 'a'
  else
    assign wrapper = 'div'
  endif
%}

<{{ wrapper }}
  class="image {{ class }}"
  {% if url %}
    href="{{ url }}"
  {% endif %}
>
  {{ image | image_url: width: width, height: height, crop: crop | image_tag }}
</{{ wrapper }}>

{% stylesheet %}
  .image {
    display: block;
    position: relative;
    overflow: hidden;
    width: 100%;
    height: auto;
  }

  .image > img {
    width: 100%;
    height: auto;
  }
{% endstylesheet %}
```

## `blocks/text.liquid` — Text

Theme-editor text block with title, subtitle, and normal styles plus alignment.

```liquid
{% doc %}
  Renders a text block.
{% enddoc %}

<div
  class="text {{ block.settings.text_style }}"
  style="--text-align: {{ block.settings.alignment }}"
  {{ block.shopify_attributes }}
>
  {{ block.settings.text }}
</div>

{% stylesheet %}
  .text {
    text-align: var(--text-align);
  }
  .text--title {
    font-size: 2rem;
    font-weight: 700;
  }
  .text--subtitle {
    font-size: 1.5rem;
  }
{% endstylesheet %}
```

## `blocks/group.liquid` — Group

Nestable horizontal or vertical flex layout controlled in the theme editor.

```liquid
{% doc %}
  Renders a group of blocks with configurable layout direction, gap and alignment.
{% enddoc %}

<div
  class="group {{ block.settings.layout_direction }}"
  style="
    --padding: {{ block.settings.padding }}px;
    --alignment: {{ block.settings.alignment }};
  "
  {{ block.shopify_attributes }}
>
  {% content_for 'blocks' %}
</div>

{% stylesheet %}
  .group {
    display: flex;
    flex-wrap: nowrap;
    overflow: hidden;
    width: 100%;
  }

  .group--horizontal {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    padding: 0 var(--padding);
  }

  .group--vertical {
    flex-direction: column;
    align-items: var(--alignment);
    padding: var(--padding) 0;
  }
{% endstylesheet %}
```

## Existing asset icons

- `assets/icon-account.svg` — customer account icon
- `assets/icon-cart.svg` — shopping bag/cart icon

