---
name: project-planning
description: Planificación de nuevas funcionalidades y componentes para el tema Shopify break-theme — estimaciones, dependencias y orden de implementación
---

# Project Planning — break-theme

Framework de planificación para nuevas funcionalidades en el tema Shopify.

## Proceso de planificación de un nuevo componente

### 1. Clasificación del componente

Antes de planificar, determinar:
- **Tipo:** section / block / snippet
- **¿Afecta layout global?** → Modifica `layout/theme.liquid` o section groups
- **¿Requiere JS?** → Considerar eventos del theme editor (`shopify:block:select`, etc.)
- **¿Requiere nuevas traducciones?** → Preparar claves en `locales/en.default.json`
- **¿Usa metafields?** → Planificar namespace y definiciones

### 2. Template de planning de tarea

```markdown
## [Nombre del componente]

**Tipo:** section | block | snippet
**Archivos a crear:**
- sections/nombre.liquid
- locales/en.default.json (nuevas claves)

**Archivos a modificar:**
- (ninguno) | config/settings_schema.json | layout/theme.liquid

**Settings del schema:**
- [ ] setting_1: tipo, descripción
- [ ] setting_2: tipo, descripción

**Dependencias:**
- Usa snippet: image, css-variables
- Requiere bloque: nombre-block

**Traducciones nuevas:**
- sections.nombre.title: "Título"
- sections.nombre.subtitle: "Subtítulo"

**Criterios de aceptación:**
- [ ] Se puede añadir desde el theme editor
- [ ] Settings visibles y funcionales
- [ ] Responsive mobile/desktop
- [ ] Texto traducible
- [ ] LiquidDoc / doc tag completo
```

### 3. Orden de implementación

Para funcionalidades complejas, seguir este orden:

```
1. Traducciones → locales/en.default.json
2. Snippets base (si reutilizables)
3. Blocks hijos (si la section tiene blocks)
4. Section principal
5. Config global (si afecta settings_schema.json)
6. Verificación en theme editor
```

## Checklist de lanzamiento de componente

### Código
- [ ] `{% doc %}` presente en snippets y blocks estáticos
- [ ] `{{ block.shopify_attributes }}` en raíz de cada block
- [ ] `{% content_for 'blocks' %}` en sections/blocks con hijos
- [ ] Schema válido según `schemas/section.json` o `schemas/theme_block.json`
- [ ] Sin texto hardcodeado (todo via `| t`)

### CSS/JS
- [ ] Estilos en `{% stylesheet %}` (no en `critical.css` salvo que sea crítico)
- [ ] CSS variables para settings de 1 propiedad
- [ ] Clases CSS para settings de múltiples propiedades
- [ ] JS en `{% javascript %}` si aplica

### i18n
- [ ] Claves añadidas a `locales/en.default.json`
- [ ] Claves de schema en `locales/en.default.schema.json` si aplica

### Testing
- [ ] Verificado en `shopify theme dev --store=dahodev.myshopify.com`
- [ ] Settings funcionan en theme editor
- [ ] Mobile responsivo

## Tipos de tareas recurrentes

| Tarea | Archivos involucrados | Complejidad |
|-------|----------------------|-------------|
| Nuevo block simple | `blocks/nombre.liquid`, `locales/` | Baja |
| Nuevo block con hijos | `blocks/nombre.liquid`, `locales/` | Media |
| Nueva section | `sections/nombre.liquid`, `locales/` | Media |
| Nuevo snippet | `snippets/nombre.liquid` | Baja |
| Nuevo setting global | `config/settings_schema.json`, `snippets/css-variables.liquid` | Media |
| Feature con metafields | `sections/`, metafield definitions en Shopify admin | Alta |

## Estimaciones orientativas

- Snippet simple: 30-60 min
- Block básico: 1-2 horas
- Block complejo con animaciones: 3-5 horas
- Section completa: 2-4 horas
- Feature con metafields: 4-8 horas
