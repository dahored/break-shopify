---
name: i18n-workflow
description: Flujo de trabajo de internacionalización para break-theme — añadir claves, estructura jerárquica, y convenciones de traducción
---

# i18n Workflow — break-theme

Gestión de traducciones para el tema Shopify usando `locales/en.default.json`.

## Archivos de locales

```
locales/
├── en.default.json        # Traducciones de la tienda (strings en templates)
└── en.default.schema.json # Traducciones del theme editor (labels, options, etc.)
```

## Estructura actual de `en.default.json`

```json
{
  "404": { ... },
  "blog": { ... },
  "cart": { "checkout", "title", "update", "remove" },
  "customers": { "login": { ... } },
  "collections": { "title" },
  "gift_card": { ... },
  "password": { ... },
  "search": { "title", "placeholder", "submit", "no_results_html", "results_for_html" }
}
```

## Jerarquía de claves recomendada

```json
{
  "general": {
    "typography": "...",
    "layout": "...",
    "colors": "...",
    "group": "...",
    "text": "...",
    "row": "...",
    "column": "..."
  },
  "labels": {
    "text": "...",
    "text_style": "...",
    "alignment": "...",
    "layout_direction": "...",
    "padding": "...",
    "background": "...",
    "foreground": "...",
    "page_width": "...",
    "page_margin": "..."
  },
  "options": {
    "text_style": {
      "title": "...",
      "subtitle": "...",
      "normal": "..."
    },
    "direction": {
      "horizontal": "...",
      "vertical": "..."
    },
    "alignment": {
      "left": "...",
      "center": "...",
      "right": "..."
    }
  },
  "sections": {
    "nombre_section": {
      "title": "...",
      "description": "..."
    }
  }
}
```

## Reglas de nomenclatura

1. **snake_case** para nombres de clave: `add_to_cart`, no `addToCart`
2. **Máximo 3 niveles de profundidad:** `sections.hero.title` ✅ / `sections.hero.cta.button.text` ❌
3. **Sentence case** para valores: `"Add to cart"` no `"Add To Cart"`
4. **Variables en doble llave:** `"Page {{ page }} of {{ pages }}"`

## Cómo añadir una nueva clave

### 1. Añadir a `en.default.json`

```json
{
  "sections": {
    "hero": {
      "title": "Welcome to our store",
      "subtitle": "Discover our collections",
      "cta": "Shop now"
    }
  }
}
```

### 2. Usar en Liquid

```liquid
<h1>{{ 'sections.hero.title' | t }}</h1>
<p>{{ 'sections.hero.subtitle' | t }}</p>
<a href="/collections">{{ 'sections.hero.cta' | t }}</a>
```

### 3. Con variables de interpolación

```json
{
  "search": {
    "results_count": "{{ count }} results for \"{{ terms }}\""
  }
}
```

```liquid
{{ 'search.results_count' | t: count: search.results_count, terms: search.terms }}
```

## Claves de schema (`en.default.schema.json`)

Las claves de schema son para labels y options del **theme editor**:

```json
{
  "sections": {
    "hero": {
      "description": "Hero banner section"
    }
  },
  "blocks": {
    "text": {
      "description": "Text block"
    }
  }
}
```

En el schema de sections/blocks:
```json
{
  "name": "t:sections.hero.description",
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "t:labels.title"
    }
  ]
}
```

## Checklist de i18n al crear un componente

- [ ] ¿Hay texto visible al usuario? → Añadir a `en.default.json`
- [ ] ¿Hay labels/options en el schema? → Añadir a `en.default.schema.json`
- [ ] ¿Los valores están en sentence case?
- [ ] ¿Las variables de interpolación están documentadas?
- [ ] ¿Se usó `| t` en Liquid para todos los strings?

## Uso de `t:` en schemas

Los schemas de sections y blocks referencian claves con prefijo `t:`:

```json
{
  "name": "t:general.hero",
  "settings": [
    {
      "label": "t:labels.title",
      "type": "text",
      "id": "title"
    },
    {
      "label": "t:labels.layout",
      "type": "select",
      "options": [
        { "value": "full", "label": "t:options.page_width.wide" },
        { "value": "narrow", "label": "t:options.page_width.narrow" }
      ]
    }
  ]
}
```
