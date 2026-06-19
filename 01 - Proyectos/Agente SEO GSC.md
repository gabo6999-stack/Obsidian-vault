---
tags: [proyecto, pys, agente, seo, google-search-console]
estado: Configurado
ruta: C:\Users\gabom\ecommerce-agent\
---

# Agente SEO — Google Search Console

> Estado: 🟢 Configurado (OAuth + Service Account)
> Desplegado en Railway — ecommerce-agent

## Descripción

Agente Flask que consume Google Search Console para analizar rendimiento de palabras clave, páginas y oportunidades SEO en peptidosysuplementos.mx. También puede operar en [[grupoptm.com]] vía API WordPress.

## Autenticación GSC

| Método | Uso |
|--------|-----|
| OAuth 2.0 | Acceso interactivo |
| Service Account | Headless / Railway (automático) |

## Capacidades

- Análisis de queries (CTR, impresiones, posición promedio)
- Páginas con mejor/peor rendimiento
- Oportunidades: keywords en posición 4-20
- Comparación de períodos
- Exportar a CSV/JSON

## Operación en WordPress (PYS y PTM)

El agente puede escribir en dos sitios WordPress:

| Sitio | Variable | Estado |
|-------|----------|--------|
| peptidosysuplementos.mx | WP_USER / WP_PASSWORD | ✅ Activo |
| grupoptm.com | PTM_WP_USER / PTM_WP_PASSWORD + PTM_URL | ✅ Configurado en Railway |

Convenciones Elementor (ambos sitios):
- Solo modificar `title`, `meta`, `slug`, `rank_math_title`, `rank_math_description` vía API
- Nunca tocar el `content` de páginas Elementor

## Integración con otros agentes

- Alimenta al [[Agente de Blogs]] con temas de alto potencial (keywords posición 4-15)
- Páginas con CTR bajo → candidatos para `/edit`

## Notas importantes

- Sitios usan **Rank Math** (no Yoast) — campos: `rank_math_title`, `rank_math_description`
- `PTM_URL` default en código es `https://grupoptm.com` — correcto, no requiere variable Railway explícita



---

## Sesion 2026-06-09 - Raditech GSC
Token OAuth separado (RADITECH_GSC_REFRESH_TOKEN) configurado en Railway. Funciones gsc_raditech_* desbloqueadas. Analisis GSC ejecutado: branded keywords dominan, 4 redirects 301 activos en URLs viejas, landing hospital-elipse retirada. Sistema-his-medsi pendiente indexacion.