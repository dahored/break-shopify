---
name: code-review
description: Checklist de code review específico para el tema Shopify break-theme — Liquid, CSS, performance y accesibilidad
---

# Code Review — break-theme

Checklist de revisión de código para archivos Liquid del tema Shopify.

## Checklist por tipo de archivo

### SNIPPETS

- [ ] `{% doc %}` present como primera línea
- [ ] Todos los `@param` documentados con tipo y descripción
- [ ] Parámetros opcionales marcados con `[nombre]`
- [ ] Al menos un `@example` funcional
- [ ] NO accede a variables locales del padre (solo globales y params)
- [ ] CSS en `{% stylesheet %}`, JS en `{% javascript %}`

### BLOCKS

- [ ] `{% doc %}` si es renderizado estáticamente
- [ ] `{{ block.shopify_attributes }}` en el elemento raíz HTML
- [ ] `{% content_for 'blocks' %}` si acepta bloques hijos
- [ ] Schema tiene `"presets"` con al menos un preset
- [ ] Nombre del schema usa clave de traducción (`t:general.nombre`)
- [ ] CSS en `{% stylesheet %}`, JS en `{% javascript %}`

### SECTIONS

- [ ] Schema tiene `"name"` con clave de traducción
- [ ] Schema tiene `"presets"` para aparecer en "Add section"
- [ ] `{% content_for 'blocks' %}` si acepta bloques
- [ ] CSS en `{% stylesheet %}`, JS en `{% javascript %}`
- [ ] Sin texto hardcodeado visible al usuario

### TODOS LOS ARCHIVOS LIQUID

- [ ] No hay texto en inglés hardcodeado — todo usa `{{ 'clave' | t }}`
- [ ] Sentence case en traducciones ("Add to cart", no "Add To Cart")
- [ ] Nuevas claves añadidas a `locales/en.default.json`
- [ ] Sin `console.log` en producción
- [ ] Sin SQL injection risk (N/A en Liquid, pero verificar interpolaciones en URLs)

## Revisión de CSS

- [ ] Estilos de componente en `{% stylesheet %}`, no en `critical.css`
- [ ] CSS variables para settings de 1 propiedad CSS
- [ ] Clases BEM-like para settings de múltiples propiedades CSS
- [ ] Sin `!important` innecesario
- [ ] Responsive considerado (mobile-first preferido)
- [ ] Nombres de clases siguen convención del componente: `.block-name--modifier`

## Revisión de JavaScript

- [ ] Sin `var` — usar `const`/`let`
- [ ] Event listeners limpios (considerar `removeEventListener` si aplica)
- [ ] Verificar existencia del elemento antes de manipular DOM
- [ ] Compatible con theme editor events si el block es seleccionable:
  ```js
  document.addEventListener('shopify:block:select', (e) => { ... })
  document.addEventListener('shopify:block:deselect', (e) => { ... })
  ```

## Revisión de performance

- [ ] Imágenes usan `image_url` con `width` apropiado (no full-res)
- [ ] `image_tag` incluye `loading: 'lazy'` para imágenes below the fold
- [ ] Fuentes críticas preloaded en `layout/theme.liquid`
- [ ] CSS no crítico en `{% stylesheet %}`, no en `critical.css`
- [ ] Sin loops que hagan fetch en cada iteración

## Revisión de accesibilidad

- [ ] Imágenes tienen `alt` significativo (no vacío salvo decorativas)
- [ ] Botones tienen texto o `aria-label`
- [ ] Contraste suficiente entre `--color-background` y `--color-foreground`
- [ ] Formularios tienen `<label>` asociado al input
- [ ] Navegación por teclado funcional en interactivos

## Revisión de schema

- [ ] Validado contra `schemas/section.json` o `schemas/theme_block.json`
- [ ] Tipos de settings correctos (no `text` donde debería ir `richtext`)
- [ ] `visible_if` usado para settings condicionales (no mostrar settings irrelevantes)
- [ ] Labels usan claves de traducción (`t:labels.nombre`)
- [ ] Options values usan prefijo del clase CSS si aplica (`block--full-width`)
- [ ] Defaults son valores razonables

## Red flags automáticos

```liquid
{# ❌ Texto hardcodeado #}
<button>Add to cart</button>

{# ✅ Con traducción #}
<button>{{ 'cart.add' | t }}</button>

{# ❌ Sin atributos de Shopify en block #}
<div class="block">

{# ✅ Con atributos #}
<div class="block" {{ block.shopify_attributes }}>

{# ❌ CSS global innecesario #}
{# en critical.css: .my-specific-block { color: red } #}

{# ✅ CSS de componente #}
{% stylesheet %}
  .my-specific-block { color: red }
{% endstylesheet %}
```
