---
tags: [proyecto, pys, agente, blogs, seo, railway]
estado: Activo
ruta: C:\Users\gabom\agente-blogs\
---

# Agente de Blogs

> Publica 4 blogs/semana en peptidosysuplementos.mx automáticamente.

## Descripción

Agente autónomo que detecta tendencias de Google, redacta blogs optimizados para SEO con Claude AI, añade imagen de Unsplash y publica en WordPress.

**Schedule:** Lunes, Martes, Jueves, Viernes @ 9am

## Stack

| Componente | Tecnología |
|-----------|-----------|
| Tendencias | Google Trends (PyTrends) |
| Redacción | Claude API (`writer.py`) |
| Imágenes | Unsplash API |
| Publicación | WordPress REST API + Rank Math |
| Deploy | Railway (`Procfile`) |
| Log | `logs/blog_log.json` |

## Estructura

```
agente-blogs/
├── .env                  ← credenciales
├── config.py             ← configuración de sitios
├── pipeline.py           ← pipeline principal + scheduler
├── test_run.py           ← prueba manual (1 blog ahora)
├── prompts/system.py     ← prompt del agente escritor
├── tools/
│   ├── trends.py
│   ├── writer.py         ← Claude API + web_search
│   ├── images.py
│   ├── wordpress.py      ← WP REST API + Rank Math
│   └── logger.py
└── logs/blog_log.json
```

## Patrones Importantes

- `/image` → solo genera imagen (query en **inglés**)
- `/edit` → edita contenido existente
- **Nunca mezclar** `/image` y `/edit` en el mismo comando
- Siempre `rank_math_title` + `rank_math_description` (no Yoast)

## Variables de Entorno

```env
SITE1_WP_URL=https://peptidosysuplementos.mx
SITE1_WP_USER=usuario
SITE1_WP_PASSWORD=password
ANTHROPIC_API_KEY=sk-ant-xxx
UNSPLASH_ACCESS_KEY=access_key
```

## Comandos

```bash
python test_run.py    # publicar 1 blog ahora (prueba)
python pipeline.py    # iniciar agente autónomo
```
