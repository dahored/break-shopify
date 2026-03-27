---
name: project-manager
description: Estado actual del proyecto break-theme, componentes existentes, y tracking de trabajo pendiente
---

# Project Manager — break-theme

Tema Shopify basado en **Skeleton Theme** de Shopify. Store: `dahodev.myshopify.com`.

## Estado del proyecto

**Tema:** break-theme (Shopify Skeleton Theme)
**Versión:** 0.1.0
**Autor base:** Shopify → fork personalizado

## Inventario de componentes

### Blocks (`blocks/`)
| Archivo | Propósito | Estado |
|---------|-----------|--------|
| `group.liquid` | Contenedor flex horizontal/vertical con padding y alineación | ✅ Base |
| `text.liquid` | Texto con estilo (title/subtitle/normal) y alineación | ✅ Base |

### Sections (`sections/`)
| Archivo | Propósito |
|---------|-----------|
| `header.liquid` | Cabecera global del sitio |
| `footer.liquid` | Pie de página global |
| `product.liquid` | Página de producto |
| `collection.liquid` | Página de colección |
| `cart.liquid` | Página de carrito |
| `blog.liquid` | Listado de artículos |
| `article.liquid` | Artículo individual |
| `search.liquid` | Resultados de búsqueda |
| `404.liquid` | Página de error |
| `page.liquid` | Páginas estáticas |
| `collections.liquid` | Listado de colecciones |
| `password.liquid` | Página de contraseña |
| `custom-section.liquid` | Sección ejemplo con imagen de fondo |
| `hello-world.liquid` | Sección ejemplo básica |

### Snippets (`snippets/`)
| Archivo | Propósito |
|---------|-----------|
| `css-variables.liquid` | Variables CSS globales del tema |
| `image.liquid` | Imagen responsiva con enlace opcional |
| `meta-tags.liquid` | SEO: title, description, Open Graph |

### Templates (`templates/`)
`index.json`, `product.json`, `collection.json`, `cart.json`,
`blog.json`, `article.json`, `search.json`, `page.json`,
`404.json`, `list-collections.json`, `password.json`, `gift_card.liquid`

### Layout (`layout/`)
- `theme.liquid` — Layout principal con header-group y footer-group
- `password.liquid` — Layout para tienda en modo contraseña

### Config global
- **Tipografía:** `type_primary_font` (Work Sans por defecto)
- **Layout:** `max_page_width` (90rem/110rem), `min_page_margin` (10-100px)
- **Colores:** `background_color` (#FFF), `foreground_color` (#333)
- **Inputs:** `input_corner_radius` (0-10px)

### Traducciones (`locales/en.default.json`)
Claves existentes: `404`, `blog`, `cart`, `customers`, `collections`,
`gift_card`, `password`, `search`

## Comandos del proyecto

```bash
# Desarrollo local
shopify theme dev --store=dahodev.myshopify.com

# Push al store
shopify theme push --store=dahodev.myshopify.com

# Pull del store
shopify theme pull --store=dahodev.myshopify.com
```

## Criterios de calidad

- [ ] Todo texto visible usa `| t` filter
- [ ] Todos los snippets tienen `{% doc %}` header
- [ ] Todos los blocks tienen `{{ block.shopify_attributes }}`
- [ ] CSS en `{% stylesheet %}` (no inline excepto CSS variables)
- [ ] Nuevas claves en `locales/en.default.json`
