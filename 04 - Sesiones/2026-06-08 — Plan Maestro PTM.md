---
tags: [sesion, grupoptm, ptm-novo, 2026-06]
fecha: 2026-06-08 02:11
proyecto: grupoptm.com
---

# Sesión 2026-06-08 — Plan Maestro PTM (grupoptm.com)

## Qué se hizo
- Definido el plan maestro completo para construir grupoptm.com desde cero con WordPress en Hostinger
- Identificado el rol de cada plataforma: grupoptm.com = marketing/SEO, PTM Novo Railway = donde el cliente actúa
- Definidas las 5 landings estratégicas con keywords objetivo y estructura de contenido
- Diseñado el flujo de trabajo: Elementor para diseño visual + agente SEO para optimización SEO
- Documentado el proceso de conexión del agente SEO a grupoptm.com (variables Railway pendientes)
- Identificado bug crítico: PTM_URL en código del agente tiene default grupoptm.com pero dominio real es grupoptm.com
- Estructurado el plan de cross-linking PTM ↔ PYS con instrucciones concretas para el agente SEO

## Decisiones técnicas
- Usar Rank Math en grupoptm.com (igual que PYS — el agente ya sabe usarlo, mismos campos rank_math_title/rank_math_description)
- Elementor Free + tema Astra/Hello Elementor para construcción visual de landings
- JWT Authentication for WP REST API para que el agente SEO pueda escribir en el sitio
- Patrón Elementor igual que PYS: agente solo modifica title/meta/slug/rank_math, nunca content
- PTM_URL debe ser https://grupoptm.com (agregar como variable Railway, no usar el default del código)

## Problemas encontrados
- El código del agente SEO tiene default PTM_URL = 'https://grupoptm.com' pero el dominio del usuario es grupoptm.com — requiere variable Railway explícita
- 3 variables Railway pendientes antes de poder usar el agente en grupoptm.com: PTM_WP_USER, PTM_WP_PASSWORD, PTM_URL
- Plugins WP pendientes de instalar en grupoptm.com: JWT Auth, Rank Math

## Próximos pasos
- [x] Instalar JWT Authentication for WP-API en grupoptm.com
- [ ] Editar wp-config.php: JWT_AUTH_SECRET_KEY y JWT_AUTH_CORS_ENABLE=true
- [ ] Instalar Rank Math en grupoptm.com
- [ ] Crear Application Password en WP admin → agregar al agente SEO
- [x] Agregar en Railway ecommerce-agent: PTM_WP_USER, PTM_WP_PASSWORD, PTM_URL=https://grupoptm.com
- [ ] Crear las 5 páginas en WP con título y slug correcto
- [ ] Verificar con get_ptm_pages() desde el chat del agente SEO
- [ ] Diseñar cada landing en Elementor
- [ ] Optimizar SEO de las 5 landings via agente (update_ptm_page)
- [ ] Activar cross-links PTM -> PYS y PYS -> PTM


---

**MOC:** [[MOC - Ecosistema PTM-PYS]]
