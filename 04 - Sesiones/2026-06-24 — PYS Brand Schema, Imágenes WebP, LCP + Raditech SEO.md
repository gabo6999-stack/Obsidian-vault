---
tags: [sesion, pys, raditech, seo, ecommerce-agent, performance, rank-math, litespeed, 2026-06]
fecha: 2026-06-24
proyecto: SEO PYS + Raditech
relacionado: [[PYS Ecommerce]], [[Raditech]], [[Ecommerce Agent]], [[2026-06-23 — PYS SKU y Precios, Raditech LCP, Omega-3 Elementor]]
---

# Sesión 2026-06-24 — PYS (Brand schema, WebP, LCP) + Raditech (SEO landings, autor)

> Continuación que **cerró los pendientes** de la sesión [[2026-06-23 — PYS SKU y Precios, Raditech LCP, Omega-3 Elementor]]. Trabajo vía agente SEO (Railway), REST API y **wp-admin directo** (Chrome) en peptidosysuplementos.mx y raditech.mx. Todo verificado contra HTML renderizado / BD, no contra la palabra del agente.

## Qué se hizo

### Raditech
- **Caché de objetos** reactivada (confirmado por el usuario).
- **5 landings optimizadas** (SEO title + meta description + focus keyword) vía `/raditech-page-update`: monitores-medicos-radiologia (879), servicio-teleradiologia (871), portal-x-card (881), teleradiologia-tomografia-cardiaca (894), teleradiologia-resonancia-cardiovascular (892). Verificado en HTML tras purgar LiteSpeed.
- **Home (id 10): autor corregido** de `enlace@grupoptm.com` → **Dr. Antonio Gavito Hernández** (user id 6). Elimina la referencia a grupoptm del JSON-LD. Requirió agregar soporte de `author` al endpoint.
- El ítem de menú "Acceso a Plataforma" → `pacs.grupoptm.com` **se conserva** (no se puede mover de dominio, por decisión del dueño).

### PYS — FAQPage Omega-3 (#3)
- Inyectado `rank_math_schema_FAQPage` con **6 Q&A** en el Omega-3 (producto 1151) vía `meta_data` de wc/v3 (HTTPBasicAuth). Queda integrado en el `@graph` de Rank Math como `["WebPage","FAQPage"]`. Verificado en HTML.

### PYS — Brand en Product schema de los 16 (#2)
- **Términos asignados:** 5 **Nutricost** + 11 **PyS** (marca propia decidida para péptidos). NAD+ corregido a Nutricost (su nombre lo lleva). Endpoint nuevo `/pys-assign-house-brand` (idempotente, dry_run).
- **Fix raíz (lo no obvio):** asignar el término NO basta. Rank Math no emitía el brand porque **Ajustes Generales → WooCommerce → "Select Brand" estaba en "Ninguno"**. Cambiado a **"Marcas"** (taxonomía `product_brand`) en wp-admin → ahora los 16 emiten brand. Ver [[pys-brand-schema]] (memoria).
- Verificado: productos que nunca toqué (Zinc) emiten brand tras el cambio = fix global, no parche.

### PYS — Imágenes QUIC.cloud + PageSpeed (#1)
- La optimización estaba **atascada en el primer ciclo** (`Images Pulled: -`); un "Send Optimization Request" manual la destrabó y el auto-cron procesó todo.
- Final: **1,077 imágenes optimizadas, 184.28 MB de reducción**, WebP generado, originales re-comprimidos (Optimize Original ON). Purgado LSCache + Object Cache.
- **PageSpeed móvil medido** (pagespeed.web.dev): Rendimiento **78**, FCP 1.8s, LCP 5.6s, TBT 20ms, CLS 0, SEO 100.

### PYS — Fix de LCP (mejora del hero)
- Causa: el **fondo de página del home** (`body.elementor-page-21`, `background-general.png` = **216 KB servido como PNG**). LiteSpeed reemplaza a WebP los `<img>` (reescribe el HTML) pero **NO los fondos CSS**.
- Fix: override en **Apariencia → Personalizar → CSS adicional** con `image-set()` (WebP 29 KB + fallback PNG), **reversible**, sin tocar el layout de Elementor.
- Resultado: **LCP 5.6s → 4.5s (−1.1s)**, Rendimiento **78 → 81**, fondo 216KB → 29KB (−87%).

### PYS — Async CSS (probado y REVERTIDO)
- Activado "Load CSS Asynchronously" + "CCSS Per URL" (config segura para Elementor), pero el **Critical CSS de QUIC.cloud no se generó** (cola "waiting for cron"; ~14 disparos de wp-cron no la procesaron). Sin CCSS no se aplica el async → CSS siguió render-blocking, **sin FOUC ni rotura**.
- **Revertido** al estado verificado por no poder validar en sesión. Reintentar con tiempo / "Run CCSS Queue Manually".

## Decisiones técnicas
- **Productos WooCommerce:** el **JWT da 401** en endpoints de edición/Rank Math de productos. Usar **WC_KEY/WC_SECRET (HTTPBasicAuth)** vía wc/v3 (`meta_data`, `brands`). El JWT solo sirve para posts/pages. Ver [[pys-brand-schema]].
- **Schema FAQPage en productos Elementor:** escribir meta `rank_math_schema_FAQPage` por `meta_data` (no por post_content, que Elementor no renderiza).
- **WebP en fondos CSS:** LiteSpeed no los convierte; usar `image-set()` en CSS adicional apuntando al `.png.webp` generado, con fallback PNG.
- **PSI API keyless:** sin cuota (429); medir en pagespeed.web.dev.

## Cambios en código (ecommerce-agent, en main)
- `/raditech-page-update` acepta `author`.
- `/pys-product-update` acepta `faq_main_entity`, `product_meta_data`, `brand_id`.
- Nuevo `/pys-assign-house-brand` (marca propia idempotente).
- Endpoint diagnóstico `/pys-debug-brands` agregado y luego **eliminado** (cumplió su función).

## Estado / próximos pasos
- [x] Raditech: caché objetos · 5 landings SEO · home autor (Dr. Antonio Gavito)
- [x] PYS: FAQPage Omega-3 · Brand 16/16 (5 Nutricost + 11 PyS) · imágenes WebP (1077, 184MB) · LCP 5.6→4.5s
- [ ] **PYS rendimiento a verde (90+):** render-blocking de 22 CSS de Elementor. Reintentar async CSS con CCSS generado (sesión supervisada). LCP residual 4.5s aún mejorable.
- [ ] **PYS:** re-medir PageSpeed cuando QUIC.cloud termine cualquier optimización residual.

---

**MOC:** [[MOC - Ecosistema PTM-PYS]] | [[MOC - Raditech]] | [[MOC SEO]]
