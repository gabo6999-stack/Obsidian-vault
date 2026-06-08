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
