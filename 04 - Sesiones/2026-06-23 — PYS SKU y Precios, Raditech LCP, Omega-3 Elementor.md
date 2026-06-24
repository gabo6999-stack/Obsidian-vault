---
tags: [sesion, pys, raditech, seo, ecommerce-agent, elementor, performance, 2026-06]
fecha: 2026-06-23
proyecto: SEO PYS + Raditech
relacionado: [[PYS Ecommerce]], [[Raditech]], [[Agente SEO GSC]], [[Ecommerce Agent]]
---

# Sesión 2026-06-23 — PYS (SKU, precios, Omega-3) + Raditech LCP

> Sesión operando el agente SEO web (Railway) y, sobre todo, **directamente vía REST API (cookie + X-WP-Nonce) y paneles de wp-admin** en peptidosysuplementos.mx y raditech.mx. Todo verificado contra BD/código fuente, no contra la palabra del agente.

## Qué se hizo

### PYS — catálogo (16 productos)
- **Auditoría de Product schema:** el agente reportó "0/16 con schema" (falso negativo: su `fetch_url` es ciego al `<script type="application/ld+json">`). Verificado en el código fuente real: **los 16 YA tienen Product schema** (Rank Math, dentro de un `@graph` con `MedicalWebPage` + `FAQPage`). Nada que agregar.
- **SKU:** asignados a los 16 con esquema `PREFIX-DOSIS` (patrón existente `RETAT-30MG`). Confirmado que el SKU de WooCommerce **fluye automáticamente** al Product schema de Rank Math.
- **MOTS-c slugs cruzados (bug real):** ID 793 (40mg, $3200) vivía en `/mots-c-10mg/` y el 790 (10mg, $1200) en `/mots-c-40mg/`. Corregido con **slug cycling vía wp/v2** (790→tmp → 793→`mots-c-40mg` → 790→`mots-c-10mg`). Verificado: cada URL sirve su dosis correcta.
- **Agua Bacteriostática (799):** slug decía `10ml` pero todo el contenido es `3ml` → cambiado a `agua-bacteriostatica-3ml` (la URL vieja redirige 301).
- **Omega-3 (1151) — contenido:** descripción ampliada a ~450 palabras con H2, "¿Qué contiene?", "Especificaciones técnicas" y **6 FAQs**, con specs exactos de la etiqueta (EPA 1,200 mg / DHA 850 mg / 3 softgels / 40 porciones / 120 softgels). **Publicado vía Elementor** (ver Decisiones técnicas).
- **Imágenes (QUIC.cloud):** activado QUIC.cloud (cuenta gabo6999@gmail.com, **sin CDN**), Image Optimization con **WebP** + Auto Cron. Optimización corriendo en segundo plano (PYS home tenía ~4.6 MB de imágenes). **Pendiente** re-medir.

### Raditech — performance (LCP)
- **Causa raíz del LCP de 10.5 s:** el "SVG" hero `pacs-raditech-interfaz.svg` pesaba **1.5 MB** (¡era un raster PNG/JPG incrustado en base64 dentro de un SVG! → LiteSpeed Image Optimization no lo toca).
- **Fix:** generé un **WebP optimizado con canvas** del propio navegador (1000px, q0.80) → **47.5 KB (−96.9%)**, subido a Multimedia (ID 950); reemplacé las 2 referencias del hero en `_elementor_data` + limpié caché Elementor + LiteSpeed.
- **Resultado:** **LCP 10.5 s → 2.6 s (−75%)**, CLS 0.001, FCP 1.8 s, TBT 160 ms. Verificado en PageSpeed.

## Decisiones técnicas

- **Las páginas de PYS y Raditech son Elementor.** El contenido visible vive en el postmeta **`_elementor_data`**, NO en `post_content`. Editar `post_content` (por el agente o por REST) NO se ve, porque Elementor renderiza `_elementor_data` vía su filtro `the_content`.
- **`_elementor_data` SÍ es editable por wp/v2** (Elementor lo registra `show_in_rest`). Método: `GET meta._elementor_data` → `JSON.parse` → editar (URL del hero, o `settings.editor` del widget `text-editor`) → `JSON.stringify` → `POST {meta:{_elementor_data:...}}`.
- **SIEMPRE limpiar la caché de Elementor tras editar por API:** `Elementor → Tools → "Clear Files & Data"`. Elementor cachea el render de elementos y una edición por REST NO dispara su invalidación. **Este fue el paso decisivo** (sin él, nada se ve aunque la BD esté correcta).
- **Optimizar imágenes con canvas del navegador:** cargar la imagen en un `<canvas>`, redimensionar, `canvas.toBlob(b, 'image/webp', 0.8)`, subir vía `POST /wp-json/wp/v2/media` con `X-WP-Nonce` + `Content-Disposition`. Funciona para imágenes que LiteSpeed no puede optimizar (SVG, base64).
- **Verificar siempre contra la fuente real**, no contra el agente ni el render cacheado: `wp/v2/...?context=edit` (raw, BD), Store API `wc/store/v1/products/{id}` (BD, sin caché de página), y la cabecera `x-litespeed-cache: hit/miss`.

