---
aliases: [raditech.mx, RadiTech]
tags: [proyecto, raditech, seo, wordpress]
estado: Activo
sitio: https://raditech.mx
agente-seo: https://web-production-3743c.up.railway.app
agente-blogs: https://web-production-2809e.up.railway.app
---

# Raditech

> Estado: 🟢 Activo
> Nicho B2B: Sistemas PACS/RIS, Teleradiología, Monitores médicos para hospitales/centros de diagnóstico

## Stack
- WordPress en Hostinger
- Elementor + tema Astra
- Rank Math SEO
- Agente SEO (ecommerce-agent) + Agente de Blogs (agente-blogs-raditech)

## Páginas clave (landings de servicios)

| ID | Slug | Estado SEO |
|----|------|------------|
| 883 | /sistema-pacs-ris/ | ✅ Optimizado |
| 874 | /teleradiologia-alta-especialidad/ | ✅ Optimizado |
| 890 | /sistema-his-medsi/ | ✅ Optimizado |
| 879 | /monitores-medicos-radiologia/ | ⏳ Pendiente |
| 871 | /servicio-teleradiologia/ | ⏳ Pendiente |
| 881 | /portal-x-card/ | ⏳ Pendiente |
| 894 | /teleradiologia-tomografia-cardiaca/ | ⏳ Pendiente SEO (contenido OK) |
| 892 | /teleradiologia-resonancia-cardiovascular/ | ⏳ Pendiente |
| 10 | / (home) | ⏳ Pendiente — remover referencia Grupo PTM |

## Flujo correcto para editar landings con HTML grande (>30KB)

El chat del agente SEO timoueó con HTML de 30KB+. Flujo alternativo validado:
1. `GET https://raditech.mx/wp-json/wp/v2/pages/{id}` — API pública sin auth
2. Editar HTML localmente en PowerShell
3. `POST https://web-production-3743c.up.railway.app/raditech-page-update`
4. Verificar con la misma API (no WebFetch — caché 15 min)

---

## Sesión 2026-06-10
Eliminadas 3 secciones de la landing 894 (teleradiología tomografía cardiaca): CTA "Sin costo de instalación", y 2 FAQs (tiempo de reporte + post-procesamiento 3D). JSON-LD schema también limpiado. HTML: 32,777 → 29,355 chars.



---

## Sesión 2026-06-13 — SEO 100/100
Score SEO llevado de 86.5 a 100/100 (52/52 checks). 9 redirects 301 activos sin loops. Title homepage corregido (56c), alt texto logo actualizado, caché LiteSpeed purgado. Técnica clave: slug cycling via REST API para que Rank Math genere redirects correctamente. Ver [[2026-06-13 - Raditech SEO Score 100]].


---

## Sesion 2026-06-13
Identificada firma vieja en posts del blog: 'Dr. Antonio Gavito Hernandez - Medico Radiologo - Especialista PACS-RIS'. Pendiente eliminarla de todos los posts.


---

## Sesión 2026-06-14
Revisada firma 'Médico Radiólogo / PACS-RIS' en posts de blog. Usuario decidió mantenerla — no se elimina.


---

## Sesión 2026-06-14 — Redirects 301 (corrección)
**Corrección importante:** la nota "SEO 100/100 / 9 redirects activos" del 13-jun era inexacta — 7 de 8 redirects daban 404. Causa: `_wp_old_slug` es meta protegido y la REST API ignoraba los writes. Solución: snippet WPCode PHP "Redirects 301 old-slugs Raditech" (ID 932) con `template_redirect` + mapa + `wp_safe_redirect(...,301)`. **8/8 verificados (anónimo y logueado)**. También: eliminado el snippet roto 931 que imprimía PHP como texto en el sitio, y purgada la caché LiteSpeed. Gotchas: WPCode requiere 2.º guardado para ejecutar; la caché privada de LiteSpeed engaña al admin. Ver [[2026-06-14 — Raditech]].


---

## Sesión 2026-06-24 — Fix canibalización (header/footer) + loop 874
Diagnosticada canibalización: el tema custom (markup `rt-`, inyectado vía WPCode snippets 867 Header / 870 Footer, NO archivos de tema) enlazaba a slugs viejos en cada página. Migrados los 6 slugs viejos→nuevos en ambos snippets. Eliminado el loop 301 de la landing 874 (`/teleradiologia-alta-especialidad/`), causado por una regla de Rank Math Redirections que apuntaba a sí misma (148 hits) → ahora 200 OK. Purgado LiteSpeed, verificado anónimo. Pendiente: cards `rt-btn` del home (página 10, contenido Elementor). Ver [[2026-06-24 — Raditech]].