---
name: commit
description: Convenciones de commits git para el tema Shopify break-theme con conventional commits
---

# Git Commit Expert

Convenciones de commits para el proyecto **break-theme** (Shopify Skeleton Theme).

## Formato de commit

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

## Tipos permitidos

| Tipo | Uso |
|------|-----|
| `feat` | Nuevo bloque, sección, snippet o funcionalidad |
| `fix` | Corrección de bug en Liquid, CSS o JS |
| `style` | Cambios de estilos CSS sin afectar lógica |
| `refactor` | Reestructuración de código sin cambiar comportamiento |
| `i18n` | Añadir/modificar claves de traducción en locales/ |
| `schema` | Cambios en schemas de sections o blocks |
| `config` | Cambios en config/settings_schema.json o settings_data.json |
| `assets` | Añadir/modificar archivos en assets/ |
| `docs` | Documentación, LiquidDoc, comentarios |
| `chore` | Mantenimiento general, .gitignore, etc. |

## Scopes por directorio

- `blocks` — archivos en blocks/
- `sections` — archivos en sections/
- `snippets` — archivos en snippets/
- `layout` — archivos en layout/
- `templates` — archivos en templates/
- `locales` — archivos en locales/
- `config` — archivos en config/
- `assets` — archivos en assets/

## Ejemplos

```bash
feat(blocks): add carousel block with autoplay settings
fix(sections): correct product price display on mobile
style(blocks): improve group block flex alignment
i18n(locales): add translation keys for hero section
schema(sections): add background overlay opacity setting
refactor(snippets): simplify image snippet parameter handling
config(settings): add accent color to global theme settings
```

## Reglas

1. Descripción en inglés, imperativo presente ("add" no "added")
2. Máximo 72 caracteres en la primera línea
3. Si el cambio afecta múltiples scopes, usar el más relevante
4. Referenciar issue/ticket en el footer si aplica: `Refs: #123`

## Flujo de trabajo

```bash
# Ver estado
git status
git diff

# Staging selectivo (NUNCA git add -A sin revisar)
git add blocks/carousel.liquid
git add locales/en.default.json

# Commit
git commit -m "feat(blocks): add carousel block with autoplay"

# Push
git push origin <branch>
```
