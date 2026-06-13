---
tags: [sesion, agente-blogs, grupoptm, peptidosysuplementos, wordpress, seo, 2026-06]
fecha: 2026-06-08
proyecto: Agente de Blogs / Ecommerce Agent
---

# Sesión 2026-06-08 — Fixes Agentes PYS y PTM

## Qué se hizo

### Agente Blogs — Categorías automáticas en posts nuevos
- Todos los posts nuevos se publicaban sin categoría (aparecían como "Uncategorized")
- Agregado `default_categories: ["Blog"]` en `config.py` para ambos sitios (`peptidosysuplementos` y `grupoptm`)
- Nueva función `get_or_create_categories(wp_url, headers, names)` en `tools/wordpress.py`
  - Busca categoría por nombre exacto (case-insensitive)
  - Si no existe, la crea automáticamente via WP REST API
- `publish_post` ahora llama a `get_or_create_categories` con las categorías del `blog_data` o el default del sitio
- Bug encontrado: `wp_author_name` tenía un non-breaking space (char 160) — se limpió al reescribir `config.py`

### Ecommerce Agent — Miniaturas en listing de grupoptm.com
- `grupoptm.com/blog/` no mostraba imágenes en los posts del listing
- Diagnóstico: la página Blog (ID 22007) usa un bloque `wp:latest-posts` de Gutenberg sin `displayFeaturedImage:true`
- Nuevo endpoint `POST /fix-blog-thumbnails` en `ecommerce-agent/web.py`:
  - Lee la página 22007 via WP REST API con `context=edit`
  - Busca el bloque `<!-- wp:latest-posts ... /-->` con regex
  - Agrega `displayFeaturedImage:true, featuredImageSizeSlug:"medium"` a los atributos
  - Guarda el contenido actualizado
- Ejecutado el endpoint → imágenes aparecen en el listing

## Archivos modificados

| Archivo | Repo | Cambio |
|---|---|---|
| `config.py` | agente-blogs | `default_categories: ["Blog"]` en ambos sitios |
| `tools/wordpress.py` | agente-blogs | Nueva función `get_or_create_categories`, llamada en `publish_post` |
| `web.py` | ecommerce-agent | Nuevo endpoint `POST /fix-blog-thumbnails` |

## Pendientes descartados

- Fix author name PTM → ya estaba resuelto, eliminado de pendientes
- Notificaciones WhatsApp/Telegram → descartado por el usuario

## Pendiente activo

- Preview antes de publicar (borrador para aprobar antes de que salga al blog)
- Optimización automática de productos nuevos en PYS
- Agente Instagram → bloqueado por API de Meta



---

**MOC:** [[MOC - PTM y PYS]]