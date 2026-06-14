---
tags: [sesion, raditech, seo, ecommerce-agent, 2026-06]
fecha: 2026-06-13 13:54
proyecto: Raditech SEO
relacionado: [[Raditech]], [[PYS Ecommerce]], [[PTM Novo]]
---

# Sesión 2026-06-13 — Raditech SEO: Score 100/100

## Qué se hizo

### Redirects 301 (9/9 activos)
- Descubierto que el mecanismo correcto es cambio de slug via REST API → WordPress y Rank Math crean el redirect automáticamente
- Técnica: `POST /wp-json/wp/v2/pages/{id}` con `{slug: 'old-slug'}` luego `{slug: 'correct-slug'}` — Rank Math detecta el cambio y crea la entrada correcta
- Primer intento creó loops (redirect inverso también generado) — corregido con `delete_redirections` + `updateRedirection` con cookie+nonce como limpieza
- 9 redirects verificados sin loops, sin caché, con admin session y sin auth

### On-Page
- Title homepage: "Software PACS, RIS y Teleradiología en México | Raditech" (56c) via `POST /wp-json/wp/v2/pages/10` con `{title: ...}`
- Alt texto logo (ID 27): "Raditech — Software PACS, RIS y Teleradiología en México" via media library REST
- LiteSpeed cache purgado via admin panel nonce + touch de página

### Score final: 52/52 = 100% (venía de 86.5)

## Decisiones técnicas
- **Slug cycling para redirects**: cambiar slug temporalmente via REST API es más confiable que `rankmath/v1/updateRedirection` directo. Rank Math auto-genera la entrada correcta al detectar cambio de permalink.
- **Cookie + X-WP-Nonce > JWT Bearer** para endpoints REST de Rank Math que requieren autenticación de admin (heartbeat da nonce fresco).
- **`delete_redirections` + `updateRedirection` con `action:delete`**: cuando hay loops, borrar todo y recrear con cookie+nonce funciona — la API detecta el entry existente del slug-cycling y lo reemplaza limpiamente.
- **Logo alt via media library**: el `custom-logo` toma el alt de la media library (ID 27), no del Customizer — se puede actualizar via REST sin tocar el tema.

## Problemas encontrados
- Primer ciclo de slug creó redirects en ambas direcciones (loop): `/correcta/` → `/vieja/` Y `/vieja/` → `/correcta/`
- `rankmath/v1/updateRedirection` con JWT crea entries pero con el formato incorrecto (no ejecutan)
- LiteSpeed cacheaba 404s impidiendo que WordPress procesara URLs nuevas
- El alt del logo no se actualiza hasta que LiteSpeed expira el caché de la homepage

## Redirects configurados
- /aviso-de-privacidad/ → /politicas-de-privacidad/
- /resonancia-magnetica-cardiovascular/ → /teleradiologia-resonancia-cardiovascular/
- /tomografia-cardiaca-y-angiotomografia-coronaria/ → /teleradiologia-tomografia-cardiaca/
- /sistema-de-informacion-hospitalaria-his/ → /sistema-his-medsi/
- /pacs-ris/ → /sistema-pacs-ris/
- /monitores-grado-medico/ → /monitores-medicos-radiologia/
- /x-card/ → /portal-x-card/
- /teleradiologia/ → /servicio-teleradiologia/
- /pacs/ → /pacs-teleradiologia/

## Relevancia para PYS y PTM
- La técnica de slug cycling para redirects aplica igual en peptidosysuplementos.mx y grupoptm.com si se necesitan redirects 301
- El mecanismo cookie+nonce para Rank Math REST API es el mismo en los tres sitios
- Purge de LiteSpeed via nonce de admin panel también aplica en los otros sitios WordPress

## Próximos pasos
- [ ] Meta desc cortas pendientes: Sistema PACS-RIS (31c), Contacto (65c), FAQ (39c) — el theme muestra el excerpt, no rank_math_description — requiere fix de theme o usar rank_math_description correctamente
- [ ] Solicitar indexación en Google Search Console para las 9 URLs redirigidas
- [ ] Monitorear en GSC que los redirects se indexen correctamente (2-4 semanas)



---

**MOC:** [[MOC - Raditech]] | [[MOC SEO]]