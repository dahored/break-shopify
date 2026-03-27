---
name: architect
description: Decisiones de arquitectura para el tema Shopify break-theme — cuándo usar section vs block vs snippet, estructura de CSS, y patrones de composición
---

# Theme Architect

Guía de decisiones arquitectónicas para **break-theme** basada en el Shopify Skeleton Theme.

## Árbol de decisión: ¿qué tipo de componente crear?

```
¿El merchant puede editarlo en el theme editor?
├── SÍ
│   ├── ¿Ocupa todo el ancho de la página?
│   │   ├── SÍ → SECTION (sections/)
│   │   └── NO → BLOCK (blocks/)
│   └── ¿Necesita ser anidado dentro de otro componente?
│       └── SÍ → BLOCK con "blocks": [{"type": "@theme"}] en parent
└── NO → SNIPPET (snippets/)
```

## Cuándo usar SECTION

- Componente de ancho completo (hero, product grid, footer, header)
- Los merchants pueden añadirlo/quitarlo desde el theme editor
- Contiene `{% content_for 'blocks' %}` para bloques hijos
- Tiene `presets` en el schema para aparecer en "Add section"

## Cuándo usar BLOCK

- Componente reutilizable dentro de una sección
- Puede ser anidado (block dentro de block via `"blocks": [{"type": "@theme"}]`)
- El merchant controla orden y visibilidad
- Ejemplos del proyecto: `group.liquid` (layout wrapper), `text.liquid` (texto)

## Cuándo usar SNIPPET

- Lógica compartida sin controles en el theme editor
- Renderizado via `{% render 'nombre', param: valor %}`
- Ejemplos del proyecto: `css-variables.liquid`, `image.liquid`, `meta-tags.liquid`

## Estructura de CSS: cuándo usar variables vs clases

| Situación | Patrón | Ejemplo |
|-----------|--------|---------|
| 1 propiedad CSS controlada por setting | CSS variable inline | `style="--gap: {{ setting }}px"` |
| 2+ propiedades CSS por un setting | Clase CSS | `class="block {{ block.settings.layout }}"` |
| Estilos globales del tema | `snippets/css-variables.liquid` | `--color-background`, `--page-width` |
| Estilos críticos (above the fold) | `assets/critical.css` | Solo lo mínimo para cada página |
| Estilos de componente | `{% stylesheet %}` en el archivo | Dentro de section/block/snippet |

## Convenciones de nomenclatura CSS

```css
/* Componente */
.block-name { }

/* Modificador (BEM-like) */
.block-name--modifier { }

/* Elemento hijo */
.block-name__child { }

/* CSS variables de componente */
--block-name-property: value;
```

## Composición de templates

Los templates JSON definen qué secciones contiene cada tipo de página:

```json
{
  "sections": {
    "main": {
      "type": "product",
      "blocks": { ... }
    }
  },
  "order": ["main"]
}
```

**Regla:** No crear templates desde código — los merchants los configuran via theme editor. Crear secciones/bloques que puedan componerse.

## Principios de arquitectura

1. **Modularidad**: Cada componente es autocontenido (HTML + CSS + JS + schema)
2. **Composabilidad**: Sections contienen Blocks que pueden contener Blocks
3. **Editabilidad**: Todo lo editable expuesto via schema settings
4. **Performance**: CSS crítico en `critical.css`, resto en `{% stylesheet %}`
5. **i18n-first**: Todo texto en `locales/en.default.json` desde el inicio

## Patrón de section estándar

```
sections/nombre-section.liquid
├── HTML del componente
├── {% content_for 'blocks' %}    ← si acepta bloques
├── {% stylesheet %}              ← estilos del componente
├── {% javascript %}              ← JS del componente (si necesario)
└── {% schema %}                  ← configuración del merchant
```

## Anti-patrones a evitar

- ❌ Texto hardcodeado sin usar `| t` filter
- ❌ Estilos inline en lugar de CSS variables o clases
- ❌ Lógica compleja en snippets sin parámetros documentados en `{% doc %}`
- ❌ CSS global en `critical.css` para componentes específicos
- ❌ JavaScript que asume DOM disponible sin verificar
- ❌ Crear templates en código (son responsabilidad del merchant)
