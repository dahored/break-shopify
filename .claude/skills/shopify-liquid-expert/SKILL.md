---
name: shopify-liquid-expert
description: Expert en creación de sections, blocks, snippets y templates de Shopify Liquid para el tema break-theme (Skeleton Theme)
---

# Shopify Liquid Expert

Eres un experto en desarrollo de temas Shopify usando Liquid. Este tema sigue la arquitectura Skeleton Theme de Shopify.

## Arquitectura del tema

```
break-theme/
├── assets/          # Solo critical.css y archivos estáticos globales
├── blocks/          # Componentes reutilizables y anidables (group.liquid, text.liquid)
├── config/          # settings_schema.json + settings_data.json
├── layout/          # theme.liquid, password.liquid
├── locales/         # en.default.json, en.default.schema.json
├── sections/        # Módulos de página completa
├── snippets/        # Fragmentos reutilizables (css-variables.liquid, image.liquid, meta-tags.liquid)
└── templates/       # JSON templates por tipo de página
```

## Variables CSS globales (snippets/css-variables.liquid)

```liquid
--font-primary--family
--font-primary--style
--font-primary--weight
--page-width          (settings.max_page_width: 90rem | 110rem)
--page-margin         (settings.min_page_margin: 10-100px)
--color-background    (settings.background_color: #FFFFFF default)
--color-foreground    (settings.foreground_color: #333333 default)
--style-border-radius-inputs (settings.input_corner_radius: 0-10px)
```

## Reglas de creación de SNIPPETS

- SIEMPRE incluir `{% doc %}` como header
- Aceptan parámetros vía `render` tag
- No pueden acceder a variables locales del archivo padre (solo globales y parámetros)
- Usar `{% stylesheet %}` y `{% javascript %}` para estilos/scripts del componente

```liquid
{% doc %}
  @param {tipo} nombre - Descripción
  @param {tipo} [nombre] - Parámetro opcional

  @example
  {% render 'nombre-snippet', param: valor %}
{% enddoc %}
```

## Reglas de creación de BLOCKS

- SIEMPRE incluir `{% doc %}` si se renderizan estáticamente via `content_for 'block'`
- Usar `{{ block.shopify_attributes }}` en el elemento raíz
- Incluir `{% schema %}` con nombre, settings y presets
- Validar schema con `schemas/theme_block.json`
- CSS via `{% stylesheet %}`, JS via `{% javascript %}`
- Para renderizar hijos: `{% content_for 'blocks' %}`

```liquid
{% schema %}
{
  "name": "t:general.nombre",
  "blocks": [{ "type": "@theme" }],  // si acepta bloques hijos
  "settings": [...],
  "presets": [{ "name": "t:general.nombre" }]
}
{% endschema %}
```

## Reglas de creación de SECTIONS

- Incluir `{% schema %}` con name, blocks, settings y presets
- Validar schema con `schemas/section.json`
- Renderizar bloques con `{% content_for 'blocks' %}`
- CSS via `{% stylesheet %}`, JS via `{% javascript %}`

## Patrón CSS variables vs clases

**Una propiedad CSS → CSS variable:**
```liquid
<div style="--gap: {{ block.settings.gap }}px">
{% schema %}{ "type": "range", "id": "gap" }{% endschema %}
```

**Múltiples propiedades → clase CSS:**
```liquid
<div class="component {{ block.settings.layout }}">
{% schema %}{ "type": "select", "id": "layout", "values": [{"value": "component--full", ...}] }{% endschema %}
```

## Traducciones

- TODO texto visible al usuario DEBE usar `{{ 'clave' | t }}`
- Actualizar `locales/en.default.json` con cada nueva clave
- Usar sentence case: "Add to cart" NO "Add To Cart"
- Estructura jerárquica máximo 3 niveles: `sections.hero.title`

## Bloques existentes

- `blocks/group.liquid` — Contenedor flex con dirección, alineación y padding
- `blocks/text.liquid` — Texto con estilos (title, subtitle, normal) y alineación

## Snippets existentes

- `snippets/css-variables.liquid` — Variables CSS globales del tema
- `snippets/image.liquid` — Imagen responsiva con enlace opcional
- `snippets/meta-tags.liquid` — Meta tags SEO y Open Graph

## Layout principal (layout/theme.liquid)

```liquid
<html lang="{{ request.locale.iso_code }}">
  <head>
    {% render 'css-variables' %}
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

## Secciones existentes

`header.liquid`, `footer.liquid`, `product.liquid`, `collection.liquid`,
`cart.liquid`, `blog.liquid`, `article.liquid`, `search.liquid`,
`404.liquid`, `page.liquid`, `collections.liquid`, `password.liquid`,
`custom-section.liquid`, `hello-world.liquid`

## Templates existentes

`index.json`, `product.json`, `collection.json`, `cart.json`,
`blog.json`, `article.json`, `search.json`, `page.json`,
`404.json`, `list-collections.json`, `password.json`, `gift_card.liquid`

## Comando de desarrollo

```bash
shopify theme dev --store=dahodev.myshopify.com
```
