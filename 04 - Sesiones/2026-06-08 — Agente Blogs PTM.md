---
tags: [sesion, agente-blogs, grupoptm, multisitio, 2026-06]
fecha: 2026-06-08
proyecto: Agente de Blogs
---

# Sesión 2026-06-08 — Agente Blogs: PTM Multi-sitio

## Qué se hizo

- Revisado el agente de blogs: arquitectura ya era multi-sitio (todo keyed por `site_key`)
- Agregado `grupoptm` como segundo sitio en `config.py`
- `notify_seo_agent` convertida de global a site-aware (usa `seo_agent_url` por sitio)
- `grupoptm.seo_agent_url = None` (skip optimización SEO hasta agregar endpoint PTM)
- Resuelto conflict de rebase con cambios del remoto (remote había agregado `unsplash_fallback` y `edit_blog` features)
- Push a GitHub → Railway redeploy automático

## Decisiones técnicas

- Nicho grupoptm: "telemedicina de péptidos y salud hormonal en México"
- Keywords: telemedicina, semaglutide, retatrutide, GLP-1, consulta médica péptidos, etc.
- Mismo schedule que PYS: Lunes/Martes/Jueves/Viernes @ 9am
- `unsplash_fallback` para grupoptm: "telemedicine doctor consultation health"

## Pendiente

- [ ] Agregar en Railway (agente-blogs): `SITE2_WP_URL=https://grupoptm.com`, `SITE2_WP_USER`, `SITE2_WP_PASSWORD`
- [ ] Publicar 1 blog de prueba en grupoptm via dashboard
- [ ] Futuro: agregar `/optimize-ptm-blog` en ecommerce-agent y conectar `seo_agent_url` de grupoptm
