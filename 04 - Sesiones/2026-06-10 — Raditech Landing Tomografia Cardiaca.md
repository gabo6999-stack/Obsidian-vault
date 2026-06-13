---
tags: [sesion, raditech, 2026-06]
fecha: 2026-06-10 12:17
proyecto: Raditech
---

# Sesión 2026-06-10 — Raditech Landing Tomografía Cardiaca

## Qué se hizo
- Identificada landing ID 894: `/teleradiologia-tomografia-cardiaca/`
- Eliminada sección CTA completa: "Sin costo de instalación ni equipamiento adicional" (687 chars)
- Eliminado FAQ item: "¿Cuánto tiempo tarda el reporte de tomografía cardiaca?"
- Eliminado FAQ item: "¿El reporte incluye post-procesamiento 3D?"
- Eliminadas esas 2 preguntas del JSON-LD schema FAQPage
- HTML reducido de 32,777 → 29,355 caracteres. Cambios confirmados vía WP REST API

## Decisiones técnicas
- Chat del agente SEO timoueó 3 veces con HTML de 30KB+ → flujo alternativo directo
- **Flujo validado:** `GET https://raditech.mx/wp-json/wp/v2/pages/{id}` → edición PowerShell → `POST /raditech-page-update` bypass
- Eliminaciones con `.Replace()` sobre substrings exactos por índice
- JSON-LD parcheado con `ConvertFrom-Json` → filter → `ConvertTo-Json`
- Para verificar cambios inmediatos: volver a llamar la API REST de WP (no WebFetch que tiene caché 15 min)

## Problemas encontrados
- Agente SEO chat endpoint timoueó (120s, 180s, 240s) con HTML grande — workaround documentado
- WebFetch tiene caché de 15 min, no sirve para verificación inmediata post-edición
- Clase CSS de la CTA era `rtac-cta` (no `ptm-band` como en otras landings del ecosistema)

## Próximos pasos
- [ ] Optimizar SEO landing 894: agregar meta title + meta description en Rank Math
- [ ] Revisar landings pendientes: 892 (resonancia cardiovascular), 871, 881, 879
- [ ] Nota: página 892 `/teleradiologia-resonancia-cardiovascular/` ya está publicada (confirmado esta sesión)
