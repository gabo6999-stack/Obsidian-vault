---
tags: [proyecto, pys, wordpress, woocommerce, seo]
estado: Operando
ruta: Remoto (peptidosysuplementos.mx)
---

# PYS — Péptidos y Suplementos MX

> Farmacia online de péptidos y suplementos en México.
> URL: peptidosysuplementos.mx

## Descripción

Tienda WooCommerce con enfoque SEO que vende péptidos y suplementos. Es el brazo farmacéutico de [[PTM Novo]] — los pacientes de PTM Novo compran aquí sus péptidos.

## Stack

| Componente | Tecnología |
|-----------|-----------|
| CMS | WordPress |
| Ecommerce | WooCommerce |
| SEO | Rank Math (NO Yoast) |
| Page Builder | Elementor |

## Convenciones Importantes

- **SEO:** Usar `rank_math_title` y `rank_math_description` (nunca Yoast)
- **Elementor:** Solo editar title/meta/slug/rank_math vía API; el diseño solo en editor Elementor
- **API REST:** WordPress REST API para operaciones programáticas

## Agentes Conectados

| Agente | Función |
|--------|---------|
| [[Agente de Blogs]] | Publica 4 blogs/semana automáticamente |
| [[Chatbot PYS]] | Atiende preguntas de clientes (RAG) |
| [[Ecommerce Agent]] | Consulta productos, órdenes, inventario |
| [[Social Video Agent]] | Genera contenido video para redes |

## Estado

🟢 Operando — tienda activa con múltiples agentes corriendo en Railway.



---

## Nota técnica 2026-06-13 — Redirects 301 y SEO WP
En Raditech se perfeccionó técnica para redirects 301 en WordPress via slug cycling REST API (Rank Math auto-genera la entrada). También: cookie+nonce > JWT para Rank Math REST, purge LiteSpeed via nonce admin panel. Estas técnicas aplican igual en peptidosysuplementos.mx. Ver [[2026-06-13 - Raditech SEO Score 100]].