## Problemas encontrados

- **⚠️ Incidente: WooCommerce Quick Edit borró precios y stock.** El Quick Edit de este sitio NO precarga `_sku/_regular_price/_stock_status` (salen vacíos), así que al guardar el SKU dejó **15 productos a $0** y puso "en stock" a 9 que estaban agotados. **Detectado y restaurado por completo** (16/16) con los valores del audit previo. **Regla:** al usar Quick Edit hay que fijar EXPLÍCITAMENTE todos los campos a preservar.
- **El agente SEO es poco fiable:** falsos "guardado OK", ciego al JSON-LD, y **no puede** escribir `sku` ni `slug` (su `update_product_full` no expone esos campos). Por eso casi todo se hizo por REST/wp-admin directo.
- **wp/v2 no escribe `_sku`** (meta protegida) → SKU se puso por Quick Edit; **`slug` sí** se cambia por wp/v2.
- **Falsos positivos del agente descartados:** Tirzepatida 60mg está bien; el problema de specs 800/600 del Omega-3 solo estaba en `post_content` (que no se muestra) — el Elementor visible ya tenía 1,200/850.
- **Larga cacería de caché en Raditech:** se descartó caché de página/objetos de LiteSpeed, caché de objetos de Hostinger e incluso transients antes de identificar que era Elementor.

## Estado / próximos pasos

- [x] MOTS-c slugs corregidos · Agua 3ml · 16 SKU · precios/stock restaurados
- [x] Raditech hero WebP (LCP 10.5→2.6 s) en vivo
- [x] Omega-3 contenido rico + 6 FAQs en vivo (vía Elementor)
- [x] **PYS imágenes QUIC.cloud — COMPLETO** (2026-06-23). La optimización estaba atascada en el primer ciclo (`Images Pulled: -`); un **"Send Optimization Request" manual** la destrabó y el auto-cron proceso todo. Final: detección 100% ("all gathered"), **1,077 imágenes optimizadas, 184.28 MB de reducción**, WebP generado (ej. Retatruida.png 283 KB → .webp 58 KB, −79%), originales re-comprimidos (Optimize Original ON). Purgado LSCache + Object Cache. Ajustes: Auto Request Cron ON, WebP ON, backups conservados.
- [x] **PYS PageSpeed medido** (móvil, 24 jun 2026 vía pagespeed.web.dev): **Rendimiento 78** · FCP 1.8s · **LCP 5.6s (🔴 cuello de botella)** · TBT 20ms · CLS 0 · SI 3.0s · Accesibilidad 96 · Best Practices 96 · **SEO 100**.
- [x] **PYS LCP mejorado** (2026-06-23): causa = fondo de página del home (`body.elementor-page-21`, `background-general.png` **216KB servido como PNG**; LiteSpeed reemplaza webp en `<img>` pero NO en fondos CSS). Fix = override en **Apariencia → Personalizar → CSS adicional** con `image-set()` (webp 29KB + fallback png), reversible, sin tocar Elementor. Resultado: **LCP 5.6s → 4.5s (−1.1s)**, Rendimiento **78 → 81**, CLS 0, SEO 100. Purgado LSCache.
- [ ] **PYS rendimiento a verde (90+)** — pendiente. Probado 2026-06-23: LiteSpeed "Load CSS Asynchronously" + CCSS Per URL (ON, config segura para Elementor), pero **el Critical CSS de QUIC.cloud no se generó** (quedó en cola "waiting for cron"; ~14 disparos de wp-cron no la procesaron). Sin CCSS no se aplicó el async (CSS siguió render-blocking, sin FOUC ni rotura). **REVERTIDO** al estado verificado por no poder validar el resultado en sesión. Reintentar con tiempo para que QUIC.cloud procese el CCSS (o "Run CCSS Queue Manually") y verificar el home antes de dejarlo. Render-blocking: 22 CSS de Elementor (post-*.css) es la palanca pendiente.
- [x] **Brand** en Product schema de los 16 — COMPLETO (2026-06-23). 5 Nutricost + 11 PyS, los 16 emiten `brand` en el schema. Dos partes: (1) términos asignados vía endpoint `/pys-assign-house-brand`; (2) **fix raíz** = activar **Rank Math → Ajustes Generales → WooCommerce → "Select Brand" = Marcas** (`product_brand`), que estaba en "Ninguno" — por eso el schema Product por defecto no emitía brand. Tras guardar + purgar LSCache + Object Cache, los 16 emiten. Marca propia: **PyS** para péptidos; **NAD+ es Nutricost** (corregido, su nombre lleva Nutricost).
- [x] **FAQPage schema** del Omega-3 (1151) — inyectado vía `rank_math_schema_FAQPage` (meta_data wc/v3), 6 Q&A integradas en el `@graph` de Rank Math como `["WebPage","FAQPage"]`. Verificado en HTML.
- [x] **Reactivar caché de objetos de Raditech** — hecho por el usuario al cierre

---

**MOC:** [[MOC SEO]] | [[MOC - Raditech]] | [[MOC - Ecosistema PTM-PYS]]
