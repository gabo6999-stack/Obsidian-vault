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

## SEO post-publicación (implementado 2026-06-08)

- `/optimize-ptm-blog` agregado en ecommerce-agent
  - Interlinks: 2-4 links a otros posts de grupoptm.com
  - Cross-links: 2-3 links a productos de peptidosysuplementos.mx (estrategia PTM→PYS)
  - Links externos: 3-5 fuentes científicas (PubMed, NIH, Mayo Clinic)
  - CTA en conclusión → agendar consulta médica en grupoptm.com
- `seo_optimize_path` por sitio en config: PYS→`/optimize-blog`, PTM→`/optimize-ptm-blog`
- Ambos repos pusheados → Railway redesplegando

## Pendiente

- [ ] Publicar 1 blog de prueba en grupoptm y verificar que SEO se aplique automáticamente



---

**MOC:** [[MOC - Ecosistema PTM-PYS]]
