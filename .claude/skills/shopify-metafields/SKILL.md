---
name: shopify-metafields
description: Uso de Metafields y Metaobjects como base de datos en temas Shopify — definiciones, acceso en Liquid, y patrones de uso
---

# Shopify Metafields & Metaobjects — DB Layer

Metafields y Metaobjects son el sistema de almacenamiento de datos extendido de Shopify para temas.

## Metafields — datos adicionales en recursos existentes

### Recursos que soportan metafields
- `product` — productos
- `variant` — variantes
- `collection` — colecciones
- `customer` — clientes
- `order` — pedidos
- `page` — páginas
- `blog` / `article` — blogs y artículos
- `shop` — la tienda

### Acceso en Liquid

```liquid
{# Sintaxis directa #}
{{ product.metafields.namespace.key }}
{{ article.metafields.custom.subtitle }}

{# Con filtros #}
{{ product.metafields.specs.weight | metafield_text }}
{{ product.metafields.media.gallery | metafield_tag }}

{# Verificar existencia #}
{% if product.metafields.custom.subtitle %}
  {{ product.metafields.custom.subtitle }}
{% endif %}
```

### Tipos de metafield disponibles

| Tipo | Descripción | Uso en Liquid |
|------|-------------|---------------|
| `single_line_text_field` | Texto corto | `{{ metafield.value }}` |
| `multi_line_text_field` | Texto largo | `{{ metafield.value | newline_to_br }}` |
| `rich_text_field` | HTML enriquecido | `{{ metafield.value }}` |
| `number_integer` | Número entero | `{{ metafield.value }}` |
| `number_decimal` | Número decimal | `{{ metafield.value }}` |
| `boolean` | Verdadero/falso | `{% if metafield.value %}` |
| `date` | Fecha | `{{ metafield.value | date: "%B %d, %Y" }}` |
| `date_time` | Fecha y hora | `{{ metafield.value | date: format }}` |
| `color` | Color hex | `{{ metafield.value }}` |
| `url` | URL | `<a href="{{ metafield.value }}">` |
| `json` | JSON libre | `{{ metafield.value.key }}` |
| `file_reference` | Imagen/archivo | `{{ metafield.value | image_url: width: 800 | image_tag }}` |
| `product_reference` | Referencia a producto | `{{ metafield.value.title }}` |
| `collection_reference` | Referencia a colección | `{{ metafield.value.products }}` |
| `page_reference` | Referencia a página | `{{ metafield.value.content }}` |
| `list.*` | Lista de cualquier tipo | `{% for item in metafield.value %}` |

### Exponer metafields en settings de sections

```liquid
{% schema %}
{
  "settings": [
    {
      "type": "text",
      "id": "metafield_key",
      "label": "Metafield key (namespace.key)"
    }
  ]
}
{% endschema %}

{# En el Liquid: #}
{% assign mf_parts = section.settings.metafield_key | split: '.' %}
{% assign ns = mf_parts[0] %}
{% assign key = mf_parts[1] %}
{{ product.metafields[ns][key] }}
```

## Metaobjects — tipos de datos personalizados

Los metaobjects son como tablas de base de datos personalizadas. Ideal para: testimonios, FAQ, miembros del equipo, características de producto.

### Acceso en Liquid

```liquid
{# Acceso via global metaobjects #}
{% for testimonial in metaobjects.testimonials.values %}
  <div class="testimonial">
    <p>{{ testimonial.fields.quote.value }}</p>
    <cite>{{ testimonial.fields.author.value }}</cite>
    {% if testimonial.fields.avatar.value %}
      {{ testimonial.fields.avatar.value | image_url: width: 80 | image_tag }}
    {% endif %}
  </div>
{% endfor %}

{# Metaobject referenciado desde un producto #}
{{ product.metafields.custom.specs.value.fields.material.value }}

{# En template de metaobject (/metaobject/tipo) #}
{{ metaobject.fields.title.value }}
{{ metaobject.fields.description.value }}
```

### Template de metaobject

Crear `templates/metaobject.tipo.json` para páginas de detalle de metaobjet:

```json
{
  "sections": {
    "main": {
      "type": "metaobject-detail",
      "settings": {}
    }
  },
  "order": ["main"]
}
```

## Patrones comunes

### Galería de imágenes en producto

```liquid
{# Metafield tipo: list.file_reference #}
{% if product.metafields.custom.gallery %}
  {% for image in product.metafields.custom.gallery.value %}
    {{ image | image_url: width: 600 | image_tag }}
  {% endfor %}
{% endif %}
```

### FAQ con metaobjects

```liquid
{# Metaobject: faq con fields: question, answer #}
{% for item in metaobjects.faq.values %}
  <details>
    <summary>{{ item.fields.question.value }}</summary>
    <div>{{ item.fields.answer.value }}</div>
  </details>
{% endfor %}
```

### Color personalizado por producto

```liquid
{# Metafield tipo: color en namespace custom #}
{% assign custom_color = product.metafields.custom.accent_color.value %}
{% if custom_color %}
  <div style="--accent: {{ custom_color }}">
{% else %}
  <div style="--accent: var(--color-foreground)">
{% endif %}
```

## Definición de metafields (Admin API)

Los metafields se definen en **Shopify Admin → Settings → Custom data** o via GraphQL Admin API. En el tema, solo se consumen — no se definen.

**Namespace recomendado para este tema:** `custom` (namespace por defecto de Shopify)
