---
aliases: [PYS, Péptidos y Suplementos MX, peptidosysuplementos.mx]
tags: [proyecto, pys, wordpress, woocommerce, seo]
estado: Operando
ruta: Remoto (peptidosysuplementos.mx)
---

# PYS — Péptidos y Suplementos MX

> Farmacia online de péptidos y suplementos en México.
> URL: peptidosysuplementos.mx

## Descripción

Tienda WooCommerce con enfoque SEO que vende péptidos y suplementos. **Empresa independiente** (actualizado 2026-06-19): ya NO es "brazo farmacéutico" de [[PTM Novo]] ni parte de su cadena de venta. PYS vende a quien le compre, sin relación de cobro con PTM. Ver [[Ecosistema PTM-PYS]] (nota de separación).

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
## Nota 2026-06-19 — Calculadora de Dosis de Péptidos
Se publicó la **Calculadora de Dosis de Péptidos** (`/calculadora-de-dosis-de-peptidos/`, post 2040) — widget HTML autocontenido en Elementor que replica la UX de particlepeptides.com en español (jeringas/viales SVG, toggle mcg/mg, regla con marcador). Botón "Calculadora" añadido al menú del header (template Elementor **#1099**, widget HTML `c442c6a` — el header NO usa menú de WP). Ver [[2026-06-19 — PYS Calculadora de Dosis]].

---

## Sesión 2026-06-23 — Catálogo (SKU, slugs, precios) + Omega-3 + imágenes
- **SKU** asignados a los 16 productos (esquema `PREFIX-DOSIS`; fluyen al Product schema de Rank Math). Los 16 YA tenían Product schema (el agente dio falso negativo — su `fetch_url` no ve el JSON-LD).
- **MOTS-c** slugs cruzados corregidos (793=40mg→`/mots-c-40mg/`, 790=10mg→`/mots-c-10mg/`, slug cycling wp/v2). **Agua Bacteriostática** slug → `agua-bacteriostatica-3ml`.
- **⚠️ Incidente:** WooCommerce Quick Edit borró precios (15 a $0) y stock al guardar SKU → **restaurado 16/16**. Quick Edit no precarga `_sku/_regular_price/_stock_status`; fijarlos siempre.
- **Omega-3 (1151):** descripción rica (~450 palabras, H2, specs EPA 1,200/DHA 850, 6 FAQs) publicada **editando el widget `text-editor` de `_elementor_data`** (no post_content) + Elementor "Clear Files & Data" + LiteSpeed. 🔑 **El contenido visible de los productos vive en `_elementor_data`** — matiza la convención de arriba: el TEXTO sí se puede editar por API vía `_elementor_data` (siempre limpiar caché Elementor después), no solo title/meta/slug.
- **Imágenes:** QUIC.cloud activado (WebP + auto cron, sin CDN); home tenía ~4.6 MB de imágenes. Optimización en curso → **pendiente** re-medir PageSpeed + purgar LiteSpeed al terminar.
- Ver [[2026-06-23 — PYS SKU y Precios, Raditech LCP, Omega-3 Elementor]].
